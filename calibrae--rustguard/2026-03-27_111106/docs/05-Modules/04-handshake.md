# handshake

> `rustguard-core/src/handshake.rs` — Noise\_IKpsk2 handshake implementation: initiator and responder functions, MAC1 computation, and transport-key derivation.

## Overview

`handshake.rs` implements the WireGuard Noise\_IKpsk2 1-RTT key exchange inside `rustguard-core`. The exchange is two messages: the initiator sends an encrypted ephemeral key, encrypted static key, and an encrypted TAI64N timestamp; the responder replies with its own ephemeral key and an encrypted empty payload. After both messages are processed, each peer independently derives a matching pair of symmetric transport keys.

The file is dual `std`/`no_std`. The `std`-gated convenience functions (`create_initiation`, `process_initiation`, `create_initiation_psk`, `process_initiation_psk`) source entropy from `OsRng` and read the system clock. The `_with` variants (`create_initiation_with`, `process_initiation_with`) accept the ephemeral key and timestamp as explicit arguments and compile without `std`.

All sensitive handshake state is held in `InitiatorHandshake`, which implements `Zeroize + ZeroizeOnDrop`. The chaining key, handshake hash, ephemeral secret, and PSK are erased from memory when the struct is dropped.

For crate-level context — including `TransportSession`, `ReplayWindow`, cookie machinery, and session timers — see [rustguard-core](05-Modules/11-rustguard-core.md).

## Noise\_IKpsk2 DH Sequence

The handshake threads every operation through the same chaining key (`ck`) and handshake hash (`h`). The sequence executed during initiation:

| Step | Operation | Effect |
|------|-----------|--------|
| 1 | `mix_hash(h, responder_public)` | Bind responder identity |
| 2 | `mix_hash(h, eph_public)` | Bind ephemeral |
| 3 | `DH(ephemeral, responder_static)` → `mix_key` | Forward secrecy |
| 4 | `encrypt_and_hash(key, our_static_public)` | Authenticate initiator |
| 5 | `DH(our_static, responder_static)` → `mix_key` | Mutual authentication |
| 6 | `encrypt_and_hash(key2, timestamp)` | Bind freshness |

The responder mirrors steps 1–6 and then performs two additional DH operations (ephemeral↔ephemeral, ephemeral↔initiator\_static) before the PSK phase:

| Step | Operation |
|------|-----------|
| 7 | `DH(resp_eph, init_eph)` → `mix_key` |
| 8 | `DH(resp_eph, init_static)` → `mix_key` |
| 9 | `hkdf(ck, psk)` → new `ck`, temporary key, AEAD key (PSK phase) |
| 10 | `encrypt_and_hash(key3, empty)` → 16-byte AEAD tag in response |

Final transport keys are derived with `hkdf(ck, &[])`. The initiator uses the first output as the send key; the responder uses the second as the send key (i.e., the keys are directional).

## Security Properties

- **MAC1 verified before any DH.** `process_initiation_with` checks MAC1 with `constant_time_eq` before performing any Diffie-Hellman operation. Unauthenticated packets cannot burn CPU.
- **Timestamp replay protection.** When `last_timestamp: Some(&Tai64n)` is supplied to the responder, the incoming timestamp must satisfy `Tai64n::is_after(last)`. Stale or replayed initiations return `None`.
- **ZeroizeOnDrop on `InitiatorHandshake`.** The fields `ck`, `h`, `ephemeral`, and `psk` are zeroized on drop. `sender_index` and `their_public` are skipped (`#[zeroize(skip)]`).
- **Constant-time MAC comparison.** The internal `constant_time_eq` function delegates to `subtle::ConstantTimeEq`.
- **Nonce fixed to zero for handshake AEAD.** Each AEAD call within the handshake uses nonce `0`; nonces are not reused because a fresh ephemeral key is generated per handshake.

## Public API

### `InitiatorHandshake`

Opaque intermediate state held by the initiator between sending message 1 and receiving message 2. Passed by value into `process_response`, which consumes and zeroizes it.

```rust
pub struct InitiatorHandshake { /* opaque — ZeroizeOnDrop */ }
```

### `compute_mac1`

```rust
pub fn compute_mac1(
    responder_public: &PublicKey,
    msg_bytes: &[u8],
) -> [u8; 16]
```

Computes `MAC1 = BLAKE2s-MAC(HASH(LABEL_MAC1 ‖ responder_public), msg_bytes)`. Called internally by both `create_initiation_with` and `process_initiation_with` to stamp and verify the first 116 bytes of the initiation message, and the first 60 bytes of the response.

### Initiator — `std` convenience functions

```rust
// No PSK (psk treated as [0u8; 32]).
#[cfg(feature = "std")]
pub fn create_initiation(
    our_static: &StaticSecret,
    their_public: &PublicKey,
    sender_index: u32,
) -> (Initiation, InitiatorHandshake)

// With pre-shared key.
#[cfg(feature = "std")]
pub fn create_initiation_psk(
    our_static: &StaticSecret,
    their_public: &PublicKey,
    sender_index: u32,
    psk: &[u8; 32],
) -> (Initiation, InitiatorHandshake)
```

Both functions source the ephemeral key from `OsRng` and the timestamp from the system clock. The returned `Initiation` has `mac1` populated; `mac2` is zeroed (cookie mechanism not yet applied at this layer — see [cookie](05-Modules/03-cookie.md)).

### Initiator — `no_std` core function

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

The caller supplies the ephemeral key and timestamp directly. Used by the kernel module and test infrastructure (deterministic inputs).

### Initiator — response processing

```rust
pub fn process_response(
    state: InitiatorHandshake,
    our_static: &StaticSecret,
    msg: &Response,
) -> Option<TransportSession>
```

Completes the handshake. Verifies that `msg.receiver_index` matches the `sender_index` stored in `state`, then performs the remaining DH operations, the PSK phase, and derives transport keys. Returns `None` if the receiver index does not match or if the AEAD decryption of the empty payload fails. Consumes `state` — it is zeroized on return.

### Responder — `std` convenience functions

```rust
#[cfg(feature = "std")]
pub fn process_initiation(
    our_static: &StaticSecret,
    msg: &Initiation,
    responder_index: u32,
) -> Option<(PublicKey, Tai64n, Response, TransportSession)>

#[cfg(feature = "std")]
pub fn process_initiation_psk(
    our_static: &StaticSecret,
    msg: &Initiation,
    responder_index: u32,
    psk: &[u8; 32],
    last_timestamp: Option<&Tai64n>,
) -> Option<(PublicKey, Tai64n, Response, TransportSession)>
```

`process_initiation` is a wrapper around `process_initiation_psk` with `psk = [0u8; 32]` and `last_timestamp = None`.

On success, the tuple contains:
- `PublicKey` — the authenticated initiator static public key.
- `Tai64n` — the decrypted and validated initiation timestamp (store this for replay prevention on the next initiation from the same peer).
- `Response` — the response message to transmit back to the initiator.
- `TransportSession` — the ready session (send and receive keys established).

On any authentication failure — bad MAC1, decryption failure, or stale timestamp — the function returns `None` without leaking timing information about which step failed.

### Responder — `no_std` core function

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

Accepts the responder ephemeral key explicitly. All other behaviour is identical to `process_initiation_psk`.

## Examples

### Complete two-message handshake (no PSK)

```rust
use rustguard_core::handshake;
use rustguard_crypto::StaticSecret;

fn main() {
    // Both peers generate long-term static keys.
    let initiator_static = StaticSecret::random();
    let responder_static = StaticSecret::random();
    let responder_public = responder_static.public_key();

    // ── Message 1: Initiator → Responder ────────────────────────────
    let (init_msg, init_state) =
        handshake::create_initiation(&initiator_static, &responder_public, 1);

    // ── Message 2: Responder → Initiator ────────────────────────────
    let (peer_pubkey, timestamp, resp_msg, mut resp_session) =
        handshake::process_initiation(&responder_static, &init_msg, 2)
            .expect("initiation rejected");

    // Responder has authenticated the initiator's static key.
    assert_eq!(peer_pubkey, initiator_static.public_key());

    // ── Initiator finalises ──────────────────────────────────────────
    let mut init_session =
        handshake::process_response(init_state, &initiator_static, &resp_msg)
            .expect("response rejected");

    // Both sessions are ready for transport traffic.
    let (counter, ciphertext) = init_session.encrypt(b"ping").unwrap();
    let plaintext = resp_session.decrypt(counter, &ciphertext).unwrap();
    assert_eq!(&plaintext, b"ping");
}
```

### PSK mode with timestamp replay protection

```rust
use rustguard_core::handshake;
use rustguard_crypto::{StaticSecret, Tai64n};

fn main() {
    let initiator_static = StaticSecret::random();
    let responder_static = StaticSecret::random();
    let responder_public = responder_static.public_key();
    let psk = [0xABu8; 32];

    let (init_msg, init_state) =
        handshake::create_initiation_psk(&initiator_static, &responder_public, 42, &psk);

    // First initiation — no previous timestamp recorded.
    let (_, ts, resp_msg, mut resp_session) =
        handshake::process_initiation_psk(&responder_static, &init_msg, 99, &psk, None)
            .expect("first initiation rejected");

    let mut init_session =
        handshake::process_response(init_state, &initiator_static, &resp_msg)
            .expect("response rejected");

    // Replaying the same initiation is now rejected because ts is the stored threshold.
    let result =
        handshake::process_initiation_psk(&responder_static, &init_msg, 100, &psk, Some(&ts));
    assert!(result.is_none(), "replayed initiation must be rejected");
}
```

### `no_std` path with explicit ephemeral and timestamp

```rust
use rustguard_core::handshake;
use rustguard_crypto::{EphemeralSecret, StaticSecret, Tai64n};

// Inject deterministic values from the caller (e.g., kernel module RNG).
let initiator_static = StaticSecret::random();
let responder_static = StaticSecret::random();
let responder_public = responder_static.public_key();

let ephemeral = EphemeralSecret::random(); // caller-supplied
let timestamp = Tai64n::now();             // caller-supplied

let (init_msg, init_state) = handshake::create_initiation_with(
    &initiator_static,
    &responder_public,
    1,
    &[0u8; 32], // no PSK
    ephemeral,
    timestamp,
);

let resp_eph = EphemeralSecret::random(); // caller-supplied
let (_, _, resp_msg, _resp_session) = handshake::process_initiation_with(
    &responder_static,
    &init_msg,
    2,
    &[0u8; 32],
    None,
    resp_eph,
).expect("initiation rejected");

let _init_session =
    handshake::process_response(init_state, &initiator_static, &resp_msg)
        .expect("response rejected");
```

## See Also

- [rustguard-core](05-Modules/11-rustguard-core.md) — Crate-level reference covering `TransportSession`, `ReplayWindow`, `SessionTimers`, `CookieChecker`, and wire message formats
- [rustguard-crypto](05-Modules/12-rustguard-crypto.md) — Cryptographic primitives (`X25519`, `ChaCha20-Poly1305`, `BLAKE2s`, `HKDF`, `TAI64N`) consumed directly by this module
- [cookie](05-Modules/03-cookie.md) — DoS-mitigation cookie mechanism that gates handshake processing under load
- [tunnel](05-Modules/09-tunnel.md) — Tunnel loop that calls `create_initiation` / `process_initiation` and manages `InitiatorHandshake` state
- [server](05-Modules/08-server.md) — Enrollment server that stores pending handshake state with expiry tracking
- [Core Concepts](02-Architecture/02-Core-Concepts.md) — Noise protocol background and WireGuard key terminology
- [Data Flow](02-Architecture/03-Data-Flow.md) — End-to-end packet path showing where the handshake fits in the tunnel loop