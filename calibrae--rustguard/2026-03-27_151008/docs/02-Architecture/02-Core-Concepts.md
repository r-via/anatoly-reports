# Core Concepts

> Glossary and explanation of the key domain concepts, protocol terms, and security primitives used across RustGuard.

## Overview

RustGuard implements the WireGuard protocol from scratch across seven crates. This page defines the terminology used throughout the documentation and explains the cryptographic, protocol, and operational concepts that appear in architecture diagrams, API references, and guides. For structural context, see [System Overview](01-System-Overview.md). For how these concepts interact at runtime, see [Data Flow](03-Data-Flow.md).

---

## Cryptographic Primitives

All cryptographic operations reside in `rustguard-crypto`, a `no_std`-compatible crate shared by both the userspace daemon and the kernel module.

### X25519

Elliptic-curve Diffie–Hellman over Curve25519. Used during the Noise_IKpsk2 handshake to derive shared secrets from static and ephemeral key pairs.

- `StaticSecret` — long-lived peer identity key, 32 bytes, zeroized on drop.
- `EphemeralSecret` — single-use per-handshake key, 32 bytes, zeroized on drop.
- `PublicKey` — 32-byte Curve25519 point, compared in constant time.
- `SharedSecret` — 32-byte DH output, zeroized on drop.

Both initiator and responder generate a fresh `EphemeralSecret` per handshake. Static keypairs are long-lived peer identities shared out-of-band (or via enrollment).

### ChaCha20-Poly1305

Authenticated encryption with associated data (AEAD). All transport packets are encrypted with ChaCha20-Poly1305. The nonce is constructed as 4 zero bytes followed by the 8-byte little-endian session counter. The 16-byte Poly1305 authentication tag is appended to every ciphertext.

`AEAD_TAG_LEN = 16`

XChaCha20-Poly1305 — the extended-nonce variant with a 24-byte nonce — is used separately for cookie reply encryption.

`encrypt()` returns `Option` on nonce exhaustion rather than panicking.

### HMAC-BLAKE2s

RFC 2104 HMAC construction over BLAKE2s-256. Used as the pseudorandom function (PRF) throughout the HKDF key derivation chain and for computing MAC1 and MAC2 values on handshake messages.

> The initial implementation incorrectly used keyed BLAKE2s directly instead of the RFC 2104 ipad/opad construction; this produced non-interoperable KDF output and was corrected in the Phase 2 security audit. See [Design Decisions](04-Design-Decisions.md).

### HKDF

HMAC-based Key Derivation Function. Derives session keys, chaining keys, and intermediate key material from the handshake's running hash and DH outputs. All calls use HMAC-BLAKE2s as the underlying PRF and return three 32-byte outputs (`T1`, `T2`, `T3`).

### TAI64N

A 96-bit external timestamp: 8-byte TAI64 seconds (offset by `0x4000_0000_0000_000a`) followed by 4-byte nanoseconds, all big-endian.

`Tai64n` is embedded in handshake Initiation messages. The responder rejects any initiation whose timestamp is not strictly greater than the most recent accepted timestamp from that initiator — `is_after()` enforces this strict ordering — preventing replay of captured initiation packets.

### Protocol String Constants

```rust
// rustguard-crypto/src/lib.rs
pub const CONSTRUCTION: &[u8] = b"Noise_IKpsk2_25519_ChaChaPoly_BLAKE2s";
pub const IDENTIFIER:   &[u8] = b"WireGuard v1 zx2c4 Jason@zx2c4.com";
pub const LABEL_MAC1:   &[u8] = b"mac1----";
pub const LABEL_COOKIE: &[u8] = b"cookie--";
```

---

## Handshake Protocol

### Noise_IKpsk2

The WireGuard handshake pattern, encoded in the name `Noise_IKpsk2_25519_ChaChaPoly_BLAKE2s`:

| Token | Meaning |
|-------|---------|
| `I` | Initiator's static key is transmitted during the handshake (encrypted) |
| `K` | Responder's static key is known to the initiator in advance |
| `psk2` | Pre-shared key mixed in at the second message position |
| `25519` | X25519 for DH |
| `ChaChaPoly` | ChaCha20-Poly1305 for AEAD |
| `BLAKE2s` | BLAKE2s-256 for hash and PRF |

A PSK of all-zeroes is functionally equivalent to no PSK; the `psk2` modifier is backward-compatible with peers that do not configure a PSK.

### Initiator and Responder

The two roles in a Noise_IKpsk2 handshake. The **initiator** sends a type-1 Initiation message to open a new session. The **responder** replies with a type-2 Response. Either peer can act as initiator; roles are per-handshake, not per-peer.

`InitiatorHandshake` holds the handshake's running state (chaining key `ck`, hash `h`, ephemeral secret, PSK) and is zeroized on drop via `ZeroizeOnDrop`.

### Wire Message Types

| Constant | Value | Size |
|----------|-------|------|
| `MSG_INITIATION` | 1 | 148 bytes |
| `MSG_RESPONSE` | 2 | 92 bytes |
| `MSG_COOKIE_REPLY` | 3 | 64 bytes |
| `MSG_TRANSPORT` | 4 | 16-byte header + payload |

### MAC1 and MAC2

Two authentication codes present on every handshake message.

- **MAC1** — always present. An HMAC-BLAKE2s over the message using a key derived from the responder's public key. The responder verifies MAC1 **before** any DH computation, ensuring unauthenticated packets are rejected at minimal cost.
- **MAC2** — required when the responder is under load. A cookie-keyed MAC that proves the sender's IP address is reachable.

### Cookie Mechanism (DoS Protection)

The cookie mechanism is implemented by two types in `rustguard-core`:

- **`CookieChecker`** (server side) — holds a rotating random secret with a lifetime of `COOKIE_SECRET_LIFETIME = 120 s`. When `under_load` is set, it sends a type-3 Cookie Reply message encrypted with XChaCha20-Poly1305. The 16-byte cookie (`COOKIE_LEN = 16`) is derived from the client's IP address and the rotating secret.
- **`CookieState`** (client side) — stores the most recently received cookie. `compute_mac2()` includes it as MAC2 in subsequent handshake messages.

Cookie encoding includes the sender's IP and port: 6 bytes for IPv4, 18 bytes for IPv6.

### Sender Index and Receiver Index

Four-byte identifiers generated by a CSPRNG (`getrandom` crate) by each peer to tag its outbound sessions. The sender index is carried in every wire message. On receipt, the peer looks up the session by the receiver index it previously assigned.

---

## Transport Sessions

### Session

A pair of symmetric keys — `key_send` and `key_recv` — derived at the end of a successful handshake via HKDF. Each direction uses an independent 64-bit counter. Sessions are managed by `TransportSession` in `rustguard-core`.

```rust
pub struct TransportSession {
    pub our_index:   u32,    // Our sender index
    pub their_index: u32,    // Their sender index
    // key_send, key_recv: [u8; 32] — zeroized on drop
    // send_counter: u64
    // recv_window: ReplayWindow
}
```

### Send Counter and Nonce

A 64-bit integer incremented for every encrypted transport packet. A session is torn down and renegotiated before the counter reaches `REKEY_AFTER_MESSAGES = (1 << 60) - 1` to prevent nonce reuse. `encrypt()` returns `Option` and refuses once the counter reaches `REJECT_AFTER_MESSAGES = u64::MAX - (1 << 13)`.

### AllowedIPs

A per-peer set of CIDR prefixes. On the outbound path, the routing table selects the peer session whose AllowedIPs matches the destination IP. On the inbound path, the decrypted packet's source IP is verified against the sending peer's AllowedIPs; mismatches are dropped.

---

## Replay Protection

### Replay Window

A 2048-bit sliding bitmap maintained per `TransportSession` in `rustguard-core`. Each bit corresponds to one receive-counter position relative to the highest counter seen so far (`WINDOW_SIZE = 2048`). Duplicate or out-of-order packets within the window are detected and dropped. The algorithm follows RFC 6479, matching the Linux kernel WireGuard implementation.

The window exposes two distinct operations — by design:

| Operation | Timing | Purpose |
|-----------|--------|---------|
| `check(counter)` | Before AEAD decryption | Reject already-seen counters without mutating state |
| `update(counter)` | After successful AEAD decryption | Advance the bitmap |

Calling `update()` only after verified decryption prevents an attacker from poisoning the window with garbage packets carrying arbitrary counter values. A combined `check_and_update()` convenience method is also provided.

---

## Timer State Machine

`rustguard-core` implements the WireGuard timer state machine governing session lifecycle via `SessionTimers`. All timers include jitter to prevent synchronized retries across peers.

| Constant | Value | Trigger |
|----------|-------|---------|
| `REKEY_AFTER_TIME` | 120 s | Initiate rekey |
| `REKEY_AFTER_MESSAGES` | 2⁶⁰ − 1 | Initiate rekey |
| `REJECT_AFTER_TIME` | 180 s | Stop accepting packets on session |
| `REKEY_TIMEOUT` | 5 s | Retry handshake if no response |
| `REKEY_ATTEMPT_TIME` | 90 s | Total retry budget |
| `DEAD_SESSION_TIMEOUT` | 240 s | Tear down and discard session |
| `COOKIE_SECRET_LIFETIME` | 120 s | Rotate cookie secret |

`needs_rekey()` returns true when either the time or message-count threshold is reached. `is_expired()` and `is_dead()` signal session shutdown conditions. `needs_keepalive()` fires when no outbound traffic has been sent within a configured keepalive interval.

---

## TUN Backends

### TUN Device

A virtual network interface whose packets are readable and writable as file descriptors in userspace. RustGuard reads plaintext IP packets from the TUN device on the outbound path and writes decrypted packets on the inbound path. The `rustguard-tun` crate manages these devices; all file descriptors are opened with `O_CLOEXEC`.

### utun (macOS)

The macOS kernel control socket interface for creating virtual network interfaces, accessed directly via `PF_SYSTEM` / `SYSPROTO_CONTROL` socket calls without any helper library.

### Multi-Queue TUN

A Linux TUN configuration that opens multiple file descriptors to the same interface via `IFF_MULTI_QUEUE`, enabling parallel packet processing across CPU cores.

### AF_XDP

A Linux socket family providing kernel-bypass, zero-copy packet I/O between userspace and the NIC driver. `rustguard-tun` includes an AF_XDP backend that eliminates the copy overhead of the standard TUN path.

### io_uring

A Linux asynchronous I/O interface. `rustguard-tun` provides an io_uring backend for batched TUN reads and writes, reducing syscall overhead on the hot path.

---

## Enrollment Protocol

### Enrollment Token

A shared secret passed to both `rustguard serve` and `rustguard join`. It is used to derive an XChaCha20-Poly1305 key that encrypts the key exchange during enrollment, requiring no config files or manual public-key exchange.

### IP Pool

A CIDR block configured on the server (e.g., `10.150.0.0/24`). `rustguard-enroll` allocates IPs sequentially: the server takes `.1`; joining clients receive the next available address. Pool state is persisted to `~/.rustguard/state.json` and survives restarts.

### Pairing Window

A time-bounded enrollment window inspired by Zigbee physical-presence pairing. The server starts with enrollment closed. `rustguard open <seconds>` opens the window; it closes automatically via an atomic expiry timestamp. Existing peer sessions are unaffected by window state changes.

### Control Socket

A UNIX domain socket created by `rustguard-enroll` at daemon startup. Accepts `open`, `close`, and `status` commands while the tunnel is live, enabling runtime window management without restarting the process.

---

## `no_std` Compatibility

`rustguard-crypto` and `rustguard-core` carry no dependency on the Rust standard library. This allows them to be compiled into `rustguard-kmod`, which cannot link against `std`. The `std` feature flag on each crate re-enables convenience implementations (e.g., `std::error::Error`, clock access via `std::time::SystemTime`) for userspace builds. No-std variants of timer methods accept a `now_ns: u64` parameter in place of a system clock call.

---

## Examples

The example below exercises the handshake and transport session API directly, demonstrating the concepts of sender index, session keys, and replay window.

```rust
use rustguard_crypto::{StaticSecret, PublicKey};
use rustguard_core::{
    create_initiation, process_initiation_psk, process_response,
    TransportSession,
};

// --- Key generation ---
let initiator_static = StaticSecret::random();
let responder_static = StaticSecret::random();
let responder_public  = responder_static.public_key();

let psk = [0u8; 32]; // zero PSK == no PSK

// --- Initiator: build the Initiation message ---
let (initiation, handshake_state) = create_initiation(
    &initiator_static,
    &responder_public,
    /* sender_index */ 0xdeadbeef,
);

// --- Responder: process the Initiation, emit Response ---
let our_index = 0xcafebabe;
let (peer_pub, _timestamp, response, responder_session) =
    process_initiation_psk(
        &responder_static,
        &initiation,
        our_index,
        &psk,
        /* last_timestamp */ None,
    )
    .expect("valid initiation");

// --- Initiator: process the Response, derive session keys ---
let mut initiator_session: TransportSession =
    process_response(handshake_state, &initiator_static, &response)
        .expect("valid response");

// --- Transport: encrypt a packet ---
let plaintext = b"hello, wireguard";
let (counter, ciphertext) = initiator_session
    .encrypt(plaintext)
    .expect("nonce not exhausted");

// --- Transport: decrypt on the other side ---
let mut resp_session = responder_session; // move into mutable binding
let decrypted = resp_session
    .decrypt(counter, &ciphertext)
    .expect("valid ciphertext");

assert_eq!(decrypted, plaintext);
```

The enrollment flow below demonstrates the token, pairing window, and IP pool concepts:

```bash
# Server: start with a /24 pool
rustguard serve --pool 10.150.0.0/24 --token mysecret

# Server: open the pairing window for 60 seconds
rustguard open 60

# Client A: enroll — receives 10.150.0.2/24
rustguard join 198.51.100.1:51820 --token mysecret

# Client B: enroll — receives 10.150.0.3/24
rustguard join 198.51.100.1:51820 --token mysecret

# Server: inspect window state and peer count
rustguard status

# Server: close the window immediately
rustguard close
```

---

## See Also

- [System Overview](01-System-Overview.md) — Crate map and component diagram showing where each concept is implemented
- [Data Flow](03-Data-Flow.md) — Runtime packet trace showing how the replay window, AllowedIPs, and session keys interact per packet
- [Design Decisions](04-Design-Decisions.md) — ADRs explaining why key security decisions (MAC1 ordering, replay window split, HMAC-BLAKE2s correction) were made
- [Public API](../04-API-Reference/01-Public-API.md) — Exported function signatures for `rustguard-core` and `rustguard-crypto`
- [Common Workflows](../03-Guides/01-Common-Workflows.md) — Step-by-step guides for tunnel setup, enrollment, and key generation