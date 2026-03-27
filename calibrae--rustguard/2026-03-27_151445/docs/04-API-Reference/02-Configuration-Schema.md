# Configuration Schema

> Complete reference for all RustGuard configuration formats: the INI-style `wg.conf` file, the `serve` command flags, the `join` command flags, and the enrollment persistence state file.

## Overview

RustGuard supports two distinct operational modes, each with its own configuration surface:

- **Standard tunnel mode** (`rustguard up`) — reads a WireGuard-compatible INI configuration file (`wg0.conf` format). Compatible with existing WireGuard tooling.
- **Zero-config enrollment mode** (`rustguard serve` / `rustguard join`) — fully CLI-driven. No config file required. Server state is persisted automatically to a binary state file.

Key encoding throughout all formats uses standard base64 (RFC 4648, with padding).

---

## `wg.conf` — Standard Tunnel Configuration

The daemon configuration file parsed by `Config::from_file` and `Config::parse` in `rustguard-daemon/src/config.rs`. The format is the standard WireGuard INI dialect understood by all WireGuard implementations.

### File Structure

```ini
[Interface]
# ... interface fields ...

[Peer]
# ... peer fields ...

[Peer]
# ... additional peers ...
```

Lines beginning with `#` and blank lines are ignored. Keys are case-insensitive. Multiple `[Peer]` sections are supported.

---

### `[Interface]` Section

| Key | Type | Required | Default | Description |
|-----|------|----------|---------|-------------|
| `PrivateKey` | base64 string (32-byte key) | **Yes** | — | Local private key. Generate with `rustguard genkey`. |
| `Address` | CIDR string or comma-separated list | **Yes** | — | TUN interface address(es). Accepts IPv4 (`10.0.0.1/24`), IPv6 (`fd00::1/64`), or both comma-separated. |
| `ListenPort` | integer (0–65535) | No | `51820` | UDP port to listen on for incoming WireGuard packets. |

**IPv4 address** sets both `InterfaceConfig.address` and derives `InterfaceConfig.netmask` from the prefix length via `prefix_to_netmask`.

**IPv6 address** (optional) is stored in `InterfaceConfig.address_v6` as `(Ipv6Addr, prefix_len)`.

Both families may appear in a single `Address` line: `Address = 10.0.0.1/24, fd00::1/64`.

---

### `[Peer]` Section

One section per remote peer. Multiple `[Peer]` sections are allowed.

| Key | Type | Required | Default | Description |
|-----|------|----------|---------|-------------|
| `PublicKey` | base64 string (32-byte key) | **Yes** | — | Peer's static public key. |
| `AllowedIPs` | comma-separated CIDR list | No | *(empty)* | IP ranges routed to this peer. Supports both v4 and v6. `0.0.0.0/0` routes all traffic. |
| `Endpoint` | `host:port` | No | *(none)* | Peer's UDP endpoint. Required for the initiating side. |
| `PresharedKey` | base64 string (32-byte key) | No | *(none)* | Optional pre-shared symmetric key for post-quantum resistance. |
| `PersistentKeepalive` | integer (seconds) | No | *(disabled)* | Send keepalive packets every N seconds. Recommended: `25` for NAT traversal. |

`AllowedIPs` entries are parsed into `CidrAddr` values. IPv4 entries default to `/32` prefix and IPv6 entries default to `/128` prefix when no prefix is provided.

---

### Rust Types

```
Config
├── interface: InterfaceConfig
│   ├── private_key: [u8; 32]
│   ├── listen_port: u16            (default 51820)
│   ├── address: Ipv4Addr
│   ├── netmask: Ipv4Addr           (derived from prefix)
│   └── address_v6: Option<(Ipv6Addr, u8)>
└── peers: Vec<PeerConfig>
    ├── public_key: [u8; 32]
    ├── preshared_key: Option<[u8; 32]>
    ├── endpoint: Option<SocketAddr>
    ├── allowed_ips: Vec<CidrAddr>
    └── persistent_keepalive: Option<u16>
```

`CidrAddr` exposes `contains(IpAddr) -> bool`, `contains_v4(Ipv4Addr) -> bool`, and `contains_v6(Ipv6Addr) -> bool` for AllowedIPs routing lookups.

---

## `rustguard serve` — Enrollment Server Flags

Configured entirely via CLI flags. Parsed into `ServeConfig` in `rustguard-enroll/src/server.rs`.

| Flag | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `--pool <CIDR>` | IPv4 CIDR | **Yes** | — | IP pool for peer assignment. Server takes `.1`; clients receive sequential addresses. Example: `10.150.0.0/24`. |
| `--token <string>` | string | **Yes** | — | Shared secret used to derive the enrollment encryption key. Must be known by joining clients. |
| `--port <N>` | integer | No | `51820` | UDP listen port. |
| `--open` | flag | No | *(closed)* | Open enrollment immediately on startup. Without this flag, enrollment starts closed and must be opened with `rustguard open`. |
| `--xdp <ifname>` | string | No | *(disabled)* | Network interface name for AF_XDP zero-copy fast path (Linux only). Falls back to standard UDP on failure. |
| `--queues <N>` | integer | No | `1` | Number of multi-queue TUN queues (Linux only). Values `0` or `1` create a single queue. |
| `--uring` | flag | No | *(disabled)* | Use `io_uring` for batched TUN I/O (Linux only). |

`ServeConfig` struct fields map directly to these flags:

```
ServeConfig {
    listen_port: u16,
    pool_network: Ipv4Addr,
    pool_prefix: u8,
    token: String,
    open_immediately: bool,
    state_path: Option<PathBuf>,   // always set to default_state_path()
    xdp_ifname: Option<String>,
    tun_queues: usize,
    use_uring: bool,
}
```

`state_path` is always populated with `rustguard_enroll::state::default_state_path()` — the CLI does not expose it as a flag.

---

## `rustguard join` — Enrollment Client Flags

Configured entirely via CLI flags. Parsed into `JoinConfig` in `rustguard-enroll/src/client.rs`.

| Flag | Type | Required | Description |
|------|------|----------|-------------|
| `<endpoint>` | `host:port` | **Yes** | Server address and port (e.g. `1.2.3.4:51820`). |
| `--token <string>` | string | **Yes** | Shared secret matching the server's `--token`. |

```
JoinConfig {
    server_endpoint: SocketAddr,
    token: String,
}
```

The client generates a fresh ephemeral keypair on each invocation. No keys or state are written to disk by the client.

---

## Enrollment State File — `peers.state`

Persists enrolled peer state across server restarts. Managed automatically by `rustguard-enroll/src/state.rs`.

**Default path:** `/var/lib/rustguard/peers.state`  
**Format:** plain text, one peer per line: `<base64_pubkey> <ipv4_addr>`

```
HhMN8JntZEa8iF6bc+BdJD8MGD9shwefov5Gt+95Ky8= 10.150.0.2
tNwlJsp4WIaPpeCYNsleoE8QzJFVBLHYEHLgHVQ4NyQ= 10.150.0.3
```

Writes are atomic — the file is written to a `.tmp` sibling and renamed into place. Private keys are **not** stored; the server generates a new keypair on each restart and clients re-handshake automatically.

`PersistedPeer` struct:

```
PersistedPeer {
    public_key: [u8; 32],
    assigned_ip: Ipv4Addr,
}
```

The parent directory (`/var/lib/rustguard/`) is created automatically on first save. A missing state file returns an empty peer list without error.

---

## Examples

### Minimal point-to-point `wg0.conf`

```ini
[Interface]
PrivateKey = cGiPH7CqyNOCaW6ykZLH9K3Bt0enk5rDiTcv1O3A+JA=
ListenPort = 51820
Address = 10.0.0.1/24

[Peer]
PublicKey = HhMN8JntZEa8iF6bc+BdJD8MGD9shwefov5Gt+95Ky8=
AllowedIPs = 10.0.0.2/32
Endpoint = 203.0.113.1:51820
PersistentKeepalive = 25
```

### Dual-stack `wg0.conf` with PSK

```ini
[Interface]
PrivateKey = cGiPH7CqyNOCaW6ykZLH9K3Bt0enk5rDiTcv1O3A+JA=
ListenPort = 51820
Address = 10.0.0.1/24, fd00::1/64

[Peer]
PublicKey = HhMN8JntZEa8iF6bc+BdJD8MGD9shwefov5Gt+95Ky8=
PresharedKey = aBcDeFgHiJkLmNoPqRsTuVwXyZ0123456789AAAAAAA=
AllowedIPs = 10.0.0.2/32, fd00::2/128
Endpoint = 203.0.113.1:51820
```

### Parsing a config file in Rust

```rust
use std::path::Path;
use rustguard_daemon::config::Config;

fn main() -> std::io::Result<()> {
    let config = Config::from_file(Path::new("/etc/rustguard/wg0.conf"))?;

    println!("listen port: {}", config.interface.listen_port);
    println!("address: {}/{:?}", config.interface.address, config.interface.netmask);

    for peer in &config.peers {
        let ep = peer.endpoint.map(|e| e.to_string()).unwrap_or_default();
        println!("peer endpoint: {ep}");
        for cidr in &peer.allowed_ips {
            println!("  allowed: {cidr}");
        }
    }

    Ok(())
}
```

### Zero-config enrollment

```bash
# Server — open pool, start enrollment
rustguard serve --pool 10.150.0.0/24 --token mysecret --open

# In another terminal on a different machine
rustguard join 192.168.1.10:51820 --token mysecret

# Manage the enrollment window at runtime
rustguard open 60    # open for 60 seconds
rustguard close      # close immediately
rustguard status     # print window state and peer count
```

### Zero-config with performance flags (Linux)

```bash
# Multi-queue TUN + AF_XDP fast path + io_uring
rustguard serve \
  --pool 10.150.0.0/24 \
  --token mysecret \
  --queues 4 \
  --xdp enp7s0 \
  --uring
```

---

## See Also

- [Public API](01-Public-API.md) — `Config::from_file`, `Config::parse`, and `CidrAddr` method signatures
- [Types and Interfaces](03-Types-and-Interfaces.md) — `Config`, `InterfaceConfig`, `PeerConfig`, `CidrAddr`, `ServeConfig`, `JoinConfig`, `PersistedPeer` type definitions
- [Common Workflows](../03-Guides/01-Common-Workflows.md) — End-to-end setup walkthroughs for both tunnel modes
- [rustguard-cli](../05-Modules/10-rustguard-cli.md) — CLI command reference
- [rustguard-daemon](../05-Modules/13-rustguard-daemon.md) — Daemon crate and tunnel runner
- [rustguard-enroll](../05-Modules/14-rustguard-enroll.md) — Enrollment protocol and server/client internals