# Dependency Graph

> Inter-crate dependency relationships and build order for the RustGuard Cargo workspace.

## Overview

The RustGuard workspace contains six Cargo crates arranged in a strict directed acyclic graph (DAG). Two leaf crates — `rustguard-crypto` and `rustguard-tun` — carry no internal dependencies and are consumed by every higher layer. `rustguard-cli` sits at the top as the only binary crate and depends, directly or transitively, on all five library crates.

`rustguard-kmod` is out-of-tree and is not a workspace member. It consumes `no_std`-compiled sources from `rustguard-crypto` and `rustguard-core` via a staging step in its `Makefile`.

For crate roles and build targets see [Package Overview](01-Package-Overview.md).

## Crate Dependency Graph

```
rustguard-cli  (binary: rustguard)
├── rustguard-daemon
│   ├── rustguard-core
│   │   └── rustguard-crypto
│   ├── rustguard-crypto
│   └── rustguard-tun
├── rustguard-enroll
│   ├── rustguard-core
│   │   └── rustguard-crypto
│   ├── rustguard-crypto
│   ├── rustguard-daemon
│   │   └── (see above)
│   └── rustguard-tun
└── rustguard-crypto

rustguard-kmod  (out-of-tree, not a workspace member)
├── rustguard-crypto  [no_std, staged via Makefile]
└── rustguard-core    [no_std, staged via Makefile]
```

## Per-Crate Dependencies

### `rustguard-crypto`

Leaf crate. No internal dependencies. Compiles in both `std` and `no_std` modes.

| Dependency | Version | Notes |
|---|---|---|
| `x25519-dalek` | 2 | X25519 key exchange; `static_secrets` + `zeroize` features |
| `chacha20poly1305` | 0.10 | ChaCha20-Poly1305 and XChaCha20-Poly1305 AEAD |
| `blake2` | 0.10 | BLAKE2s for HMAC and HKDF |
| `subtle` | 2 | Constant-time comparisons |
| `zeroize` | 1 | `ZeroizeOnDrop` on key material |
| `rand_core` | 0.6 | RNG trait bounds |
| `getrandom` | 0.2 | OS entropy (optional; gated on `std` feature) |

Dev dependency: `hex` 0.4 (test vector decoding).

### `rustguard-core`

Depends on `rustguard-crypto`. Compiles in both `std` and `no_std` modes.

| Dependency | Version | Notes |
|---|---|---|
| `rustguard-crypto` | workspace | Handshake crypto, AEAD, HKDF |
| `subtle` | 2 | Constant-time helpers |
| `zeroize` | 1 | Zeroise handshake chaining key and hash on drop |
| `getrandom` | 0.2 | Session index CSPRNG (optional; gated on `std` feature) |

Dev dependency: `hex` 0.4.

### `rustguard-tun`

Leaf crate. No internal dependencies. Platform-aware external dependencies only.

| Dependency | Version | Notes |
|---|---|---|
| `libc` | 0.2 | Syscall constants, `ifreq`, file descriptor helpers |
| `io-uring` | 0.7 | Linux-only; gated on `cfg(target_os = "linux")` |

### `rustguard-daemon`

Depends on three workspace crates.

| Dependency | Version | Notes |
|---|---|---|
| `rustguard-core` | workspace | Handshake, transport sessions, timers |
| `rustguard-crypto` | workspace | Key generation, direct crypto calls |
| `rustguard-tun` | workspace | TUN device read/write |
| `base64` | 0.22 | `.conf` file key encoding/decoding |
| `getrandom` | 0.2 | Peer index generation |
| `libc` | 0.2 | Route management, socket operations |

### `rustguard-enroll`

Depends on all four lower-layer workspace crates.

| Dependency | Version | Notes |
|---|---|---|
| `rustguard-core` | workspace | Session management for enrolled peers |
| `rustguard-crypto` | workspace | Token-derived XChaCha20 key exchange |
| `rustguard-daemon` | workspace | Delegates to daemon after enrollment |
| `rustguard-tun` | workspace | TUN device setup for enrolled interface |
| `base64` | 0.22 | Key serialisation |
| `getrandom` | 0.2 | Token entropy |
| `libc` | 0.2 | UNIX domain control socket |

### `rustguard-cli`

Top-level binary crate. Produces the `rustguard` executable.

| Dependency | Version | Notes |
|---|---|---|
| `rustguard-daemon` | workspace | `up` command, `wg.conf`-compatible tunnel |
| `rustguard-enroll` | workspace | `serve`, `join`, `open`, `close`, `status` commands |
| `rustguard-crypto` | workspace | `genkey`, `pubkey` commands |
| `base64` | 0.22 | Key display formatting |

## Build Order

Cargo resolves the DAG automatically. The topological build order for a full `--workspace` build is:

1. `rustguard-crypto`
2. `rustguard-tun`
3. `rustguard-core`
4. `rustguard-daemon`
5. `rustguard-enroll`
6. `rustguard-cli`

Steps 1 and 2 are independent and will be compiled in parallel by Cargo. Steps 4 and 5 may also overlap partially depending on available parallelism.

## `no_std` Boundary

`rustguard-crypto` and `rustguard-core` each declare a conditional `no_std` attribute:

```rust
#![cfg_attr(not(feature = "std"), no_std)]
```

The `std` feature is on by default. Disabling it removes `getrandom` from both crates and pulls `rand_core` without its `getrandom` integration, making these crates suitable for kernel and bare-metal targets. All crates above `rustguard-core` in the graph (`rustguard-tun`, `rustguard-daemon`, `rustguard-enroll`, `rustguard-cli`) require `std` and are never built `no_std`.

`rustguard-kmod` stages `no_std` builds of `rustguard-crypto` and `rustguard-core` into its `src/gen/` directory during the `make stage` step.

## Examples

### Build only the leaf crates

```bash
cargo build -p rustguard-crypto -p rustguard-tun
```

### Verify the `no_std` boundary for dual-mode crates

```bash
cargo build -p rustguard-crypto --no-default-features --target thumbv7em-none-eabihf
cargo build -p rustguard-core   --no-default-features --target thumbv7em-none-eabihf
```

### Print the full dependency tree (including external crates)

```bash
cargo tree -p rustguard-cli
```

### Confirm no cycles exist in the workspace graph

```bash
cargo metadata --format-version 1 | jq '.resolve.nodes[] | select(.deps | length > 0) | .id'
```

### Build the kernel module after staging protocol sources

```bash
cd rustguard-kmod
make stage
make KERNELDIR=/lib/modules/$(uname -r)/build modules
```

## See Also

- [Package Overview](01-Package-Overview.md) — Crate roles, build targets, and workspace configuration
- [Shared Conventions](03-Shared-Conventions.md) — Cross-package coding standards
- [System Overview](../02-Architecture/01-System-Overview.md) — Component responsibilities and execution environments
- [Source Tree](../06-Development/01-Source-Tree.md) — Annotated file-level map of every crate
- [Build and Test](../06-Development/02-Build-and-Test.md) — Full build commands, test runner, and CI setup
- [rustguard-kmod](../05-Modules/06-rustguard-kmod.md) — Kernel module build system and C shim details