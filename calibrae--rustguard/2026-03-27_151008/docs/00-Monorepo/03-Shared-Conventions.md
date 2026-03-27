# Shared Conventions

> Cross-package coding standards, naming rules, and structural patterns that apply uniformly to every crate in the RustGuard workspace.

## Overview

RustGuard is a Cargo workspace of six crates plus one out-of-tree kernel module. Because `rustguard-crypto` and `rustguard-core` are reused in both userspace and kernel builds, their API contracts and internal patterns must be consistent enough for callers in any environment. This page documents the conventions that span crate boundaries — workspace-level configuration, naming uniformity, `lib.rs` structure, API pairing rules, the concurrency model, and where WireGuard's protocol constants live.

For per-file style, doc-comment rules, `unsafe` policy, and error-handling guidelines see [Code Conventions](../06-Development/03-Code-Conventions.md). For crate roles and the workspace release profile see [Package Overview](01-Package-Overview.md). For the inter-crate dependency graph see [Dependency Graph](02-Dependency-Graph.md).

## Workspace Inheritance

Every member crate inherits `version`, `edition`, and `license` from the workspace root rather than declaring them locally:

```toml
[package]
name    = "rustguard-core"
version.workspace = true
edition.workspace = true
license.workspace = true
```

Internal dependencies are expressed as path references using `workspace = true` and without a version field:

```toml
[dependencies]
rustguard-crypto = { path = "../rustguard-crypto", default-features = false }
```

Specifying `default-features = false` on internal path references is mandatory for crates that consume `rustguard-crypto` or `rustguard-core` in a `no_std` context, and is the established pattern even in `std`-only consumers so that feature selection remains explicit.

External dependency versions are declared directly in the member `Cargo.toml` — the workspace does not use `[workspace.dependencies]` tables. All crates that share an external dependency must pin the same semver-compatible version to keep the workspace lock stable.

## Naming Conventions

Naming is uniform across all crates. There are no crate-local naming exceptions.

### Types

Public types use `PascalCase`. Names are chosen to be self-explanatory without the crate prefix, since callers qualify them at the use site:

| Crate | Examples |
|---|---|
| `rustguard-crypto` | `StaticSecret`, `EphemeralSecret`, `PublicKey`, `SharedSecret`, `Tai64n` |
| `rustguard-core` | `TransportSession`, `ReplayWindow`, `SessionTimers`, `CookieChecker`, `CookieState` |
| `rustguard-tun` | `Tun`, `TunConfig` |
| `rustguard-daemon` | `Config`, `InterfaceConfig`, `PeerConfig`, `CidrAddr` |
| `rustguard-enroll` | `IpPool`, `EnrolledPeer`, `PeerState`, `ServerState` |

### Functions

Public functions use `snake_case`. Naming follows `verb_noun` order. Functions that return a variant without allocation are suffixed `_to` (writes into a caller-supplied buffer) or `_in_place` (overwrites the input buffer):

| Allocating | Zero-alloc |
|---|---|
| `seal(key, counter, aad, plaintext) -> Vec<u8>` | `seal_to(key, counter, plaintext, buf) -> usize` |
| `open(key, counter, aad, ciphertext) -> Option<Vec<u8>>` | `open_to(key, counter, buf, ct_len) -> Option<usize>` |
| `encrypt(&mut self, plaintext) -> Option<(u64, Vec<u8>)>` | `encrypt_to(&mut self, plaintext, out) -> Option<(u64, usize)>` |
| `decrypt(&mut self, counter, ciphertext) -> Option<Vec<u8>>` | `decrypt_in_place(&mut self, counter, buf, ct_len) -> Option<usize>` |

### Constants

Public constants use `SCREAMING_SNAKE_CASE`. All WireGuard wire constants are defined in `rustguard-crypto` and `rustguard-core` — not redefined in upper-layer crates:

```rust
// rustguard-crypto/src/lib.rs
pub const CONSTRUCTION: &[u8] = b"Noise_IKpsk2_25519_ChaChaPoly_BLAKE2s";
pub const IDENTIFIER:   &[u8] = b"WireGuard v1 zx2c4 Jason@zx2c4.com";
pub const LABEL_MAC1:   &[u8] = b"mac1----";
pub const LABEL_COOKIE: &[u8] = b"cookie--";
pub const AEAD_TAG_LEN:     usize = 16;
pub const MAX_PACKET_SIZE:  usize = 1500 + AEAD_TAG_LEN;

// rustguard-core/src/messages.rs
pub const INITIATION_SIZE:        usize        = 148;
pub const RESPONSE_SIZE:          usize        = 92;
pub const COOKIE_REPLY_SIZE:      usize        = 64;
pub const TRANSPORT_HEADER_SIZE:  usize        = 16;
pub const MSG_INITIATION:         u32          = 1;
pub const MSG_RESPONSE:           u32          = 2;
pub const MSG_COOKIE_REPLY:       u32          = 3;
pub const MSG_TRANSPORT:          u32          = 4;
pub const REKEY_AFTER_TIME:       Duration     = Duration::from_secs(120);
pub const REJECT_AFTER_TIME:      Duration     = Duration::from_secs(180);
pub const REKEY_AFTER_MESSAGES:   u64          = (1u64 << 60) - 1;
pub const WINDOW_SIZE:            u64          = 2048;
```

Upper-layer crates import these constants rather than hardcoding their values.

## Library API Surface Pattern

Each library crate follows the same `lib.rs` structure: private `mod` declarations followed by selective `pub use` re-exports that form the public API. Internal module structure is an implementation detail hidden from callers.

```rust
// rustguard-crypto/src/lib.rs
#![cfg_attr(not(feature = "std"), no_std)]
extern crate alloc;

mod x25519;
mod aead;
mod blake2s;
mod tai64n;

pub use self::x25519::{StaticSecret, PublicKey, SharedSecret, EphemeralSecret};
pub use self::aead::{seal, open, seal_to, open_to, xseal, xopen, AEAD_TAG_LEN, MAX_PACKET_SIZE};
pub use self::blake2s::{hash, mac, hkdf};
pub use self::tai64n::Tai64n;
```

```rust
// rustguard-core/src/lib.rs
#![cfg_attr(not(feature = "std"), no_std)]
extern crate alloc;

pub mod cookie;
pub mod handshake;
pub mod messages;
pub mod replay;
pub mod session;
pub mod timers;

pub use rustguard_crypto as crypto;
```

The rule: a module is `mod` (private) if its internal organisation does not need to be visible to callers; it is `pub mod` if callers need to qualify sub-items (e.g., `handshake::create_initiation`). The crypto crate uses the stricter private-then-re-export pattern because its items are consumed by name at the top level. The core crate uses `pub mod` because callers typically import from specific submodules.

## Allocating and Zero-Alloc API Pairs

Every crypto and session operation that produces output is provided in two forms: an allocating form that returns a `Vec<u8>`, and a zero-alloc form that writes into a caller-supplied `&mut [u8]`. The zero-alloc variants are used in hot paths (the daemon tunnel loop, enrollment server packet path) to eliminate heap pressure.

The pairing rule:

- **Allocating** — named `verb(...)` returning `Vec<u8>` or `Option<Vec<u8>>`. Suitable for control paths and tests.
- **Zero-alloc** — named `verb_to(...)` or `verb_in_place(...)` returning `usize` or `Option<usize>`. The caller is responsible for sizing the output buffer correctly. `seal_to` and `encrypt_to` assert that the buffer is large enough (minimum `plaintext.len() + AEAD_TAG_LEN`).

Both forms must produce identical output. New operations added to `rustguard-crypto` or `rustguard-core` that produce variable-length output must provide both forms.

## Feature Flag Anatomy

`rustguard-crypto` and `rustguard-core` are the only crates that declare a `std` feature. All upper-layer crates require `std` unconditionally and do not declare feature flags.

The canonical pattern for a dual-mode crate:

```toml
# Cargo.toml
[features]
default = ["std"]
std     = ["getrandom", "rand_core/getrandom"]
```

```rust
// src/lib.rs
#![cfg_attr(not(feature = "std"), no_std)]
extern crate alloc;
```

Functions that require OS entropy or a system clock are gated on the `std` feature:

```rust
#[cfg(feature = "std")]
pub fn random() -> Self {
    Self(x25519_dalek::StaticSecret::random_from_rng(rand_core::OsRng))
}
```

For every `std`-gated convenience function there is a companion `_with` variant that accepts the entropy or timestamp as a parameter, making it usable from `no_std` callers:

```rust
// std convenience wrapper
#[cfg(feature = "std")]
pub fn create_initiation(
    static_key: &StaticSecret,
    their_public: &PublicKey,
    sender_index: u32,
) -> (Initiation, InitiatorHandshake) { … }

// Core — usable from both std and no_std
pub fn create_initiation_with(
    static_key: &StaticSecret,
    their_public: &PublicKey,
    sender_index: u32,
    ephemeral: EphemeralSecret,
    timestamp: Tai64n,
) -> (Initiation, InitiatorHandshake) { … }
```

Never use `std::` types directly in `rustguard-crypto` or `rustguard-core`. Use `core::` and `alloc::` equivalents.

## Concurrency Model

The daemon and enrollment server use OS threads, not an async runtime. Shared state follows a two-level locking pattern:

- `Arc<RwLock<PeerMap>>` guards the peer table — concurrent reads are the common case; writes occur only on enrollment or peer removal.
- `Arc<Mutex<PeerState>>` guards individual peer state — peers are locked independently so one peer's I/O does not block another's.

The lock is released before any I/O operation. Packet encryption and decryption happen after releasing the peer map read lock so that routing lookups do not block the TUN write path.

`mpsc` channels are not used for the packet path. The tunnel loop uses three threads (TUN→UDP, UDP→TUN, timer) communicating through the shared peer state, not through channels, to avoid additional copies.

## Testing Organization

Unit tests live in `#[cfg(test)] mod tests` blocks at the bottom of each source file. Integration tests that exercise multiple crates together live in `tests/` directories at the crate root.

Test function names describe the behaviour being verified, not the function under test:

```rust
// Preferred: behaviour-oriented names
fn seal_then_open() { … }
fn wrong_key_fails() { … }
fn tampered_ciphertext_fails() { … }
fn replay_window_boundary_exact() { … }

// Avoid: function-oriented names
fn test_seal() { … }
fn test_open() { … }
```

Each security-relevant property gets its own test function. Combining multiple failure modes in a single test is prohibited.

`unwrap()` is permitted only in `#[test]` and `#[cfg(test)]` contexts. Library code paths must return `Option<T>` or `io::Result<T>` — see [Code Conventions](../06-Development/03-Code-Conventions.md) for the full error-handling policy.

## Examples

The following example adds a hypothetical `rustguard-metrics` workspace crate following all shared conventions: workspace inheritance in `Cargo.toml`, private-then-re-export structure in `lib.rs`, and a zero-alloc/allocating API pair.

```toml
# rustguard-metrics/Cargo.toml
[package]
name    = "rustguard-metrics"
version.workspace = true
edition.workspace = true
license.workspace = true

[dependencies]
rustguard-core   = { path = "../rustguard-core", default-features = false }
rustguard-crypto = { path = "../rustguard-crypto", default-features = false }
```

```rust
// rustguard-metrics/src/lib.rs
mod counters;
mod snapshot;

pub use self::counters::SessionCounters;
pub use self::snapshot::{snapshot, snapshot_to, Snapshot};

// SCREAMING_SNAKE_CASE for public constants
pub const MAX_SNAPSHOT_LEN: usize = 256;
```

```rust
// rustguard-metrics/src/snapshot.rs

/// Serialise session counters into a JSON byte string.
///
/// Allocating form — suitable for control paths and logging.
pub fn snapshot(counters: &SessionCounters) -> Vec<u8> { … }

/// Zero-alloc form: writes into `buf`, returns bytes written.
///
/// `buf` must be at least `MAX_SNAPSHOT_LEN` bytes.
pub fn snapshot_to(counters: &SessionCounters, buf: &mut [u8]) -> usize { … }
```

```rust
// rustguard-metrics/src/counters.rs
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn snapshot_roundtrip() { … }

    #[test]
    fn buf_too_small_does_not_panic() { … }
}
```

## See Also

- [Package Overview](01-Package-Overview.md) — Workspace configuration, release profile, and crate inventory
- [Dependency Graph](02-Dependency-Graph.md) — Inter-crate dependency topology and the `no_std` boundary
- [Code Conventions](../06-Development/03-Code-Conventions.md) — Per-file style, `unsafe` policy, doc-comment rules, and error-handling patterns
- [Build and Test](../06-Development/02-Build-and-Test.md) — `cargo fmt`, `cargo clippy`, and test runner commands
- [rustguard-crypto](../05-Modules/12-rustguard-crypto.md) — Cryptographic primitives that define the allocating/zero-alloc pairs and protocol constants
- [rustguard-core](../05-Modules/11-rustguard-core.md) — Protocol state machine that extends those patterns to session and handshake operations