# Build and Test

> Reference for building, testing, and verifying the RustGuard workspace and kernel module.

## Overview

RustGuard is a Cargo workspace containing six Rust crates plus a separate out-of-tree Linux kernel module (`rustguard-kmod`) built with Kbuild. The six userspace crates are managed together through the workspace root `Cargo.toml`. The kernel module is built independently via its own `Makefile` and requires staged source files from the workspace crates before compilation.

## Prerequisites

| Requirement | Notes |
|---|---|
| Rust stable toolchain | Edition 2024; install via [rustup](https://rustup.rs) |
| `cargo` | Included with the Rust toolchain |
| `libc` development headers | Required by `rustguard-tun` on Linux |
| Linux kernel headers (`linux-headers-$(uname -r)`) | Required only for `rustguard-kmod`; targets kernel 6.10+ |
| LLVM / `clang` | Required only for `rustguard-kmod`; Kbuild is invoked with `LLVM=1` |
| `clang` + BPF toolchain | Required only to recompile `rustguard-tun/bpf/xdp_wg.c`; a pre-built `.o` is committed |

The `rustguard-kmod` directory is intentionally excluded from the Cargo workspace. It shares protocol code with `rustguard-crypto` and `rustguard-core` through a source-staging step rather than Cargo dependency resolution.

## Workspace Layout

```
Cargo.toml                  # workspace root (resolver = "2", edition = "2024")
rustguard-crypto/           # no_std-compatible cryptographic primitives
rustguard-core/             # Noise_IK handshake, transport sessions, timers
rustguard-tun/              # TUN/AF_XDP/io_uring platform abstraction
rustguard-daemon/           # wg.conf tunnel mode daemon
rustguard-enroll/           # zero-config enrollment protocol
rustguard-cli/              # rustguard binary (entry point)
rustguard-kmod/             # out-of-tree kernel module (Makefile, not Cargo)
tests/                      # shared test fixtures (peer_a.conf, peer_b.conf)
```

## Building

### Build All Workspace Crates (Debug)

```bash
cargo build --workspace
```

### Build the `rustguard` Binary (Release)

```bash
cargo build --release -p rustguard-cli
```

The release profile applies the following optimisations defined in the workspace `Cargo.toml`:

```toml
[profile.release]
strip = true
lto = true
opt-level = "z"
codegen-units = 1
panic = "abort"
```

The resulting binary is at `target/release/rustguard`.

### Build a Single Crate

```bash
cargo build -p rustguard-core
cargo build -p rustguard-crypto
```

### Build the Kernel Module

The kernel module cannot be built with `cargo`. It requires Linux kernel headers matching the running kernel (6.10+) and a source-staging step that copies and rewrites shared crate sources into the module tree.

```bash
# 1. Stage shared crate sources into rustguard-kmod/src/gen/
make -C rustguard-kmod stage

# 2. Build the kernel module against the running kernel's build tree
make -C rustguard-kmod modules

# 3. Clean staged sources and build artefacts
make -C rustguard-kmod clean
```

To target a different kernel version, set `KDIR` explicitly:

```bash
make -C rustguard-kmod modules KDIR=/usr/src/linux-headers-6.10.0-custom
```

The `stage` target copies sources from `rustguard-crypto/src/` and `rustguard-core/src/` into `rustguard-kmod/src/gen/`, rewrites `use rustguard_crypto::` imports to `use crate::crypto::`, and strips `#[cfg(test)]` blocks and `std`-gated code. The generated directory is `.gitignore`d and must be regenerated after any upstream crate changes.

## Testing

### Run All Tests

```bash
cargo test --workspace
```

### Run Tests for a Specific Crate

```bash
cargo test -p rustguard-core
cargo test -p rustguard-crypto
```

### Integration Tests (`rustguard-core`)

All integration tests live in `rustguard-core/tests/integration.rs` and exercise the full WireGuard protocol in memory — no network I/O, no TUN devices, no root privileges required.

| Test | What it verifies |
|---|---|
| `full_handshake_transport_roundtrip` | 100 encrypted packets each direction after a complete Noise_IK handshake |
| `replay_attack_blocked` | Same `(counter, ciphertext)` pair is rejected on the second delivery |
| `out_of_order_delivery` | 10 packets encrypted in order, decrypted in reverse; all replays rejected afterward |
| `wire_format_roundtrip` | `Initiation`, `Response`, and `Transport` messages serialise and deserialise correctly |
| `multiple_independent_handshakes` | Responder handles 5 concurrent initiators; each session is independent |
| `tampered_transport_rejected` | A single flipped bit in ciphertext causes `decrypt` to return `None` |
| `empty_transport_packet` | Empty payload (WireGuard keepalive) encrypts and decrypts to an empty slice |
| `max_size_transport_packet` | 1400-byte payload (near MTU limit) round-trips correctly |

### Run a Single Named Test

```bash
cargo test -p rustguard-core replay_attack_blocked -- --nocapture
```

### TUN Echo Smoke Test (Linux/macOS, requires root)

`rustguard-tun` ships an example that creates a utun/TUN device, responds to ICMP pings, and exits on SIGINT. This is a manual integration check for the platform TUN layer.

```bash
sudo cargo run --example tun_echo -p rustguard-tun
```

The example configures the device with address `10.0.0.1` and echoes back ICMP echo requests.

## Test Fixtures

Two `.conf` files under `tests/` configure a loopback two-peer topology for manual end-to-end testing:

```
tests/peer_a.conf   # 10.150.0.1/24, ListenPort 51821, endpoint 127.0.0.1:51822
tests/peer_b.conf   # 10.150.0.2/24, ListenPort 51822, endpoint 127.0.0.1:51821
```

These are used with `rustguard up` to bring up both sides on the same machine:

```bash
# Terminal 1
sudo cargo run --bin rustguard -- up tests/peer_a.conf

# Terminal 2
sudo cargo run --bin rustguard -- up tests/peer_b.conf

# Verify tunnel
ping 10.150.0.2
```

## Feature Flags

`rustguard-crypto` and `rustguard-core` are dual `std`/`no_std` crates controlled by a `std` feature flag that is enabled by default. To build without the standard library (e.g., for the kernel module staging path):

```bash
cargo build -p rustguard-crypto --no-default-features
cargo build -p rustguard-core --no-default-features
```

## Examples

### Complete Test Run with Output

```bash
# Run all workspace tests, showing stdout on failure
cargo test --workspace -- --nocapture

# Run only the protocol integration tests
cargo test -p rustguard-core -- --test-output immediate

# Check that no_std builds compile cleanly
cargo check -p rustguard-crypto --no-default-features
cargo check -p rustguard-core --no-default-features
```

### Kernel Module Build (End-to-End)

```bash
# Ensure kernel headers are installed (Debian/Ubuntu)
sudo apt-get install linux-headers-$(uname -r)

# Stage, build, and verify the module was produced
make -C rustguard-kmod stage
make -C rustguard-kmod modules
ls rustguard-kmod/*.ko
```

## See Also

- [Source Tree](06-Development/01-Source-Tree.md) — annotated directory structure and per-module descriptions
- [Code Conventions](06-Development/03-Code-Conventions.md) — style guide and patterns used across the workspace
- [rustguard-core](05-Modules/11-rustguard-core.md) — handshake, session, and replay window implementation
- [rustguard-crypto](05-Modules/12-rustguard-crypto.md) — cryptographic primitives tested by the integration suite
- [rustguard-kmod](05-Modules/06-rustguard-kmod.md) — kernel module architecture and source staging details
- [rustguard-tun](05-Modules/07-rustguard-tun.md) — TUN/AF_XDP platform abstraction and the `tun_echo` example