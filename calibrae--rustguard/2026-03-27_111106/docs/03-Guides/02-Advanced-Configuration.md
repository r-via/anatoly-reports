# Advanced Configuration

> Tuning, performance flags, and deployment options for RustGuard beyond the defaults.

## Overview

RustGuard exposes two operating modes — standard tunnel mode (`rustguard up`) driven by a `wg0.conf` file, and zero-config enrollment mode (`rustguard serve` / `rustguard join`). Both modes accept options that go beyond the minimum required configuration. This page documents those knobs: PSK hardening, dual-stack addressing, multi-queue TUN, AF_XDP, io_uring, enrollment window control, and state persistence.

## Standard-Mode Configuration (`wg0.conf`)

Standard mode parses the INI-style WireGuard configuration format. The parser lives in `rustguard-daemon/src/config.rs` and produces a `Config` struct with `interface: InterfaceConfig` and `peers: Vec<PeerConfig>`.

### Pre-Shared Keys

Each `[Peer]` block optionally accepts a `PresharedKey` field — a base64-encoded 32-byte symmetric key. When present it participates in the Noise_IKpsk2 handshake as an additional layer of forward secrecy on top of X25519.

```ini
[Peer]
PublicKey = HhMN8JntZEa8iF6bc+BdJD8MGD9shwefov5Gt+95Ky8=
PresharedKey = SomeBase64EncodedPSKHere32ByteExact=
AllowedIPs = 10.0.0.2/32
```

PSK must match on both sides. `PresharedKey` is stored as `Option<[u8; 32]>` in `PeerConfig.preshared_key`.

### Persistent Keepalive

`PersistentKeepalive` (seconds, `u16`) keeps sessions alive through NAT. It is useful when the local peer is behind a NAT and does not receive inbound traffic regularly.

```ini
[Peer]
PublicKey = HhMN8JntZEa8iF6bc+BdJD8MGD9shwefov5Gt+95Ky8=
Endpoint = 203.0.113.1:51820
AllowedIPs = 10.0.0.2/32
PersistentKeepalive = 25
```

A value of `25` sends a keepalive every 25 seconds. Omitting the field disables keepalives for that peer.

### AllowedIPs: Split Tunnel vs Full Tunnel

`AllowedIPs` is a comma-separated list of CIDR ranges. Packets whose destination falls within a range are routed through the tunnel to that peer.

**Split tunnel** — only specific subnets traverse the tunnel:

```ini
AllowedIPs = 10.0.0.2/32, 192.168.1.0/24
```

**Full tunnel** — all traffic, including internet, routes through the peer:

```ini
AllowedIPs = 0.0.0.0/0, ::/0
```

`CidrAddr.contains()` performs the match. A prefix of `0` matches every address of its family.

### Dual-Stack IPv6 Address

The `Address` field in `[Interface]` accepts comma-separated IPv4 and IPv6 CIDR values. Provide both to configure a dual-stack tunnel interface.

```ini
[Interface]
PrivateKey = cGiPH7CqyNOCaW6ykZLH9K3Bt0enk5rDiTcv1O3A+JA=
ListenPort = 51820
Address = 10.0.0.1/24, fd15::1/64
```

IPv4 is stored in `InterfaceConfig.address` (with a derived netmask via `prefix_to_netmask`). IPv6 is stored in `InterfaceConfig.address_v6` as `Option<(Ipv6Addr, u8)>`.

### Custom Listen Port

The default is `51820`. Override with `ListenPort`:

```ini
[Interface]
ListenPort = 51822
```

## Enrollment Server Flags (`rustguard serve`)

`rustguard serve` constructs a `ServeConfig` struct from CLI flags before calling `rustguard_enroll::server::run`.

### Reference Table

| Flag | Default | Description |
|------|---------|-------------|
| `--pool <cidr>` | *(required)* | IPv4 CIDR pool. Server takes `.1`; clients receive `.2`+. Minimum prefix: `/30`. |
| `--token <token>` | *(required)* | Shared secret. Derived into a 32-byte XChaCha20 key via `protocol::derive_token_key`. |
| `--port <port>` | `51820` | UDP listen port. |
| `--open` | enrollment closed | Open enrollment immediately on start; equivalent to running `rustguard open 3600` automatically. |
| `--queues <n>` | `1` | Number of multi-queue TUN file descriptors (Linux only). Spawns one outbound thread per queue. |
| `--uring` | disabled | Use io_uring for TUN reads (Linux only). Replaces per-queue polling threads with a single submission ring. |
| `--xdp <ifname>` | disabled | AF_XDP fast path: loads a BPF program onto `<ifname>`, creates an `XdpSocket` (queue 0), and receives WireGuard UDP frames zero-copy. Falls back to standard UDP on error. |

### Multi-Queue TUN (`--queues`)

On Linux, `MultiQueueTun::create` opens `N` TUN file descriptors with `IFF_MULTI_QUEUE`. Each queue is consumed by a dedicated outbound thread, allowing multi-core packet forwarding. `--queues` has no effect on non-Linux platforms.

```bash
# 4 TUN queues on a 4-core server
rustguard serve --pool 10.150.0.0/24 --token s3cret --queues 4
```

### io_uring (`--uring`)

When `--uring` is set, the outbound path replaces the per-thread `read()` loop with a `UringTun` submission ring (`rustguard_tun::uring`). The ring batches TUN reads via `submit_and_wait`, reducing syscall overhead on high-throughput paths. Requires Linux. `--uring` and `--queues > 1` are mutually exclusive at runtime — `--uring` takes precedence and the multi-queue outbound threads are not started.

```bash
rustguard serve --pool 10.150.0.0/24 --token s3cret --uring
```

### AF_XDP (`--xdp`)

AF_XDP bypasses the kernel networking stack for inbound UDP. The BPF program is loaded and attached to `<ifname>` via `XdpProgram::load_and_attach`; an `XdpSocket` is created on queue 0. If setup fails the server emits a warning and falls back to standard UDP automatically — the tunnel continues to function.

```bash
# Attach AF_XDP to the physical NIC (Linux only)
rustguard serve --pool 10.150.0.0/24 --token s3cret --xdp enp7s0
```

`XdpSocket` is configured with `frame_size: 4096`, `num_frames: 4096`, `ring_size: 2048`. These values are fixed in the current implementation.

## Enrollment Window Management

The server starts with enrollment closed by default. The UNIX domain control socket (managed by `rustguard_enroll::control`) accepts three commands:

```bash
# Open enrollment for N seconds (default: 60)
rustguard open 120

# Close enrollment immediately
rustguard close

# Show current window state and peer count
rustguard status
```

`rustguard open` without an argument defaults to 60 seconds. The window auto-closes when the timer expires via an atomic timestamp check. Running `rustguard open` while enrollment is already open resets the expiry. Existing peers are unaffected by window state.

Starting the server with `--open` opens a 3600-second window at startup — use `rustguard close` to lock it immediately after the initial enrollment pass.

## State Persistence

The enrollment server persists peer state to `/var/lib/rustguard/peers.state` (returned by `default_state_path()`). The file format is one peer per line:

```text
HhMN8JntZEa8iF6bc+BdJD8MGD9shwefov5Gt+95Ky8= 10.150.0.2
tNwlJsp4WIaPpeCYNsleoE8QzJFVBLHYEHLgHVQ4NyQ= 10.150.0.3
```

Each line contains the base64-encoded public key and the assigned IPv4 address, separated by a space. **Private keys are not stored** — the server generates a fresh ephemeral static key on every restart. After a restart, clients must complete a new Noise_IK handshake; their assigned IP addresses are preserved.

Writes use an atomic rename-from-temp pattern to prevent corruption on crash.

## Examples

### Annotated `wg0.conf` with All Optional Fields

```ini
[Interface]
PrivateKey = cGiPH7CqyNOCaW6ykZLH9K3Bt0enk5rDiTcv1O3A+JA=
ListenPort = 51820
Address    = 10.0.0.1/24, fd15::1/64

[Peer]
# Remote road-warrior peer with PSK, keepalive, and dual-stack routes
PublicKey           = HhMN8JntZEa8iF6bc+BdJD8MGD9shwefov5Gt+95Ky8=
PresharedKey        = AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=
Endpoint            = 203.0.113.1:51820
AllowedIPs          = 10.0.0.2/32, fd15::2/128
PersistentKeepalive = 25

[Peer]
# Passive peer (no endpoint — waits for inbound handshake)
PublicKey  = tNwlJsp4WIaPpeCYNsleoE8QzJFVBLHYEHLgHVQ4NyQ=
AllowedIPs = 10.0.0.3/32
```

### High-Performance Enrollment Server

```bash
# Multi-queue TUN + AF_XDP, enrollment open for first 5 minutes
rustguard serve \
  --pool  10.150.0.0/22 \
  --token "$(cat /etc/rustguard/token)" \
  --port  51820 \
  --queues 8 \
  --xdp  enp7s0 \
  --open
```

### io_uring Server

```bash
rustguard serve \
  --pool  10.150.0.0/24 \
  --token mysecret \
  --uring
```

### Key Generation Pipeline

```bash
# Generate a private key and derive its public key
rustguard genkey | tee /etc/rustguard/private.key | rustguard pubkey
```

## See Also

- [rustguard-cli](../05-Modules/10-rustguard-cli.md) — CLI command reference
- [rustguard-enroll](../05-Modules/14-rustguard-enroll.md) — Enrollment protocol internals
- [rustguard-daemon](../05-Modules/13-rustguard-daemon.md) — Standard tunnel daemon
- [rustguard-tun](../05-Modules/07-rustguard-tun.md) — TUN device backends (multi-queue, AF_XDP, io_uring)
- [Configuration Schema](../04-API-Reference/02-Configuration-Schema.md) — `Config`, `InterfaceConfig`, `PeerConfig` field reference
- [Common Workflows](01-Common-Workflows.md) — Step-by-step setup guides
- [Troubleshooting](03-Troubleshooting.md) — Diagnostics and common errors