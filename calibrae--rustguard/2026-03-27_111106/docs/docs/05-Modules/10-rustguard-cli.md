# rustguard-cli

> The `rustguard` binary — a unified CLI for tunnel management, zero-config enrollment, and key utilities.

## Overview

`rustguard-cli` is the single-binary entry point for the RustGuard project, compiled as the `rustguard` executable. It dispatches to the appropriate subsystem — `rustguard-daemon` for standard WireGuard tunnel mode, `rustguard-enroll` for zero-config enrollment, or `rustguard-crypto` for key generation — based on the first positional argument.

**Source:** `rustguard-cli/src/main.rs` (267 LOC)  
**Crate type:** binary (`[[bin]]`, name `rustguard`)  
**Dependencies:** `rustguard-daemon`, `rustguard-enroll`, `rustguard-crypto`, `base64`

The CLI requires no configuration file for enrollment-based workflows. Standard WireGuard `wg0.conf` files remain fully supported via `rustguard up`.

## Commands

### `up` — Standard tunnel mode

```
rustguard up <config>
```

Reads a `wg0.conf`-style INI configuration file via `Config::from_file`, then calls `rustguard_daemon::tunnel::run`. The process blocks until the tunnel exits or an error occurs. Exits with code `1` on config parse failure or tunnel error.

**Required argument:** path to a WireGuard config file.

---

### `serve` — Enrollment server

```
rustguard serve --pool <cidr> --token <token> [options]
```

Starts an enrollment server using `rustguard_enroll::server::run`. The server listens on UDP (default port `51820`), assigns IP addresses from the given CIDR pool, and authenticates new peers using a token-derived XChaCha20 key.

Peer state (public keys and assigned IPs) is persisted to the path returned by `rustguard_enroll::state::default_state_path()` (`/var/lib/rustguard/peers.state`). The server survives restarts — persisted peers are restored without re-enrollment.

A UNIX domain socket is created at `/tmp/rustguard.sock` for runtime control via `open`, `close`, and `status` commands.

| Flag | Required | Default | Description |
|------|----------|---------|-------------|
| `--pool <cidr>` | Yes | — | IPv4 CIDR pool, e.g. `10.150.0.0/24`. Server takes `.1`. |
| `--token <token>` | Yes | — | Shared secret used to authenticate enrollment requests. |
| `--port <n>` | No | `51820` | UDP listen port. |
| `--open` | No | `false` | Open enrollment immediately on startup. |
| `--xdp <ifname>` | No | — | Enable AF_XDP fast path on the named interface. |
| `--queues <n>` | No | `1` | Number of multi-queue TUN queues. |
| `--uring` | No | `false` | Use io_uring for TUN I/O (Linux only). |

**Prerequisite:** `--xdp` requires a Linux kernel with AF_XDP support and the named interface to be available. `--uring` requires Linux kernel with io_uring support.

---

### `join` — Zero-config client enrollment

```
rustguard join <endpoint> --token <token>
```

Enrolls a new peer with a running enrollment server. Internally calls `rustguard_enroll::client::run` with a `JoinConfig`. The client:

1. Generates a fresh `StaticSecret` keypair via `rustguard_crypto`.
2. Sends an encrypted enrollment request (token-derived XChaCha20 key) to `<endpoint>`.
3. Receives the server's public key and an assigned IP address.
4. Creates a TUN device, adds a route for the pool network.
5. Performs a Noise_IK handshake and enters the tunnel loop.

The process blocks for the lifetime of the tunnel. No config file is written or required.

**Required argument:** server endpoint in `<host>:<port>` form, e.g. `1.2.3.4:51820`.

**Prerequisite:** The server's enrollment window must be open. See `open`.

---

### `open` — Open enrollment window

```
rustguard open [seconds]
```

Sends an `OPEN <seconds>` command to the running server's control socket at `/tmp/rustguard.sock` via `rustguard_enroll::control::send_command`. The enrollment window auto-closes after the specified duration.

**Optional argument:** duration in seconds (default `60`).

---

### `close` — Close enrollment window

```
rustguard close
```

Sends a `CLOSE` command to the control socket, immediately disabling new enrollments. Existing peer sessions are unaffected.

---

### `status` — Server status

```
rustguard status
```

Sends a `STATUS` command to the control socket. The server responds with either:

```
OPEN <n>s remaining, <count> peers
```
or
```
CLOSED, <count> peers
```

---

### `genkey` — Generate private key

```
rustguard genkey
```

Generates a cryptographically random X25519 private key via `rustguard_crypto::StaticSecret::random` and prints it as base64 to stdout. The output is suitable for use in `wg0.conf` as `PrivateKey`.

---

### `pubkey` — Derive public key

```
rustguard pubkey
```

Reads a base64-encoded 32-byte private key from stdin, derives the corresponding X25519 public key via `StaticSecret::from_bytes` and `StaticSecret::public_key`, and prints the result as base64 to stdout.

## Exit Codes

| Code | Meaning |
|------|---------|
| `0` | Success |
| `1` | Usage error, config parse failure, tunnel error, or control socket unreachable |

## Examples

### Generate a keypair

```bash
# Generate private key, then derive the public key
PRIVATE=$(rustguard genkey)
echo "$PRIVATE" | rustguard pubkey
```

### Bring up a standard WireGuard tunnel

```bash
rustguard up /etc/wireguard/wg0.conf
```

### Zero-config enrollment workflow

```bash
# On the server: start enrollment server, then open the window
rustguard serve --pool 10.150.0.0/24 --token mysecret &
rustguard open 120

# On any client:
rustguard join 192.168.1.10:51820 --token mysecret
```

### Performance-oriented server with AF_XDP and multi-queue TUN

```bash
rustguard serve \
  --pool 10.150.0.0/24 \
  --token mysecret \
  --xdp enp7s0 \
  --queues 4 \
  --open
```

### Check enrollment status

```bash
rustguard status
# OPEN 47s remaining, 3 peers
```

### Close enrollment window early

```bash
rustguard close
# OK enrollment closed
```

## See Also

- [rustguard-daemon](05-Modules/13-rustguard-daemon.md) — `Config::from_file` and `tunnel::run` used by `up`
- [rustguard-enroll](05-Modules/14-rustguard-enroll.md) — enrollment server, client, and control socket implementations
- [rustguard-crypto](05-Modules/12-rustguard-crypto.md) — `StaticSecret` and key derivation primitives
- [Common Workflows](03-Guides/01-Common-Workflows.md) — step-by-step guides for typical use cases
- [Configuration Schema](04-API-Reference/02-Configuration-Schema.md) — `wg0.conf` format reference for `rustguard up`
- [client](05-Modules/01-client.md) — enrollment client internals (`JoinConfig`, `run`)
- [server](05-Modules/08-server.md) — enrollment server internals (`ServeConfig`, `run`)