# Code Conventions

> Style guide, patterns, and anti-patterns for contributing to RustGuard.

## Overview

RustGuard follows a set of conventions that emerged organically from its implementation goals: correctness first, minimal dependencies, and performance through well-structured code rather than clever tricks. These conventions apply across all seven crates in the workspace. Deviations require justification in the PR description.

## Formatting and Linting

All code must pass `cargo fmt` and `cargo clippy` before committing. These are non-negotiable — the CI gate will fail if either reports issues.

```bash
cargo fmt --all
cargo clippy --workspace -- -D warnings
```

The workspace uses Rust **edition 2024** (set in `Cargo.toml`). Format style follows the edition defaults with no additional `rustfmt.toml` overrides.

## Dependency Policy

Keep dependencies minimal. Every new crate added to a `Cargo.toml` requires justification. The bar is: "can this be done in 20 lines of project code?" If yes, do not add a crate.

Dependencies that are already established across the workspace and are permitted without further justification:

| Crate | Purpose |
|---|---|
| `chacha20poly1305` | AEAD encryption |
| `zeroize` | Key material destruction |
| `subtle` | Constant-time comparisons |
| `getrandom` | OS entropy |
| `libc` | Syscall bindings (platform crates only) |
| `serde` / `serde_json` | Config and state serialization |
| `clap` | CLI argument parsing |

## `unsafe` Rules

`unsafe` is permitted only in `rustguard-tun` and `rustguard-kmod`, where direct syscall interfaces (`libc`, kernel FFI) make it unavoidable. All other crates must remain safe Rust.

When `unsafe` is required:

1. Wrap it in a safe public API. Callers must never write `unsafe` blocks to use it.
2. Annotate the safety preconditions in a comment immediately before the block, using the `// Safety:` prefix:

```rust
// Safety: Ring pointers come from mmap and are valid for the socket lifetime.
unsafe impl Send for XdpSocket {}
```

3. Keep each `unsafe` block as small as possible — one logical operation per block.
4. For `#[repr(C)]` structs that cross the FFI boundary, document the kernel layout they mirror.

## Documentation Comments

Every public item (`pub fn`, `pub struct`, `pub enum`, `pub const`, `pub trait`) requires a doc comment. Module-level documentation uses `//!` inner doc comments at the top of the file:

```rust
//! Anti-replay sliding window.
//!
//! WireGuard uses a 2048-bit bitmap to track which nonces have been seen.
//! The window slides forward as new (higher) counters arrive.
```

Item-level documentation uses `///` outer doc comments. The first sentence is the summary line and must fit on one line. Further paragraphs elaborate on behavior, edge cases, and preconditions:

```rust
/// Check if a counter would be acceptable (without marking it).
/// Call `update()` only after the packet is authenticated.
pub fn check(&self, counter: u64) -> bool { … }
```

Private helper functions do not require doc comments but benefit from them when the logic is non-obvious. Inline `//` comments are preferred over block comments.

## Error Handling

**Never use `panic!` or `unwrap()` in library code paths.** The crypto and core crates (`rustguard-crypto`, `rustguard-core`) are no_std-compatible and used from the kernel module — panics are categorically unacceptable there.

The pattern for fallible operations:
- Return `Option<T>` for "this can legitimately fail at runtime" (e.g., authentication failure, nonce exhaustion, replay rejection).
- Return `io::Result<T>` for platform I/O in daemon and TUN crates.
- Reserve `expect()` for cases where failure represents an invariant violation that should have been caught earlier, and document the invariant:

```rust
// buf is sized to plaintext.len() + AEAD_TAG_LEN by the caller contract.
buf[len..len + AEAD_TAG_LEN].copy_from_slice(&tag);
```

`unwrap()` is acceptable only in `#[test]` blocks.

## Key Material Handling

All structs that hold cryptographic key material must derive `ZeroizeOnDrop`. Use the `zeroize` crate. Fields that should not be zeroed (indices, public state) are annotated `#[zeroize(skip)]`:

```rust
#[derive(Zeroize, ZeroizeOnDrop)]
pub struct InitiatorHandshake {
    ck: [u8; 32],
    h: [u8; 32],
    ephemeral: EphemeralSecret,
    #[zeroize(skip)]
    sender_index: u32,
    #[zeroize(skip)]
    their_public: PublicKey,
    psk: [u8; 32],
}
```

Never compare key material or MACs with `==`. Always use `subtle::ConstantTimeEq`:

```rust
fn constant_time_eq(a: &[u8; 16], b: &[u8; 16]) -> bool {
    use subtle::ConstantTimeEq;
    a.ct_eq(b).into()
}
```

## `no_std` Compatibility

`rustguard-crypto` and `rustguard-core` are dual `std`/`no_std` crates. Both crates begin with:

```rust
#![cfg_attr(not(feature = "std"), no_std)]

extern crate alloc;
```

The `std` feature gates code that requires the standard library (OS RNG, system clock, `Instant`). Functions that require OS entropy or wall-clock time in `std` mode must have a `_with` variant that accepts those values as parameters for `no_std` callers:

```rust
// std convenience — uses OsRng and system clock.
#[cfg(feature = "std")]
pub fn create_initiation(…) -> (Initiation, InitiatorHandshake) { … }

// Core function — usable from both std and no_std.
pub fn create_initiation_with(…, ephemeral: EphemeralSecret, timestamp: Tai64n) -> … { … }
```

Do not use `std::` types directly in `rustguard-crypto` or `rustguard-core` — use `core::` and `alloc::` equivalents.

## Security-Critical Ordering

Two patterns from the security audit must be preserved in any code that touches the handshake or transport path:

**MAC1 before DH.** The MAC1 check must occur before any Diffie-Hellman operation on the responder side. DH is expensive; processing unauthenticated packets burns CPU under load:

```rust
// ── Verify MAC1 FIRST — cheap check before any DH. ──
let expected_mac1 = compute_mac1(&our_public, &wire[..116]);
if !constant_time_eq(&msg.mac1, &expected_mac1) {
    return None;
}
// DH operations follow only if MAC1 is valid.
```

**Check before update in the replay window.** The `ReplayWindow` exposes separate `check()` and `update()` methods deliberately. Never call `update()` before the AEAD decryption succeeds:

```rust
// In transport receive:
if !replay_window.check(counter) { return None; }
let plaintext = crypto::open(&key, counter, &[], ciphertext)?;
replay_window.update(counter); // Only after successful authentication.
```

## File Descriptors

All file descriptors opened in platform code must use `O_CLOEXEC` at creation time. On error paths, close the fd before returning the error — never leak:

```rust
fn close_and_error(fd: i32) -> io::Error {
    let err = io::Error::last_os_error();
    unsafe { libc::close(fd) };
    err
}
```

## Testing

Tests live in `#[cfg(test)] mod tests` blocks at the bottom of each source file. The crypto and core tests are the most important — do not skip them when making protocol changes.

Test naming uses `snake_case` and describes the behavior being verified, not the function name:

```rust
#[test]
fn wrong_key_fails() { … }

#[test]
fn replay_window_boundary_exact() { … }
```

Each security-relevant property must have its own dedicated test. Do not combine multiple failure modes in a single test function.

Run the full suite before opening a PR:

```bash
cargo test --workspace
```

To run a single crate:

```bash
cargo test -p rustguard-core
cargo test -p rustguard-crypto
```

## Examples

The following example demonstrates the full set of conventions: a security-critical function with `ZeroizeOnDrop`, `Option` returns, MAC-first ordering, and a companion `_with` variant for `no_std` callers.

```typescript
// This is a Rust project — conventions illustrated in Rust, not TypeScript.
```

```rust
use rustguard_core::handshake::{create_initiation, process_initiation, process_response};
use rustguard_crypto::StaticSecret;

fn example_handshake() {
    // Keys are ZeroizeOnDrop — material is wiped when they go out of scope.
    let initiator_static = StaticSecret::random();
    let responder_static = StaticSecret::random();
    let responder_public = responder_static.public_key();

    // create_initiation is the std convenience wrapper over create_initiation_with.
    let (init_msg, init_state) = create_initiation(&initiator_static, &responder_public, 1);

    // process_initiation verifies MAC1 before any DH — per security conventions.
    // Returns None on any authentication failure; never panics.
    let (peer_pubkey, _ts, resp_msg, mut resp_session) =
        process_initiation(&responder_static, &init_msg, 2)
            .expect("responder accepts valid initiation");

    // process_response derives the transport session on the initiator side.
    let mut init_session =
        process_response(init_state, &initiator_static, &resp_msg)
            .expect("initiator accepts valid response");

    // Transport: encrypt returns None on nonce exhaustion (requires rekey).
    let plaintext = b"hello from the trenches";
    let (counter, ciphertext) = init_session.encrypt(plaintext).unwrap();

    // Decrypt: replay window check happens before AEAD, update happens after.
    let decrypted = resp_session.decrypt(counter, &ciphertext).unwrap();
    assert_eq!(&decrypted, plaintext);
}
```

## See Also

- [Build and Test](02-Build-and-Test.md) — how to run `cargo fmt`, `cargo clippy`, and the test suite
- [Source Tree](01-Source-Tree.md) — crate layout and which crates are `no_std`-compatible
- [Shared Conventions](../00-Monorepo/03-Shared-Conventions.md) — cross-package standards
- [rustguard-crypto](../05-Modules/12-rustguard-crypto.md) — crypto primitives that apply the key-material and constant-time conventions
- [rustguard-core](../05-Modules/11-rustguard-core.md) — protocol state machine where the security-ordering rules are enforced