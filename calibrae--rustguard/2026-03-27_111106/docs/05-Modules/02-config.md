# config

> WireGuard configuration file parser for `rustguard-daemon` — reads and validates the standard INI-style `wg0.conf` format.

## Overview

The `config` module is part of the `rustguard-daemon` crate (`rustguard-daemon/src/config.rs`, 292 LOC). It parses WireGuard's standard INI configuration format into strongly-typed Rust structures consumed by the tunnel loop. Configuration files are wire-compatible with `wg-quick` and `wg setconf` — any config that works with the reference implementation parses identically here.

All parsing errors surface as `io::Error` with `io::ErrorKind::InvalidData`. Keys are matched case-insensitively. Unknown keys are silently ignored for forward compatibility.

## Types

### `Config`

Top-level parsed configuration returned by `Config::from_file` and `Config::parse`.

```rust
pub struct Config {
    pub interface: InterfaceConfig,
    pub peers: Vec<PeerConfig>,
}
```

| Field | Type | Description |
|-------|------|-------------|
| `interface` | `InterfaceConfig` | Local interface settings |
| `peers` | `Vec<PeerConfig>` | Zero or more peer definitions |

---

### `InterfaceConfig`

Settings for the local WireGuard interface derived from the `[Interface]` section.

```rust
pub struct InterfaceConfig {
    pub private_key: [u8; 32],
    pub listen_port: u16,
    pub address: Ipv4Addr,
    pub netmask: Ipv4Addr,
    pub address_v6: Option<(Ipv6Addr, u8)>,
}
```

| Field | Type | Source key | Required | Default |
|-------|------|------------|----------|---------|
| `private_key` | `[u8; 32]` | `PrivateKey` | Yes | — |
| `listen_port` | `u16` | `ListenPort` | No | `51820` |
| `address` | `Ipv4Addr` | `Address` (first v4 CIDR) | Yes | — |
| `netmask` | `Ipv4Addr` | Derived from `Address` prefix | — | — |
| `address_v6` | `Option<(Ipv6Addr, u8)>` | `Address` (first v6 CIDR) | No | `None` |

`PrivateKey` must be a base64-encoded 32-byte X25519 private key. `Address` accepts a comma-separated list of CIDR ranges; the first IPv4 range sets `address`/`netmask` and the first IPv6 range sets `address_v6`.

---

### `PeerConfig`

Per-peer settings derived from each `[Peer]` section.

```rust
pub struct PeerConfig {
    pub public_key: [u8; 32],
    pub preshared_key: Option<[u8; 32]>,
    pub endpoint: Option<SocketAddr>,
    pub allowed_ips: Vec<CidrAddr>,
    pub persistent_keepalive: Option<u16>,
}
```

| Field | Type | Source key | Required |
|-------|------|------------|----------|
| `public_key` | `[u8; 32]` | `PublicKey` | Yes |
| `preshared_key` | `Option<[u8; 32]>` | `PresharedKey` | No |
| `endpoint` | `Option<SocketAddr>` | `Endpoint` | No |
| `allowed_ips` | `Vec<CidrAddr>` | `AllowedIPs` | No |
| `persistent_keepalive` | `Option<u16>` | `PersistentKeepalive` | No |

`AllowedIPs` accepts a comma-separated list of CIDR ranges, each parsed into a `CidrAddr`. `Endpoint` is a `host:port` string supporting both IPv4 and IPv6 addresses.

---

### `CidrAddr`

An IP address with a prefix length, representing a CIDR range. Supports both IPv4 and IPv6.

```rust
pub struct CidrAddr {
    pub addr: IpAddr,
    pub prefix_len: u8,
}
```

Implements `Display` as `addr/prefix_len` (e.g., `10.0.0.0/24`).

#### Methods

##### `contains_v4`

```rust
pub fn contains_v4(&self, ip: Ipv4Addr) -> bool
```

Returns `true` if `ip` falls within this CIDR range. Returns `false` if the range itself is IPv6. A prefix length of `0` matches all addresses.

##### `contains_v6`

```rust
pub fn contains_v6(&self, ip: Ipv6Addr) -> bool
```

Returns `true` if `ip` falls within this IPv6 CIDR range. Returns `false` if the range itself is IPv4.

##### `contains`

```rust
pub fn contains(&self, ip: IpAddr) -> bool
```

Dispatches to `contains_v4` or `contains_v6` based on the variant of `ip`. A family mismatch (e.g., an IPv4 range tested against an IPv6 address) always returns `false`.

## Functions

### `Config::from_file`

```rust
pub fn from_file(path: &Path) -> io::Result<Self>
```

Reads the file at `path` to a string and delegates to `Config::parse`. Returns `io::Error` on I/O failure or parse error.

### `Config::parse`

```rust
pub fn parse(input: &str) -> io::Result<Self>
```

Parses an in-memory WireGuard INI string. Processes lines sequentially, accumulating key-value pairs per section, then constructs `InterfaceConfig` and `Vec<PeerConfig>`. An `[Interface]` section is required; `[Peer]` sections are optional.

**Parsing rules:**
- Blank lines and lines starting with `#` are skipped.
- Section headers and key names are matched case-insensitively.
- A line that is not blank, not a comment, and not a section header must contain `=`; otherwise `InvalidData` is returned.
- Keys appearing outside any section return `InvalidData`.
- Unknown keys within a section are silently ignored.

### `prefix_to_netmask`

```rust
pub fn prefix_to_netmask(prefix: u8) -> Ipv4Addr
```

Converts a CIDR prefix length (0–32) to an IPv4 netmask. `prefix = 0` returns `0.0.0.0`; `prefix >= 32` returns `255.255.255.255`. Used internally to populate `InterfaceConfig::netmask` and re-exported for use by `rustguard-enroll`.

## Configuration File Format

```ini
[Interface]
PrivateKey = <base64-encoded 32-byte X25519 private key>
ListenPort = 51820
Address    = 10.0.0.1/24, fd00::1/64

[Peer]
PublicKey           = <base64-encoded 32-byte X25519 public key>
PresharedKey        = <base64-encoded 32-byte PSK>     # optional
Endpoint            = 203.0.113.1:51820                # optional
AllowedIPs          = 10.0.0.2/32, 192.168.1.0/24
PersistentKeepalive = 25                               # optional, seconds

[Peer]
PublicKey  = <another peer public key>
AllowedIPs = 10.0.0.3/32
```

- Multiple `[Peer]` sections are supported.
- `Address` and `AllowedIPs` accept comma-separated lists of v4 and v6 CIDR ranges.
- `Endpoint` is optional for peers that only receive connections (road warriors).
- Default prefix for a bare IP in `AllowedIPs` is `/32` (IPv4) or `/128` (IPv6).

## Examples

### Load a configuration file and inspect peers

```rust
use std::path::Path;
use rustguard_daemon::config::Config;

fn main() -> std::io::Result<()> {
    let config = Config::from_file(Path::new("/etc/wireguard/wg0.conf"))?;

    println!("Interface address : {}", config.interface.address);
    println!("Interface netmask : {}", config.interface.netmask);
    println!("Listen port       : {}", config.interface.listen_port);

    for (i, peer) in config.peers.iter().enumerate() {
        println!("Peer {i}: {:?}", peer.endpoint);
        for cidr in &peer.allowed_ips {
            println!("  AllowedIP: {cidr}");
        }
    }
    Ok(())
}
```

### Parse an in-memory config string

```rust
use rustguard_daemon::config::Config;

let raw = r#"
[Interface]
PrivateKey = cGiPH7CqyNOCaW6ykZLH9K3Bt0enk5rDiTcv1O3A+JA=
ListenPort = 51820
Address    = 10.0.0.1/24

[Peer]
PublicKey  = HhMN8JntZEa8iF6bc+BdJD8MGD9shwefov5Gt+95Ky8=
AllowedIPs = 10.0.0.2/32
Endpoint   = 203.0.113.1:51820
"#;

let config = Config::parse(raw)?;
assert_eq!(config.peers.len(), 1);
assert_eq!(config.interface.listen_port, 51820);
```

### CIDR membership check

```rust
use std::net::{IpAddr, Ipv4Addr};
use rustguard_daemon::config::CidrAddr;

let subnet = CidrAddr {
    addr: IpAddr::V4(Ipv4Addr::new(10, 0, 0, 0)),
    prefix_len: 24,
};

assert!(subnet.contains_v4(Ipv4Addr::new(10, 0, 0, 42)));
assert!(!subnet.contains_v4(Ipv4Addr::new(10, 0, 1, 1)));

// Generic IpAddr variant
assert!(subnet.contains(IpAddr::V4(Ipv4Addr::new(10, 0, 0, 1))));
```

### Convert a prefix length to a netmask

```rust
use rustguard_daemon::config::prefix_to_netmask;

assert_eq!(prefix_to_netmask(24).to_string(), "255.255.255.0");
assert_eq!(prefix_to_netmask(0).to_string(),  "0.0.0.0");
assert_eq!(prefix_to_netmask(32).to_string(), "255.255.255.255");
```

## See Also

- [Configuration Schema](../04-API-Reference/02-Configuration-Schema.md) — Complete reference for all configuration fields, CLI flags, and enrollment state file format
- [Types and Interfaces](../04-API-Reference/03-Types-and-Interfaces.md) — Public type inventory across all crates
- [tunnel](../05-Modules/09-tunnel.md) — Consumes `Config` to drive the tunnel event loop
- [rustguard-daemon](../05-Modules/13-rustguard-daemon.md) — Parent crate that exports this module
- [Common Workflows](../03-Guides/01-Common-Workflows.md) — Step-by-step guide for creating and using configuration files