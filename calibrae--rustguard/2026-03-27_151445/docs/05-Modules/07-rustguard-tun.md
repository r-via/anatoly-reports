# rustguard-tun

> Cross-platform TUN device abstraction providing standard, multi-queue, io_uring, and AF_XDP packet I/O backends for RustGuard.

## Overview

`rustguard-tun` is the network I/O layer of the RustGuard stack. It abstracts TUN device creation and packet I/O across macOS and Linux, and — on Linux — exposes three additional high-performance backends: multi-queue TUN (`IFF_MULTI_QUEUE`), an io_uring-driven engine, and an AF_XDP zero-copy UDP socket. The BPF loader submodule attaches a pre-compiled XDP filter to steer WireGuard UDP traffic (port 51820) directly into AF_XDP, bypassing the kernel networking stack.

The crate is part of the RustGuard workspace and has two runtime dependencies: `libc` (all platforms) and `io-uring = "0.7"` (Linux only).

**Files in this module (5 source files):**

| File | Description |
|---|---|
| `src/lib.rs` | Public `Tun` struct and `TunConfig` — cross-platform entry point |
| `src/linux.rs` | Linux TUN via `/dev/net/tun`, `IFF_TUN \| IFF_NO_PI` |
| `src/macos.rs` | macOS utun via kernel control socket (`com.apple.net.utun_control`) |
| `src/linux_mq.rs` | Multi-queue TUN (`IFF_MULTI_QUEUE`) for parallel per-core I/O |
| `src/uring.rs` | io_uring async I/O engine with pre-registered buffer pool |
| `src/xdp.rs` | AF_XDP zero-copy UDP socket with UMEM ring buffers |
| `src/bpf_loader.rs` | Minimal ELF loader for the pre-compiled `xdp_wg.o` XDP program |

## Public Types

### `TunConfig`

Configuration passed to `Tun::create` and `MultiQueueTun::create`.

```rust
pub struct TunConfig {
    /// Desired interface name. None = let the OS pick.
    pub name: Option<String>,
    /// MTU for the interface. WireGuard standard default: 1420.
    pub mtu: u16,
    /// Local address to assign (e.g. 10.0.0.1).
    pub address: std::net::Ipv4Addr,
    /// Peer/destination address for the point-to-point link.
    pub destination: std::net::Ipv4Addr,
    /// Netmask (e.g. 255.255.255.0).
    pub netmask: std::net::Ipv4Addr,
}
```

On macOS, `name` must be of the form `"utunN"` if provided; any other value returns `InvalidInput`. On Linux, `name` is the interface name passed to `TUNSETIFF` (e.g. `"tun0"`); `None` lets the kernel assign one.

---

### `Tun`

A single-queue TUN device. Available on both macOS and Linux.

```rust
impl Tun {
    pub fn create(config: &TunConfig) -> io::Result<Self>;
    pub fn raw_fd(&self) -> i32;
    pub fn name(&self) -> &str;
    pub fn read(&self, buf: &mut [u8]) -> io::Result<usize>;
    pub fn write(&self, packet: &[u8]) -> io::Result<usize>;
}
```

`read` strips the 4-byte `AF` header on macOS automatically; on Linux (`IFF_NO_PI`) no header is present. `write` prepends the 4-byte header on macOS (deriving the address family from the IP version nibble); on Linux the IP packet is written directly. `raw_fd` returns the underlying file descriptor, needed when handing the TUN fd to `UringTun::new`.

`Tun` implements `Drop`, which closes the file descriptor.

---

### `MultiQueueTun` *(Linux only)*

A TUN device backed by multiple file descriptors for parallel per-core I/O. Created with `IFF_TUN | IFF_NO_PI | IFF_MULTI_QUEUE`. The kernel distributes incoming packets across queues via flow hashing; outgoing packets may be written to any queue.

```rust
impl MultiQueueTun {
    pub fn create(config: &TunConfig, num_queues: usize) -> io::Result<Self>;
    pub fn num_queues(&self) -> usize;
    pub fn name(&self) -> &str;
    pub fn queue_fd(&self, queue: usize) -> i32;
    pub fn read_queue(&self, queue: usize, buf: &mut [u8]) -> io::Result<usize>;
    pub fn write_queue(&self, queue: usize, packet: &[u8]) -> io::Result<usize>;
}
```

`queue_fd(queue)` wraps with `queue % num_queues`, so it is safe to pass any value. `MultiQueueTun` implements `Drop`, closing all file descriptors.

---

### `UringTun` *(Linux only)*

An io_uring-backed TUN I/O engine. Replaces blocking `read`/`write` on a TUN fd with batched async submissions, reducing per-packet context-switch overhead. Uses a pre-registered `BufferPool` of 256 slots × 2 KB each (`NUM_BUFS = 256`, `BUF_SIZE = 2048`). On construction, `NUM_BUFS / 2` read SQEs are pre-submitted to keep the pipeline saturated.

```rust
pub struct UringTun {
    pub bufs: BufferPool,
    // ...
}

impl UringTun {
    pub fn new(tun_fd: i32) -> io::Result<Self>;
    pub fn submit_write(&mut self, buf_idx: usize, len: usize) -> io::Result<()>;
    pub fn submit_and_wait(&mut self, min_complete: usize) -> io::Result<Vec<Completion>>;
    pub fn poll(&mut self) -> io::Result<Vec<Completion>>;
}
```

`submit_and_wait` blocks until at least `min_complete` CQEs arrive, then harvests all available completions and refills read SQEs. `poll` is non-blocking: it submits pending SQEs and harvests whatever is available without waiting.

#### `BufferPool`

```rust
impl BufferPool {
    pub fn slot(&self, idx: usize) -> &[u8];
    pub fn slot_mut(&mut self, idx: usize) -> &mut [u8];
    pub fn alloc(&mut self) -> Option<usize>;
    pub fn free(&mut self, idx: usize);
}
```

#### `Completion`

```rust
pub struct Completion {
    pub buf_idx: usize,
    pub is_read: bool,
    pub result: i32,  // bytes transferred, or negative errno
}
```

User data encoding: bit 63 set = read completion; bits 0–31 = buffer slot index.

---

### `XdpSocket` *(Linux only)*

An `AF_XDP` socket with shared UMEM and four ring buffers (RX, TX, fill, completion). Provides zero-copy frame-based I/O for the encrypted UDP side of the tunnel.

```rust
pub struct XdpConfig {
    pub ifname: String,
    pub queue_id: u32,
    pub frame_size: u32,   // default: 4096
    pub num_frames: u32,   // default: 4096
    pub ring_size: u32,    // default: 2048
}

impl Default for XdpConfig { ... }

impl XdpSocket {
    pub fn create(config: &XdpConfig) -> io::Result<Self>;
    pub fn rx_poll(&self) -> Vec<(u64, &[u8])>;
    pub fn rx_release(&mut self, frames: &[u64]);
    pub fn tx_send(&mut self, data: &[u8]) -> bool;
    pub fn tx_complete(&mut self);
    pub fn fd(&self) -> i32;
}
```

`rx_poll` returns `(frame_addr, packet_data)` pairs without consuming the frames; `rx_release` must be called after processing to return the addresses to the fill ring. `tx_send` returns `false` when no free TX frames are available. `tx_complete` reclaims completed TX frames from the completion ring. `XdpSocket` implements `Drop`, closing the fd and unmapping UMEM.

`XdpDesc` is publicly exported for callers that need to inspect raw descriptor fields:

```rust
#[repr(C)]
pub struct XdpDesc {
    pub addr: u64,
    pub len: u32,
    pub options: u32,
}
```

---

### `XdpProgram` *(Linux only)*

Manages a loaded XDP program and its associated XSKMAP.

```rust
pub struct XdpProgram {
    pub prog_fd: i32,
    pub xsks_map_fd: i32,
    // ...
}

impl XdpProgram {
    pub fn load_and_attach(ifname: &str) -> io::Result<Self>;
    pub fn register_xsk(&self, queue_id: u32, xsk_fd: i32) -> io::Result<()>;
}
```

`load_and_attach` embeds the pre-compiled `bpf/xdp_wg.o` (approximately 1.4 KB), parses the ELF, patches map `fd` references, loads the BPF program, and attaches it to `ifname` via netlink `RTM_SETLINK`. It falls back to shelling out to `ip link set dev <ifname> xdpgeneric pinned <fd>` if netlink is unavailable. `register_xsk` inserts an `XdpSocket` fd into the XSKMAP for a given queue, enabling the XDP program to redirect matching packets to that socket. `XdpProgram` implements `Drop`, detaching the XDP program and closing both file descriptors.

> **Prerequisite:** The `ip` binary must be present on the system for the fallback attachment path. The netlink path has no additional dependencies.

## Platform Matrix

| Feature | macOS | Linux |
|---|---|---|
| `Tun` | ✓ utun | ✓ `/dev/net/tun` |
| `MultiQueueTun` | — | ✓ `IFF_MULTI_QUEUE` |
| `UringTun` | — | ✓ `io-uring = "0.7"` |
| `XdpSocket` | — | ✓ kernel ≥ 4.18 |
| `XdpProgram` (BPF loader) | — | ✓ kernel ≥ 4.18 |

## Examples

### Basic TUN device — ICMP echo reflector

```typescript
// Example is in Rust — see rustguard-tun/examples/tun_echo.rs
```

```bash
# Run the echo smoke test (requires root):
sudo cargo run --example tun_echo -p rustguard-tun

# In a second terminal, send pings to the peer address:
ping 10.0.0.2
```

```rust
use rustguard_tun::{Tun, TunConfig};
use std::net::Ipv4Addr;

let config = TunConfig {
    name: None,
    mtu: 1420,
    address: Ipv4Addr::new(10, 0, 0, 1),
    destination: Ipv4Addr::new(10, 0, 0, 2),
    netmask: Ipv4Addr::new(255, 255, 255, 255),
};

let tun = Tun::create(&config).expect("failed to create TUN (need root?)");
println!("interface: {}", tun.name());

let mut buf = [0u8; 1500];
loop {
    let n = tun.read(&mut buf)?;
    // buf[..n] contains a raw IP packet
    tun.write(&buf[..n])?;
}
```

### Multi-queue TUN — 4 parallel worker threads

```rust
use rustguard_tun::linux_mq::MultiQueueTun;
use rustguard_tun::TunConfig;
use std::net::Ipv4Addr;
use std::sync::Arc;

let config = TunConfig {
    name: Some("tun0".into()),
    mtu: 1420,
    address: Ipv4Addr::new(10, 0, 0, 1),
    destination: Ipv4Addr::new(10, 0, 0, 2),
    netmask: Ipv4Addr::new(255, 255, 255, 0),
};

let mq = Arc::new(MultiQueueTun::create(&config, 4)?);
println!("{} queues on {}", mq.num_queues(), mq.name());

for q in 0..mq.num_queues() {
    let mq = Arc::clone(&mq);
    std::thread::spawn(move || {
        let mut buf = [0u8; 1500];
        loop {
            match mq.read_queue(q, &mut buf) {
                Ok(n) => { mq.write_queue(q, &buf[..n]).ok(); }
                Err(e) => { eprintln!("queue {q} read error: {e}"); break; }
            }
        }
    });
}
```

### io_uring engine — batched async TUN I/O

```rust
use rustguard_tun::{Tun, TunConfig};
use rustguard_tun::uring::UringTun;
use std::net::Ipv4Addr;

let config = TunConfig {
    name: None,
    mtu: 1420,
    address: Ipv4Addr::new(10, 150, 0, 1),
    destination: Ipv4Addr::new(10, 150, 0, 2),
    netmask: Ipv4Addr::new(255, 255, 255, 255),
};

let tun = Tun::create(&config)?;
let mut engine = UringTun::new(tun.raw_fd())?;

loop {
    let completions = engine.submit_and_wait(1)?;
    for c in completions {
        if c.is_read && c.result > 0 {
            // Packet is in engine.bufs.slot(c.buf_idx)[..c.result as usize]
            let _pkt = &engine.bufs.slot(c.buf_idx)[..c.result as usize];
            // Write the (processed) packet back via a different buffer slot
            engine.submit_write(c.buf_idx, c.result as usize)?;
        } else if !c.is_read {
            engine.bufs.free(c.buf_idx);
        }
    }
}
```

### AF_XDP zero-copy — load BPF filter and receive packets

```rust
use rustguard_tun::bpf_loader::XdpProgram;
use rustguard_tun::xdp::{XdpConfig, XdpSocket};

// Attach the pre-compiled XDP WireGuard filter to eth0.
let prog = XdpProgram::load_and_attach("eth0")?;

let xdp_config = XdpConfig {
    ifname: "eth0".into(),
    queue_id: 0,
    ..XdpConfig::default()  // frame_size=4096, num_frames=4096, ring_size=2048
};
let mut xsk = XdpSocket::create(&xdp_config)?;

// Register the socket in the XDP program's XSKMAP.
prog.register_xsk(0, xsk.fd())?;

loop {
    let packets = xsk.rx_poll();
    if packets.is_empty() { continue; }
    let addrs: Vec<u64> = packets.iter().map(|(addr, _)| *addr).collect();
    for (_addr, data) in &packets {
        // data is the raw UDP payload — decrypt and forward to TUN
        println!("received {} bytes", data.len());
    }
    xsk.rx_release(&addrs);
    xsk.tx_complete(); // reclaim any completed TX frames
}
```

## See Also

- [rustguard-daemon](05-Modules/13-rustguard-daemon.md) — daemon that calls `Tun::create` to bring up the WireGuard tunnel
- [rustguard-kmod](05-Modules/06-rustguard-kmod.md) — kernel module alternative that eliminates the TUN layer entirely
- [tunnel](05-Modules/09-tunnel.md) — tunnel loop that drives `Tun::read` / `Tun::write`
- [Data Flow](02-Architecture/03-Data-Flow.md) — how plaintext packets traverse TUN → encrypt → UDP
- [System Overview](02-Architecture/01-System-Overview.md) — component placement of `rustguard-tun` in the full stack
- [Build and Test](06-Development/02-Build-and-Test.md) — how to build and run the `tun_echo` example