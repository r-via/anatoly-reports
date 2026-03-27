# RustGuard — Overview

> A clean-room WireGuard implementation in Rust, delivering both a cross-platform userspace daemon and an out-of-tree Linux kernel module built without libwg.

## Overview

RustGuard is a full-stack WireGuard implementation written from scratch in Rust. It covers every layer of the protocol — from cryptographic primitives through the Noise_IKpsk2 handshake to packet routing — and ships in two deployment forms: a userspace daemon that runs on Linux and macOS, and an out-of-tree Linux kernel module targeting kernel 6.10+.

The project contains 8,500+ lines of Rust across 7 crates, with 80 tests and no dependency on libwg or the reference C implementation.

Two goals drove its design:

1. **Protocol fidelity** — implement WireGuard wire-compatible from the whitepaper, including the cookie/DoS-protection mechanism, PSK support, replay protection, and TAI64N timestamps.
2. **Performance exploration** — measure and close the gap between userspace TUN throughput and kernel-native packet processing, culminating in a kernel module that reaches 95.5% of the throughput of the C WireGuard module on the same hardware.

## What RustGuard Is Not

RustGuard is not a drop-in replacement for the upstream WireGuard kernel module in production environments. It was built as an engineering experiment with full production-quality security hardening (constant-time comparisons, zeroization, replay protection, MAC-first validation), but has not undergone third-party cryptographic audit.

## Crate Structure

RustGuard is organized as a Cargo workspace. Each crate has a single, focused responsibility:

| Crate | Responsibility |
|---|---|
| `rustguard-crypto` | X25519, ChaCha20-Poly1305, HMAC-BLAKE2s, HKDF, TAI64N. Dual `std`/`no_std`. |
| `rustguard-core` | Noise_IKpsk2 handshake, transport sessions, 2048-bit replay window, timer state machine. Dual `std`/`no_std`. |
| `rustguard-tun` | TUN device backends: macOS utun, Linux TUN, multi-queue TUN, AF_XDP, io_uring, BPF loader. |
| `rustguard-daemon` | Standard `wg.conf`-compatible tunnel mode (`rustguard up`). |
| `rustguard-enroll` | Zero-config enrollment: token-authenticated key exchange, CIDR IP pool, Zigbee-style pairing window, state persistence. |
| `rustguard-cli` | Entry point. Dispatches `up`, `serve`, `join`, `open`, `close`, `status`, `genkey`, `pubkey`. |
| `rustguard-kmod` | Out-of-tree Linux kernel module (Rust + C shim). Registers `wg0` as a `net_device`, handling encryption in softirq or per-CPU workqueue. |

## Operating Modes

### Standard mode

Compatible with existing `wg.conf` configuration files. `rustguard up` reads the config, configures a TUN interface, and runs the tunnel loop.

### Zero-config enrollment mode

Eliminates manual key exchange. `rustguard serve` starts an enrollment server with a shared token; `rustguard join` connects a client and receives an assigned IP address automatically. Enrollment access is controlled by a time-limited pairing window (`rustguard open`, `rustguard close`), modeled on Zigbee-style physical-presence pairing.

### Kernel module mode

The `rustguard-kmod` out-of-tree module registers a `wg0` network device directly in the kernel. Packets are encrypted and decrypted without crossing the TUN boundary, achieving throughputs of 1.53–2.13 Gbps on 2.5 GbE hardware.

## Security Properties

The implementation includes the full set of WireGuard security mechanisms:

- MAC1 verified before any Diffie-Hellman operations (prevents CPU-burn attacks from unauthenticated packets)
- Timestamp replay enforcement on handshake initiations
- 2048-bit sliding-window anti-replay bitmap with split `check()`/`update()` to prevent window poisoning
- Constant-time equality via the `subtle` crate (`ConstantTimeEq`)
- `ZeroizeOnDrop` on all handshake state (chaining key, hash, PSK)
- `O_CLOEXEC` on all file descriptors
- CSPRNG via the `getrandom` crate

## Examples

### Standard tunnel

```bash
# Bring up a WireGuard tunnel from a wg.conf-format config file
rustguard up /etc/wireguard/wg0.conf
```

### Zero-config server and client

```bash
# Start enrollment server (Linux)
rustguard serve --pool 10.150.0.0/24 --token shared-secret --port 51820

# Open enrollment for 60 seconds (physical-presence model)
rustguard open 60

# Join from a client machine
rustguard join 203.0.113.1:51820 --token shared-secret

# Inspect enrolled peers and window state
rustguard status
```

### Key generation

```bash
# Generate a private key
rustguard genkey

# Derive the corresponding public key
rustguard genkey | rustguard pubkey
```

### Performance flags (Linux only)

```bash
# Multi-queue TUN: parallel TUN I/O across 2 CPU cores (~472 Mbps on 2.5 GbE)
rustguard serve --pool 10.150.0.0/24 --token s --queues 2

# AF_XDP zero-copy UDP receive on a specific interface
rustguard serve --pool 10.150.0.0/24 --token s --xdp enp7s0

# io_uring-based TUN engine
rustguard serve --pool 10.150.0.0/24 --token s --uring
```

## See Also

- [Installation](01-Getting-Started/02-Installation.md) — build prerequisites and install steps
- [Quick Start](01-Getting-Started/04-Quick-Start.md) — end-to-end tutorial
- [Configuration](01-Getting-Started/03-Configuration.md) — `wg.conf` options and enrollment config
- [System Overview](02-Architecture/01-System-Overview.md) — component diagram and responsibilities
- [Core Concepts](02-Architecture/02-Core-Concepts.md) — Noise protocol, replay window, enrollment flow
- [Package Overview](00-Monorepo/01-Package-Overview.md) — workspace crates and dependency relationships
- [rustguard-cli](05-Modules/10-rustguard-cli.md) — CLI command reference
- [rustguard-enroll](05-Modules/14-rustguard-enroll.md) — enrollment protocol internals