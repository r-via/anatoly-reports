# rustguard-core

> Protocol implementation crate providing the WireGuard Noise_IK handshake, transport sessions, anti-replay protection, DoS cookie mechanism, wire message formats, and session lifecycle timers.

## Overview

`rustguard-core` is the central protocol crate of RustGuard. It implements the full WireGuard protocol above the cryptographic layer: the Noise_IKpsk2 handshake exchange, symmetric transport sessions derived from that handshake, a 2048-bit sliding-window anti-replay bitmap, session lifecycle timers matching the WireGuard whitepaper, and the DoS-mitigation cookie mechanism.

The crate is dual `std`/`no_std` — the `std` feature gate enables `Instant`-based timers, `OsRng`, and `SocketAddr` helpers. Without it, callers (such as `rustguard-kmod`) inject their own monotonic timestamps and RNG, and the crate compiles to a `#![no_std]` + `alloc` target.

All handshake state structs implement `ZeroizeOnDrop` to ensure key material is erased when dropped.

```
rustguard-core/src/
├── cookie.rs     CookieChecker (server) + CookieState (client)
├── handshake.rs  Noise_IKpsk2 initiator and responder
├── lib.rs        Crate root, re-exports rustguard_crypto
├── messages.rs   Wire message structs and serialisation
├── replay.rs     ReplayWindow — 2048-bit anti-replay bitmap
├── session.rs    TransportSession — encrypt/decrypt with replay guard
└── timers.rs     SessionTimers + WireGuard timer constants
```

---

## Module: `handshake`

Implements the Noise_IKpsk2 1-RTT key exchange. After the two-message exchange both peers derive matching symmetric transport keys.

### `create_initiation` (`std` only)

```rust
pub fn create_initiation(
    our_static: &StaticSecret,
    their_public: &PublicKey,
    sender_index: u32,
) -> (Initiation, InitiatorHandshake)
```

Creates a handshake initiation message using `OsRng` and the system clock. Returns the wire message and an `InitiatorHandshake` state object that must be passed to `process_response`.

### `create_initiation_psk` (`std` only)

```rust
pub fn create_initiation_psk(
    our_static: &StaticSecret,
    their_public: &PublicKey,
    sender_index: u32,
    psk: &[u8; 32],
) -> (Initiation, InitiatorHandshake)
```

Variant of `create_initiation` that incorporates a pre-shared key into the Noise_IKpsk2 transcript.

### `create_initiation_with` (no_std compatible)

```rust
pub fn create_initiation_with(
    our_static: &StaticSecret,
    their_public: &PublicKey,
    sender_index: u32,
    psk: &[u8; 32],
    ephemeral: EphemeralSecret,
    timestamp: Tai64n,
) -> (Initiation, InitiatorHandshake)
```

Core initiator function usable from `no_std`. The caller supplies the ephemeral key and TAI64N timestamp directly.

### `process_initiation` (`std` only)

```rust
pub fn process_initiation(
    our_static: &StaticSecret,
    msg: &Initiation,
    responder_index: u32,
) -> Option<(PublicKey, Tai64n, Response, TransportSession)>
```

Processes an incoming initiation on the responder side. Verifies MAC1 first (before any DH), then decrypts and authenticates the initiator's static key and timestamp. Returns the initiator's `PublicKey`, the authenticated `Tai64n` timestamp, the response message to send back, and a ready `TransportSession`. Returns `None` on any authentication failure.

### `process_initiation_psk` (`std` only)

```rust
pub fn process_initiation_psk(
    our_static: &StaticSecret,
    msg: &Initiation,
    responder_index: u32,
    psk: &[u8; 32],
    last_timestamp: Option<&Tai64n>,
) -> Option<(PublicKey, Tai64n, Response, TransportSession)>
```

Responder-side processing with PSK and optional timestamp-replay guard. When `last_timestamp` is `Some`, the incoming timestamp must be strictly newer — stale or replayed initiations return `None`.

### `process_initiation_with` (no_std compatible)

```rust
pub fn process_initiation_with(
    our_static: &StaticSecret,
    msg: &Initiation,
    responder_index: u32,
    psk: &[u8; 32],
    last_timestamp: Option<&Tai64n>,
    resp_eph: EphemeralSecret,
) -> Option<(PublicKey, Tai64n, Response, TransportSession)>
```

Core responder function; the caller supplies the ephemeral secret directly.

### `process_response`

```rust
pub fn process_response(
    state: InitiatorHandshake,
    our_static: &StaticSecret,
    msg: &Response,
) -> Option<TransportSession>
```

Completes the handshake on the initiator side. Verifies the responder's reply and derives transport keys. Consumes `state` (which is zeroized on drop). Returns `None` if `receiver_index` does not match or decryption fails.

### `compute_mac1`

```rust
pub fn compute_mac1(responder_public: &PublicKey, msg_bytes: &[u8]) -> [u8; 16]
```

Computes `MAC1 = BLAKE2s-MAC(HASH(LABEL_MAC1 ‖ responder_public), msg_bytes)`. Used by both sides when constructing and verifying handshake messages.

### `InitiatorHandshake`

Intermediate state between sending the initiation and receiving the response. Implements `Zeroize + ZeroizeOnDrop` — the chaining key, hash, ephemeral secret, and PSK are zeroed when dropped.

```rust
pub struct InitiatorHandshake { /* opaque */ }
```

---

## Module: `session`

### `TransportSession`

A symmetric session derived from a completed handshake. Holds separate send/receive keys, a monotonically incrementing send counter, and a `ReplayWindow` for incoming packets.

```rust
pub struct TransportSession {
    pub our_index:   u32,
    pub their_index: u32,
    // key material and state are private
}
```

#### `new`

```rust
pub fn new(
    our_index: u32,
    their_index: u32,
    key_send: [u8; 32],
    key_recv: [u8; 32],
) -> TransportSession
```

#### `encrypt`

```rust
pub fn encrypt(&mut self, plaintext: &[u8]) -> Option<(u64, Vec<u8>)>
```

Encrypts `plaintext` with ChaCha20-Poly1305, returning `(counter, ciphertext)`. Returns `None` when the 64-bit nonce counter is exhausted — the session must be rekeyed.

#### `decrypt`

```rust
pub fn decrypt(&mut self, counter: u64, ciphertext: &[u8]) -> Option<Vec<u8>>
```

Decrypts an incoming transport packet. The replay window is checked before decryption; the window is only updated after successful AEAD verification, preventing an attacker from poisoning the window with unauthenticated garbage. Returns `None` on replay detection or decryption failure.

#### `encrypt_to` / `decrypt_in_place`

Zero-allocation variants for kernel and high-performance paths.

```rust
pub fn encrypt_to(&mut self, plaintext: &[u8], out: &mut [u8]) -> Option<(u64, usize)>
pub fn decrypt_in_place(&mut self, counter: u64, buf: &mut [u8], ct_len: usize) -> Option<usize>
```

`encrypt_to` requires `out.len() >= plaintext.len() + 16`. `decrypt_in_place` overwrites `buf[..ct_len]` with the plaintext and returns its length.

#### `send_counter`

```rust
pub fn send_counter(&self) -> u64
```

Returns the current outbound nonce counter value.

---

## Module: `replay`

### `ReplayWindow`

A 2048-bit sliding bitmap tracking which nonce counters have been seen. Follows the RFC 6479 / WireGuard kernel algorithm.

```rust
pub struct ReplayWindow { /* private */ }
```

- **Window size**: 2048 counters.
- Counter strictly below the window floor: rejected.
- Counter already set in the bitmap: rejected.
- Counter above the current top: always accepted (advances the window).

#### `new`

```rust
pub fn new() -> ReplayWindow
```

#### `check`

```rust
pub fn check(&self, counter: u64) -> bool
```

Returns `true` if `counter` would be accepted. Does not modify the window.

#### `update`

```rust
pub fn update(&mut self, counter: u64)
```

Marks `counter` as seen. Must only be called after the packet has been authenticated.

#### `check_and_update`

```rust
pub fn check_and_update(&mut self, counter: u64) -> bool
```

Convenience method combining `check` and `update` in one call.

---

## Module: `timers`

### Timer Constants

All values are sourced from the WireGuard whitepaper, §6.

| Constant | Value | Meaning |
|---|---|---|
| `REKEY_AFTER_TIME` | 120 s | Initiate a new handshake after this session age |
| `REJECT_AFTER_TIME` | 180 s | Refuse to send with a keypair older than this |
| `REKEY_TIMEOUT` | 5 s | Retry a pending handshake after this interval |
| `REKEY_AFTER_MESSAGES` | 2⁶⁰ − 1 | Initiate rekey after this many messages |
| `REJECT_AFTER_MESSAGES` | u64::MAX − 2¹³ | Hard reject threshold |
| `REKEY_ATTEMPT_TIME` | 90 s | Give up on a handshake attempt after this |
| `DEAD_SESSION_TIMEOUT` | 240 s | Zero session keys after this session age |

### `SessionTimers`

Tracks per-peer session lifecycle timestamps and drives rekey/keepalive decisions.

```rust
pub struct SessionTimers {
    pub session_established: Option<Timestamp>,
    pub last_handshake_sent: Option<Timestamp>,
    pub last_received:       Option<Timestamp>,
    pub last_sent:           Option<Timestamp>,
    pub rekey_requested:     bool,
}
```

Key methods (all `std` variants shown; `no_std` variants take explicit `now_ns: u64`):

| Method | Description |
|---|---|
| `session_started()` | Record a new session establishment |
| `packet_sent()` | Record an outbound packet |
| `packet_received()` | Record receipt of an authenticated packet |
| `needs_rekey(send_counter)` | True if time or message threshold is exceeded |
| `is_expired(send_counter)` | True if session is too old or over message limit |
| `is_dead()` | True if session keys should be zeroed |
| `needs_keepalive(interval)` | True if a keepalive should be sent |
| `should_retry_handshake()` | True if a pending handshake should be retried |
| `handshake_timed_out()` | True if the handshake attempt window has expired |

---

## Module: `cookie`

Implements WireGuard's DoS-mitigation cookie mechanism. Under load, the responder sends a `CookieReply` instead of processing the full handshake, requiring the initiator to prove reachability before the exchange proceeds.

### `CookieChecker` (server side)

```rust
pub struct CookieChecker {
    pub under_load: bool,
    // secret and timing are private
}
```

| Method | Description |
|---|---|
| `new(our_public)` (`std`) | Construct with a fresh random secret |
| `new_with(our_public, secret, now_ts)` | Construct with an explicit secret and timestamp (no_std / kernel) |
| `create_reply(receiver_index, mac1, src)` (`std`) | Build a `CookieReply` for a `SocketAddr` |
| `create_reply_from_bytes(receiver_index, mac1, addr_bytes)` | Build a `CookieReply` from raw address bytes |
| `verify_mac1(msg_bytes, mac1)` | Constant-time MAC1 check |
| `verify_mac2(msg_with_mac1, mac2, src)` (`std`) | Verify MAC2 against the current cookie for a `SocketAddr` |
| `verify_mac2_from_bytes(msg_with_mac1, mac2, addr_bytes)` | Verify MAC2 from raw address bytes |

The cookie secret rotates every 120 seconds.

### `CookieState` (client side)

```rust
pub struct CookieState { /* private */ }
```

| Method | Description |
|---|---|
| `new()` | Construct with no stored cookie |
| `process_reply(reply, their_public, mac1)` | Decrypt and store a cookie from a `CookieReply` |
| `compute_mac2(msg_with_mac1)` | Produce MAC2 from the stored cookie; returns `[0u8; 16]` if no fresh cookie |

---

## Module: `messages`

Wire format structs with `to_bytes` / `from_bytes` serialisation.

| Type | Size | Message type constant |
|---|---|---|
| `Initiation` | 148 bytes | `MSG_INITIATION = 1` |
| `Response` | 92 bytes | `MSG_RESPONSE = 2` |
| `CookieReply` | 64 bytes | `MSG_COOKIE_REPLY = 3` |
| `Transport` | 16 + N bytes | `MSG_TRANSPORT = 4` |

All multi-byte integers are little-endian on the wire. `Transport::from_bytes` returns `Option<Transport>` and checks for minimum header length.

---

## Examples

### Complete handshake and data exchange

```rust
use rustguard_core::handshake;
use rustguard_core::session::TransportSession;
use rustguard_crypto::StaticSecret;

fn main() {
    // Generate static key pairs for both peers.
    let initiator_static = StaticSecret::random();
    let responder_static = StaticSecret::random();
    let responder_public = responder_static.public_key();

    // Initiator builds the first message.
    let (init_msg, init_state) =
        handshake::create_initiation(&initiator_static, &responder_public, 1);

    // Responder processes the initiation and produces a response.
    let (peer_pubkey, _timestamp, resp_msg, mut resp_session) =
        handshake::process_initiation(&responder_static, &init_msg, 2)
            .expect("initiation rejected");

    assert_eq!(peer_pubkey, initiator_static.public_key());

    // Initiator processes the response — both sides now have transport sessions.
    let mut init_session =
        handshake::process_response(init_state, &initiator_static, &resp_msg)
            .expect("response rejected");

    // Encrypt a packet from the initiator.
    let plaintext = b"hello from initiator";
    let (counter, ciphertext) = init_session.encrypt(plaintext).unwrap();

    // Decrypt on the responder side.
    let decrypted = resp_session.decrypt(counter, &ciphertext).unwrap();
    assert_eq!(&decrypted, plaintext);

    // Bidirectional — responder can reply.
    let reply = b"hello back";
    let (counter, ciphertext) = resp_session.encrypt(reply).unwrap();
    let decrypted = init_session.decrypt(counter, &ciphertext).unwrap();
    assert_eq!(&decrypted, reply);
}
```

### Session timers and rekey decision

```rust
use rustguard_core::timers::{SessionTimers, REKEY_AFTER_MESSAGES};

let mut timers = SessionTimers::new();

// Record a new session.
timers.session_started();

// After each encrypted packet, check whether rekeying is needed.
let send_counter: u64 = 42;
if timers.needs_rekey(send_counter) {
    // Initiate a new handshake.
}

// Rekey threshold reached by message count.
assert!(timers.needs_rekey(REKEY_AFTER_MESSAGES));
```

### Anti-replay window standalone usage

```rust
use rustguard_core::replay::ReplayWindow;

let mut window = ReplayWindow::new();

// Packets can arrive out of order within the 2048-counter window.
assert!(window.check_and_update(5));
assert!(window.check_and_update(3));
assert!(window.check_and_update(7));

// Replayed counter is rejected.
assert!(!window.check_and_update(5));

// Counter outside the window floor is rejected.
assert!(window.check_and_update(3000));
assert!(!window.check_and_update(0)); // age > 2048
```

---

## See Also

- [rustguard-crypto](05-Modules/12-rustguard-crypto.md) — Cryptographic primitives (`X25519`, `ChaCha20-Poly1305`, `BLAKE2s`, `HKDF`, `TAI64N`) consumed by this crate
- [rustguard-tun](05-Modules/07-rustguard-tun.md) — TUN device layer that feeds plaintext into `TransportSession::encrypt`
- [rustguard-daemon](05-Modules/13-rustguard-daemon.md) — Standard `wg.conf` tunnel mode built on top of this crate
- [rustguard-kmod](05-Modules/06-rustguard-kmod.md) — Linux kernel module that uses the `no_std` build of this crate
- [Data Flow](02-Architecture/03-Data-Flow.md) — End-to-end packet path from TUN read through handshake to wire
- [Core Concepts](02-Architecture/02-Core-Concepts.md) — Glossary and protocol background