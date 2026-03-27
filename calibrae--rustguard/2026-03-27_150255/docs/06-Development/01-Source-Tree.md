# Source Tree

> Annotated reference of every file and directory in the RustGuard repository.

## Overview

RustGuard is a Cargo workspace containing six Rust crates plus one out-of-tree kernel module. All Rust crates live at the repository root as peer directories. The kernel module (`rustguard-kmod`) uses its own `Kbuild`/`Makefile` build system and is excluded from the workspace `Cargo.toml`.

The repository totals approximately 9,700 lines of Rust across 41 `.rs` files and 7 crate directories.

## Repository Root

```
rustguard/
├── Cargo.toml              # Workspace manifest — 6 Cargo members, edition 2024
├── Cargo.lock
├── README.md
├── CONTRIBUTING.md
├── LICENSE-APACHE
├── LICENSE-MIT
├── tests/                  # Cross-crate integration tests
│   └── integration.rs      # 199 LOC — end-to-end handshake and transport tests
├── rustguard-crypto/
├── rustguard-core/
├── rustguard-tun/
├── rustguard-daemon/
├── rustguard-enroll/
├── rustguard-cli/
└── rustguard-kmod/         # Out-of-tree kernel module (not a Cargo member)
```

The workspace `Cargo.toml` sets the following release profile, applied to all member crates:

```toml
[profile.release]
strip = true
lto = true
opt-level = "z"
codegen-units = 1
panic = "abort"
```

## Crate: `rustguard-crypto`

Pure cryptographic primitives. Dual `std`/`no_std` — usable inside the kernel module.

```
rustguard-crypto/
└── src/
    ├── lib.rs       # 19 LOC  — re-exports and WireGuard protocol constants
    ├── x25519.rs    # 144 LOC — X25519 key exchange (StaticSecret, PublicKey, EphemeralSecret)
    ├── aead.rs      # 158 LOC — ChaCha20-Poly1305 and XChaCha20-Poly1305 (seal/open/xseal/xopen)
    ├── blake2s.rs   # 168 LOC — BLAKE2s hash, HMAC-BLAKE2s, and HKDF key derivation
    └── tai64n.rs    #  82 LOC — TAI64N timestamp encoding
```

`lib.rs` re-exports all public symbols and exposes the Noise protocol string constants (`CONSTRUCTION`, `IDENTIFIER`, `LABEL_MAC1`, `LABEL_COOKIE`).

## Crate: `rustguard-core`

Protocol state machines. Dual `std`/`no_std`. No I/O — operates on byte slices.

```
rustguard-core/
├── src/
│   ├── lib.rs        #  12 LOC — module declarations, re-exports rustguard-crypto
│   ├── handshake.rs  # 476 LOC — Noise_IKpsk2 initiator and responder state machines
│   ├── messages.rs   # 231 LOC — wire-format structs for all four WireGuard message types
│   ├── session.rs    # 178 LOC — per-session symmetric keys and nonce counter
│   ├── replay.rs     # 205 LOC — 2048-bit sliding window anti-replay bitmap
│   ├── cookie.rs     # 367 LOC — CookieChecker (server) and CookieState (client)
│   └── timers.rs     # 232 LOC — rekey, keepalive, retry, and session-expiry timer logic
└── tests/
    └── integration.rs # 199 LOC — handshake round-trip and replay window tests
```

## Crate: `rustguard-tun`

TUN device abstraction for macOS and Linux, plus Linux-specific high-performance I/O paths.

```
rustguard-tun/
├── src/
│   ├── lib.rs        # 105 LOC — Tun struct and TunConfig; platform dispatch via #[cfg]
│   ├── macos.rs      # 296 LOC — utun device via kernel control socket (macOS)
│   ├── linux.rs      # 212 LOC — /dev/net/tun with IFF_TUN | IFF_NO_PI (Linux)
│   ├── linux_mq.rs   # 260 LOC — multi-queue TUN (parallel packet processing)
│   ├── uring.rs      # 243 LOC — io_uring submission/completion ring integration
│   ├── xdp.rs        # 458 LOC — AF_XDP zero-copy socket path
│   └── bpf_loader.rs # 470 LOC — BPF program loading and XSK map management
└── examples/
    └── tun_echo.rs   # 103 LOC — minimal echo device for manual testing
```

The `Tun::create`, `Tun::read`, and `Tun::write` methods dispatch to the correct platform implementation at compile time. `Tun::raw_fd` exposes the underlying file descriptor for io_uring and AF_XDP integration.

## Crate: `rustguard-daemon`

Standard `wg.conf`-based tunnel mode, invoked by `rustguard up`.

```
rustguard-daemon/
└── src/
    ├── lib.rs    #   3 LOC — module declarations
    ├── config.rs # 405 LOC — Config::from_file — parses wg.conf INI format
    ├── peer.rs   #  58 LOC — Peer struct — endpoint, public key, allowed IPs
    └── tunnel.rs # 580 LOC — tunnel::run — main I/O loop (TUN ↔ UDP encrypt/decrypt)
```

`tunnel::run` is the main entry point called by `rustguard up`. It owns the event loop: reading plaintext from TUN, encrypting via `rustguard-core`, sending UDP, receiving UDP, decrypting, and writing back to TUN.

## Crate: `rustguard-enroll`

Zero-config enrollment protocol — token-authenticated key exchange with IP pool allocation.

```
rustguard-enroll/
└── src/
    ├── lib.rs        #   9 LOC — module declarations
    ├── protocol.rs   # 153 LOC — enrollment wire-format messages (EnrollRequest/Response)
    ├── packet.rs     # 110 LOC — packet framing and XChaCha20-encrypted envelope
    ├── pool.rs       # 150 LOC — CIDR-based sequential IP pool allocator
    ├── state.rs      # 123 LOC — ServerState persistence to ~/.rustguard/state.json
    ├── server.rs     # 577 LOC — server::run — enrollment server + WireGuard tunnel
    ├── client.rs     # 226 LOC — client::run — enrollment client (rustguard join)
    ├── control.rs    # 171 LOC — UNIX domain socket for open/close/status commands
    └── fast_udp.rs   # 131 LOC — send_mmsg / recv_mmsg wrappers for batch UDP I/O
```

`state::default_state_path()` returns `~/.rustguard/state.json`. The server persists enrolled peer keys and assigned IPs there across restarts.

## Crate: `rustguard-cli`

Single binary entry point. Parses `argv` and delegates to the appropriate crate.

```
rustguard-cli/
└── src/
    └── main.rs  # 267 LOC — command dispatch for up/serve/join/open/close/status/genkey/pubkey
```

Subcommand routing:

| Subcommand | Delegates to |
|------------|-------------|
| `up`       | `rustguard_daemon::tunnel::run` |
| `serve`    | `rustguard_enroll::server::run` |
| `join`     | `rustguard_enroll::client::run` |
| `open`     | `rustguard_enroll::control::send_command("OPEN …")` |
| `close`    | `rustguard_enroll::control::send_command("CLOSE")` |
| `status`   | `rustguard_enroll::control::send_command("STATUS")` |
| `genkey`   | `rustguard_crypto::StaticSecret::random` |
| `pubkey`   | `rustguard_crypto::StaticSecret::from_bytes` + `.public_key()` |

## Crate: `rustguard-kmod` (out-of-tree kernel module)

Linux kernel module targeting kernel 6.10+. Built with `Kbuild`/`LLVM` — not a Cargo workspace member.

```
rustguard-kmod/
├── Kbuild     # obj-m and per-object source list (wg_net, wg_crypto, wg_socket, …)
├── Makefile   # stage + modules + clean targets; KDIR defaults to running kernel headers
└── src/
    ├── lib.rs          # 937 LOC — kernel module init/exit, netdev ops, GenL family
    ├── noise.rs        # 540 LOC — in-kernel Noise_IKpsk2 handshake and transport
    ├── allowedips.rs   # 211 LOC — radix-tree AllowedIPs route table
    ├── cookie.rs       # 205 LOC — in-kernel cookie mechanism
    ├── replay.rs       #  98 LOC — anti-replay bitmap (kernel variant)
    ├── timers.rs       # 154 LOC — kernel timer integration (rekey, keepalive, expiry)
    └── gen/            # Generated at build time by `make stage` — do not edit
        ├── crypto/     # Staged copies of rustguard-crypto sources, rewritten for no_std
        └── protocol/   # Staged copies of rustguard-core sources, rewritten for no_std
```

The `make stage` target copies source files from `rustguard-crypto` and `rustguard-core` into `src/gen/`, rewrites `use rustguard_crypto::` to `use crate::crypto::`, strips `#[cfg(test)]` blocks, and forces `#![no_std]`. The `make modules` target then invokes the kernel build system with `LLVM=1`.

## Examples

### Walking the codebase from a subcommand to its implementation

The following illustrates how `rustguard serve` flows through the source tree:

```
rustguard-cli/src/main.rs          cmd_serve()
    └─ builds ServeConfig
    └─ calls rustguard_enroll::server::run(config)

rustguard-enroll/src/server.rs     server::run()
    ├─ rustguard_enroll::pool        IP pool allocation
    ├─ rustguard_enroll::state       persistence to ~/.rustguard/state.json
    ├─ rustguard_enroll::control     UNIX socket for open/close/status
    └─ rustguard_core::handshake     Noise_IKpsk2 for enrolled peers

rustguard-core/src/handshake.rs    Handshake state machine
    └─ rustguard_crypto::*           X25519, ChaCha20-Poly1305, BLAKE2s
```

### Staging shared sources for the kernel module

```bash
# From the rustguard-kmod directory:
make stage    # copies rustguard-crypto and rustguard-core into src/gen/, rewrites imports
make modules  # invokes kernel build system: make -C /lib/modules/$(uname -r)/build M=$PWD LLVM=1 modules
make clean    # removes .ko artefacts and the entire src/gen/ tree
```

### Locating a specific protocol component

```bash
# Find all files implementing the replay window
grep -rl "ReplayWindow\|replay" rustguard-core/src/ rustguard-kmod/src/

# Count lines across the whole workspace
find . -name "*.rs" | xargs wc -l | sort -rn | head -20
```

## See Also

- [Package Overview](../00-Monorepo/01-Package-Overview.md) — one-line descriptions and inter-package dependency table
- [Dependency Graph](../00-Monorepo/02-Dependency-Graph.md) — build order and crate dependency edges
- [Build and Test](02-Build-and-Test.md) — how to compile, run tests, and lint
- [Code Conventions](03-Code-Conventions.md) — style rules and patterns used across all crates
- [System Overview](../02-Architecture/01-System-Overview.md) — high-level component diagram