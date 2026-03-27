# Configuration

> Reference for all RustGuard configuration formats, fields, and runtime controls.

## Overview

RustGuard supports two distinct configuration modes that can coexist:

- **File mode** — standard WireGuard INI-style `.conf` files, used with `rustguard up`. Compatible with any existing WireGuard configuration.
- **Enrollment mode** — zero-config peer discovery via `rustguard serve` and `rustguard join`. No config files are required on the client side.

Both modes share the same underlying cryptographic and tunnel primitives. File mode is appropriate when peer identities are known in advance; enrollment mode is designed for homelab and ad-hoc scenarios where manual key exchange is impractical.

---

## Standard Configuration File

The parser in `rustguard-daemon/src/config.rs` reads standard WireGuard INI-style files. The format is wire-compatible with `wg-quick` configurations.

### File Format

```ini
[Interface]
PrivateKey  = <base64-encoded 32-byte key>
ListenPort  = 51820
Address     = 10.0.0.1/24

[Peer]
PublicKey          = <base64-encoded 32-byte key>
AllowedIPs         = 10.0.0.2/32, 192.168.1.0/24
Endpoint           = 203.0.113.1:51820
PersistentKeepalive = 25

[Peer]
PublicKey  = <base64-encoded 32-byte key>
AllowedIPs = 10.0.0.3/32
```

- Lines beginning with `#` and blank lines are ignored.
- Key names are **case-insensitive** (`privatekey`, `PrivateKey`, and `PRIVATEKEY` are equivalent).
- Whitespace around `=` is trimmed.
- Multiple `[Peer]` sections are supported.

### `[Interface]` Fields

| Field | Type | Default | Required | Description |
|-------|------|---------|----------|-------------|
| `PrivateKey` | base64 string (32 bytes) | — | **Yes** | This node's WireGuard private key. |
| `ListenPort` | `u16` | `51820` | No | UDP port to bind for incoming connections. |
| `Address` | CIDR string | — | **Yes** | IPv4 (and optionally IPv6) tunnel address. Comma-separated for dual-stack. |

The `Address` field accepts comma-separated values combining IPv4 and IPv6 CIDR notation. Both addresses are configured on the TUN interface simultaneously:

```ini
Address = 10.0.0.1/24, fd00::1/64
```

If no prefix length is specified, `/24` is assumed for IPv4.

### `[Peer]` Fields

| Field | Type | Default | Required | Description |
|-------|------|---------|----------|-------------|
| `PublicKey` | base64 string (32 bytes) | — | **Yes** | The peer's static public key. |
| `AllowedIPs` | comma-separated CIDRs | — | No | IP ranges whose traffic is accepted from this peer and routed to it. Accepts mixed IPv4/IPv6. |
| `Endpoint` | `host:port` | — | No | Peer's UDP endpoint. Required for the initiating side; can be omitted for passive peers. |
| `PresharedKey` | base64 string (32 bytes) | — | No | Optional symmetric pre-shared key for post-quantum resistance. |
| `PersistentKeepalive` | `u16` (seconds) | — | No | Interval for sending keepalive packets. Recommended when this peer is behind NAT. |

`AllowedIPs = 0.0.0.0/0` routes all traffic through the tunnel. A prefix of `/0` matches every address.

### Loading a Config File

```rust
use rustguard_daemon::config::Config;
use std::path::Path;

let config = Config::from_file(Path::new("/etc/rustguard/wg0.conf"))?;

println!("listen port: {}", config.interface.listen_port);
println!("address:     {}", config.interface.address);

for peer in &config.peers {
    println!("peer: endpoint={:?}", peer.endpoint);
    for cidr in &peer.allowed_ips {
        println!("  allowed: {}", cidr);
    }
}
```

### Key Generation

Use the CLI to generate keys:

```bash
# Generate a private key
rustguard genkey

# Derive the corresponding public key
echo "<private-key>" | rustguard pubkey
```

---

## Enrollment Mode Configuration

Enrollment mode requires no configuration file on the client. The server is configured entirely via CLI flags.

### Server — `rustguard serve`

```bash
rustguard serve --pool 10.150.0.0/24 --token mysecret [OPTIONS]
```

The server generates a fresh key-pair on every start. Enrolled peer state (public keys and assigned IPs) is persisted to disk and reloaded on restart. The server IP within the pool is always `.1` (e.g. `10.150.0.1` for a `/24`).

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--pool <CIDR>` | IPv4 CIDR | — | **Required.** IP address pool for enrolled peers (e.g. `10.150.0.0/24`). |
| `--token <STRING>` | string | — | **Required.** Shared secret used to authenticate enrollment requests. |
| `--port <PORT>` | `u16` | `51820` | UDP listen port. |
| `--open` | flag | `false` | Open enrollment immediately on start (otherwise enrollment starts closed). |
| `--xdp <IFNAME>` | string | — | Linux only. Enable the AF_XDP zero-copy fast path on the named interface. |
| `--queues <N>` | `usize` | `1` | Linux only. Number of multi-queue TUN queues. |
| `--uring` | flag | `false` | Linux only. Use `io_uring` for batched TUN reads. |

#### Persistent State

By default, enrolled peer state is saved to `/var/lib/rustguard/peers.state`. The file is a plain-text list:

```text
HhMN8JntZEa8iF6bc+BdJD8MGD9shwefov5Gt+95Ky8= 10.150.0.2
tNwlJsp4WIaPpeCYNsleoE8QzJFVBLHYEHLgHVQ4NyQ= 10.150.0.3
```

Each line is `<base64-public-key> <assigned-ipv4>`. Private keys are never written to disk; the server generates a new key-pair on every restart and clients re-handshake automatically.

### Client — `rustguard join`

```bash
rustguard join <SERVER_ADDR:PORT> --token <STRING>
```

The join command contacts the enrollment server, authenticates with the shared token, receives an assigned IP and the server's public key, then writes the resulting tunnel configuration and brings up the interface. No manual key exchange or config file editing is required.

---

## Enrollment Window Control

The enrollment server starts with enrollment **closed** by default. The `open`, `close`, and `status` subcommands communicate with the running server over a UNIX domain socket at `/tmp/rustguard.sock`.

```bash
# Open enrollment for 60 seconds (default)
rustguard open

# Open for a custom duration
rustguard open 120

# Close enrollment immediately
rustguard close

# Show enrollment status and peer count
rustguard status
```

The control socket is world-writable (`0o666`) so non-root processes can send commands while the daemon runs as root. The enrollment window closes automatically when the deadline expires; no manual intervention is needed.

### Control Socket Protocol

The control socket at `/tmp/rustguard.sock` accepts newline-terminated commands:

| Command | Response |
|---------|----------|
| `OPEN <seconds>` | `OK enrollment open for Ns` |
| `CLOSE` | `OK enrollment closed` |
| `STATUS` | `OPEN Ns remaining, N peers` or `CLOSED, N peers` |

---

## Examples

### Minimal two-peer setup (file mode)

Generate keys for both peers, then create config files:

```bash
# On peer A
PRIV_A=$(rustguard genkey)
PUB_A=$(echo "$PRIV_A" | rustguard pubkey)

# On peer B
PRIV_B=$(rustguard genkey)
PUB_B=$(echo "$PRIV_B" | rustguard pubkey)
```

```ini
# /etc/rustguard/peer-a.conf
[Interface]
PrivateKey = <PRIV_A>
ListenPort = 51820
Address    = 10.0.0.1/24

[Peer]
PublicKey  = <PUB_B>
AllowedIPs = 10.0.0.2/32
Endpoint   = 203.0.113.2:51820
```

```bash
rustguard up /etc/rustguard/peer-a.conf
```

### Zero-config enrollment

```bash
# Start the enrollment server
rustguard serve --pool 10.150.0.0/24 --token homelab

# Open enrollment for 90 seconds
rustguard open 90

# On each client machine
rustguard join 192.168.1.1:51820 --token homelab
```

### Dual-stack interface address

```ini
[Interface]
PrivateKey = <base64key>
ListenPort = 51820
Address    = 10.0.0.1/24, fd00::1/64
```

### Pre-shared key between two peers

```ini
[Peer]
PublicKey     = <base64key>
PresharedKey  = <base64key>
AllowedIPs    = 10.0.0.2/32
Endpoint      = 203.0.113.2:51820
```

---

## See Also

- [Quick Start](01-Getting-Started/04-Quick-Start.md) — End-to-end setup tutorial.
- [Common Workflows](03-Guides/01-Common-Workflows.md) — Step-by-step guides for frequent use cases.
- [Advanced Configuration](03-Guides/02-Advanced-Configuration.md) — Performance tuning, AF_XDP, io_uring, multi-queue TUN.
- [Configuration Schema](04-API-Reference/02-Configuration-Schema.md) — Complete field schema with types and validation rules.
- [rustguard-cli](05-Modules/10-rustguard-cli.md) — CLI subcommand reference (`up`, `serve`, `join`, `open`, `close`, `status`, `genkey`, `pubkey`).
- [rustguard-daemon](05-Modules/13-rustguard-daemon.md) — `Config::from_file` and `Config::parse` API reference.
- [rustguard-enroll](05-Modules/14-rustguard-enroll.md) — Enrollment protocol and `ServeConfig` / `JoinConfig` types.