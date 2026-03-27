# cookie

> Implements WireGuard's rate-limiting cookie mechanism to protect the responder from CPU-exhaustion denial-of-service attacks.

## Overview

The `cookie` module provides the two-sided state machine that WireGuard uses to guard expensive Diffie–Hellman operations. When a responder detects that it is under load, it issues a **Cookie Reply** instead of processing the handshake immediately. The initiator must decrypt the cookie, attach it as MAC2, and retransmit. Because the cookie is keyed to the initiator's source IP and port, a valid MAC2 proves the initiator can receive traffic at its claimed address.

Two independent implementations exist:

| Location | Environment | Timekeeping | RNG |
|---|---|---|---|
| `rustguard-core/src/cookie.rs` | `std` / `no_std` dual | `std::time::Instant` / atomic stub | `getrandom` crate |
| `rustguard-kmod/src/cookie.rs` | Linux kernel module | `wg_ktime_get_ns()` FFI | `wg_get_random_bytes()` FFI |

Both implementations expose structurally identical `CookieChecker` and `CookieState` types; the kernel variant is `pub(crate)` only, while the core variant is fully public.

## Constants

| Constant | Value | Description |
|---|---|---|
| `COOKIE_LEN` | `16` | Cookie size in bytes. |
| `COOKIE_SECRET_LIFETIME` | `120 s` | Rotation interval for the server secret (core). |
| `COOKIE_SECRET_LIFETIME_NS` | `120_000_000_000` ns | Same interval expressed in nanoseconds (kmod). |
| `LABEL_COOKIE` | `b"cookie--"` | Domain-separation label for cookie encryption key derivation. |
| `LABEL_MAC1` | `b"mac1----"` | Domain-separation label for MAC1 key derivation. |

## Types

### `CookieChecker`

Server-side state. Holds the responder's static public key, a 32-byte rotating secret, and the `under_load` flag that the caller must set when rate-limiting kicks in.

```
CookieChecker {
    our_public:       PublicKey      // responder's static public key
    secret:           [u8; 32]       // rotates every 120 s
    secret_generated: Timestamp      // when the secret was last replaced
    under_load:       bool           // public — caller-managed flag
}
```

#### `CookieChecker::new` (`std` only)

```rust
pub fn new(our_public: PublicKey) -> CookieChecker
```

Creates a `CookieChecker` with a fresh random secret and `under_load = false`. Uses `getrandom` for the initial secret and records the current `Instant`.

#### `CookieChecker::new_with`

```rust
pub fn new_with(
    our_public: PublicKey,
    secret: [u8; 32],
    now_ts: Timestamp,
) -> CookieChecker
```

`no_std`-compatible constructor that accepts an externally supplied secret and timestamp. Used by the kernel module's own cookie checker, which manages timekeeping and the RNG through FFI calls.

#### `CookieChecker::verify_mac1`

```rust
pub fn verify_mac1(&self, msg_bytes: &[u8], mac1: &[u8; 16]) -> bool
```

Verifies MAC1 on an incoming handshake message. The MAC1 key is derived as `BLAKE2s(LABEL_MAC1 || our_public)`. Comparison is performed with `subtle::ConstantTimeEq` (core) or `wg_crypto_memneq` (kmod) to prevent timing side-channels. **MAC1 must be verified before any DH operations.**

#### `CookieChecker::create_reply` (`std` only)

```rust
pub fn create_reply(
    &mut self,
    receiver_index: u32,
    mac1: &[u8; 16],
    src: &std::net::SocketAddr,
) -> CookieReply
```

Builds a `CookieReply` message (wire type 3). Internally calls `create_reply_from_bytes` after encoding the `SocketAddr` to a 6-byte (IPv4) or 18-byte (IPv6) slice.

#### `CookieChecker::create_reply_from_bytes`

```rust
pub fn create_reply_from_bytes(
    &mut self,
    receiver_index: u32,
    mac1: &[u8; 16],
    addr_bytes: &[u8],
) -> CookieReply
```

`no_std`-compatible variant. Accepts a pre-encoded address slice (IP bytes followed by big-endian port bytes). The cookie is computed as `BLAKE2s-MAC(secret, addr_bytes)[..16]`, encrypted with XChaCha20-Poly1305 using a fresh random 24-byte nonce and authenticated against `mac1`.

#### `CookieChecker::verify_mac2` (`std` only)

```rust
pub fn verify_mac2(
    &mut self,
    msg_with_mac1: &[u8],
    mac2: &[u8; 16],
    src: &std::net::SocketAddr,
) -> bool
```

Verifies MAC2 on a retransmitted handshake. Recomputes the expected cookie for `src` (triggering secret rotation if needed) and checks `BLAKE2s-MAC(cookie, msg_with_mac1)[..16]` against `mac2`.

#### `CookieChecker::verify_mac2_from_bytes`

```rust
pub fn verify_mac2_from_bytes(
    &mut self,
    msg_with_mac1: &[u8],
    mac2: &[u8; 16],
    addr_bytes: &[u8],
) -> bool
```

`no_std`-compatible variant of `verify_mac2` taking a pre-encoded address slice.

---

### `CookieState`

Client-side state. Stores the most recently received cookie and the timestamp at which it was received. A cookie is considered fresh while it is less than 120 seconds old; `compute_mac2` returns `[0u8; 16]` (the "no cookie" sentinel) for stale or absent cookies.

```
CookieState {
    cookie:   Option<[u8; 16]>  // decrypted cookie, if present
    received: Option<Timestamp> // when the cookie was stored
}
```

#### `CookieState::new`

```rust
pub fn new() -> CookieState
```

Returns an empty `CookieState` with no stored cookie.

#### `CookieState::process_reply`

```rust
pub fn process_reply(
    &mut self,
    reply: &CookieReply,
    their_public: &PublicKey,
    mac1: &[u8; 16],
) -> bool
```

Decrypts the cookie inside a `CookieReply`. The decryption key is derived as `BLAKE2s(LABEL_COOKIE || their_public)`. The `mac1` value from the original handshake attempt is used as the AEAD additional-data, preventing replay of cookie replies across different sessions. Returns `true` and stores the cookie on success; returns `false` if AEAD authentication fails.

#### `CookieState::compute_mac2`

```rust
pub fn compute_mac2(&self, msg_with_mac1: &[u8]) -> [u8; 16]
```

Computes `BLAKE2s-MAC(cookie, msg_with_mac1)[..16]` if a fresh cookie is available. Returns `[0u8; 16]` when no cookie has been received or the stored cookie has expired.

## DoS Protection Flow

```
Initiator                                  Responder (under load)
─────────                                  ──────────────────────
HandshakeInitiation (MAC2 = 0)  ────────►
                                           verify_mac1(msg, mac1)  ✓
                                           under_load == true
                                ◄────────  create_reply(idx, mac1, src)

process_reply(reply, pub, mac1)
compute_mac2(new_msg_with_mac1)

HandshakeInitiation (MAC2 ≠ 0)  ────────►
                                           verify_mac1(msg, mac1)  ✓
                                           verify_mac2(msg, mac2, src) ✓
                                           → process handshake
```

Secret rotation is transparent: both `make_cookie_from_bytes` (inside `create_reply*`) and `verify_mac2*` call `maybe_rotate_secret` before computing the cookie, so both sides always use the current 2-minute window.

## Examples

### Server: issuing a Cookie Reply

```rust
use rustguard_core::cookie::CookieChecker;
use rustguard_crypto::StaticSecret;
use std::net::SocketAddr;

let server_secret = StaticSecret::random();
let server_public = server_secret.public_key();

let mut checker = CookieChecker::new(server_public);
checker.under_load = true;

// Called when a HandshakeInitiation arrives under load.
let src: SocketAddr = "203.0.113.42:51820".parse().unwrap();
let mac1: [u8; 16] = /* extracted from the handshake message */ [0x11; 16];
let receiver_index: u32 = 0xdeadbeef;

let reply = checker.create_reply(receiver_index, &mac1, &src);
// Serialize `reply` and send it back to the initiator.
```

### Client: storing the cookie and computing MAC2

```rust
use rustguard_core::cookie::CookieState;
use rustguard_crypto::StaticSecret;

let server_secret = StaticSecret::random();
let server_public = server_secret.public_key();

let mut state = CookieState::new();
let mac1: [u8; 16] = [0x11; 16]; // MAC1 from the original handshake attempt

// `reply` received from the server.
if state.process_reply(&reply, &server_public, &mac1) {
    // Re-build the handshake initiation message with MAC1 appended.
    let msg_with_mac1: Vec<u8> = /* serialized message || mac1 */ vec![0u8; 116];
    let mac2 = state.compute_mac2(&msg_with_mac1);
    // Embed mac2 in the retransmitted HandshakeInitiation.
}
```

### Server: verifying MAC2 on retransmission

```rust
use std::net::SocketAddr;

let src: SocketAddr = "203.0.113.42:51820".parse().unwrap();
let msg_with_mac1: &[u8] = /* the retransmitted message bytes up to and including mac1 */ &[];
let mac2: [u8; 16] = /* extracted from the message */ [0u8; 16];

if checker.verify_mac2(msg_with_mac1, &mac2, &src) {
    // MAC2 valid — proceed with expensive DH operations.
}
```

## Security Notes

- **MAC1 before DH**: `verify_mac1` must be called and must succeed before any Diffie–Hellman computation. This is enforced by the handshake layer; see the security audit notes in the README (Commit 5).
- **Constant-time comparisons**: both MAC comparisons use `subtle::ConstantTimeEq` in `rustguard-core` and the kernel's `crypto_memneq` (guaranteed non-optimisable) in `rustguard-kmod`.
- **Secret zeroisation**: the rotating secret is a plain `[u8; 32]` field; callers that require zeroisation on drop should wrap `CookieChecker` with a `ZeroizeOnDrop` guard.
- **Source binding**: MAC2 is bound to the sender's IP and port. A cookie obtained via IP spoofing is useless because the real initiator cannot produce a matching MAC2 from a different address.

## See Also

- [rustguard-core](05-Modules/11-rustguard-core.md) — the crate that owns this module's primary implementation
- [handshake](05-Modules/04-handshake.md) — calls `CookieChecker` to enforce MAC1/MAC2 ordering
- [server](05-Modules/08-server.md) — sets `CookieChecker::under_load` based on rate-limit state
- [rustguard-kmod](05-Modules/06-rustguard-kmod.md) — the kernel-module variant of this module
- [rustguard-crypto](05-Modules/12-rustguard-crypto.md) — provides `mac`, `hash`, `xseal`, `xopen`, and key types used here
- [Core Concepts](02-Architecture/02-Core-Concepts.md) — WireGuard protocol glossary including MAC1, MAC2, and Cookie Reply