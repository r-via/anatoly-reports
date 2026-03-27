# Design Decisions

> Architecture Decision Records (ADRs) documenting the key technical choices made in RustGuard.

## Overview

This page records the major design decisions made across RustGuard's seven-crate architecture. Each ADR explains what was decided, why, what alternatives were considered, and what consequences follow. Decisions are ordered by the phase in which they were made, corresponding to the project's commit history.

---

## ADR-001 — No External WireGuard Library (libwg)

**Status:** Accepted

**Decision:** Implement the complete WireGuard protocol from scratch rather than binding to `libwg` or any existing WireGuard userspace library.

**Context:** Several Go and C userspace WireGuard implementations exist. Binding to them from Rust would have been faster to ship.

**Rationale:** The project goal is to understand WireGuard internals by building them. A binding would defeat that purpose and introduce a C dependency incompatible with the `no_std` kernel module target. Every primitive — X25519, ChaCha20-Poly1305, BLAKE2s, HKDF, TAI64N — is implemented in `rustguard-crypto` under `#![cfg_attr(not(feature = "std"), no_std)]`.

**Consequences:** Full control over all cryptographic code paths; dual `std`/`no_std` support is possible; the codebase is self-contained; interoperability with official WireGuard requires strict adherence to the whitepaper wire format.

---

## ADR-002 — Dual std/no_std Across `rustguard-crypto` and `rustguard-core`

**Status:** Accepted

**Decision:** The `rustguard-crypto` and `rustguard-core` crates compile under both `std` and `no_std` (with `alloc`).

**Context:** The kernel module (`rustguard-kmod`) runs in ring 0 where the standard library is unavailable. Sharing protocol logic between userspace and the kernel module without code duplication requires a common `no_std` foundation.

**Rationale:** Time-dependent types (`Instant`, system clock) are abstracted behind a `Timestamp` type alias and `cfg(feature = "std")` gates. The `no_std` path falls back to monotonic counters or caller-injected timestamps (e.g., `session_started_at(now_ns: u64)` vs `session_started()`). RNG calls (`getrandom`) are gated behind `std`; the kernel module provides its own RNG via `get_random_bytes()`.

**Consequences:** All public API functions that need a clock or RNG have both a `std` convenience wrapper and a `_with` / `_at` variant for `no_std` callers. The cookie checker exposes `CookieChecker::new_with(our_public, secret, now_ts)` alongside `CookieChecker::new(our_public)`.

---

## ADR-003 — Noise_IKpsk2 Handshake (Not IK)

**Status:** Accepted

**Decision:** Implement Noise_IKpsk2 (with pre-shared key support) rather than plain Noise_IK.

**Context:** The WireGuard whitepaper specifies Noise_IKpsk2. PSK support adds quantum-resistance as a second layer over the elliptic-curve DH.

**Rationale:** The protocol constant `CONSTRUCTION = b"Noise_IKpsk2_25519_ChaChaPoly_BLAKE2s"` is baked into the chaining key derivation. PSK is threaded through `process_response` and `process_initiation_with` via the standard HKDF mix:

```rust
// PSK phase — from rustguard-core/src/handshake.rs
let (new_ck, t, key) = crypto::hkdf(&ck, psk);
ck = new_ck;
h = mix_hash(&h, &t);
```

When PSK is unused, callers pass `&[0u8; 32]`, which is backward-compatible and produces a distinct but deterministic key schedule.

**Consequences:** All handshake entry points expose both a PSK variant (`create_initiation_psk`, `process_initiation_psk`) and a no-PSK convenience wrapper that supplies the zero PSK automatically.

---

## ADR-004 — MAC1 Verified Before DH Operations

**Status:** Accepted — security fix from Phase 2 audit.

**Decision:** MAC1 is verified as the first operation in `process_initiation_with`, before any Diffie-Hellman computation.

**Context:** The original implementation performed MAC1 verification after the DH step. An attacker sending malformed handshake packets with a valid wire format but invalid MAC1 would cause the responder to burn CPU on expensive DH operations before the packet was rejected.

**Rationale:** MAC1 is a BLAKE2s-MAC keyed with a hash of the responder's public key — a cheap, allocation-free check. Placing it first converts unauthenticated DH cost from O(attacker throughput) to O(0).

```rust
// From rustguard-core/src/handshake.rs — MAC1 checked first
let wire = msg.to_bytes();
let expected_mac1 = compute_mac1(&our_public, &wire[..116]);
if !constant_time_eq(&msg.mac1, &expected_mac1) {
    return None;
}
// DH operations follow only after this gate.
```

**Consequences:** All handshake processing enforces this ordering. Regression is prevented by the existing tamper-rejection tests in `handshake.rs`.

---

## ADR-005 — Split `check()` / `update()` on the Replay Window

**Status:** Accepted — security fix from Phase 2 audit.

**Decision:** `ReplayWindow` exposes separate `check(counter)` and `update(counter)` methods. `TransportSession::decrypt` calls `check` before AEAD, then `update` only on successful decryption.

**Context:** The original implementation advanced the replay bitmap before verifying the AEAD tag. An attacker could send packets with valid-range counter values but garbage ciphertext, advancing the window and causing legitimate later packets with those counters to be rejected.

**Rationale:** The check-before-update split ensures the window state is only mutated when a packet is proven authentic. `TransportSession::decrypt` encodes this invariant:

```rust
// From rustguard-core/src/session.rs
pub fn decrypt(&mut self, counter: u64, ciphertext: &[u8]) -> Option<Vec<u8>> {
    if !self.recv_window.check(counter) {
        return None;
    }
    // AEAD must succeed before the window is updated.
    let plaintext = crypto::open(&self.key_recv, counter, &[], ciphertext)?;
    self.recv_window.update(counter);
    Some(plaintext)
}
```

The same two-phase discipline applies to `decrypt_in_place`.

**Consequences:** The combined `check_and_update` method is retained for test convenience only. Production code paths always use the split form.

---

## ADR-006 — Timestamp Enforcement to Prevent Handshake Replay

**Status:** Accepted — security fix from Phase 2 audit.

**Decision:** `process_initiation_with` rejects handshake initiations whose TAI64N timestamp is not strictly after the last accepted timestamp for that peer.

**Context:** Without timestamp enforcement, a captured handshake initiation packet could be replayed later to hijack an active session. The WireGuard whitepaper mandates strictly monotonic timestamps.

**Rationale:** `Tai64n::is_after` compares the full 12-byte TAI64N value. The server passes the peer's `last_timestamp: Option<&Tai64n>`. When `Some`, any initiation with a non-advancing timestamp returns `None`.

**Consequences:** Peers must have approximately synchronized clocks (TAI64N is absolute time). Clock skew does not cause false rejects for legitimate initiations because the check is monotonic relative to the peer's own last accepted timestamp, not wall clock time.

---

## ADR-007 — `subtle::ConstantTimeEq` for MAC Comparisons

**Status:** Accepted — security fix from Phase 2 audit.

**Decision:** All MAC comparisons use `subtle::ConstantTimeEq` rather than hand-rolled constant-time functions.

**Context:** The Phase 2 audit found that hand-rolled comparisons using `core::hint::black_box` were unreliable — compiler and CPU optimizations can still introduce timing variation. Timing side-channels on MAC comparisons can leak secret key material.

**Rationale:** The `subtle` crate provides a well-audited, formally-specified constant-time equality primitive. `CookieChecker::verify_mac1`, `CookieChecker::verify_mac2_from_bytes`, and the handshake `constant_time_eq` all delegate to `subtle::ConstantTimeEq`.

**Consequences:** `subtle` is a dependency of both `rustguard-core` (MAC checks) and `rustguard-crypto` (future primitive-level use). The trade-off is a small dependency; the benefit is audited constant-time guarantees.

---

## ADR-008 — DoS Cookie Mechanism with Rotating Secrets

**Status:** Accepted — implemented in Phase 1, Commit 3.

**Decision:** Implement the full WireGuard cookie mechanism: MAC1 always present, MAC2 required when `CookieChecker::under_load` is set, cookie secret rotates every 120 seconds.

**Context:** Without the cookie mechanism, a responder processing handshake initiations can be CPU-exhausted by an attacker spoofing source IPs, because the responder cannot verify the initiator's IP reachability before performing DH.

**Rationale:** The cookie is `MAC(secret, sender_ip_port)` encrypted with XChaCha20-Poly1305. The rotating secret ensures cookies expire. The flow is:

```
Initiator → Responder: Handshake Initiation (mac2 = zeros)
Responder → Initiator: Cookie Reply (under load)
Initiator → Responder: Handshake Initiation (mac2 = MAC(cookie, msg || mac1))
Responder:             verifies MAC2, processes handshake
```

`CookieState` (client side) stores the decrypted cookie and its freshness window. `CookieChecker` (server side) generates cookies and verifies MAC2. Both are in `rustguard-core/src/cookie.rs`.

**Consequences:** The server must set `checker.under_load = true` when under attack; `false` skips MAC2 verification for normal operation. Cookie lifetime matches the secret rotation period (120 s).

---

## ADR-009 — Zero-Config Enrollment Protocol

**Status:** Accepted — implemented in Phase 3.

**Decision:** Add a custom enrollment protocol (`rustguard-enroll`) that eliminates manual key exchange and IP planning. Standard `wg.conf` mode (`rustguard up`) is preserved unchanged.

**Context:** The standard WireGuard setup requires generating keys on both sides, exchanging public keys out-of-band, manually assigning IP addresses, and writing config files — which is painful for homelab use.

**Rationale:** A shared enrollment token derives an XChaCha20-Poly1305 encryption key via `derive_token_key(token)` — a BLAKE2s hash keyed with a domain label. The token gates who can enroll without being transmitted on the wire. The server's CIDR IP pool allocator assigns sequential addresses; the server takes `.1`. The two wire messages are compact fixed-size UDP datagrams (76 and 81 bytes respectively). The raw token never appears on the wire.

```rust
// From rustguard-enroll/src/protocol.rs
pub fn derive_token_key(token: &str) -> [u8; 32] {
    crypto::hash(&[b"rustguard-enroll-v1", token.as_bytes()])
}
```

**Consequences:** `rustguard-enroll` depends on `rustguard-crypto` but not `rustguard-core` — the enrollment channel is a bootstrap step; the full Noise_IK handshake follows via the normal tunnel path.

---

## ADR-010 — Pairing Window Model (Physical Presence)

**Status:** Accepted — implemented in Phase 3, Commit 8.

**Decision:** Enrollment is closed by default. A UNIX domain control socket (`rustguard open <seconds>`, `rustguard close`) gates when new peers can enroll. An atomic timestamp drives auto-close with no background thread.

**Context:** An always-open enrollment endpoint means any party with the token can enroll at any time, including after the intended enrollment window has passed.

**Rationale:** Closing enrollment by default limits the attack surface. The control socket allows server operators to open a bounded window for physical-presence-model enrollment (analogous to Zigbee pairing). The expiry is checked atomically on each incoming enrollment request — no timer goroutine or thread is needed.

**Consequences:** Operators must explicitly `rustguard open` before new peers can join. Existing sessions are not affected by window state. `rustguard status` shows current window state and peer count.

---

## ADR-011 — AF_XDP for Zero-Copy Encrypted UDP I/O

**Status:** Accepted — performance optimization, Phase 4.

**Decision:** On Linux, replace the standard UDP socket with an `AF_XDP` socket (`XdpSocket`) backed by a UMEM region and lock-free ring buffers. The encrypted packet path bypasses the kernel network stack.

**Context:** Profiling showed the standard `recvmmsg`/`sendmmsg` path saturating at ~750 Mbps on the test rig due to per-packet system call overhead and kernel network stack processing.

**Rationale:** `AF_XDP` routes packets from the NIC directly to a user-space ring via a BPF XDP filter that selects UDP port 51820. The UMEM region is `mmap`-shared between user space and the kernel. Ring indices are coordinated with `AtomicU32` loads/stores (`Acquire`/`Release` ordering). The TUN path (plaintext side) is unaffected.

```
NIC → XDP BPF (UDP:51820) → AF_XDP RX ring → decrypt → TUN
TUN → encrypt → AF_XDP TX ring → NIC
```

**Consequences:** `XdpSocket::create` requires `CAP_NET_ADMIN` and an XDP-capable NIC driver. A BPF program must be loaded via `rustguard-tun/src/bpf_loader.rs` before the socket is bound. Standard socket mode remains available as a fallback.

---

## ADR-012 — `ZeroizeOnDrop` on Handshake and Session Key Material

**Status:** Accepted — security hardening, Phase 2 audit.

**Decision:** `InitiatorHandshake` and `TransportSession` derive `zeroize::ZeroizeOnDrop`. Session keys, chaining keys, and PSKs are zeroed when these structs are dropped.

**Context:** Key material held in memory after a session ends can be recovered from core dumps, swap, or cold-boot attacks.

**Rationale:** `zeroize` ensures secrets are overwritten at drop time, preventing them from lingering in allocator free lists or being written to swap. Fields that are not secret (indices, replay window) are annotated `#[zeroize(skip)]` to avoid unnecessary work.

**Consequences:** Care must be taken not to copy key material into non-zeroizing structures before the drop occurs. The `encrypt` and `decrypt` methods on `TransportSession` borrow the key in place and do not clone it.

---

## Examples

The following example demonstrates the MAC1-first guard (ADR-004) and the check/update split (ADR-005) as they appear in real integration between `rustguard-core` crates:

```rust
use rustguard_crypto::StaticSecret;
use rustguard_core::handshake::{create_initiation, process_initiation, process_response};

fn establish_session() {
    let initiator_static = StaticSecret::random();
    let responder_static = StaticSecret::random();
    let responder_public = responder_static.public_key();

    // Initiator builds msg1 — MAC1 computed internally over wire bytes.
    let (init_msg, init_state) = create_initiation(&initiator_static, &responder_public, 1);

    // Responder verifies MAC1 FIRST, then DH, then timestamp check.
    let (peer_key, _ts, resp_msg, mut resp_session) =
        process_initiation(&responder_static, &init_msg, 2)
            .expect("responder accepts valid initiation");

    assert_eq!(peer_key, initiator_static.public_key());

    // Initiator completes handshake, derives transport keys.
    let mut init_session =
        process_response(init_state, &initiator_static, &resp_msg)
            .expect("initiator accepts valid response");

    // Transport: replay window check happens before AEAD (ADR-005).
    let plaintext = b"hello";
    let (counter, ciphertext) = init_session.encrypt(plaintext).unwrap();
    let decrypted = resp_session.decrypt(counter, &ciphertext).unwrap();
    assert_eq!(&decrypted, plaintext);

    // Replay is rejected.
    assert!(resp_session.decrypt(counter, &ciphertext).is_none());
}
```

## See Also

- [System Overview](02-Architecture/01-System-Overview.md) — component diagram and responsibilities
- [Core Concepts](02-Architecture/02-Core-Concepts.md) — glossary of domain terms used in these ADRs
- [Data Flow](02-Architecture/03-Data-Flow.md) — how packets move through the system end-to-end
- [rustguard-crypto module](05-Modules/12-rustguard-crypto.md) — X25519, ChaCha20-Poly1305, BLAKE2s, HKDF, TAI64N primitives
- [rustguard-core module](05-Modules/11-rustguard-core.md) — Noise_IK handshake, sessions, replay window, timers
- [rustguard-tun module](05-Modules/07-rustguard-tun.md) — TUN devices, AF_XDP, io_uring
- [rustguard-enroll module](05-Modules/14-rustguard-enroll.md) — zero-config enrollment protocol
- [handshake module](05-Modules/04-handshake.md) — handshake function signatures and wire format
- [cookie module](05-Modules/03-cookie.md) — CookieChecker and CookieState API