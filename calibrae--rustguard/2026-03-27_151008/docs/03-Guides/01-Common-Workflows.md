# Common Workflows

> Step-by-step guides for the most frequent RustGuard use cases.

## Overview

RustGuard supports two distinct operating modes that cover different deployment scenarios. **Standard mode** (`rustguard up`) reads a hand-crafted `wg0.conf` file and brings up a tunnel — this is the muscle-memory-compatible path for anyone familiar with WireGuard. **Zero-config enrollment mode** (`rustguard serve` / `rustguard join`) handles key exchange, IP allocation, and peer registration automatically, removing the need to write config files or coordinate public keys manually.

This page documents the five most common tasks performed with RustGuard.

---

## Workflow 1: Generate a Key Pair

Key generation is a prerequisite for writing a `wg0.conf` file. `rustguard genkey` writes a base64-encoded 32-byte X25519 private key to stdout. `rustguard pubkey` derives the corresponding public key from a private key supplied on stdin.

```bash
# Generate a private key and save it
rustguard genkey > privatekey

# Derive the matching public key
rustguard pubkey < privatekey > publickey

# Inspect both
cat privatekey
cat publickey
```

Both keys are base64-encoded 32-byte values. The private key must be kept secret; the public key is shared with peers.

---

## Workflow 2: Bring Up a Tunnel from a Config File

Standard mode parses an INI-style `wg0.conf` compatible with the WireGuard specification. `Config::from_file` is called internally by `rustguard up`, which then hands the parsed config to `rustguard_daemon::tunnel::run`.

### Minimal config file

```ini
[Interface]
PrivateKey = cGiPH7CqyNOCaW6ykZLH9K3Bt0enk5rDiTcv1O3A+JA=
ListenPort = 51820
Address = 10.0.0.1/24

[Peer]
PublicKey = HhMN8JntZEa8iF6bc+BdJD8MGD9shwefov5Gt+95Ky8=
AllowedIPs = 10.0.0.2/32
Endpoint = 203.0.113.2:51820
PersistentKeepalive = 25
```

### Bring the tunnel up

```bash
rustguard up /etc/rustguard/wg0.conf
```

The process runs in the foreground. Send `SIGINT` or `SIGTERM` to trigger a clean shutdown with route cleanup.

### Config file field reference

| Field | Section | Required | Notes |
|---|---|---|---|
| `PrivateKey` | `[Interface]` | Yes | Base64-encoded 32-byte X25519 key |
| `ListenPort` | `[Interface]` | No | Defaults to `51820` |
| `Address` | `[Interface]` | Yes | IPv4 CIDR; comma-separate to add an IPv6 address |
| `PublicKey` | `[Peer]` | Yes | Base64-encoded 32-byte X25519 key |
| `PresharedKey` | `[Peer]` | No | Optional 32-byte PSK for additional symmetric security |
| `Endpoint` | `[Peer]` | No | `host:port`; required for the initiating side |
| `AllowedIPs` | `[Peer]` | Yes | Comma-separated CIDR list; supports v4 and v6 |
| `PersistentKeepalive` | `[Peer]` | No | Keepalive interval in seconds |

Dual-stack addresses are specified as a comma-separated value in a single `Address` line:

```ini
Address = 10.0.0.1/24, fd00::1/64
```

---

## Workflow 3: Run a Zero-Config Enrollment Server

`rustguard serve` starts the enrollment server. It allocates IP addresses from a given CIDR pool (the server receives `.1`; clients are assigned sequential IPs). Enrollment starts **closed** by default — the `--open` flag opens it immediately, or use `rustguard open` after the server is running.

```bash
rustguard serve \
  --pool 10.150.0.0/24 \
  --token mysecrettoken \
  --port 51820
```

Optional flags:

| Flag | Default | Description |
|---|---|---|
| `--pool <cidr>` | — | Required. IPv4 CIDR block for the tunnel network |
| `--token <token>` | — | Required. Shared secret used to authenticate enrollment |
| `--port <port>` | `51820` | UDP listen port |
| `--open` | closed | Open the enrollment window immediately on start |
| `--xdp <ifname>` | disabled | Bind an AF_XDP socket to the named interface |
| `--queues <n>` | `1` | Number of multi-queue TUN file descriptors |
| `--uring` | disabled | Use io_uring for I/O instead of the default path |

Server state (peer keys and assigned IPs) is persisted automatically to `~/.rustguard/state.json` and survives restarts.

---

## Workflow 4: Join a Server (Zero-Config Client)

`rustguard join` performs the enrollment handshake with a running server, receives an assigned IP, and brings up the tunnel — no config files required.

```bash
rustguard join 203.0.113.1:51820 --token mysecrettoken
```

The endpoint must be a `host:port` socket address. The token must match the value passed to `rustguard serve`. On success, the tunnel is active immediately.

---

## Workflow 5: Manage the Enrollment Window

The enrollment window controls whether new peers can join an already-running server. This is a physical-presence security model: enrollment is opened for a limited window when a new device needs to join, then closed again.

`rustguard open`, `rustguard close`, and `rustguard status` communicate with the server over a UNIX domain control socket.

```bash
# Open the enrollment window for 90 seconds
rustguard open 90

# Open for the default duration (60 seconds)
rustguard open

# Close immediately (without waiting for the window to expire)
rustguard close

# Show current window state and peer count
rustguard status
```

The window auto-closes on expiry via an atomic timestamp. Existing connected peers are unaffected when the window closes.

---

## Examples

### Complete site-to-site setup using standard mode

The following example connects two hosts using hand-written config files.

**Host A** (`10.0.0.1`) — server side:

```bash
# Generate keys on host A
rustguard genkey > /etc/rustguard/private.key
rustguard pubkey < /etc/rustguard/private.key
# Output: <HOST_A_PUBLIC_KEY>
```

```ini
# /etc/rustguard/wg0.conf on host A
[Interface]
PrivateKey = <HOST_A_PRIVATE_KEY>
ListenPort = 51820
Address = 10.0.0.1/24

[Peer]
PublicKey = <HOST_B_PUBLIC_KEY>
AllowedIPs = 10.0.0.2/32
```

**Host B** (`10.0.0.2`) — client side:

```ini
# /etc/rustguard/wg0.conf on host B
[Interface]
PrivateKey = <HOST_B_PRIVATE_KEY>
ListenPort = 51820
Address = 10.0.0.2/24

[Peer]
PublicKey = <HOST_A_PUBLIC_KEY>
Endpoint = <HOST_A_IP>:51820
AllowedIPs = 10.0.0.1/32
PersistentKeepalive = 25
```

```bash
# Bring up both tunnels
rustguard up /etc/rustguard/wg0.conf   # run on each host
```

### Zero-config homelab setup

```bash
# On the gateway machine — start the server, open enrollment for 2 minutes
rustguard serve --pool 10.150.0.0/24 --token homelab2026 &
rustguard open 120

# On each client machine — join while the window is open
rustguard join 192.168.1.1:51820 --token homelab2026

# Close enrollment once all devices have joined
rustguard close

# Verify peer count
rustguard status
```

## See Also

- [Configuration Schema](../04-API-Reference/02-Configuration-Schema.md)
- [Advanced Configuration](../03-Guides/02-Advanced-Configuration.md)
- [Troubleshooting](../03-Guides/03-Troubleshooting.md)
- [rustguard-daemon module](../05-Modules/13-rustguard-daemon.md)
- [rustguard-enroll module](../05-Modules/14-rustguard-enroll.md)
- [rustguard-cli module](../05-Modules/10-rustguard-cli.md)
- [Quick Start](../01-Getting-Started/04-Quick-Start.md)