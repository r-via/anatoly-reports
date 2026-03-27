# rustguard-enroll

> Zero-configuration peer enrollment for RustGuard — token-authenticated key exchange, dynamic IP allocation, and a Zigbee-style pairing window.

## Overview

`rustguard-enroll` implements a custom enrollment protocol that replaces the manual WireGuard setup flow (key generation on both sides, public-key exchange, IP assignment, config file authoring) with a single shared token. A client calls `rustguard join <server>:<port> --token <token>` and emerges with a configured TUN interface and an active WireGuard session.

The crate is structured into eight modules:

| Module | Responsibility |
|--------|---------------|
| `protocol` | Wire-format serialisation and encryption of enrollment messages |
| `pool` | CIDR-based IPv4 address allocator |
| `state` | Atomic persistence of enrolled peers to disk |
| `control` | UNIX domain socket for runtime enrollment-window management |
| `server` | Enrollment + tunnel server (`rustguard serve`) |
| `client` | Enrollment + tunnel client (`rustguard join`) |
| `fast_udp` | Batched UDP I/O via `recvmmsg`/`sendmmsg` (Linux) |
| `packet` | Raw Ethernet frame parser for AF_XDP receive paths |

The crate depends on `rustguard-crypto`, `rustguard-core`, `rustguard-tun`, and `rustguard-daemon`.

## Enrollment Protocol

The enrollment channel sits on the same UDP port as WireGuard traffic and is distinguished by a four-byte magic prefix. All messages are encrypted with XChaCha20-Poly1305 under a key derived from the shared token — preventing passive observers from learning public keys — but the real security boundary is the Noise_IK handshake that follows enrollment.

```
EnrollRequest  (76 bytes):  magic(4) | nonce(24) | XChaCha20-Poly1305(client_pubkey[32]) | tag(16)
EnrollResponse (81 bytes):  magic(4) | nonce(24) | XChaCha20-Poly1305(server_pubkey[32] | ip[4] | prefix[1]) | tag(16)
```

Magic values: `RGE\x01` (request), `RGE\x02` (response).

### `protocol` module

#### Constants

```rust
pub const ENROLL_REQUEST_SIZE:  usize = 76;
pub const ENROLL_RESPONSE_SIZE: usize = 81;
```

#### `derive_token_key`

```rust
pub fn derive_token_key(token: &str) -> [u8; 32]
```

Derives a 32-byte symmetric key from the enrollment token using `BLAKE2s("rustguard-enroll-v1" || token)`. The raw token never appears on the wire.

#### `build_request` / `parse_request`

```rust
pub fn build_request(token_key: &[u8; 32], our_pubkey: &[u8; 32]) -> [u8; ENROLL_REQUEST_SIZE]
pub fn parse_request(token_key: &[u8; 32], buf: &[u8]) -> Option<[u8; 32]>
```

`build_request` serialises and encrypts a client enrollment request. `parse_request` verifies the magic prefix, decrypts, and returns the client's public key. Returns `None` on any authentication or length failure.

#### `EnrollmentOffer`

```rust
pub struct EnrollmentOffer {
    pub server_pubkey: [u8; 32],
    pub assigned_ip:   std::net::Ipv4Addr,
    pub prefix_len:    u8,
}
```

Payload carried in the enrollment response: the server's static public key, the IPv4 address assigned to the enrolling client, and the pool prefix length.

#### `build_response` / `parse_response`

```rust
pub fn build_response(token_key: &[u8; 32], offer: &EnrollmentOffer) -> [u8; ENROLL_RESPONSE_SIZE]
pub fn parse_response(token_key: &[u8; 32], buf: &[u8]) -> Option<EnrollmentOffer>
```

`build_response` serialises and encrypts an `EnrollmentOffer`. `parse_response` verifies, decrypts, and deserialises the response. Returns `None` on authentication or format failure.

## IP Address Pool

### `IpPool`

```rust
pub struct IpPool {
    pub prefix_len:  u8,
    pub server_addr: Ipv4Addr,
    // ...
}
```

Manages a CIDR block and hands out addresses to enrolling peers. The `.1` address within the range is always reserved for the server.

#### Methods

```rust
pub fn new(network: Ipv4Addr, prefix_len: u8) -> Option<Self>
```
Constructs a pool from a network address and prefix length. Returns `None` for prefix lengths greater than 30 (fewer than two usable addresses).

```rust
pub fn allocate(&mut self) -> Option<Ipv4Addr>
```
Allocates the next available address starting at `.2`. Returns `None` when the pool is exhausted.

```rust
pub fn allocate_specific(&mut self, addr: Ipv4Addr)
```
Marks a specific address as allocated; used when restoring persisted peers on restart.

```rust
pub fn release(&mut self, addr: Ipv4Addr)
```
Returns an address to the free pool. The server address (`.1`) is silently ignored.

```rust
pub fn contains(&self, addr: Ipv4Addr) -> bool
```
Returns `true` if `addr` falls within the pool's CIDR range.

```rust
pub fn assigned_count(&self) -> usize
```
Returns the number of currently allocated addresses, including the server's.

## Persistence

### `state` module

Enrolled peers are persisted to a flat text file so they survive server restarts. Private keys are **not** stored; they are regenerated on each restart and clients re-handshake.

#### `PersistedPeer`

```rust
pub struct PersistedPeer {
    pub public_key:  [u8; 32],
    pub assigned_ip: Ipv4Addr,
}
```

#### Functions

```rust
pub fn default_state_path() -> PathBuf   // → /var/lib/rustguard/peers.state
pub fn save(path: &Path, peers: &[PersistedPeer]) -> io::Result<()>
pub fn load(path: &Path) -> io::Result<Vec<PersistedPeer>>
```

`save` writes atomically via a `.tmp` sibling file followed by a `rename`. `load` returns an empty `Vec` when the file does not exist; it does not treat a missing file as an error.

File format: one peer per line, `<base64-pubkey> <ipv4-address>`.

## Enrollment Window (Control Socket)

### `control` module

The server starts with enrollment **closed**. A physical-presence model gates new peers: an operator runs `rustguard open <seconds>` to open a time-bounded window, and `rustguard close` to close it immediately. State is shared between threads via an atomic deadline timestamp.

#### Types

```rust
pub type EnrollmentWindow = Arc<AtomicI64>;
```
Stores the UNIX timestamp (seconds) at which enrollment closes. Zero means closed.

#### Window functions

```rust
pub fn new_window() -> EnrollmentWindow
pub fn is_open(window: &EnrollmentWindow) -> bool
pub fn open_window(window: &EnrollmentWindow, duration_secs: u64)
pub fn close_window(window: &EnrollmentWindow)
pub fn remaining(window: &EnrollmentWindow) -> u64   // seconds; 0 if closed
```

#### Control socket

```rust
pub fn socket_path() -> PathBuf                         // → /tmp/rustguard.sock
pub fn start_listener(
    window: EnrollmentWindow,
    peer_count: Arc<std::sync::Mutex<usize>>,
) -> io::Result<PathBuf>
pub fn send_command(cmd: &str) -> io::Result<String>
pub fn cleanup(path: &Path)
```

`start_listener` binds `/tmp/rustguard.sock` with `0o666` permissions (allowing the non-root CLI to send commands to a root-owned daemon) and spawns a background thread. `send_command` connects, writes a line, and reads the single-line response with a 5-second timeout.

**Control protocol** (line-oriented text over UNIX socket):

| Command | Response |
|---------|----------|
| `OPEN <seconds>` | `OK enrollment open for Ns` |
| `CLOSE` | `OK enrollment closed` |
| `STATUS` | `OPEN Ns remaining, N peers` or `CLOSED, N peers` |

## Server

### `ServeConfig`

```rust
pub struct ServeConfig {
    pub listen_port:    u16,
    pub pool_network:   Ipv4Addr,
    pub pool_prefix:    u8,
    pub token:          String,
    pub open_immediately: bool,
    pub state_path:     Option<std::path::PathBuf>,
    /// AF_XDP interface name. None = standard UDP socket.
    pub xdp_ifname:     Option<String>,
    /// Multi-queue TUN queue count. 0 or 1 = single queue.
    pub tun_queues:     usize,
    /// Use io_uring for TUN I/O (Linux only).
    pub use_uring:      bool,
}
```

### `server::run`

```rust
pub fn run(config: ServeConfig) -> io::Result<()>
```

Starts the enrollment server. On entry, `run`:

1. Generates a fresh X25519 keypair.
2. Creates a TUN interface with the pool's `.1` address.
3. Restores persisted peers from `state_path` (if configured).
4. Starts the control socket listener.
5. Optionally attaches an AF_XDP socket to `xdp_ifname`.
6. Spawns outbound TUN→UDP worker threads (one per TUN queue, or an io_uring variant).
7. Runs the inbound UDP→TUN loop on the calling thread, handling enrollment requests and WireGuard handshake/transport messages.

The function blocks until the running flag is cleared (e.g. by a signal). On Linux, `recvmmsg` batches up to 32 UDP datagrams per syscall. AF_XDP and io_uring paths are opt-in via `ServeConfig` fields and require Linux with appropriate kernel support.

## Client

### `JoinConfig`

```rust
pub struct JoinConfig {
    pub server_endpoint: SocketAddr,
    pub token:           String,
}
```

### `client::run`

```rust
pub fn run(config: JoinConfig) -> io::Result<()>
```

Performs enrollment and then runs the tunnel. On entry, `run`:

1. Generates a fresh X25519 keypair.
2. Sends an `EnrollRequest` to `server_endpoint`.
3. Receives and decrypts the `EnrollResponse`; extracts server public key and assigned IP.
4. Creates a TUN interface with the assigned IP and adds a route for the pool CIDR.
5. Initiates the Noise_IK handshake with the server.
6. Enters the tunnel loop: outbound TUN→encrypt→UDP and inbound UDP→decrypt→TUN.

## Batched UDP I/O (`fast_udp`)

```rust
pub const BATCH_SIZE: usize = 32;
pub const PKT_BUF_SIZE: usize = 2048;

pub struct RecvBatch {
    pub bufs:  [[u8; PKT_BUF_SIZE]; BATCH_SIZE],
    pub lens:  [usize; BATCH_SIZE],
    pub addrs: [Option<SocketAddr>; BATCH_SIZE],
    pub count: usize,
}

impl RecvBatch {
    pub fn new() -> Self
}

pub fn recv_batch(sock: &UdpSocket, batch: &mut RecvBatch) -> io::Result<usize>
pub fn send_packet(sock: &UdpSocket, buf: &[u8], addr: SocketAddr) -> io::Result<usize>
```

On Linux, `recv_batch` calls `recvmmsg(2)` with `MSG_WAITFORONE` — blocks until at least one packet arrives, then drains up to 32 without blocking. On macOS the function falls back to a single `recv_from`. Returns `0` on timeout (`WouldBlock`).

## AF_XDP Frame Parser (`packet`)

```rust
pub struct ParsedUdp<'a> {
    pub src_addr: SocketAddr,
    pub payload:  &'a [u8],
}

pub fn parse_eth_udp(frame: &[u8]) -> Option<ParsedUdp<'_>>
```

Strips the Ethernet (14 bytes), IPv4/IPv6, and UDP headers from a raw AF_XDP frame and returns the UDP payload with its source address. Returns `None` for non-UDP frames, truncated frames, or unsupported Ethernet types. IPv6 extension headers are not supported.

## Examples

### Server: start enrollment and accept one peer

```rust
use std::net::Ipv4Addr;
use rustguard_enroll::server::{ServeConfig, run};

fn main() -> std::io::Result<()> {
    run(ServeConfig {
        listen_port: 51820,
        pool_network: Ipv4Addr::new(10, 150, 0, 0),
        pool_prefix: 24,
        token: "correct-horse-battery-staple".to_string(),
        open_immediately: false,
        state_path: Some(rustguard_enroll::state::default_state_path()),
        xdp_ifname: None,
        tun_queues: 1,
        use_uring: false,
    })
}
```

### Client: enroll and start tunnel

```rust
use rustguard_enroll::client::{JoinConfig, run};

fn main() -> std::io::Result<()> {
    run(JoinConfig {
        server_endpoint: "198.51.100.1:51820".parse().unwrap(),
        token: "correct-horse-battery-staple".to_string(),
    })
}
```

### Opening the enrollment window at runtime

```rust
use rustguard_enroll::control;

fn main() -> std::io::Result<()> {
    // Open for 60 seconds via the running server's control socket.
    let response = control::send_command("OPEN 60")?;
    println!("{}", response.trim()); // "OK enrollment open for 60s"

    let status = control::send_command("STATUS")?;
    println!("{}", status.trim()); // "OPEN 59s remaining, 0 peers"
    Ok(())
}
```

### Protocol round-trip (library usage)

```rust
use rustguard_enroll::protocol::{
    derive_token_key, build_request, parse_request,
    build_response, parse_response, EnrollmentOffer,
};
use std::net::Ipv4Addr;

let token_key = derive_token_key("my-enrollment-token");

// Client side
let client_pubkey = [0x42u8; 32];
let wire_request = build_request(&token_key, &client_pubkey);

// Server side
let received_pubkey = parse_request(&token_key, &wire_request).unwrap();
assert_eq!(received_pubkey, client_pubkey);

let offer = EnrollmentOffer {
    server_pubkey: [0xAAu8; 32],
    assigned_ip: Ipv4Addr::new(10, 150, 0, 2),
    prefix_len: 24,
};
let wire_response = build_response(&token_key, &offer);

// Client side
let received_offer = parse_response(&token_key, &wire_response).unwrap();
assert_eq!(received_offer.assigned_ip, Ipv4Addr::new(10, 150, 0, 2));
assert_eq!(received_offer.prefix_len, 24);
```

### IP pool allocation

```rust
use rustguard_enroll::pool::IpPool;
use std::net::Ipv4Addr;

let mut pool = IpPool::new(Ipv4Addr::new(10, 150, 0, 0), 24).unwrap();
assert_eq!(pool.server_addr, Ipv4Addr::new(10, 150, 0, 1));

let peer_ip = pool.allocate().unwrap(); // 10.150.0.2
pool.release(peer_ip);                  // returns address to the pool
```

## See Also

- [rustguard-cli](05-Modules/10-rustguard-cli.md) — CLI commands (`serve`, `join`, `open`, `close`, `status`) that invoke this crate
- [rustguard-core](05-Modules/11-rustguard-core.md) — Noise_IK handshake and transport session used after enrollment
- [rustguard-crypto](05-Modules/12-rustguard-crypto.md) — XChaCha20-Poly1305 and BLAKE2s primitives underlying the enrollment channel
- [rustguard-tun](05-Modules/07-rustguard-tun.md) — TUN device, multi-queue TUN, AF_XDP, and io_uring used by `server::run` and `client::run`
- [Common Workflows](03-Guides/01-Common-Workflows.md) — End-to-end enrollment walkthrough
- [System Overview](02-Architecture/01-System-Overview.md) — How the enrollment crate fits into the broader architecture