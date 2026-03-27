# server

> Enrollment server for `rustguard-enroll` — accepts new peers, assigns IPs, and runs the full WireGuard tunnel loop.

## Overview

`server.rs` is the 577-line core of the `rustguard-enroll` crate. It implements a zero-config VPN enrollment server that combines two distinct responsibilities:

1. **Peer enrollment** — new clients authenticate with a shared token, receive an assigned IPv4 address, and have their public key persisted to disk.
2. **Packet forwarding** — after enrollment, the server drives a full WireGuard Noise_IK tunnel: TUN reads, encryption, UDP sends, UDP receives, decryption, and TUN writes.

The packet path is optimized for throughput: concurrent peer reads via `RwLock`, per-peer `Mutex` to avoid head-of-line blocking, zero-heap-allocation crypto using stack buffers, and optional Linux fast paths (multi-queue TUN, `io_uring`, AF_XDP).

## Public API

### `ServeConfig`

```rust
pub struct ServeConfig {
    pub listen_port: u16,
    pub pool_network: Ipv4Addr,
    pub pool_prefix: u8,
    pub token: String,
    pub open_immediately: bool,
    pub state_path: Option<std::path::PathBuf>,
    /// Interface name for AF_XDP fast path. None = standard UDP socket.
    pub xdp_ifname: Option<String>,
    /// Number of TUN queues (multi-queue TUN). 0 or 1 = single queue.
    pub tun_queues: usize,
    /// Use io_uring for TUN I/O.
    pub use_uring: bool,
}
```

| Field | Type | Description |
|---|---|---|
| `listen_port` | `u16` | UDP port the server binds to (typically `51820`). |
| `pool_network` | `Ipv4Addr` | Network address of the IP pool (e.g. `10.150.0.0`). |
| `pool_prefix` | `u8` | CIDR prefix length (e.g. `24`). Must be ≤ 30. |
| `token` | `String` | Shared enrollment token; hashed via BLAKE2s before use. |
| `open_immediately` | `bool` | If `true`, enrollment window opens for 3600 s on startup. |
| `state_path` | `Option<PathBuf>` | Path for persisting enrolled peers; `None` disables persistence. |
| `xdp_ifname` | `Option<String>` | Linux NIC name for AF_XDP zero-copy path (e.g. `"enp7s0"`). |
| `tun_queues` | `usize` | Number of multi-queue TUN queues. `0` or `1` selects single-queue. |
| `use_uring` | `bool` | Use `io_uring` for batched TUN reads on Linux. |

### `run`

```rust
pub fn run(config: ServeConfig) -> io::Result<()>
```

Starts the enrollment server and blocks until all worker threads exit. The function:

1. Generates an ephemeral server key pair (`StaticSecret::random()`).
2. Validates and initialises the IP pool (`IpPool::new`); server always receives the `.1` address.
3. Derives a 32-byte token key via `protocol::derive_token_key`.
4. Creates TUN interface(s) via `rustguard_tun`.
5. Restores previously enrolled peers from `state_path` (if provided).
6. Starts a UNIX domain control socket at `/tmp/rustguard.sock` for enrollment window commands.
7. Spawns outbound worker thread(s) (TUN → encrypt → UDP).
8. Spawns a single inbound thread (UDP/XDP → decrypt → TUN, plus enrollment and handshake handling).
9. Joins all threads and cleans up the socket file on return.

Returns `io::Error` on any initialisation failure (invalid CIDR, TUN creation failure, socket bind failure).

## Internal Structures

These types are private but shape the runtime data model:

### `EnrolledPeer`

```rust
struct EnrolledPeer {
    public_key: PublicKey,
    assigned_ip: Ipv4Addr,
    state: Mutex<PeerState>,
}
```

### `PeerState`

```rust
struct PeerState {
    endpoint: Option<SocketAddr>,
    session: Option<rustguard_core::session::TransportSession>,
    timers: rustguard_core::timers::SessionTimers,
}
```

The `endpoint` field is updated on every inbound packet, enabling NAT traversal without explicit keepalive coordination.

### `ServerState`

```rust
struct ServerState {
    our_static: StaticSecret,
    our_public_bytes: [u8; 32],
    token_key: [u8; 32],
    pool: Mutex<IpPool>,
    peers: RwLock<Vec<Arc<EnrolledPeer>>>,
    pending_handshakes: Mutex<Vec<(u32, std::time::Instant, handshake::InitiatorHandshake)>>,
    state_path: Option<std::path::PathBuf>,
}
```

`peers` uses a `RwLock`: enrollment takes an exclusive write lock; packet forwarding acquires only a shared read lock.

## Packet Processing

### Outbound (TUN → UDP)

Each outbound worker thread:

1. Reads one plaintext packet from TUN (multi-queue queue `queue_id`, or single-queue fallback).
2. Extracts the destination IPv4 from bytes 16–19 (IP header).
3. Finds the matching `EnrolledPeer` by `assigned_ip` under a read lock.
4. Encrypts in-place into a stack buffer using `session.encrypt_to`.
5. Prepends the WireGuard transport header (`MSG_TRANSPORT`, receiver index, counter).
6. Sends to the peer's last-known UDP endpoint.

On Linux with `use_uring = true`, a single `UringTun` thread replaces per-queue threads and submits batched read completions.

### Inbound (UDP → TUN)

The single inbound thread collects packets from AF_XDP (if active) and standard UDP via `fast_udp::RecvBatch`, then dispatches by message type:

| Discriminant | Handling |
|---|---|
| `[0x52, 0x47, 0x45, 0x01]` (76 bytes) | Enrollment request: validate window, decrypt with token key, allocate IP, persist, send response. |
| `MSG_INITIATION` | WireGuard Noise_IK initiation: `handshake::process_initiation`, store `TransportSession` on peer. |
| `MSG_TRANSPORT` | Transport data: look up by receiver index, `session.decrypt_in_place`, write to TUN. |

### Enrollment Protocol

The enrollment wire format is defined in [rustguard-enroll](14-rustguard-enroll.md). Messages use XChaCha20-Poly1305 keyed from `derive_token_key(token)`.

If a peer with the same public key is already enrolled, the server re-issues the offer with the original assigned IP (idempotent re-enrollment).

## Linux Fast Paths

All three fast paths gracefully degrade to the standard path if unavailable:

| Feature | Condition | Fallback |
|---|---|---|
| Multi-queue TUN | `tun_queues > 1`, Linux only | Single-queue `Tun` |
| `io_uring` | `use_uring = true`, Linux only | Standard outbound threads |
| AF_XDP | `xdp_ifname = Some(...)`, Linux only | Standard `UdpSocket` recv |

When AF_XDP is active, the BPF program is attached to the NIC and held alive by `_xdp_prog` until `run` returns, at which point dropping the value detaches it from the interface.

## Control Socket

The server binds a UNIX domain socket at `/tmp/rustguard.sock` (mode `0o666`) for out-of-band enrollment window control. The `rustguard open`, `rustguard close`, and `rustguard status` CLI commands communicate via this socket. The enrollment window is implemented as an `Arc<AtomicI64>` storing a UNIX deadline timestamp; `0` means closed.

## Examples

### Minimal server startup

```rust
use std::net::Ipv4Addr;
use std::path::PathBuf;
use rustguard_enroll::server::{run, ServeConfig};

fn main() -> std::io::Result<()> {
    run(ServeConfig {
        listen_port: 51820,
        pool_network: Ipv4Addr::new(10, 150, 0, 0),
        pool_prefix: 24,
        token: "mysecret".to_string(),
        open_immediately: false,
        state_path: Some(PathBuf::from("/var/lib/rustguard/state.json")),
        xdp_ifname: None,
        tun_queues: 1,
        use_uring: false,
    })
}
```

### High-performance Linux server with AF_XDP and multi-queue TUN

```rust
use std::net::Ipv4Addr;
use std::path::PathBuf;
use rustguard_enroll::server::{run, ServeConfig};

fn main() -> std::io::Result<()> {
    run(ServeConfig {
        listen_port: 51820,
        pool_network: Ipv4Addr::new(10, 100, 0, 0),
        pool_prefix: 20,          // 4094 client addresses
        token: "homelab-token".to_string(),
        open_immediately: true,   // enrollment open immediately for 3600s
        state_path: Some(PathBuf::from("/home/user/.rustguard/state.json")),
        xdp_ifname: Some("enp7s0".to_string()),
        tun_queues: 4,
        use_uring: true,
    })
}
```

### Managing the enrollment window at runtime

```bash
# Open enrollment for 60 seconds
rustguard open 60

# Check status
rustguard status
# -> OPEN 47s remaining, 3 peers

# Close immediately
rustguard close
```

## See Also

- [rustguard-enroll module](../05-Modules/14-rustguard-enroll.md) — crate-level overview including `serve` and `join` commands
- [client](../05-Modules/01-client.md) — client-side enrollment counterpart to this server
- [rustguard-core](../05-Modules/11-rustguard-core.md) — `handshake`, `TransportSession`, and `SessionTimers` used by the server
- [rustguard-tun](../05-Modules/07-rustguard-tun.md) — TUN, multi-queue TUN, AF_XDP, and `io_uring` abstractions
- [Common Workflows](../03-Guides/01-Common-Workflows.md) — end-to-end enrollment walkthrough
- [Data Flow](../02-Architecture/03-Data-Flow.md) — system-level packet flow diagram