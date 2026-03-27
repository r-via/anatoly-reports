# Package Overview

> Inventory of all packages in the RustGuard workspace, their build targets, and their roles within the monorepo.

## Overview

The RustGuard repository is a Cargo workspace of six crates plus one out-of-tree kernel module. The workspace root (`Cargo.toml`) declares shared metadata — version, edition, and license — that each member crate inherits. `rustguard-kmod` is built separately using the kernel build system (Kbuild) and is not listed as a Cargo workspace member.

All workspace members share a single `Cargo.lock` and can be built, tested, and linted together with standard `cargo` commands. See [Dependency Graph](02-Dependency-Graph.md) for a full picture of inter-crate relationships, and [System Overview](../02-Architecture/01-System-Overview.md) for component responsibilities.

## Workspace Configuration

The workspace root defines shared properties inherited by every member crate:

```toml
[workspace.package]
version = "0.1.0"
edition = "2024"
license = "MIT OR Apache-2.0"
```

The release profile is tuned for binary size and security:

```toml
[profile.release]
strip         = true
lto           = true
opt-level     = "z"
codegen-units = 1
panic         = "abort"
```

| Profile flag | Effect |
|---|---|
| `strip = true` | Strips debug symbols from release binaries |
| `lto = true` | Enables full link-time optimisation across crate boundaries |
| `opt-level = "z"` | Optimises for binary size rather than speed |
| `codegen-units = 1` | Single codegen unit for maximum cross-function inlining |
| `panic = "abort"` | No unwinding; reduces binary size and eliminates unwinder overhead |

## Cargo Workspace Members

| Crate | Type | `no_std` | Primary Role |
|---|---|:---:|---|
| `rustguard-crypto` | library | ✓ | Cryptographic primitives: X25519, ChaCha20-Poly1305, XChaCha20-Poly1305, HMAC-BLAKE2s, HKDF, TAI64N |
| `rustguard-core` | library | ✓ | Noise_IKpsk2 handshake, transport sessions, replay window, timer state machine |
| `rustguard-tun` | library | — | Cross-platform TUN device abstraction: utun (macOS), Linux TUN, AF_XDP, io_uring, BPF loader |
| `rustguard-daemon` | library | — | `wg.conf`-compatible tunnel daemon, packet routing, interface management |
| `rustguard-enroll` | library | — | Zero-config enrollment: token-derived key exchange, CIDR IP pool, pairing window, persistence |
| `rustguard-cli` | binary (`rustguard`) | — | CLI dispatcher: `up`, `serve`, `join`, `open`, `close`, `status`, `genkey`, `pubkey` |

`rustguard-crypto` and `rustguard-core` compile in both `std` and `no_std` modes. Each file begins with:

```rust
#![cfg_attr(not(feature = "std"), no_std)]
```

Enabling `no_std` strips allocation and standard-library dependencies so that the same protocol and cryptographic source is reused by the kernel module without modification.

`rustguard-cli` is the only crate that produces a binary. Its `Cargo.toml` declares:

```toml
[[bin]]
name = "rustguard"
path = "src/main.rs"
```

All other members produce `rlib` libraries consumed by upstream crates or the kernel module.

## The Kernel Module — `rustguard-kmod`

`rustguard-kmod` lives at `rustguard-kmod/` in the repository root but is **not** a Cargo workspace member. It is built with the kernel's out-of-tree module build system:

```bash
cd rustguard-kmod
make KERNELDIR=/lib/modules/$(uname -r)/build
```

The `Makefile` stages `no_std`-compiled sources from `rustguard-crypto` and `rustguard-core` into `src/gen/` before invoking Kbuild, rewriting crate-relative `use` paths to local module paths. This ensures no protocol logic is duplicated between the userspace and kernel builds — both run the same source code.

A C shim (`wg_crypto.c`, `wg_genl.c`, `wg_net.c`, `wg_queue.c`, `wg_socket.c`, `wg_timer.c`) bridges Rust code to kernel API surfaces not yet covered by the upstream Rust-in-kernel bindings. The module targets Linux 6.10+.

See [rustguard-kmod](../05-Modules/06-rustguard-kmod.md) for file-level detail.

## Examples

### Build all workspace crates

```bash
cargo build --workspace
```

### Build the release binary

```bash
cargo build -p rustguard-cli --release
# Produces: target/release/rustguard
```

### Run the full test suite

```bash
cargo test --workspace
```

### Check a single crate compiles without warnings

```bash
cargo clippy -p rustguard-core -- -D warnings
```

### Verify the `no_std` boundary for dual-mode crates

```bash
cargo build -p rustguard-crypto --target thumbv7em-none-eabihf
cargo build -p rustguard-core   --target thumbv7em-none-eabihf
```

These commands confirm that no `std`-only dependency has been introduced into either dual-mode crate.

### Build the kernel module

```bash
cd rustguard-kmod
make stage
make KERNELDIR=/lib/modules/$(uname -r)/build modules
```

## See Also

- [Dependency Graph](02-Dependency-Graph.md) — Inter-package dependencies and build order
- [Shared Conventions](03-Shared-Conventions.md) — Cross-package coding standards and shared configuration
- [System Overview](../02-Architecture/01-System-Overview.md) — Component responsibilities and execution environments
- [Source Tree](../06-Development/01-Source-Tree.md) — Annotated file-level map of every crate
- [Build and Test](../06-Development/02-Build-and-Test.md) — Full build commands, test runner, and benchmark infrastructure