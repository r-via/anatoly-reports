# rustguard-daemon

> Standard `wg.conf`-compatible tunnel mode for RustGuard — parses WireGuard INI configuration files and runs a full userspace tunnel.

## Overview

`rustguard-daemon` implements the `rustguard up <config>` tunnel mode. It accepts a standard WireGuard `.conf` file, builds runtime peer state, creates a TUN device, binds a UDP socket, and runs the complete Noise_IKpsk2 handshake and packet forwarding loop.

The crate is structured into three public modules:

| Module | Responsibility |
|--------|---------------|
| `config` | INI-style `wg.conf` parser — produces `Config`, `InterfaceConfig`, `PeerConfig`, and `CidrAddr` |
| `peer`   | Runtime peer state — wraps `PeerConfig` with live session and timer fields |
| `tunnel` | Tunnel entry point — `run(config)` starts the three worker threads and blocks until shutdown |

Internally the tunnel depends on `rustguard-core` for handshake and session logic, `rustguard-crypto` for key types, and `rustguard-tun` for TUN device creation.

## Prerequisites

The tunnel spawns external system commands for interface and route management at startup and shutdown. These binaries must be present:

| Platform | Commands required |
|----------|------------------|
| Linux    | `ip` (iproute2) |
| macOS    | `ifconfig`, `route` |

TUN device support must be available on the host (Linux `/dev/net/tun`, macOS utun via kernel control sockets — provided by `rustguard-tun`).

## Module: config

### Types

#### `Config`

Top-level parsed configuration. Produced by `Config::from_file` or `Config::parse`.

```rust
pub struct Config {
    pub interface: InterfaceConfig,
    pub peers: Vec<PeerConfig>,
}
```

#### `InterfaceConfig`

Parsed `[Interface]` section.

```rust
pub struct InterfaceConfig {
    pub private_key: [u8; 32],
    pub listen_port: u16,        // default: 51820
    pub address: Ipv4Addr,
    pub netmask: Ipv4Addr,       // derived from CIDR prefix
    pub address_v6: Option<(Ipv6Addr, u8)>,  // optional dual-stack address
}
```

#### `PeerConfig`

Parsed `[Peer]` section. Multiple peers are supported.

```rust
pub struct PeerConfig {
    pub public_key: [u8; 32],
    pub preshared_key: Option<[u8; 32]>,
    pub endpoint: Option<SocketAddr>,
    pub allowed_ips: Vec<CidrAddr>,
    pub persistent_keepalive: Option<u16>,  // seconds
}
```

#### `CidrAddr`

An IP address with prefix length, supporting both IPv4 and IPv6 CIDR ranges.

```rust
pub struct CidrAddr {
    pub addr: IpAddr,
    pub prefix_len: u8,
}
```

Implements `Display` as `"addr/prefix_len"`.

### Functions

#### `Config::from_file`

```rust
pub fn from_file(path: &Path) -> io::Result<Self>
```

Reads and parses a `wg.conf`-format file from disk. Returns `io::Error` with `ErrorKind::InvalidData` on parse failures.

#### `Config::parse`

```rust
pub fn parse(input: &str) -> io::Result<Self>
```

Parses a WireGuard INI string directly. Accepts `#`-prefixed comments and blank lines. Key lookup is case-insensitive. The `Address` field accepts comma-separated IPv4 and IPv6 CIDR entries.

#### `CidrAddr::contains`

```rust
pub fn contains(&self, ip: IpAddr) -> bool
```

Returns `true` if `ip` falls within the CIDR range. Dispatches to `contains_v4` or `contains_v6` based on `ip` variant. An IPv4 `CidrAddr` never matches an IPv6 address and vice versa. A prefix length of `0` matches all addresses in the same family.

#### `CidrAddr::contains_v4` / `CidrAddr::contains_v6`

```rust
pub fn contains_v4(&self, ip: Ipv4Addr) -> bool
pub fn contains_v6(&self, ip: Ipv6Addr) -> bool
```

Family-specific containment checks using bitmask arithmetic.

#### `prefix_to_netmask`

```rust
pub fn prefix_to_netmask(prefix: u8) -> Ipv4Addr
```

Converts a CIDR prefix length (0–32) to a dotted-decimal IPv4 netmask.

## Module: peer

### `Peer`

Runtime state for a single WireGuard peer. Created from `PeerConfig` via `Peer::from_config`; the `session` field starts as `None` and is populated after a successful Noise_IK handshake.

```rust
pub struct Peer {
    pub public_key: PublicKey,
    pub psk: [u8; 32],                       // zero if no PresharedKey
    pub endpoint: Option<SocketAddr>,
    pub allowed_ips: Vec<CidrAddr>,
    pub persistent_keepalive: Option<Duration>,
    pub session: Option<TransportSession>,   // set post-handshake
    pub timers: SessionTimers,
}
```

### `Peer::from_config`

```rust
pub fn from_config(config: &PeerConfig) -> Self
```

Constructs a `Peer` from a parsed `PeerConfig`. The `psk` field defaults to `[0u8; 32]` when `PeerConfig::preshared_key` is `None`. The `session` field is always `None` on construction.

### `Peer::allows_ip`

```rust
pub fn allows_ip(&self, ip: IpAddr) -> bool
```

Returns `true` if any entry in `allowed_ips` contains `ip`. Used by the outbound thread to route TUN packets to the correct peer.

### `Peer::has_active_session`

```rust
pub fn has_active_session(&self) -> bool
```

Returns `true` when `session` is `Some` **and** the session has not been marked expired by `SessionTimers::is_expired`. A peer without an active session triggers a handshake retry from the timer thread.

## Module: tunnel

### `run`

```rust
pub fn run(config: Config) -> io::Result<()>
```

The sole public entry point of `rustguard-daemon`. Performs the following sequence and then blocks until `SIGINT` or `SIGTERM`:

1. Creates a TUN device via `rustguard-tun` with MTU 1420 using `config.interface.address` and a netmask derived from the configuration.
2. Assigns an IPv6 address if `config.interface.address_v6` is present (`ip -6 addr add` on Linux, `ifconfig inet6` on macOS).
3. Binds a UDP socket on `0.0.0.0:<listen_port>` with a 500 ms read timeout.
4. Adds host routes for all `AllowedIPs` through the TUN interface.
5. Sends `Initiation` messages to all peers that have a configured `Endpoint`.
6. Spawns three threads:
   - **Outbound** (`TUN → UDP`): reads IP packets from TUN, resolves destination IP against peer `AllowedIPs`, encrypts with the active `TransportSession`, and sends over UDP.
   - **Inbound** (`UDP → TUN`): receives UDP datagrams; dispatches `MSG_INITIATION`, `MSG_RESPONSE`, and `MSG_TRANSPORT` messages; completes handshakes; decrypts transport payloads and writes plaintext to TUN.
   - **Timer** (1 s tick): prunes stale pending handshakes (older than 30 s), sends keepalives when `SessionTimers::needs_keepalive` is true, drops dead sessions, and retries handshakes for peers without active sessions.
7. On shutdown signal, joins all threads and removes the routes added in step 4.

The pending handshake table is capped at `MAX_PENDING_HANDSHAKES = 64` entries; the oldest entry is evicted when the cap is reached.

Returns `io::Error` if the TUN device or UDP socket cannot be created.

## Examples

### Parsing a `wg0.conf` file

```rust
use std::path::Path;
use rustguard_daemon::config::Config;

let config = Config::from_file(Path::new("/etc/wireguard/wg0.conf"))?;
println!("listen port: {}", config.interface.listen_port);
println!("peers: {}", config.peers.len());
for peer in &config.peers {
    println!(
        "  peer endpoint={:?} allowed_ips={}",
        peer.endpoint,
        peer.allowed_ips.iter()
            .map(|c| c.to_string())
            .collect::<Vec<_>>()
            .join(", ")
    );
}
```

### Parsing an inline config string

```rust
use rustguard_daemon::config::Config;

let raw = r#"
[Interface]
PrivateKey = cGiPH7CqyNOCaW6ykZLH9K3Bt0enk5rDiTcv1O3A+JA=
ListenPort = 51820
Address = 10.0.0.1/24, fd00::1/64

[Peer]
PublicKey = HhMN8JntZEa8iF6bc+BdJD8MGD9shwefov5Gt+95Ky8=
Endpoint = 203.0.113.42:51820
AllowedIPs = 10.0.0.2/32, fd00::2/128
PersistentKeepalive = 25
"#;

let config = Config::parse(raw)?;
assert_eq!(config.interface.listen_port, 51820);
assert!(config.interface.address_v6.is_some());
assert_eq!(config.peers[0].persistent_keepalive, Some(25));
```

### CIDR containment check

```rust
use std::net::{IpAddr, Ipv4Addr};
use rustguard_daemon::config::CidrAddr;

let subnet = CidrAddr {
    addr: IpAddr::V4(Ipv4Addr::new(10, 0, 0, 0)),
    prefix_len: 24,
};

assert!(subnet.contains(IpAddr::V4(Ipv4Addr::new(10, 0, 0, 100))));
assert!(!subnet.contains(IpAddr::V4(Ipv4Addr::new(10, 0, 1, 1))));
```

### Running the tunnel

```rust
use std::path::Path;
use rustguard_daemon::config::Config;
use rustguard_daemon::tunnel;

fn main() -> std::io::Result<()> {
    let config = Config::from_file(Path::new("wg0.conf"))?;
    tunnel::run(config) // blocks until SIGINT/SIGTERM
}
```

## See Also

- [rustguard-cli](05-Modules/10-rustguard-cli.md) — CLI front-end that invokes `tunnel::run` via the `up` subcommand
- [rustguard-core](05-Modules/11-rustguard-core.md) — Noise_IK handshake and `TransportSession` used by the tunnel threads
- [rustguard-tun](05-Modules/07-rustguard-tun.md) — TUN device abstraction consumed by `tunnel::run`
- [rustguard-enroll](05-Modules/14-rustguard-enroll.md) — Zero-config enrollment alternative to `wg.conf`-based setup
- [Configuration Schema](04-API-Reference/02-Configuration-Schema.md) — Full reference for `wg.conf` fields and defaults
- [Data Flow](02-Architecture/03-Data-Flow.md) — End-to-end packet flow through the tunnel threads