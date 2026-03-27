# Public API

> Complete reference for all exported functions, types, and constants across the RustGuard crate library.

## Overview

RustGuard exposes its protocol implementation as a set of focused crates. Each crate has a narrow responsibility — cryptographic primitives, Noise handshake logic, TUN device management, configuration parsing, and enrollment. This page documents every public symbol worth calling from application code.

The crate hierarchy is:

```
rustguard-crypto   →   primitives (X25519, ChaCha20-Poly1305, BLAKE2s, TAI64N)
rustguard-core     →   protocol (handshake, session, replay, cookie, timers)
rustguard-tun      →   TUN device (macOS utun, Linux TUN, multi-queue, AF_XDP, io_uring)
rustguard-daemon   →   config file parser (wg.conf format)
rustguard-enroll   →   zero-config enrollment (serve + join)
```

---

## `rustguard-crypto`

### Key Types

#### `StaticSecret`

A long-lived X25519 private key. Zeroized on drop via `ZeroizeOnDrop`.

```rust
pub struct StaticSecret(_);
```

| Method | Signature | Description |
|--------|-----------|-------------|
| `random` | `() -> Self` | Generate using OS RNG (`std` only). |
| `random_from_rng` | `(rng: &mut impl CryptoRngCore) -> Self` | Generate with provided RNG. |
| `from_bytes` | `([u8; 32]) -> Self` | Deserialize from raw bytes. |
| `to_bytes` | `(&self) -> [u8; 32]` | Serialize to raw bytes. |
| `public_key` | `(&self) -> PublicKey` | Derive the corresponding public key. |
| `diffie_hellman` | `(&self, their_public: &PublicKey) -> SharedSecret` | Perform X25519 DH. |

#### `EphemeralSecret`

A single-use X25519 private key for handshake ephemeral keys. Zeroized on drop.

| Method | Signature | Description |
|--------|-----------|-------------|
| `random` | `() -> Self` | Generate using OS RNG (`std` only). |
| `random_from_rng` | `(rng: &mut impl CryptoRngCore) -> Self` | Generate with provided RNG. |
| `public_key` | `(&self) -> PublicKey` | Derive corresponding public key. |
| `diffie_hellman` | `(&self, their_public: &PublicKey) -> SharedSecret` | Perform X25519 DH. |

#### `PublicKey`

An X25519 public key (32 bytes). Uses constant-time equality via `subtle::ConstantTimeEq`.

| Method | Signature | Description |
|--------|-----------|-------------|
| `from_bytes` | `([u8; 32]) -> Self` | Construct from raw bytes. |
| `as_bytes` | `(&self) -> &[u8; 32]` | Borrow raw bytes. |

#### `SharedSecret`

The result of a DH operation. Zeroized on drop.

| Method | Signature | Description |
|--------|-----------|-------------|
| `as_bytes` | `(&self) -> &[u8; 32]` | Borrow raw bytes. |

#### `Tai64n`

A TAI64N timestamp (12 bytes: 8-byte TAI seconds + 4-byte nanoseconds). Used in handshake initiation to prevent replay attacks — each new initiation must carry a strictly greater timestamp than the last accepted one.

| Method | Signature | Description |
|--------|-----------|-------------|
| `now` | `() -> Self` | Create from system clock (`std` only). |
| `from_unix` | `(secs: u64, nanos: u32) -> Self` | Create from UNIX epoch values (`no_std` compatible). Nanoseconds are clamped to 999,999,999. |
| `from_bytes` | `([u8; 12]) -> Self` | Deserialize from wire bytes. |
| `as_bytes` | `(&self) -> &[u8; 12]` | Serialize to wire bytes. |
| `is_after` | `(&self, other: &Tai64n) -> bool` | Returns `true` if `self` is strictly after `other`. |

### AEAD Functions

```rust
pub const AEAD_TAG_LEN: usize = 16;
pub const MAX_PACKET_SIZE: usize = 1516; // 1500 + AEAD_TAG_LEN
```

| Function | Signature | Description |
|----------|-----------|-------------|
| `seal` | `(key: &[u8;32], counter: u64, aad: &[u8], plaintext: &[u8]) -> Vec<u8>` | ChaCha20-Poly1305 encrypt. Allocates. |
| `open` | `(key: &[u8;32], counter: u64, aad: &[u8], ciphertext: &[u8]) -> Option<Vec<u8>>` | ChaCha20-Poly1305 decrypt. Returns `None` on auth failure. |
| `seal_to` | `(key: &[u8;32], counter: u64, plaintext: &[u8], buf: &mut [u8]) -> usize` | Zero-alloc encrypt into caller-supplied buffer. Buffer must be ≥ `plaintext.len() + 16`. Returns ciphertext length. |
| `open_to` | `(key: &[u8;32], counter: u64, buf: &mut [u8], ct_len: usize) -> Option<usize>` | Zero-alloc decrypt in place. Returns plaintext length on success. |
| `xseal` | `(key: &[u8;32], nonce: &[u8;24], aad: &[u8], plaintext: &[u8]) -> Vec<u8>` | XChaCha20-Poly1305 encrypt (24-byte nonce). Used for cookie encryption. |
| `xopen` | `(key: &[u8;32], nonce: &[u8;24], aad: &[u8], ciphertext: &[u8]) -> Option<Vec<u8>>` | XChaCha20-Poly1305 decrypt. |

The nonce for `seal`/`open` follows the WireGuard convention: 4 zero bytes followed by the 8-byte little-endian counter.

### Hash Functions

| Function | Signature | Description |
|----------|-----------|-------------|
| `hash` | `(data: &[&[u8]]) -> [u8; 32]` | BLAKE2s-256. Accepts multiple chunks concatenated. |
| `mac` | `(key: &[u8], data: &[&[u8]]) -> [u8; 32]` | BLAKE2s-256 keyed MAC (built-in keying mode). Used for WireGuard MAC1/MAC2; callers truncate to 16 bytes. |
| `hkdf` | `(key: &[u8;32], input: &[u8]) -> ([u8;32], [u8;32], [u8;32])` | HKDF using HMAC-BLAKE2s (RFC 2104). Returns `(T1, T2, T3)`. |

> **Note:** `mac` uses BLAKE2s's built-in keying mode. `hkdf` uses full HMAC-BLAKE2s (ipad/opad construction). These produce different outputs — `hkdf` is the correct construction for WireGuard's KDF.

### Protocol Constants

```rust
pub const CONSTRUCTION: &[u8] = b"Noise_IKpsk2_25519_ChaChaPoly_BLAKE2s";
pub const IDENTIFIER:   &[u8] = b"WireGuard v1 zx2c4 Jason@zx2c4.com";
pub const LABEL_MAC1:   &[u8] = b"mac1----";
pub const LABEL_COOKIE: &[u8] = b"cookie--";
```

---

## `rustguard-core`

### `handshake` module

Implements the Noise_IKpsk2 1-RTT handshake. Initiator creates an `Initiation` message and retains `InitiatorHandshake` state; after receiving the `Response` both sides derive a `TransportSession`.

| Function | Signature | Description |
|----------|-----------|-------------|
| `create_initiation` | `(our_static, their_public, sender_index) -> (Initiation, InitiatorHandshake)` | Create initiation (std, zero PSK). |
| `create_initiation_psk` | `(our_static, their_public, sender_index, psk) -> (Initiation, InitiatorHandshake)` | Create initiation with PSK (std). |
| `create_initiation_with` | `(our_static, their_public, sender_index, psk, ephemeral, timestamp) -> (Initiation, InitiatorHandshake)` | Create initiation with explicit ephemeral and timestamp. `no_std` compatible. |
| `process_response` | `(state: InitiatorHandshake, our_static, msg: &Response) -> Option<TransportSession>` | Complete the initiator side. Returns `None` on any verification failure. |
| `process_initiation` | `(our_static, msg: &Initiation, responder_index) -> Option<(PublicKey, Tai64n, Response, TransportSession)>` | Process initiation on responder side (std, zero PSK). |
| `process_initiation_psk` | `(our_static, msg, responder_index, psk, last_timestamp) -> Option<(PublicKey, Tai64n, Response, TransportSession)>` | Process initiation with PSK and timestamp replay check (std). |
| `process_initiation_with` | `(our_static, msg, responder_index, psk, last_timestamp, resp_eph) -> Option<(PublicKey, Tai64n, Response, TransportSession)>` | Process initiation with explicit ephemeral. `no_std` compatible. |
| `compute_mac1` | `(responder_public: &PublicKey, msg_bytes: &[u8]) -> [u8; 16]` | Compute MAC1 over a message for the given responder public key. |

`InitiatorHandshake` is `ZeroizeOnDrop` — key material is erased when the struct is dropped or consumed by `process_response`.

### `session` module — `TransportSession`

A symmetric transport session derived from a completed handshake. Holds send/receive keys and an anti-replay window.

```rust
pub struct TransportSession {
    pub our_index: u32,
    pub their_index: u32,
    // keys and replay window are private
}
```

| Method | Signature | Description |
|--------|-----------|-------------|
| `new` | `(our_index, their_index, key_send, key_recv) -> Self` | Construct from raw keys. Normally obtained from `handshake::process_response` or `process_initiation`. |
| `encrypt` | `(&mut self, plaintext: &[u8]) -> Option<(u64, Vec<u8>)>` | Encrypt a plaintext packet. Returns `(counter, ciphertext)`. Returns `None` when the nonce counter is exhausted (rekey required). |
| `decrypt` | `(&mut self, counter: u64, ciphertext: &[u8]) -> Option<Vec<u8>>` | Decrypt. Checks the replay window before AEAD; marks counter as seen only after authentication succeeds. Returns `None` on replay or auth failure. |
| `encrypt_to` | `(&mut self, plaintext: &[u8], out: &mut [u8]) -> Option<(u64, usize)>` | Zero-alloc encrypt into caller buffer. Buffer must be ≥ `plaintext.len() + 16`. Returns `(counter, ct_len)`. |
| `decrypt_in_place` | `(&mut self, counter: u64, buf: &mut [u8], ct_len: usize) -> Option<usize>` | Zero-alloc decrypt in place. Returns plaintext length. |
| `send_counter` | `(&self) -> u64` | Current outgoing counter value. |

### `replay` module — `ReplayWindow`

A 2048-bit sliding window bitmap for anti-replay protection (RFC 6479 / WireGuard algorithm).

| Method | Signature | Description |
|--------|-----------|-------------|
| `new` | `() -> Self` | Create a fresh empty window. |
| `check` | `(&self, counter: u64) -> bool` | Returns `true` if the counter would be accepted. Does **not** mark it as seen. |
| `update` | `(&mut self, counter: u64)` | Mark a counter as seen. Call only after the packet is authenticated. |
| `check_and_update` | `(&mut self, counter: u64) -> bool` | Combined check + update. |

### `cookie` module

DoS protection via the WireGuard cookie mechanism. The server side uses `CookieChecker`; the client side uses `CookieState`.

#### `CookieChecker` (server side)

```rust
pub struct CookieChecker {
    pub under_load: bool,
    // ...
}
```

| Method | Signature | Description |
|--------|-----------|-------------|
| `new` | `(our_public: PublicKey) -> Self` | Create with OS-random secret (`std` only). |
| `new_with` | `(our_public, secret, now_ts) -> Self` | Create with explicit secret and timestamp (no_std / testing). |
| `create_reply` | `(&mut self, receiver_index, mac1, src: &SocketAddr) -> CookieReply` | Build a Cookie Reply for a client address (`std` only). |
| `create_reply_from_bytes` | `(&mut self, receiver_index, mac1, addr_bytes) -> CookieReply` | Build a Cookie Reply from pre-encoded address bytes (`no_std` compatible). |
| `verify_mac1` | `(&self, msg_bytes, mac1) -> bool` | Constant-time verify MAC1 on an incoming message. |
| `verify_mac2` | `(&mut self, msg_with_mac1, mac2, src: &SocketAddr) -> bool` | Verify MAC2 (`std` only). |
| `verify_mac2_from_bytes` | `(&mut self, msg_with_mac1, mac2, addr_bytes) -> bool` | Verify MAC2 from pre-encoded address bytes. |

The cookie secret rotates every 120 seconds automatically on `create_reply` / `verify_mac2` calls.

#### `CookieState` (client side)

| Method | Signature | Description |
|--------|-----------|-------------|
| `new` | `() -> Self` | Create with no stored cookie. |
| `process_reply` | `(&mut self, reply: &CookieReply, their_public, mac1) -> bool` | Decrypt and store the cookie from a Cookie Reply. Returns `false` on decryption failure. |
| `compute_mac2` | `(&self, msg_with_mac1: &[u8]) -> [u8; 16]` | Compute MAC2 using the stored cookie. Returns zeros if no fresh cookie is available. |

### `timers` module — `SessionTimers`

Session lifecycle timers based on the WireGuard whitepaper (section 6).

**Constants:**

| Constant | Value | Meaning |
|----------|-------|---------|
| `REKEY_AFTER_TIME` | 120 s | Initiate rekey after this session age. |
| `REJECT_AFTER_TIME` | 180 s | Reject sends after this session age. |
| `REKEY_AFTER_MESSAGES` | 2⁶⁰ − 1 | Initiate rekey after this many messages. |
| `REJECT_AFTER_MESSAGES` | u64::MAX − 2¹³ | Reject sends after this many messages. |
| `REKEY_ATTEMPT_TIME` | 90 s | Give up on a handshake after this long. |
| `DEAD_SESSION_TIMEOUT` | 240 s | Zero session keys after this long idle. |

**`SessionTimers` methods (`std` variants shown; `no_std` variants accept an explicit `now_ns: u64`):**

| Method | Description |
|--------|-------------|
| `new()` | Create with all timestamps unset. |
| `session_started()` | Record that a session was established. |
| `packet_sent()` | Record that a packet was sent. |
| `packet_received()` | Record that an authenticated packet was received. |
| `needs_rekey(send_counter)` | Whether to initiate a new handshake. |
| `is_expired(send_counter)` | Whether the session is too old to use for sending. |
| `is_dead()` | Whether session keys should be zeroed. |
| `needs_keepalive(interval)` | Whether to send a keepalive probe. |
| `should_retry_handshake()` | Whether to retry a pending handshake initiation. |
| `handshake_timed_out()` | Whether to give up on the current handshake attempt. |

### `messages` module

Wire format structs and serialization.

| Symbol | Size | Description |
|--------|------|-------------|
| `Initiation` | 148 bytes | Handshake initiation (type 1). |
| `Response` | 92 bytes | Handshake response (type 2). |
| `CookieReply` | 64 bytes | Cookie Reply (type 3). |
| `Transport` | variable | Transport data (type 4). Payload is `Vec<u8>`. |
| `MSG_INITIATION` | `u32 = 1` | Wire type tag. |
| `MSG_RESPONSE` | `u32 = 2` | Wire type tag. |
| `MSG_COOKIE_REPLY` | `u32 = 3` | Wire type tag. |
| `MSG_TRANSPORT` | `u32 = 4` | Wire type tag. |
| `INITIATION_SIZE` | 148 | Wire byte size. |
| `RESPONSE_SIZE` | 92 | Wire byte size. |
| `COOKIE_REPLY_SIZE` | 64 | Wire byte size. |
| `TRANSPORT_HEADER_SIZE` | 16 | Header-only size (type + receiver + counter). |

All structs implement `to_bytes()` for serialization and `from_bytes()` for deserialization.

---

## `rustguard-tun`

### `Tun`

A TUN device. On macOS: `utun` via the kernel control socket. On Linux: `/dev/net/tun` with `IFF_TUN | IFF_NO_PI`.

| Method | Signature | Description |
|--------|-----------|-------------|
| `create` | `(config: &TunConfig) -> io::Result<Self>` | Create and configure a new TUN device. |
| `read` | `(&self, buf: &mut [u8]) -> io::Result<usize>` | Read one IP packet. Strips the 4-byte AF header on macOS. |
| `write` | `(&self, packet: &[u8]) -> io::Result<usize>` | Write one IP packet. Prepends the AF header on macOS automatically. |
| `name` | `(&self) -> &str` | Interface name (e.g. `utun3`, `tun0`). |
| `raw_fd` | `(&self) -> i32` | Raw file descriptor. Required for io_uring and AF_XDP integration. |

`Tun` implements `Drop` — the file descriptor is closed when the struct is dropped.

### `TunConfig`

```rust
pub struct TunConfig {
    pub name:        Option<String>,   // None = OS-assigned
    pub mtu:         u16,              // default: 1420
    pub address:     Ipv4Addr,
    pub destination: Ipv4Addr,         // point-to-point peer address
    pub netmask:     Ipv4Addr,
}
```

### Linux-only submodules (feature-gated by `#[cfg(target_os = "linux")]`)

| Module | Description |
|--------|-------------|
| `linux_mq::MultiQueueTun` | Multi-queue TUN for parallel I/O across CPU cores. |
| `uring::UringTun` | io_uring-based TUN reader for batched, low-latency I/O. |
| `xdp::XdpSocket` / `XdpConfig` | AF_XDP zero-copy socket. |
| `bpf_loader::XdpProgram` | Loads and attaches the BPF redirect program; registers XSK in XSKMAP. |

---

## `rustguard-daemon`

### `config` module

Parser for the standard WireGuard INI-style configuration format (`wg0.conf`).

#### `Config`

```rust
pub struct Config {
    pub interface: InterfaceConfig,
    pub peers:     Vec<PeerConfig>,
}
```

| Method | Signature | Description |
|--------|-----------|-------------|
| `from_file` | `(path: &Path) -> io::Result<Self>` | Read and parse a config file from disk. |
| `parse` | `(input: &str) -> io::Result<Self>` | Parse a config from a string. |

#### `InterfaceConfig`

```rust
pub struct InterfaceConfig {
    pub private_key:  [u8; 32],
    pub listen_port:  u16,             // default: 51820
    pub address:      Ipv4Addr,
    pub netmask:      Ipv4Addr,
    pub address_v6:   Option<(Ipv6Addr, u8)>,
}
```

#### `PeerConfig`

```rust
pub struct PeerConfig {
    pub public_key:           [u8; 32],
    pub preshared_key:        Option<[u8; 32]>,
    pub endpoint:             Option<SocketAddr>,
    pub allowed_ips:          Vec<CidrAddr>,
    pub persistent_keepalive: Option<u16>,
}
```

#### `CidrAddr`

```rust
pub struct CidrAddr {
    pub addr:       IpAddr,
    pub prefix_len: u8,
}
```

| Method | Description |
|--------|-------------|
| `contains_v4(ip: Ipv4Addr) -> bool` | Whether an IPv4 address falls within this CIDR. |
| `contains_v6(ip: Ipv6Addr) -> bool` | Whether an IPv6 address falls within this CIDR. |
| `contains(ip: IpAddr) -> bool` | Dispatches to the correct v4/v6 check. |

#### `prefix_to_netmask`

```rust
pub fn prefix_to_netmask(prefix: u8) -> Ipv4Addr
```

Converts a CIDR prefix length (0–32) to a dotted-quad netmask.

---

## `rustguard-enroll`

### `server::ServeConfig` and `server::run`

```rust
pub struct ServeConfig {
    pub listen_port:     u16,
    pub pool_network:    Ipv4Addr,
    pub pool_prefix:     u8,
    pub token:           String,
    pub open_immediately: bool,
    pub state_path:      Option<PathBuf>,
    pub xdp_ifname:      Option<String>,   // Linux only: AF_XDP interface
    pub tun_queues:      usize,            // 0 or 1 = single queue
    pub use_uring:       bool,             // Linux only: io_uring TUN I/O
}

pub fn run(config: ServeConfig) -> io::Result<()>
```

Starts the enrollment server. Blocks until the process is terminated. Creates a TUN device, binds a UDP socket, starts outbound and inbound I/O threads, and opens a UNIX domain control socket for `rustguard open`/`close`/`status`.

### `client::JoinConfig` and `client::run`

```rust
pub struct JoinConfig {
    pub server_endpoint: SocketAddr,
    pub token:           String,
}

pub fn run(config: JoinConfig) -> io::Result<()>
```

Performs the enrollment handshake with a `serve` server, receives an assigned IP, creates a TUN device, and runs the tunnel loop. Blocks until the process is terminated.

---

## Examples

### Complete handshake and transport round-trip

```rust
use rustguard_crypto::StaticSecret;
use rustguard_core::handshake;

fn main() {
    // Each peer generates a long-lived static key.
    let initiator_static = StaticSecret::random();
    let responder_static = StaticSecret::random();
    let responder_public = responder_static.public_key();

    // Initiator: create the Initiation message.
    let (init_msg, init_state) =
        handshake::create_initiation(&initiator_static, &responder_public, /*sender_index=*/ 1);

    // Responder: process the initiation, produce a Response.
    let (peer_pubkey, _timestamp, resp_msg, mut resp_session) =
        handshake::process_initiation(&responder_static, &init_msg, /*responder_index=*/ 2)
            .expect("valid initiation");

    assert_eq!(peer_pubkey, initiator_static.public_key());

    // Initiator: process the response, derive transport session.
    let mut init_session =
        handshake::process_response(init_state, &initiator_static, &resp_msg)
            .expect("valid response");

    // Encrypted data exchange.
    let plaintext = b"hello from the tunnel";
    let (counter, ciphertext) = init_session.encrypt(plaintext).unwrap();
    let decrypted = resp_session.decrypt(counter, &ciphertext).unwrap();
    assert_eq!(&decrypted, plaintext);
}
```

### Parsing a `wg.conf` file

```rust
use rustguard_daemon::config::Config;
use std::path::Path;

fn main() -> std::io::Result<()> {
    let config = Config::from_file(Path::new("/etc/wireguard/wg0.conf"))?;

    println!("listen port: {}", config.interface.listen_port);
    for peer in &config.peers {
        if let Some(ep) = peer.endpoint {
            println!("peer endpoint: {ep}");
        }
        for cidr in &peer.allowed_ips {
            println!("  allowed: {cidr}");
        }
    }
    Ok(())
}
```

### Creating a TUN device

```rust
use rustguard_tun::{Tun, TunConfig};
use std::net::Ipv4Addr;

fn main() -> std::io::Result<()> {
    let tun = Tun::create(&TunConfig {
        name:        None,
        mtu:         1420,
        address:     Ipv4Addr::new(10, 200, 0, 1),
        destination: Ipv4Addr::new(10, 200, 0, 2),
        netmask:     Ipv4Addr::new(255, 255, 255, 0),
    })?;

    println!("TUN interface: {}", tun.name());

    let mut buf = [0u8; 1500];
    let n = tun.read(&mut buf)?;
    println!("received {} bytes", n);
    Ok(())
}
```

### Zero-alloc AEAD for high-performance paths

```rust
use rustguard_crypto::{seal_to, open_to, AEAD_TAG_LEN};

fn encrypt_packet(key: &[u8; 32], counter: u64, plaintext: &[u8]) -> ([u8; 1516], usize) {
    let mut buf = [0u8; 1500 + AEAD_TAG_LEN];
    let ct_len = seal_to(key, counter, plaintext, &mut buf);
    (buf, ct_len)
}

fn decrypt_packet(key: &[u8; 32], counter: u64, buf: &mut [u8], ct_len: usize) -> Option<usize> {
    open_to(key, counter, buf, ct_len)
}
```

---

## See Also

- [Types and Interfaces](04-API-Reference/03-Types-and-Interfaces.md) — Detailed struct field documentation.
- [Configuration Schema](04-API-Reference/02-Configuration-Schema.md) — `wg.conf` field reference with defaults and validation rules.
- [rustguard-crypto](05-Modules/12-rustguard-crypto.md) — Crate-level module reference.
- [rustguard-core](05-Modules/11-rustguard-core.md) — Crate-level module reference.
- [rustguard-tun](05-Modules/07-rustguard-tun.md) — TUN device module reference.
- [rustguard-enroll](05-Modules/14-rustguard-enroll.md) — Enrollment crate module reference.
- [Data Flow](02-Architecture/03-Data-Flow.md) — How packets move through the crates at runtime.
- [Common Workflows](03-Guides/01-Common-Workflows.md) — Step-by-step operational guides.