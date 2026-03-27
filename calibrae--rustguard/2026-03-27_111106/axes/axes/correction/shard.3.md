[← Back to Correction](./index.md) · [← Back to report](../public_report.md)

# Correction — Shard 3

## Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `rustguard-kmod/src/timers.rs` | NEEDS_REFACTOR | 2 | 95% | [details](../reviews/rustguard-kmod-src-timers.rev.md) |
| `rustguard-kmod/src/allowedips.rs` | NEEDS_REFACTOR | 2 | 85% | [details](../reviews/rustguard-kmod-src-allowedips.rev.md) |
| `rustguard-tun/src/uring.rs` | NEEDS_REFACTOR | 2 | 90% | [details](../reviews/rustguard-tun-src-uring.rev.md) |
| `rustguard-tun/src/lib.rs` | NEEDS_REFACTOR | 1 | 88% | [details](../reviews/rustguard-tun-src-lib.rev.md) |
| `rustguard-crypto/src/tai64n.rs` | NEEDS_REFACTOR | 1 | 87% | [details](../reviews/rustguard-crypto-src-tai64n.rev.md) |
| `rustguard-core/src/replay.rs` | NEEDS_REFACTOR | 1 | 92% | [details](../reviews/rustguard-core-src-replay.rev.md) |

## Symbol Details

### `rustguard-kmod/src/timers.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `REKEY_AFTER_MESSAGES` | L23–L23 | NEEDS_FIX | 80% | [USED] Used in needs_rekey() method at line 86 to check message count rekey t... |
| `SessionTimers` | L36–L49 | NEEDS_FIX | 88% | [USED] pub(crate) struct serving as crate-level session timer API with 6+ pub... |

### `rustguard-kmod/src/allowedips.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `TrieNode` | L16–L25 | NEEDS_FIX | 85% | [USED] Internal radix trie node struct extensively used: instantiated via Tri... |
| `AllowedIps` | L28–L31 | NEEDS_FIX | 85% | [DEAD] Exported routing table struct with 0 workspace importers; defined with... |

### `rustguard-tun/src/uring.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `BufferPool` | L26–L31 | NEEDS_FIX | 88% | [DEAD] Exported struct with 0 workspace importers. Published as pub field in ... |
| `UringTun` | L96–L102 | NEEDS_FIX | 88% | [DEAD] Exported struct with 0 workspace importers. Provides public constructo... |

### `rustguard-tun/src/lib.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `Tun` | L25–L30 | NEEDS_FIX | 78% | [USED] Public struct in library crate with documented public API methods (cre... |

### `rustguard-crypto/src/tai64n.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `Tai64n` | L9–L9 | NEEDS_FIX | 87% | [USED] Exported struct with 1 runtime importer (rustguard-crypto/src/lib.rs) ... |

### `rustguard-core/src/replay.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `ReplayWindow` | L13–L19 | NEEDS_FIX | 92% | [USED] Exported struct with 1 runtime importer: rustguard-core/src/session.rs... |

## Quick Wins

- [ ] <!-- ACT-f55ce3-1 --> **[correction · high · small]** `rustguard-core/src/replay.rs`: Fix shift_window bit-level shift: change iteration from .rev() to forward order, replace `*word >> bit_shift` with `*word << bit_shift`, and replace carry extraction `*word << (64 - bit_shift)` with `*word >> (64 - bit_shift)`. This ensures existing counter bits advance to higher-index (older) positions rather than being destroyed on every window slide. [L112]
- [ ] <!-- ACT-4d0b70-1 --> **[correction · high · small]** `rustguard-tun/src/lib.rs`: Change raw_fd() return type from i32 to BorrowedFd<'_> (std::os::unix::io::BorrowedFd) so callers cannot wrap the fd in an owning type that will close it independently, avoiding a double-close that would silently close an unrelated fd on Linux. [L60]
- [ ] <!-- ACT-310cbd-2 --> **[correction · medium · small]** `rustguard-crypto/src/tai64n.rs`: Add nanoseconds validation in `from_bytes`: if `u32::from_be_bytes(bytes[8..12].try_into().unwrap()) > 999_999_999`, either return a `Result::Err`, clamp the field, or otherwise reject the input — to uphold the byte-wise ordering invariant documented on `from_unix` and required for correct replay-protection comparisons. [L43]
- [ ] <!-- ACT-a01ecd-1 --> **[correction · medium · small]** `rustguard-kmod/src/allowedips.rs`: Implement path compression: when creating a TrieNode during insertion, set `bit` to the actual bit index being tested at that node, and update `lookup_recursive` to jump directly to `node.bit` rather than using sequential depth. Alternatively, remove the `bit` field and update the module doc to state this is an uncompressed binary trie. [L18]
- [ ] <!-- ACT-050448-1 --> **[correction · medium · small]** `rustguard-kmod/src/timers.rs`: In is_dead() (L109), replace `self.session_established` with `self.last_received` so the dead-session check measures true inactivity (time since last received packet) rather than session age. Current code kills sessions with recent traffic once they are older than DEAD_SESSION_TIMEOUT_NS, which contradicts the documented semantics and can prematurely zero live session keys. [L109]
- [ ] <!-- ACT-050448-2 --> **[correction · medium · small]** `rustguard-kmod/src/timers.rs`: In needs_keepalive() (L121-L125), change the last_sent == 0 fallback from `now.saturating_sub(self.last_received)` to `u64::MAX`. The current fallback makes since_last_send identical to since_last_recv, making the condition `>= interval && < interval` permanently false. Using u64::MAX correctly models 'we have never sent, so infinite time has elapsed since our last send' and allows keepalives to fire as expected. [L124]
- [ ] <!-- ACT-0723df-1 --> **[correction · medium · small]** `rustguard-tun/src/uring.rs`: Replace the bare Vec<u8> backing store in BufferPool with a UnsafeCell<Vec<u8>> (or use raw allocation via alloc::alloc) so that shared mutable access by the kernel through pointers derived from &self does not violate Rust's aliasing rules. Alternatively, change slot_ptr to require &mut self to align the borrow with the mutation contract. [L55]
- [ ] <!-- ACT-0723df-2 --> **[correction · medium · small]** `rustguard-tun/src/uring.rs`: Guard pending_reads decrements with a checked subtraction or a debug_assert before subtracting 1 (e.g., `debug_assert!(self.pending_reads > 0); self.pending_reads -= 1;`) in both submit_and_wait (L194) and poll (L226) to prevent underflow from unexpected/duplicate read completions silently corrupting the refill counter. [L194]
- [ ] <!-- ACT-310cbd-1 --> **[correction · low · small]** `rustguard-crypto/src/tai64n.rs`: Replace `secs + TAI64_EPOCH_OFFSET` with `secs.saturating_add(TAI64_EPOCH_OFFSET)` (or add an explicit overflow check and return an error) to prevent undefined wrapping in release builds when an out-of-range `secs` value is passed to the public `from_unix` API. [L32]
- [ ] <!-- ACT-a01ecd-2 --> **[correction · low · small]** `rustguard-kmod/src/allowedips.rs`: Remove the two dead assignments `node.cidr = 0; node.peer_idx = peer_idx;` at lines 116–117 inside the `cidr == 0` branch of `insert()`. Only the sentinel assignments (cidr=255) should remain to avoid confusion about the actual stored state. [L116]
- [ ] <!-- ACT-a01ecd-3 --> **[correction · low · small]** `rustguard-kmod/src/allowedips.rs`: Eliminate the asymmetry between the parent pre-check and the recursive-entry check in `lookup_recursive`. Remove the pre-check block entirely (lines 176–179) — it is fully subsumed by the recursive call's own entry check — or unify both checks to handle cidr==255 identically, preventing a latent incorrect-match if a non-root node ever acquires cidr==255. [L177]
- [ ] <!-- ACT-050448-3 --> **[correction · low · small]** `rustguard-kmod/src/timers.rs`: Change REKEY_AFTER_MESSAGES (L23) from `(1u64 << 60) - 1` to `1u64 << 60` to align with the WireGuard whitepaper Section 6 and the Linux kernel reference implementation. [L23]
- [ ] <!-- ACT-4d0b70-2 --> **[correction · low · small]** `rustguard-tun/src/lib.rs`: Guard the libc::close call in Drop with `if self.fd >= 0` to avoid silently swallowing an EBADF if an invalid fd sentinel ever reaches the struct (defensive correctness for error-path robustness). [L102]
- [ ] <!-- ACT-0723df-3 --> **[correction · low · small]** `rustguard-tun/src/uring.rs`: Add a bounds check in BufferPool::free() (`assert!(idx < NUM_BUFS, ...);`) to produce a clear panic message rather than an opaque index-out-of-bounds when callers pass a stale or corrupted buf_idx. [L71]
- [ ] <!-- ACT-0723df-4 --> **[correction · low · small]** `rustguard-tun/src/uring.rs`: Consider moving the fill_reads refill call (L207, L239) to after the completions are returned to the caller (i.e., have the caller trigger a separate refill pass), or pre-free error completions inside submit_and_wait/poll before calling fill_reads, so that just-completed slots are immediately available for re-submission and the read pipeline does not stall when all buffers have been handed to the caller. [L207]
