[← Back to Best Practices](./index.md) · [← Back to report](../../public_report.md)

# Best Practices — Shard 1

## Findings

| File | Verdict | BP Score | Details |
|------|---------|----------|---------|
| `rustguard-kmod/src/lib.rs` | CRITICAL | 7.5/10 | [details](#rustguard-kmodsrclibrs) |
| `rustguard-tun/src/xdp.rs` | CRITICAL | 6.5/10 | [details](#rustguard-tunsrcxdprs) |
| `rustguard-daemon/src/tunnel.rs` | NEEDS_REFACTOR | 5/10 | [details](#rustguard-daemonsrctunnelrs) |
| `rustguard-enroll/src/control.rs` | NEEDS_REFACTOR | 6/10 | [details](#rustguard-enrollsrccontrolrs) |
| `rustguard-tun/src/linux.rs` | NEEDS_REFACTOR | 6.5/10 | [details](#rustguard-tunsrclinuxrs) |
| `rustguard-kmod/src/cookie.rs` | NEEDS_REFACTOR | 6/10 | [details](#rustguard-kmodsrccookiers) |
| `rustguard-kmod/src/allowedips.rs` | NEEDS_REFACTOR | 9/10 | [details](#rustguard-kmodsrcallowedipsrs) |
| `rustguard-tun/src/uring.rs` | NEEDS_REFACTOR | 5/10 | [details](#rustguard-tunsrcuringrs) |

## Details

### `rustguard-kmod/src/lib.rs` — 7.5/10

**Failed rules:**

- Rule 3: Proper error handling with Result/Option (HIGH)

- Replace chained `.unwrap()` with `.expect()` to document the invariant and produce actionable diagnostics on any future logic error.
  - Before: `let pending = (*state).peers[pidx].as_mut().unwrap().pending_handshake.take().unwrap();`
  - After: `let pending = (*state).peers[pidx]
    .as_mut()
    .expect("invariant: pidx verified Some in loop above")
    .pending_handshake
    .take()
    .expect("invariant: pending_handshake verified Some in loop above");`
- Check the return value of `wg_timer_start` so timer failures are logged rather than silently dropped.
  - Before: `unsafe { wg_timer_start(state_raw as VoidPtr) };`
  - After: `let timer_ret = unsafe { wg_timer_start(state_raw as VoidPtr) };
if timer_ret != 0 {
    // Non-fatal: tunnel works but rekeying and keepalives will not fire.
    pr_err!("rustguard: timer start failed ({})\n", timer_ret);
}`
- Check the return value of `wg_chacha20poly1305_encrypt` in `send_keepalive` to avoid transmitting unauthenticated garbage on AEAD failure.
  - Before: `wg_chacha20poly1305_encrypt(
    session.key_send.as_ptr(), counter,
    core::ptr::null(), 0,
    core::ptr::null(), 0,
    buf.as_mut_ptr().add(WG_HEADER_SIZE),
);
wg_socket_send((*state).udp_sock, buf.as_ptr(), …);`
  - After: `let enc_ret = wg_chacha20poly1305_encrypt(
    session.key_send.as_ptr(), counter,
    core::ptr::null(), 0,
    core::ptr::null(), 0,
    buf.as_mut_ptr().add(WG_HEADER_SIZE),
);
if enc_ret != 0 { return; }
wg_socket_send((*state).udp_sock, buf.as_ptr(), …);`
- Zeroize `prev_session` and drop `pending_handshake` in `cleanup_state` to eliminate key-material residue on module unload.
  - Before: `if let Some(ref mut session) = peer.session {
    noise::zeroize(&mut session.key_send);
    noise::zeroize(&mut session.key_recv);
}`
  - After: `if let Some(ref mut session) = peer.session {
    noise::zeroize(&mut session.key_send);
    noise::zeroize(&mut session.key_recv);
}
if let Some(ref mut prev) = peer.prev_session {
    noise::zeroize(&mut prev.key_send);
    noise::zeroize(&mut prev.key_recv);
}
// Drop ephemeral DH state (InitiatorState should implement ZeroizeOnDrop).
peer.pending_handshake = None;`
- Add SAFETY comments to `unsafe impl Send/Sync` documenting the C-layer locking invariants that justify cross-thread access.
  - Before: `unsafe impl Send for DeviceState {}
unsafe impl Sync for DeviceState {}`
  - After: `// SAFETY: DeviceState is accessed from RX context (serialized by UDP socket lock)
// and TX context (netif layer). Mutable fields (session install, index_map writes)
// are only mutated in RX context. send_counter uses AtomicU64 for TX/RX sharing.
unsafe impl Send for DeviceState {}
unsafe impl Sync for DeviceState {}`

### `rustguard-tun/src/xdp.rs` — 6.5/10

**Failed rules:**

- Rule 3: Proper error handling with Result/Option (no silent ignores) (HIGH)
- Rule 11: Memory safety (no leaks via mem::forget, proper Drop impls) (HIGH)

- Check return values of setsockopt ring-size calls and propagate errors instead of silently discarding them
  - Before: `setsockopt_raw(fd, SOL_XDP, XDP_UMEM_FILL_RING, &rs);
setsockopt_raw(fd, SOL_XDP, XDP_UMEM_COMPLETION_RING, &rs);
setsockopt_raw(fd, SOL_XDP, XDP_RX_RING, &rs);
setsockopt_raw(fd, SOL_XDP, XDP_TX_RING, &rs);`
  - After: `for (opt, name) in [
    (XDP_UMEM_FILL_RING, "XDP_UMEM_FILL_RING"),
    (XDP_UMEM_COMPLETION_RING, "XDP_UMEM_COMPLETION_RING"),
    (XDP_RX_RING, "XDP_RX_RING"),
    (XDP_TX_RING, "XDP_TX_RING"),
] {
    if setsockopt_raw(fd, SOL_XDP, opt, &rs) < 0 {
        let err = io::Error::last_os_error();
        libc::close(fd);
        libc::munmap(umem as *mut _, umem_size);
        return Err(err);
    }
}`
- Store base mmap pointer and size in Ring and implement Drop to prevent kernel-mapped memory leaks
  - Before: `struct Ring {
    producer: *const AtomicU32,
    consumer: *const AtomicU32,
    flags: *const AtomicU32,
    descs: *mut u8,
    mask: u32,
}`
  - After: `struct Ring {
    base: *mut u8,
    base_len: usize,
    producer: *const AtomicU32,
    consumer: *const AtomicU32,
    flags: *const AtomicU32,
    descs: *mut u8,
    mask: u32,
}

impl Drop for Ring {
    fn drop(&mut self) {
        if !self.base.is_null() {
            // SAFETY: base and base_len were obtained from a successful mmap call
            // and have not been freed elsewhere.
            unsafe { libc::munmap(self.base as *mut _, self.base_len); }
        }
    }
}`
- Add per-block SAFETY comments to unsafe blocks inside method bodies
  - Before: `fn producer(&self) -> u32 {
    unsafe { (*self.producer).load(Ordering::Acquire) }
}`
  - After: `fn producer(&self) -> u32 {
    // SAFETY: self.producer points into a kernel-shared mmap region valid for
    // the lifetime of the Ring, aligned to 4 bytes, and the AtomicU32 layout
    // matches the kernel ring producer field.
    unsafe { (*self.producer).load(Ordering::Acquire) }
}`
- Add missing derives and documentation to public types
  - Before: `#[repr(C)]
#[derive(Clone, Copy, Default)]
pub struct XdpDesc {
    pub addr: u64,
    pub len: u32,
    pub options: u32,
}`
  - After: `/// Descriptor entry used in XDP RX and TX rings.
#[repr(C)]
#[derive(Clone, Copy, Default, Debug, PartialEq, Eq)]
pub struct XdpDesc {
    /// Offset within the UMEM region where the packet data begins.
    pub addr: u64,
    /// Length of the packet in bytes.
    pub len: u32,
    pub options: u32,
}`
- Restrict if_nametoindex visibility to pub(crate) to avoid leaking a libc helper into the public API
  - Before: `pub fn if_nametoindex(name: &str) -> io::Result<u32> {`
  - After: `pub(crate) fn if_nametoindex(name: &str) -> io::Result<u32> {`

### `rustguard-daemon/src/tunnel.rs` — 5/10

**Failed rules:**

- Rule 2: No unsafe blocks without clear justification comment (CRITICAL)

- Add // SAFETY: comments to all unsafe blocks in ctrlc_handler
  - Before: `unsafe {
    libc::signal(libc::SIGINT, signal_noop as *const () as libc::sighandler_t);
    libc::signal(libc::SIGTERM, signal_noop as *const () as libc::sighandler_t);
}`
  - After: `// SAFETY: signal_noop is an async-signal-safe no-op function with the correct
// C signature. We install it only to make SIGINT/SIGTERM non-default so that
// sigwait() in the spawned thread can intercept them reliably on all POSIX platforms.
unsafe {
    libc::signal(libc::SIGINT, signal_noop as *const () as libc::sighandler_t);
    libc::signal(libc::SIGTERM, signal_noop as *const () as libc::sighandler_t);
}`
- Add // SAFETY: comment to mem::zeroed() for sigset_t
  - Before: `let mut sigset: libc::sigset_t = unsafe { std::mem::zeroed() };`
  - After: `// SAFETY: sigset_t is a POD C struct with no invalid bit patterns;
// zeroing produces a defined initial state that sigemptyset then fully initialises.
let mut sigset: libc::sigset_t = unsafe { std::mem::zeroed() };`
- Replace manual index loop with a collect-then-iterate pattern (consistent with the timer thread's rekey_requests approach)
  - Before: `for i in 0..st.peers.len() {
    if let Some(endpoint) = st.peers[i].endpoint {
        let sender_index = rand_index();
        let (init_msg, init_state) = handshake::create_initiation(
            &st.our_static,
            &st.peers[i].public_key,
            sender_index,
        );
        st.pending_handshakes.push(...);
        st.peers[i].timers.last_handshake_sent = Some(std::time::Instant::now());
    }
}`
  - After: `let init_targets: Vec<(usize, SocketAddr)> = st.peers.iter()
    .enumerate()
    .filter_map(|(i, p)| p.endpoint.map(|ep| (i, ep)))
    .collect();
for (i, endpoint) in init_targets {
    let sender_index = rand_index();
    let (init_msg, init_state) = handshake::create_initiation(
        &st.our_static, &st.peers[i].public_key, sender_index,
    );
    // ...
}`
- Set a write timeout on the UDP socket to prevent the outbound thread from holding the Mutex indefinitely when the kernel send buffer is full
  - Before: `udp.set_read_timeout(Some(Duration::from_millis(500)))?;`
  - After: `udp.set_read_timeout(Some(Duration::from_millis(500)))?;
udp.set_write_timeout(Some(Duration::from_millis(200)))?;`
- Log route deletion failures at shutdown rather than silently discarding them
  - Before: `let _ = delete_route(&entry.route, &entry.interface, entry.is_v6);
println!("route delete {}", entry.route);`
  - After: `match delete_route(&entry.route, &entry.interface, entry.is_v6) {
    Ok(out) if out.status.success() => println!("route delete {}", entry.route),
    Ok(out) => eprintln!("route delete {} failed: {}", entry.route, String::from_utf8_lossy(&out.stderr)),
    Err(e) => eprintln!("route delete {} error: {e}", entry.route),
}`

### `rustguard-enroll/src/control.rs` — 6/10

**Failed rules:**

- Rule 1: No unwrap in production code (CRITICAL)

- Replace the three `duration_since(UNIX_EPOCH).unwrap()` calls with an infallible helper or `.unwrap_or_default()` to express intent and silence the implicit panic path.
  - Before: `let now = SystemTime::now()
    .duration_since(UNIX_EPOCH)
    .unwrap()
    .as_secs() as i64;`
  - After: `let now = SystemTime::now()
    .duration_since(UNIX_EPOCH)
    .unwrap_or_default()   // UNIX_EPOCH <= now on any sane system
    .as_secs() as i64;`
- Replace the mutex `.unwrap()` in `handle_client` with explicit poison handling to avoid propagating a panic when the lock is poisoned.
  - Before: `let count = *peer_count.lock().unwrap();`
  - After: `let count = *peer_count.lock().unwrap_or_else(|e| e.into_inner());`
- Break out of the command loop on write errors in `handle_client` instead of silently discarding them, so a disconnected client does not keep the loop spinning.
  - Before: `let _ = writer.write_all(msg.as_bytes());`
  - After: `if writer.write_all(msg.as_bytes()).is_err() {
    break;
}`
- Add a doc comment to the public `new_window` constructor.
  - Before: `pub fn new_window() -> EnrollmentWindow {`
  - After: `/// Create a new, closed enrollment window.
pub fn new_window() -> EnrollmentWindow {`

### `rustguard-tun/src/linux.rs` — 6.5/10

**Failed rules:**

- Rule 2: No unsafe blocks without clear justification comment (CRITICAL)

- Add `// SAFETY:` comments to every `unsafe` block explaining the invariants upheld. Example for the `read` call:
  - Before: `let n = unsafe { libc::read(fd, buf.as_mut_ptr() as *mut _, buf.len()) };`
  - After: `// SAFETY: `fd` is a valid open TUN file descriptor owned by the caller.
// `buf` is a live mutable slice; we pass its exact length so no out-of-bounds write can occur.
let n = unsafe { libc::read(fd, buf.as_mut_ptr() as *mut libc::c_void, buf.len()) };`
- Add `// SAFETY:` comment to the `create` unsafe block covering the ioctl pointer arguments:
  - Before: `if libc::ioctl(fd, TUNSETIFF, &mut ifr as *mut _) < 0 {`
  - After: `// SAFETY: `ifr` is a correctly-sized `IfreqFlags` (#[repr(C)]) on the stack;
// the kernel reads/writes exactly `sizeof(ifreq)` bytes via this pointer.
if libc::ioctl(fd, TUNSETIFF, &mut ifr as *mut _) < 0 {`
- Add a `///` doc comment to `pub fn create` describing its behaviour, parameters, and error conditions:
  - Before: `pub fn create(config: &TunConfig) -> io::Result<Tun> {`
  - After: `/// Create a Linux TUN interface using `/dev/net/tun` with `IFF_TUN | IFF_NO_PI`.
///
/// Opens the device, registers the interface name (or accepts a kernel-assigned one),
/// configures the address/destination/netmask/MTU via `SIOCSIF*` ioctls, and brings
/// the interface up. Returns the ready-to-use [`Tun`] handle on success.
///
/// # Errors
/// Returns `io::Error` if the device cannot be opened, the ioctl fails, or the
/// kernel returns an invalid UTF-8 interface name.
pub fn create(config: &TunConfig) -> io::Result<Tun> {`

### `rustguard-kmod/src/cookie.rs` — 6/10

**Failed rules:**

- Rule 3: Proper error handling with Result/Option (HIGH)
- Rule 11: Memory safety (HIGH)

- Check the return value of `wg_xchacha20poly1305_encrypt` in `create_reply`, consistent with how `process_reply` handles decryption failures.
  - Before: `unsafe {
    wg_xchacha20poly1305_encrypt(
        key.as_ptr(), nonce.as_ptr(),
        cookie.as_ptr(), COOKIE_LEN as u32,
        mac1.as_ptr(), 16,
        encrypted_cookie.as_mut_ptr(),
    );
}`
  - After: `let ret = unsafe {
    wg_xchacha20poly1305_encrypt(
        key.as_ptr(), nonce.as_ptr(),
        cookie.as_ptr(), COOKIE_LEN as u32,
        mac1.as_ptr(), 16,
        encrypted_cookie.as_mut_ptr(),
    )
};
if ret != 0 {
    // Encryption invariants broken; return zeroed reply to signal failure.
    return [0u8; 64];
}`
- Guard against >4 chunks in `hash()` to prevent out-of-bounds memory access in the C function.
  - Before: `unsafe { wg_blake2s_hash(ptrs.as_ptr(), lens.as_ptr(), chunks.len() as u32, out.as_mut_ptr()) };`
  - After: `let n = chunks.len().min(4);
// SAFETY: ptrs/lens are fully populated for indices 0..n; C receives the same bounded count.
unsafe { wg_blake2s_hash(ptrs.as_ptr(), lens.as_ptr(), n as u32, out.as_mut_ptr()) };`
- Add `// SAFETY:` comments to all unsafe FFI call sites explaining the invariants that make each call sound.
  - Before: `unsafe { wg_get_random_bytes(buf.as_mut_ptr(), N as u32) };`
  - After: `// SAFETY: `buf` is a valid mutable slice of exactly N bytes; N fits in u32 for all
// const instantiations used in this module (16, 24, 32).
unsafe { wg_get_random_bytes(buf.as_mut_ptr(), N as u32) };`
- Document the required external locking model for `CookieChecker` and `CookieState` to prevent accidental unsynchronised concurrent access.
  - Before: `/// Server-side: generates cookies and validates MAC2.
pub(crate) struct CookieChecker {`
  - After: `/// Server-side: generates cookies and validates MAC2.
///
/// # Thread Safety
/// Callers must hold an exclusive lock (e.g., the peer's `rwlock`) before calling
/// any `&mut self` method. The struct is intentionally not internally synchronised.
pub(crate) struct CookieChecker {`

### `rustguard-kmod/src/allowedips.rs` — 9/10

**Failed rules:**

- Rule 3: Proper error handling with Result/Option (HIGH)

- Propagate allocation errors from insert instead of silently returning
  - Before: `pub(crate) fn insert_v4(&mut self, ip: [u8; 4], cidr: u8, peer_idx: usize) {
    Self::insert(&mut self.root4, &ip, cidr, peer_idx);
}`
  - After: `pub(crate) fn insert_v4(&mut self, ip: [u8; 4], cidr: u8, peer_idx: usize) -> Result<()> {
    Self::insert(&mut self.root4, &ip, cidr, peer_idx)
}`
- Replace logically-infallible unwrap with expect to communicate the invariant
  - Before: `root.as_mut().unwrap()`
  - After: `root.as_mut().expect("root was just set to Some above")`
- Remove dead redundant assignments in the default-route arm
  - Before: `node.cidr = 0;
node.peer_idx = peer_idx;
// Store with a sentinel: cidr=255 means "this is a default route"
node.cidr = 255; // special: default
node.peer_idx = peer_idx;`
  - After: `// cidr=255 is a sentinel meaning "default route (match everything)"
node.cidr = 255;
node.peer_idx = peer_idx;`
- Simplify redundant boolean condition in lookup_recursive
  - Before: `if child.cidr == 255 || (child.cidr > 0 && child.cidr != 255) {`
  - After: `if child.cidr > 0 {`
- Document the required locking discipline on AllowedIps for concurrency safety
  - Before: `/// AllowedIPs routing table.
pub(crate) struct AllowedIps {`
  - After: `/// AllowedIPs routing table.
///
/// # Concurrency
/// Access must be serialized externally (e.g., via a `Mutex<AllowedIps>`).
/// Concurrent reads are safe only if no writer is active.
pub(crate) struct AllowedIps {`

### `rustguard-tun/src/uring.rs` — 5/10

**Failed rules:**

- Rule 2: No unsafe blocks without clear justification comment (CRITICAL)

- Add // SAFETY: comments to every unsafe block documenting the invariant that makes the operation sound.
  - Before: `unsafe {
    ring.submitter().register_buffers(&iovecs)?;
}`
  - After: `// SAFETY: `iovecs` is valid for the entire lifetime of `ring` because `bufs` is
// a field of the same struct and outlives it in drop order. The slice length
// matches NUM_BUFS as constructed by `BufferPool::iovecs()`.
unsafe {
    ring.submitter().register_buffers(&iovecs)?;
}`
- Replace the manual index loop in alloc() with an idiomatic iterator search.
  - Before: `pub fn alloc(&mut self) -> Option<usize> {
    for i in 0..NUM_BUFS {
        if !self.in_flight[i] {
            self.in_flight[i] = true;
            return Some(i);
        }
    }
    None
}`
  - After: `pub fn alloc(&mut self) -> Option<usize> {
    let i = self.in_flight.iter().position(|&f| !f)?;
    self.in_flight[i] = true;
    Some(i)
}`
- Derive Debug (and Clone/PartialEq where applicable) on public types that hold no sensitive material.
  - Before: `pub struct Completion {
    pub buf_idx: usize,
    pub is_read: bool,
    pub result: i32,
}`
  - After: `#[derive(Debug, Clone, PartialEq, Eq)]
pub struct Completion {
    pub buf_idx: usize,
    pub is_read: bool,
    pub result: i32,
}`
- Add bounds-checked accessors or return Result/Option from slot/free to prevent panic on misuse from library callers.
  - Before: `pub fn free(&mut self, idx: usize) {
    self.in_flight[idx] = false;
}`
  - After: `pub fn free(&mut self, idx: usize) {
    assert!(idx < NUM_BUFS, "BufferPool::free: idx {idx} out of range");
    self.in_flight[idx] = false;
}`
- Implement Drop for UringTun to drain in-flight operations and unregister fixed buffers before the backing Vec is deallocated.
  - Before: `// (no Drop impl)`
  - After: `impl Drop for UringTun {
    fn drop(&mut self) {
        // Drain all pending completions so no in-flight kernel I/O
        // touches the buffer pool after it is freed.
        if self.pending_reads > 0 {
            let _ = self.ring.submit_and_wait(self.pending_reads);
        }
        // SAFETY: Unregistering the same buffers that were registered in new().
        let _ = unsafe { self.ring.submitter().unregister_buffers() };
    }
}`
- Change slot_ptr to take &mut self, or wrap the buffer in UnsafeCell to avoid creating *mut through a shared reference.
  - Before: `fn slot_ptr(&self, idx: usize) -> *mut u8 {
    unsafe { self.data.as_ptr().add(idx * BUF_SIZE) as *mut u8 }
}`
  - After: `fn slot_ptr(&mut self, idx: usize) -> *mut u8 {
    self.data.as_mut_ptr().wrapping_add(idx * BUF_SIZE)
}`
