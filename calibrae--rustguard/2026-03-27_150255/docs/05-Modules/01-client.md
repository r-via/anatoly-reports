# client

> Enrollment client for `rustguard-enroll` — joins a RustGuard server with zero manual configuration.

## Overview

`client.rs` is the 226-line enrollment client inside the `rustguard-enroll` crate. It implements the `rustguard join` flow: generating a fresh keypair, exchanging it with a server over a token-encrypted enrollment protocol, receiving an assigned IP address, bringing up a TUN interface, and then completing a full WireGuard Noise_IK handshake before entering steady-state bidirectional packet forwarding.

The module is the client-side counterpart to the enrollment server documented in [`05-Modules/08-server.md`](08-server.md). After enrollment completes the tunnel is functionally identical to one started by `rustguard up` — the same `TransportSession` encrypt/decrypt path is used.

## Prerequisites

| Dependency | Purpose |
|---|---|
| `ip route` (Linux) or `route` (macOS) | Adding the pool-network route after TUN creation |
| Linux TUN driver / macOS `utun` | Kernel TUN device used by `rustguard-tun` |

## Public API

### `JoinConfig`

```rust
pub struct JoinConfig {
    pub server_endpoint: SocketAddr,
    pub token: String,
}
```

| Field | Type | Description |
|---|---|---|
| `server_endpoint` | `SocketAddr` | UDP address of the enrollment server (e.g. `1.2.3.4:51820`) |
| `token` | `String` | Shared secret from which the XChaCha20 enrollment key is derived via `protocol::derive_token_key` |

### `run`

```rust
pub fn run(config: JoinConfig) -> io::Result<()>
```

Executes the full enrollment and tunnel lifecycle. The function is blocking and does not return until both the outbound and inbound forwarding threads terminate (i.e. until the process is killed or an unrecoverable error occurs).

**Sequence of operations:**

1. **Key generation** — A new `StaticSecret` (X25519) is generated with `StaticSecret::random()`. The corresponding `PublicKey` is derived immediately.
2. **Token key derivation** — `protocol::derive_token_key` converts `config.token` into an XChaCha20 symmetric key used to protect the enrollment exchange.
3. **UDP socket bind** — An ephemeral UDP socket is bound to `0.0.0.0:0`. A 5-second read timeout is applied for the enrollment phase.
4. **Enrollment request** — `protocol::build_request` serialises and encrypts the client public key under the token key. The packet is sent to `config.server_endpoint`.
5. **Enrollment response** — The server replies with its public key, the assigned IPv4 address, and the prefix length. `protocol::parse_response` decrypts and validates the response; an `InvalidData` error is returned on failure (wrong token, corrupt packet).
6. **TUN creation** — `Tun::create` brings up a TUN interface with the assigned address. The server is assumed to be `.1` within the pool subnet. MTU is fixed at `1420`.
7. **Route installation** — The pool CIDR is installed via the OS `route` / `ip route` command pointing at the new TUN interface.
8. **WireGuard handshake** — `handshake::create_initiation` builds an Initiation message. After the server's Response is received, `handshake::process_response` derives a `TransportSession`.
9. **Packet forwarding** — Two threads are spawned:
   - **Outbound** (`TUN → UDP`): reads plaintext packets from TUN, calls `session.encrypt`, sends `Transport` messages to the server endpoint.
   - **Inbound** (`UDP → TUN`): receives `Transport` messages, calls `session.decrypt`, writes plaintext to TUN.

**Error conditions:**

| Condition | Error kind |
|---|---|
| Enrollment request times out (5 s) | Propagated `io::Error` with context message |
| Response fails decryption / parse | `io::ErrorKind::InvalidData` |
| WireGuard handshake fails | `io::ErrorKind::Other` |
| TUN creation fails | Propagated `io::Error` from `rustguard-tun` |

## Examples

### Joining a server programmatically

```rust
use std::net::SocketAddr;
use rustguard_enroll::client::{run, JoinConfig};

fn main() -> std::io::Result<()> {
    let config = JoinConfig {
        server_endpoint: "203.0.113.1:51820".parse::<SocketAddr>().unwrap(),
        token: "mysecret".to_string(),
    };

    // Blocks until the tunnel exits.
    run(config)
}
```

### Equivalent CLI invocation

```bash
# The rustguard-cli crate calls client::run internally for this command.
rustguard join 203.0.113.1:51820 --token mysecret
```

### Example console output

```text
rustguard join
enrolling with 203.0.113.1:51820...
enrolled!
  server pubkey: abc123...==
  assigned IP: 10.150.0.2/24

interface: wg0
route add 10.150.0.0/24 -> wg0
sent handshake initiation...
handshake complete — tunnel is up!
```

## Internal Helpers

These functions are private and not part of the public API; they are documented here for contributor reference.

| Function | Signature | Description |
|---|---|---|
| `rand_index` | `fn rand_index() -> u32` | Generates a random 32-bit sender index for handshake initiation using `getrandom`. |
| `base64_key` | `fn base64_key(key: &[u8; 32]) -> String` | Base64-encodes a 32-byte key for display using `base64::prelude::BASE64_STANDARD`. |
| `add_route` | `fn add_route(cidr: &str, ifname: &str)` | Shells out to `ip route add` (Linux) or `route -n add -net` (macOS). Errors are printed to stderr but do not abort the process. |

## Thread Model

After the handshake completes, `run` owns two `thread::spawn`-ed threads sharing three `Arc`-wrapped resources:

| Resource | Type | Shared between |
|---|---|---|
| TUN device | `Arc<Tun>` | Outbound + Inbound |
| UDP socket | `Arc<UdpSocket>` | Outbound + Inbound |
| Session | `Arc<Mutex<Option<TransportSession>>>` | Outbound + Inbound |

The `running` flag (`Arc<AtomicBool>`) is reserved for future graceful shutdown; the current implementation relies on thread termination via process exit. The UDP socket read timeout (500 ms) prevents the inbound thread from blocking indefinitely when idle.

## See Also

- [`05-Modules/08-server.md`](08-server.md) — Enrollment server; the counterpart that issues IP addresses and responds to `build_request` packets.
- [`05-Modules/04-handshake.md`](04-handshake.md) — `create_initiation` and `process_response` used in step 8 above.
- [`05-Modules/09-tunnel.md`](09-tunnel.md) — Steady-state tunnel loop; shares the `TransportSession` encrypt/decrypt path.
- [`05-Modules/07-rustguard-tun.md`](07-rustguard-tun.md) — `Tun` and `TunConfig` used for interface creation.
- [`05-Modules/14-rustguard-enroll.md`](14-rustguard-enroll.md) — Crate-level overview of `rustguard-enroll` including the `protocol` and `pool` modules.
- [`03-Guides/01-Common-Workflows.md`](../03-Guides/01-Common-Workflows.md) — End-to-end `serve` + `join` workflow guide.