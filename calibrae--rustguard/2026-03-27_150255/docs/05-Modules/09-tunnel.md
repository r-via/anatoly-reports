# tunnel

> `rustguard-daemon/src/tunnel.rs` — 581 LOC. Implements the WireGuard tunnel loop: three OS threads coordinating TUN I/O, UDP I/O, and timer-driven session management.

## Overview

`tunnel` is the runtime core of `rustguard-daemon`. It owns the lifecycle of a WireGuard tunnel from startup through clean shutdown. The single public entry point, `run`, accepts a parsed `Config`, creates a TUN interface, binds a UDP socket, and launches three threads:

| Thread | Direction | Responsibility |
|--------|-----------|----------------|
| Outbound | TUN → UDP | Read IP packets, look up peer by destination IP, encrypt, send |
| Inbound | UDP → TUN | Receive datagrams, dispatch on message type, decrypt or handshake |
| Timer | — | Keepalives, handshake retries, dead-session cleanup |

All three threads share `TunnelState` behind `Arc<Mutex<TunnelState>>` and observe a single `AtomicBool` shutdown flag. On SIGINT or SIGTERM the flag is set, threads drain and exit, and all routes added at startup are removed.

## Public API

### `run`

```rust
pub fn run(config: Config) -> io::Result<()>
```

Starts the tunnel and **blocks until SIGINT, SIGTERM, or a fatal error**. Returns `Ok(())` after routes have been cleaned up and all threads have joined.

**Parameters**

| Parameter | Type | Description |
|-----------|------|-------------|
| `config` | `Config` | Parsed WireGuard configuration (see `crate::config::Config`) |

**Behavior**

1. Derives the local static keypair from `config.interface.private_key`.
2. Creates a TUN device via `Tun::create` with MTU 1420, the configured address, and a destination derived from the first IPv4 AllowedIP in the peer list (falls back to `10.0.0.2`).
3. Assigns an IPv6 address to the interface if `config.interface.address_v6` is present.
4. Binds a UDP socket on `0.0.0.0:<listen_port>` with a 500 ms read timeout so the inbound thread can poll the shutdown flag.
5. Calls `add_route` for every AllowedIP across all peers.
6. Registers a SIGINT/SIGTERM handler via `ctrlc_handler` that sets the running flag.
7. Sends `Initiation` messages to every peer that has a configured `Endpoint`.
8. Spawns the outbound, inbound, and timer threads.
9. Joins all threads, then removes routes with `delete_route`.

**Errors**

Returns an `io::Error` if TUN creation or UDP bind fails. Partial errors during route addition or IPv6 assignment are logged to `stderr` but do not abort startup.

## Internal Architecture

### `TunnelState`

Private shared state protected by a `Mutex`:

```rust
struct TunnelState {
    our_static: StaticSecret,
    peers: Vec<Peer>,
    pending_handshakes: Vec<(u32, std::time::Instant, handshake::InitiatorHandshake)>,
}
```

`pending_handshakes` maps a `sender_index` to the in-flight `InitiatorHandshake`. The list is bounded by `MAX_PENDING_HANDSHAKES = 64`; the oldest entry is evicted when the cap is reached. Entries older than `HANDSHAKE_EXPIRY = 30 s` are pruned by the timer thread.

### Outbound Thread (TUN → UDP)

Reads raw IP packets (up to 1500 bytes) from the TUN device. Parses the destination address from the IPv4 or IPv6 header and finds the matching peer via `Peer::allows_ip`. If the peer has an active session, calls `session.encrypt`, wraps the ciphertext in a `Transport` message, and sends it over UDP. If `encrypt` returns `None` (nonce exhausted), the session is dropped.

### Inbound Thread (UDP → TUN)

Reads UDP datagrams and dispatches on the 4-byte little-endian message type:

| Type constant | Value | Action |
|---------------|-------|--------|
| `MSG_INITIATION` | 1 | `handshake::process_initiation` → store session, send `Response` |
| `MSG_RESPONSE` | 2 | `handshake::process_response` → promote pending handshake to session |
| `MSG_TRANSPORT` | 4 | `session.decrypt` → write plaintext to TUN |

For `MSG_INITIATION`, the peer's endpoint is updated to the source address of the arriving datagram, enabling endpoint roaming.

### Timer Thread

Runs every second. Performs three tasks in order:

1. **Stale handshake pruning** — removes entries from `pending_handshakes` older than `HANDSHAKE_EXPIRY`.
2. **Keepalives** — for each peer with an active session, calls `session.encrypt(&[])` (zero-length payload) when `peer.timers.needs_keepalive` returns true.
3. **Handshake retries** — for peers without an active session that `should_retry_handshake` and have not `handshake_timed_out`, sends a new `Initiation` and tracks it in `pending_handshakes`.

### Route Management

Routes are added and removed using platform-specific shell commands:

| Platform | Add command | Remove command |
|----------|-------------|----------------|
| Linux | `ip [-4|-6] route add <cidr> dev <iface>` | `ip [-4|-6] route del <cidr> dev <iface>` |
| macOS | `route -n add [-net|-inet6] <cidr> -interface <iface>` | `route -n delete [-net|-inet6] <cidr> -interface <iface>` |

Route entries are recorded in a `Vec<RouteEntry>` at startup and cleaned up on graceful shutdown. If a route command fails, the error is printed to `stderr` and startup continues.

## Prerequisites

- A functional `ip` binary (Linux) or `route`/`ifconfig` (macOS) must be present on `PATH` and the process must have sufficient privileges to add and remove routes and configure TUN interfaces.
- The `getrandom` crate is used internally for CSPRNG-sourced sender indices; `/dev/urandom` or the equivalent OS facility must be available.

## Examples

### Starting a tunnel from a wg.conf file

```rust
use rustguard_daemon::config::Config;
use rustguard_daemon::tunnel;
use std::path::Path;

fn main() -> std::io::Result<()> {
    let config = Config::from_file(Path::new("/etc/wireguard/wg0.conf"))?;
    // Blocks until SIGINT / SIGTERM or fatal I/O error.
    tunnel::run(config)
}
```

### Sample wg0.conf consumed by `run`

```ini
[Interface]
PrivateKey = cGiPH7CqyNOCaW6ykZLH9K3Bt0enk5rDiTcv1O3A+JA=
ListenPort = 51820
Address = 10.0.0.1/24

[Peer]
PublicKey = HhMN8JntZEa8iF6bc+BdJD8MGD9shwefov5Gt+95Ky8=
Endpoint = 203.0.113.2:51820
AllowedIPs = 10.0.0.2/32
PersistentKeepalive = 25
```

### Invoking via the CLI

```bash
rustguard up wg0.conf
```

The `rustguard-cli` crate calls `rustguard_daemon::tunnel::run(config)` directly after parsing the config file.

## See Also

- [rustguard-daemon](05-Modules/13-rustguard-daemon.md) — crate overview and `rustguard up` command entry point
- [config](05-Modules/02-config.md) — `Config`, `InterfaceConfig`, `PeerConfig`, and `CidrAddr` types consumed by `run`
- [handshake](05-Modules/04-handshake.md) — `create_initiation`, `process_initiation`, and `process_response` called by the inbound/timer threads
- [rustguard-tun](05-Modules/07-rustguard-tun.md) — `Tun` and `TunConfig` used to create the TUN device
- [rustguard-core](05-Modules/11-rustguard-core.md) — session encryption, replay window, and timer state machine
- [client](05-Modules/01-client.md) — enrollment client tunnel loop (zero-config alternative to `wg.conf`)
- [server](05-Modules/08-server.md) — enrollment server tunnel implementation with multi-queue and AF_XDP fast paths
- [Data Flow](02-Architecture/03-Data-Flow.md) — end-to-end packet path through the system