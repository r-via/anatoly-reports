# rustguard-kmod

> Linux kernel module implementing WireGuard in Rust with C shims — targets kernel 6.10+, no libwg dependency.

## Overview

`rustguard-kmod` is an out-of-tree Linux kernel module that brings the RustGuard WireGuard implementation into the kernel, eliminating the TUN bottleneck present in userspace deployments. The module is written in Rust with a minimal set of C shims that bridge kernel APIs — net device registration, UDP sockets, crypto API, workqueue, and genetlink — to the Rust protocol state machine.

The module stages `no_std`-compatible Rust sources from `rustguard-crypto` and `rustguard-core` via its Makefile and compiles them as part of the kernel module object, so no external crate linking is required at load time.

The C side handles kernel integration. The Rust side owns every aspect of the WireGuard protocol: Noise_IKpsk2 handshake, transport sessions, allowed-IP routing, anti-replay protection, DoS cookies, and session lifecycle timers.

## Directory Structure

```
rustguard-kmod/
├── Makefile           Build and source-staging pipeline
├── Kbuild             Object file manifest for the kernel build system
└── src/
    ├── lib.rs          Module entry, DeviceState/Peer, TX/RX paths, handshake dispatch
    ├── noise.rs        Noise_IKpsk2 handshake — initiator and responder
    ├── cookie.rs       DoS cookie mechanism — CookieChecker and CookieState
    ├── allowedips.rs   Compressed radix trie — cryptokey routing
    ├── replay.rs       2048-bit anti-replay sliding window (RFC 6479)
    ├── timers.rs       Session lifecycle state machine and timer constants
    ├── wg_socket.c     Zero-copy UDP socket, encap_rcv callback
    ├── wg_timer.c      250 ms periodic timer via delayed_work
    ├── wg_net.c        net_device registration, sk_buff helpers, module parameters
    ├── wg_queue.c      Per-CPU async crypto workqueue (FPU-safe)
    ├── wg_crypto.c     Kernel crypto API wrappers for ChaCha20-Poly1305, BLAKE2s, Curve25519
    └── wg_genl.c       Genetlink interface stubs (GET_DEVICE / SET_DEVICE)
```

## Architecture — Rust / C Split

The module follows a strict separation of concerns:

| Layer | Language | Responsibility |
|---|---|---|
| Protocol state machine | Rust | Noise handshake, transport sessions, routing, replay, cookies, timers |
| Kernel integration | C | `net_device`, UDP sockets, crypto API, workqueue, genetlink |
| FFI boundary | C → Rust | C shims call exported `#[no_mangle]` Rust functions |

This keeps unsafe kernel API surface isolated in C while the security-critical protocol logic remains in Rust with full type safety.

## Rust Modules

### `lib.rs` — Module Root and Data Paths

`lib.rs` defines the top-level module state and the two packet paths exposed to the C shims.

```rust
/// Per-device state owned by the kernel module.
pub struct DeviceState {
    pub static_key: StaticSecret,
    pub public_key: PublicKey,
    pub peer: Option<Peer>,
    pub handshake: Option<InitiatorState>,
    pub session: Option<TransportSession>,
    pub cookie_checker: CookieChecker,
    pub allowed_ips: AllowedIps,
    pub timers: SessionTimers,
}

/// Per-peer state.
pub struct Peer {
    pub public_key: PublicKey,
    pub psk: [u8; 32],
    pub endpoint_ip: [u8; 4],
    pub endpoint_port: u16,
}
```

The C shims call two primary Rust entry points:

| Function | Direction | Description |
|---|---|---|
| `rustguard_do_xmit(buf, len)` | TUN-in → UDP-out | Encrypts a plaintext IP packet and hands ciphertext back to C for UDP send |
| `rustguard_do_rx(buf, len)` | UDP-in → TUN-out | Decrypts an incoming WireGuard transport packet and writes plaintext to the net device |
| `rustguard_timer_tick()` | periodic | Advances the timer state machine; called every 250 ms from `wg_timer.c` |
| `rustguard_module_init(role, peer_ip, peer_port)` | init | Generates static key pair and seeds `DeviceState`; called from `wg_net.c` module init |

### `noise.rs` — Noise_IKpsk2 Handshake

`noise.rs` implements both sides of the 1-RTT Noise_IKpsk2 key exchange. The design mirrors `rustguard-core`'s handshake module but operates entirely in a `no_std` + kernel-allocator context.

| Function | Description |
|---|---|
| `create_initiation(state) -> Initiation` | Builds a handshake initiation; uses kernel RNG and `ktime_get_real_ts64` for TAI64N |
| `process_initiation(msg, state) -> Option<(PublicKey, Response, TransportKeys)>` | Responder path; MAC1 is verified before any DH operation |
| `process_response(msg, state) -> Option<TransportKeys>` | Completes the exchange on the initiator; consumes and zeroizes `InitiatorState` |

`TransportKeys` contains the derived send/receive key pair and both session indices.

### `cookie.rs` — DoS Cookie Mechanism

`CookieChecker` (server side) and `CookieState` (client side) implement WireGuard's load-based DoS protection. Under load, the responder issues a `CookieReply` instead of processing the handshake, forcing the initiator to prove IP reachability. The cookie secret rotates every 120 seconds using `get_random_bytes`.

```rust
impl CookieChecker {
    pub fn new_with(our_public: &PublicKey, secret: [u8; 32], now_ts: u64) -> Self;
    pub fn create_reply_from_bytes(
        receiver_index: u32,
        mac1: &[u8; 16],
        addr_bytes: &[u8],
    ) -> CookieReply;
    pub fn verify_mac1(msg_bytes: &[u8], mac1: &[u8; 16]) -> bool;
    pub fn verify_mac2_from_bytes(
        msg_with_mac1: &[u8],
        mac2: &[u8; 16],
        addr_bytes: &[u8],
    ) -> bool;
}

impl CookieState {
    pub fn process_reply(reply: &CookieReply, their_public: &PublicKey, mac1: &[u8; 16]);
    pub fn compute_mac2(msg_with_mac1: &[u8]) -> [u8; 16];
}
```

MAC1 is always verified before any expensive operation. MAC2 is required only when `CookieChecker::under_load` is `true`.

### `allowedips.rs` — Cryptokey Routing

A compressed radix trie providing longest-prefix-match routing from IP address to `Peer`. Supports IPv4 and IPv6 CIDR ranges. The current implementation supports up to 8 peers — sufficient for the module's point-to-point design.

```rust
pub struct AllowedIps { /* private trie nodes */ }

impl AllowedIps {
    pub fn new() -> Self;
    pub fn insert(&mut self, prefix: IpPrefix, peer_idx: usize);
    pub fn lookup(&self, addr: &[u8]) -> Option<usize>;
}
```

### `replay.rs` — Anti-Replay Window

2048-bit sliding window bitmap following RFC 6479. The window state is split into `check` and `update` — the bitmap is only updated after AEAD authentication succeeds, preventing window poisoning by unauthenticated packets.

```rust
pub struct ReplayWindow { /* 256-byte bitmap + counters */ }

impl ReplayWindow {
    pub fn check(&self, counter: u64) -> bool;
    pub fn update(&mut self, counter: u64);
}
```

### `timers.rs` — Session Lifecycle

Tracks per-peer timestamps and drives rekey/keepalive decisions. The kernel variant accepts explicit `now_ns: u64` values (from `ktime_get_ns`) rather than reading a system clock internally.

| Constant | Value | Meaning |
|---|---|---|
| `REKEY_AFTER_TIME` | 120 s | Initiate rekey after this session age |
| `REJECT_AFTER_TIME` | 180 s | Stop sending on sessions older than this |
| `REKEY_TIMEOUT` | 5 s | Retry pending handshake interval |
| `REKEY_AFTER_MESSAGES` | 2⁶⁰ − 1 | Rekey threshold by message count |
| `REKEY_ATTEMPT_TIME` | 90 s | Abandon handshake attempt after this |

## C Shims

### `wg_net.c` — net_device and Module Parameters

Registers a `net_device` named `wg0` and exposes module parameters:

| Parameter | Type | Default | Description |
|---|---|---|---|
| `peer_ip` | `charp` | — | Peer endpoint IPv4 address |
| `peer_port` | `ushort` | `51820` | Peer endpoint UDP port |
| `role` | `uint` | `0` | `0` = initiator, `1` = responder |
| `async_crypto` | `bool` | `false` | Enable per-CPU async workqueue for crypto |

### `wg_queue.c` — Async Crypto Workqueue

When `async_crypto=1`, encrypt and decrypt operations are dispatched to a per-CPU workqueue, keeping FPU operations in process context and enabling parallel crypto across cores via scatter-gather (`sg_init_one`, `sg_chain`).

### `wg_socket.c` — UDP Socket

Creates a bound UDP socket with an `encap_rcv` callback so WireGuard packets bypass the normal UDP stack and arrive directly at `rustguard_do_rx`. `wg_socket_send` implements zero-copy TX.

### `wg_crypto.c` — Kernel Crypto API Wrappers

Wraps `crypto_aead`, `crypto_shash`, and `crypto_kpp` subsystem calls. Provides:
- `wg_chacha20poly1305_encrypt` / `wg_chacha20poly1305_decrypt` (buffer and skb variants)
- `wg_xchacha20poly1305_encrypt` / `wg_xchacha20poly1305_decrypt` (cookie encryption)
- `wg_blake2s` / `wg_blake2s_hmac` / `wg_hkdf`
- `wg_curve25519` / `wg_curve25519_generate_public`
- `wg_get_random_bytes`, `wg_ktime_get_ns`, `wg_ktime_get_real_ts64`
- `wg_memzero_explicit`, `wg_crypto_memneq`

### `wg_genl.c` — Genetlink (stub)

Registers the `wireguard` genetlink family and defines policy for `GET_DEVICE` and `SET_DEVICE` commands. The handlers are stubs — full `wg(8)` tool interoperability is planned but not yet implemented.

### `wg_timer.c` — Periodic Timer

Schedules a 250 ms `delayed_work` that calls `rustguard_timer_tick()` in a kernel workqueue context. The timer reschedules itself while the device is up and cancels on module unload.

## Prerequisites

- Linux kernel **6.10 or later** with kernel headers installed
- Rust toolchain with `rustup target add` support for the kernel Rust ABI (`rustc` ≥ 1.78 recommended)
- `CONFIG_RUST=y` in the kernel build configuration
- Standard kernel module build tools: `make`, `gcc`, kernel header package

## Building and Loading

```bash
# Build the module against the running kernel
make -C /lib/modules/$(uname -r)/build M=$(pwd)/rustguard-kmod modules

# Load as initiator, connecting to 192.168.99.2:51820
sudo insmod rustguard-kmod/rustguard_kmod.ko \
  peer_ip=192.168.99.2 \
  peer_port=51820 \
  role=0 \
  async_crypto=1

# Check kernel log for handshake status
sudo dmesg | grep rustguard

# Remove the module
sudo rmmod rustguard_kmod
```

Load as responder (role=1) on the other host with no `peer_ip` — the responder learns the initiator's endpoint from the first incoming initiation.

```bash
sudo insmod rustguard-kmod/rustguard_kmod.ko role=1 async_crypto=1
```

## Examples

### Point-to-point tunnel between two hosts

```bash
# Host A (192.168.99.1) — responder
sudo insmod rustguard_kmod.ko role=1 async_crypto=1
sudo ip addr add 10.0.0.1/24 dev wg0
sudo ip link set wg0 up

# Host B (192.168.99.2) — initiator
sudo insmod rustguard_kmod.ko \
  peer_ip=192.168.99.1 \
  peer_port=51820 \
  role=0 \
  async_crypto=1
sudo ip addr add 10.0.0.2/24 dev wg0
sudo ip link set wg0 up

# Verify tunnel
ping -c 4 10.0.0.1
```

### Confirming async crypto workqueue

```bash
# Load with async_crypto enabled and watch per-CPU queue activity
sudo insmod rustguard_kmod.ko peer_ip=192.168.99.1 role=0 async_crypto=1
dmesg | grep -i "async\|queue\|rustguard"
```

## See Also

- [rustguard-core](05-Modules/11-rustguard-core.md) — Protocol crate whose `no_std` sources are staged into this module
- [rustguard-crypto](05-Modules/12-rustguard-crypto.md) — Cryptographic primitives (X25519, ChaCha20-Poly1305, BLAKE2s, HKDF, TAI64N)
- [rustguard-tun](05-Modules/07-rustguard-tun.md) — Userspace TUN/AF_XDP/io_uring device layer (alternative to kernel module)
- [rustguard-daemon](05-Modules/13-rustguard-daemon.md) — Standard `wg.conf` userspace tunnel mode
- [Data Flow](02-Architecture/03-Data-Flow.md) — End-to-end packet path through the kernel module
- [Design Decisions](02-Architecture/04-Design-Decisions.md) — Rationale for the Rust/C shim split and in-kernel approach
- [Build and Test](06-Development/02-Build-and-Test.md) — Kernel module build instructions and test infrastructure