# Installation

> Prerequisites, build steps, and first-run verification for RustGuard.

## Overview

RustGuard is a Cargo workspace composed of seven crates. The primary deliverable is the `rustguard` binary produced by `rustguard-cli`. An optional out-of-tree Linux kernel module lives in `rustguard-kmod/` and requires a separate build step.

Both the userspace binary and the kernel module are built from the same source tree. The userspace path works on Linux and macOS. The kernel module targets Linux 6.10+.

## Prerequisites

### Rust toolchain

RustGuard requires the **2024 edition** of Rust. Install or update the toolchain with [rustup](https://rustup.rs/):

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup update stable
```

Verify the installed version:

```bash
rustc --version
cargo --version
```

### Linux-specific userspace prerequisites

The `rustguard-tun` crate uses the `io_uring` interface on Linux (via the `io-uring` crate). A kernel with `io_uring` support is required — any mainline kernel ≥ 5.1 satisfies this. The `libc` crate is also required and is resolved automatically by Cargo.

No other system libraries or C headers are needed for the userspace build.

### macOS prerequisites

No additional system dependencies are required. The TUN device is created through the `utun` kernel control socket, which is available on all supported macOS versions.

### Cross-compilation (Linux target from macOS)

To cross-compile for `x86_64-unknown-linux-musl`:

```bash
rustup target add x86_64-unknown-linux-musl
```

A musl-targeting linker must also be present. On macOS this is typically provided by a cross toolchain such as `musl-cross` (via Homebrew) or a Docker-based environment.

### Kernel module prerequisites

Building `rustguard-kmod` requires:

- Linux kernel **6.10+** headers installed (`/lib/modules/$(uname -r)/build` must exist)
- Clang/LLVM toolchain (`LLVM=1` is passed to `Kbuild`)
- Standard kernel module build utilities (`make`, `kmod`)

```bash
# Debian / Ubuntu
sudo apt-get install linux-headers-$(uname -r) clang llvm
```

## Building

### Userspace binary (standard)

Build an optimised release binary from the workspace root:

```bash
cargo build --release
```

The binary is written to `target/release/rustguard`. The release profile strips symbols, enables LTO, sets `opt-level = "z"`, and uses a single codegen unit with `panic = abort`.

### Cross-compile for Linux musl

```bash
cargo build --release --target x86_64-unknown-linux-musl
```

The binary is written to `target/x86_64-unknown-linux-musl/release/rustguard`.

### Kernel module

The kernel module build is managed by a standalone `Makefile` inside `rustguard-kmod/`. It first stages shared Rust sources from `rustguard-crypto` and `rustguard-core` into `src/gen/`, rewrites crate-level imports to local module paths, strips `std`-gated code, then hands control to `Kbuild`:

```bash
cd rustguard-kmod
make modules
```

To point the build at a specific kernel tree:

```bash
make modules KDIR=/path/to/kernel/build
```

The resulting `rustguard.ko` module object is produced in the `rustguard-kmod/` directory. To clean staged sources and build artifacts:

```bash
make clean
```

## Installing the Binary

Copy the release binary to a location on `PATH`:

```bash
sudo install -m 755 target/release/rustguard /usr/local/bin/rustguard
```

Verify the installation:

```bash
rustguard genkey | rustguard pubkey
```

A base64-encoded public key is printed to stdout if the binary is working correctly.

## Examples

### Generate a key pair

```bash
# Generate a private key and derive its public key in one pipeline
rustguard genkey | tee private.key | rustguard pubkey
```

### Bring up a tunnel using a standard wg.conf-style config

```bash
sudo rustguard up /etc/wireguard/wg0.conf
```

### Zero-config enrollment — server side

```bash
sudo rustguard serve --pool 10.150.0.0/24 --token mysecret
```

### Zero-config enrollment — client side

```bash
sudo rustguard join 192.0.2.1:51820 --token mysecret
```

### Load the kernel module (after building)

```bash
sudo insmod rustguard-kmod/rustguard.ko
```

## See Also

- [Overview](01-Getting-Started/01-Overview.md) — What RustGuard is and why it exists
- [Quick Start](01-Getting-Started/04-Quick-Start.md) — End-to-end tutorial from install to first ping
- [Configuration](01-Getting-Started/03-Configuration.md) — Config file format, flags, and options
- [rustguard-cli](05-Modules/10-rustguard-cli.md) — CLI command reference
- [rustguard-tun](05-Modules/07-rustguard-tun.md) — TUN device abstraction details
- [rustguard-kmod](05-Modules/06-rustguard-kmod.md) — Kernel module reference