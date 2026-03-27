# Quick Start

> An end-to-end walkthrough that has a tunnel running in under five minutes, covering both standard config-file mode and zero-config enrollment mode.

## Overview

RustGuard exposes a single `rustguard` binary with eight subcommands. Two distinct deployment paths exist:

- **Standard mode** — bring up a tunnel from an INI-style `wg.conf` file compatible with the upstream WireGuard tooling.
- **Enrollment mode** — a server allocates addresses automatically; clients join with a shared token and no manual key exchange.

Both paths share the same underlying Noise_IK handshake and transport layer.

## Prerequisites

- Rust toolchain (edition 2024, `cargo` on `$PATH`)
- Linux or macOS (TUN device support required)
- Root or `CAP_NET_ADMIN` to create TUN interfaces and configure routes

## Build

```bash
cargo build --release -p rustguard-cli
# Binary: ./target/release/rustguard
```

For a development build without optimisations:

```bash
cargo build -p rustguard-cli
# Binary: ./target/debug/rustguard
```

## Path A — Standard Mode (`rustguard up`)

Standard mode reads a `wg.conf`-style file and brings up a tunnel. Keys are base64-encoded 32-byte values.

### Step 1 — Generate keys

```bash
# Server
./rustguard genkey > server.key
./rustguard pubkey < server.key > server.pub

# Client
./rustguard genkey > client.key
./rustguard pubkey < client.key > client.pub
```

### Step 2 — Write config files

**`server.conf`** — listens on UDP 51820, allocates `10.0.0.1/24`:

```ini
[Interface]
PrivateKey = <contents of server.key>
ListenPort = 51820
Address    = 10.0.0.1/24

[Peer]
PublicKey  = <contents of client.pub>
AllowedIPs = 10.0.0.2/32
```

**`client.conf`** — connects to the server, routes VPN subnet through it:

```ini
[Interface]
PrivateKey = <contents of client.key>
Address    = 10.0.0.2/24

[Peer]
PublicKey           = <contents of server.pub>
Endpoint            = 203.0.113.1:51820
AllowedIPs          = 10.0.0.0/24
PersistentKeepalive = 25
```

Dual-stack is supported: `Address` accepts a comma-separated list of IPv4 and IPv6 CIDRs (e.g. `10.0.0.1/24, fd15::1/64`). `AllowedIPs` similarly accepts mixed families.

### Step 3 — Bring up both sides

```bash
# On the server
sudo ./rustguard up server.conf

# On the client
sudo ./rustguard up client.conf
```

Verify connectivity:

```bash
ping 10.0.0.1
```

## Path B — Zero-Config Enrollment Mode

Enrollment mode removes manual key exchange entirely. The server owns an IP pool and assigns addresses to clients that present the correct token.

### Step 1 — Start the server

```bash
# Enrollment window starts closed. --open skips the first `rustguard open` call.
sudo ./rustguard serve --pool 10.150.0.0/24 --token mysecret --port 51820
```

The server assigns itself `10.150.0.1` and allocates subsequent addresses to joining clients. State is persisted to `/var/lib/rustguard/peers.state` and survives restarts.

Optional performance flags accepted by `serve`:

| Flag | Default | Description |
|---|---|---|
| `--port <n>` | `51820` | UDP listen port |
| `--open` | off | Open enrollment window immediately at startup |
| `--queues <n>` | `1` | Multi-queue TUN queue count |
| `--xdp <ifname>` | off | Attach AF_XDP fast path to named interface |
| `--uring` | off | Use `io_uring` for I/O |

### Step 2 — Open the enrollment window

By default the enrollment window is closed. Open it on the server before clients join:

```bash
# Open for 60 seconds (default)
./rustguard open

# Open for a custom duration
./rustguard open 120

# Close immediately
./rustguard close

# Inspect window state and connected peer count
./rustguard status
```

`open` and `close` communicate with the running daemon over a UNIX domain control socket. The window closes automatically when the timer expires.

### Step 3 — Join from a client

```bash
# Runs on any machine that can reach the server
sudo ./rustguard join 203.0.113.1:51820 --token mysecret
```

On success the client is assigned an address from the pool (e.g. `10.150.0.2`) and the tunnel comes up immediately. No config files are created or required.

## Examples

### Full enrollment session (two terminals)

```bash
# Terminal 1 — server
sudo ./rustguard serve --pool 10.150.0.0/24 --token homelab --open

# Terminal 2 — open window, enroll client, verify
./rustguard status
sudo ./rustguard join 127.0.0.1:51820 --token homelab
ping 10.150.0.1
```

### Generate and inspect a key pair

```bash
./rustguard genkey | tee private.key | ./rustguard pubkey
# Prints the public key to stdout; private.key holds the secret.
```

### Minimal wg.conf for a site-to-site link

```ini
[Interface]
PrivateKey = cGiPH7CqyNOCaW6ykZLH9K3Bt0enk5rDiTcv1O3A+JA=
ListenPort = 51820
Address    = 10.0.0.1/24

[Peer]
PublicKey  = HhMN8JntZEa8iF6bc+BdJD8MGD9shwefov5Gt+95Ky8=
AllowedIPs = 10.0.0.2/32, 192.168.1.0/24
Endpoint   = 203.0.113.1:51820
PersistentKeepalive = 25
```

## See Also

- [CLI Module Reference](../05-Modules/10-rustguard-cli.md)
- [Enrollment Module Reference](../05-Modules/14-rustguard-enroll.md)
- [Daemon Module Reference](../05-Modules/13-rustguard-daemon.md)
- [Configuration Schema](../04-API-Reference/02-Configuration-Schema.md)
- [Common Workflows](../03-Guides/01-Common-Workflows.md)
- [System Overview](../02-Architecture/01-System-Overview.md)