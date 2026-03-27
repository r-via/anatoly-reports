[← Back to Correction](./index.md) · [← Back to report](../../public_report.md)

# 🐛 Correction — Shard 3

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)
- [🧹 Hygiene](#-hygiene)

## 📊 Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `rustguard-kmod/src/timers.rs` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#rustguard-kmodsrctimersrs) |
| `rustguard-kmod/src/allowedips.rs` | 🟡 NEEDS_REFACTOR | 2 | 85% | [details](#rustguard-kmodsrcallowedipsrs) |
| `rustguard-tun/src/uring.rs` | 🟡 NEEDS_REFACTOR | 2 | 90% | [details](#rustguard-tunsrcuringrs) |
| `rustguard-tun/src/lib.rs` | 🟡 NEEDS_REFACTOR | 1 | 88% | [details](#rustguard-tunsrclibrs) |
| `rustguard-crypto/src/tai64n.rs` | 🟡 NEEDS_REFACTOR | 1 | 87% | [details](#rustguard-cryptosrctai64nrs) |
| `rustguard-core/src/replay.rs` | 🟡 NEEDS_REFACTOR | 1 | 92% | [details](#rustguard-coresrcreplayrs) |

## 🔍 Symbol Details

### `rustguard-kmod/src/timers.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `REKEY_AFTER_MESSAGES` | L23–L23 | 🟡 NEEDS_FIX | 80% | WireGuard whitepaper Section 6 and the Linux kernel reference implementation both define REKEY_AFTER_MESSAGES as 2^60 (`1ULL << 60`). This code uses `(1u64 << 60) - 1`, which is one less than the specified value. The deviation is in the conservative direction (triggers rekeying one message earlier) and does not create a security hazard, but it is a spec violation. |
| `SessionTimers` | L36–L49 | 🟡 NEEDS_FIX | 88% | Two logic bugs in the impl block. (1) is_dead() at L107-L112 measures elapsed time from session_established, but the doc comment says '240 seconds of inactivity' — inactivity must compare against last_received. A session that received a packet 5 seconds ago but was established 250 seconds ago is incorrectly declared dead, causing premature session-key zeroing. (2) needs_keepalive() at L115-L131: when last_sent == 0 (no packet ever sent), the else branch (L124) sets since_last_send = now.saturating_sub(last_received), making since_last_send == since_last_recv. The guard `since_last_send >= keepalive_interval_ns && since_last_recv < keepalive_interval_ns` then requires x >= K && x < K simultaneously, which is always false. A peer that has received data but never sent any will never emit a keepalive regardless of how long the interval has been exceeded. |

### `rustguard-kmod/src/allowedips.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `TrieNode` | L16–L25 | 🟡 NEEDS_FIX | 85% | The `bit: u32` field is documented as 'Bit position to test (0 = MSB of first byte)' — the canonical role in a compressed radix trie where nodes skip directly to the discriminating bit. However, TrieNode::new() always initialises it to 0 and no method in the impl ever writes or reads it; routing uses an implicit `depth` counter instead. The result is an uncompressed binary trie (one node per prefix bit), contradicting the module-level claim of 'compressed radix trie'. For IPv6 /128 prefixes this creates a 128-node chain, increasing memory pressure in a kernel context and risking stack-depth issues on deep recursive traversal of densely populated tables. |
| `AllowedIps` | L28–L31 | 🟡 NEEDS_FIX | 85% | Two correctness issues in the impl block. (1) In `insert()`, the cidr==0 branch executes dead assignments `node.cidr = 0; node.peer_idx = peer_idx;` that are immediately overwritten — the resulting cidr value is 255, but the double-write is evidence of an incomplete or contradictory change that could mislead future edits into restoring the cidr=0 path and silently breaking default-route lookup (which gates on cidr==255). (2) In `lookup_recursive()`, the parent-side pre-check (line 177) explicitly handles `child.cidr == 255` and updates `best`, but the recursive-entry check (line 170) excludes cidr==255 via `node.cidr != 255`. Should any code path ever produce a non-root node with cidr==255, the pre-check would fire for it while the entry check silently ignores it, causing that node's peer to be recorded once but its children never to contribute to `best` — a latent incorrect-LPM result. |

### `rustguard-tun/src/uring.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `BufferPool` | L26–L31 | 🟡 NEEDS_FIX | 88% | slot_ptr (L54-56) obtains *const u8 via self.data.as_ptr() (from a &self borrow) and casts it to *mut u8. The kernel then writes through this pointer while Rust's aliasing model considers the backing Vec<u8> immutably borrowed. Without UnsafeCell wrapping the buffer storage, this violates Rust's strict aliasing rules and is formally UB; an aggressive optimizer may cache loads from self.data and miss kernel-written updates. Additionally, free() (L70-71) performs no bounds check on idx before indexing self.in_flight, causing a panic on any caller-supplied out-of-range index. |
| `UringTun` | L96–L102 | 🟡 NEEDS_FIX | 88% | Two issues: (1) pending_reads -= 1 at L194 and L226 (in submit_and_wait and poll respectively) has no underflow guard. pending_reads is usize, so an unexpected or duplicate read completion causes a debug-mode panic or a silent release-mode wrap to usize::MAX, after which the refill logic ((NUM_BUFS/2).saturating_sub(pending_reads)) emits zero and the read pipeline permanently stalls. (2) fill_reads is called at L207/L238 while the just-completed buffer slots are still marked in_flight=true (the caller has not yet had a chance to call bufs.free()). alloc() therefore cannot reclaim those slots for the refill; the refill can only draw from slots that were free prior to this call. Under sustained load where all 256 slots are cycling through the caller, fill_reads returns 0 and read starvation occurs until the caller frees slots and calls submit_and_wait/poll again. |

### `rustguard-tun/src/lib.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `Tun` | L25–L30 | 🟡 NEEDS_FIX | 78% | Two correctness concerns in the impl blocks that belong to this type. (1) The Drop impl at line 102 calls libc::close(self.fd) unconditionally without checking fd >= 0. If a platform-specific create() ever stores an invalid sentinel fd (e.g. -1) in the struct — e.g. on a partially-constructed error path — close(-1) silently returns EBADF and the error is swallowed, masking a real bug. (2) raw_fd() returns a bare i32 rather than BorrowedFd<'_> (std::os::unix::io::BorrowedFd). The method is explicitly documented for io_uring and AF_XDP use. Callers can trivially wrap the returned integer in an OwnedFd or equivalent owning type; when that type is dropped it calls close() on the same fd, and then Tun::drop() calls close() a second time, producing a double-close. On Linux a double-close can silently close an unrelated fd that was opened between the two closes — a data-corruption-class bug, not merely a resource leak. |

### `rustguard-crypto/src/tai64n.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `Tai64n` | L9–L9 | 🟡 NEEDS_FIX | 87% | Two correctness issues in the impl block. (1) `from_unix` at L32 computes `secs + TAI64_EPOCH_OFFSET` using plain `+`, which wraps silently in release builds or panics in debug builds for any `secs` value above ~13.8e18 — not a concern for realistic Unix timestamps, but the public API accepts arbitrary u64. `saturating_add` or an explicit overflow check is the correct fix. (2) `from_bytes` at L43-L44 accepts any 12-byte payload without validating that the nanoseconds field (bytes 8–11) is ≤ 999_999_999. The doc-comment on `from_unix` explicitly names the 999_999_999 clamp as the invariant that WireGuard's byte-wise ordering relies on; a deserialized timestamp with nanos > 999_999_999 would byte-compare as strictly greater than a legitimately-created timestamp with the same second and nanos = 999_999_999, silently breaking replay-protection ordering. |

### `rustguard-core/src/replay.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `ReplayWindow` | L13–L19 | 🟡 NEEDS_FIX | 92% | The bit-level shift inside shift_window (lines 112-116) is inverted in both direction and iteration order, causing all existing counter records to be silently erased on every window advance. The bitmap layout places newer counters at lower bit-indices (bit 0 = age 0 = current top). When the window advances by shift, every recorded counter becomes older by shift positions, so its bit-index must increase — a left shift within each word. The code instead right-shifts (*word >> bit_shift) and iterates from the highest word index down to the lowest (.rev()), which moves bits toward lower (newer) positions; the carry produced at word 0 is discarded, destroying all existing age information. Concrete trace: top=1, bitmap[0]=0b10 (counter 0 at age 1), new counter 3 arrives (shift=2). Correct result: bitmap[0] should become 0b1000 (counter 0 now at age 3). Actual result: the loop sets bitmap[0]=0 and loses counter 0 entirely, allowing it to be replayed without detection. All supplied tests pass only because none replay a counter value that was accepted before a window advance; in any real deployment this defeats the anti-replay guarantee. The fix is to iterate forward (low to high word index), left-shift (*word << bit_shift), and extract carry with *word >> (64 - bit_shift). |

## ⚡ Quick Wins

- [ ] <!-- ACT-310cbd-2 --> **[correction · medium · small]** `rustguard-crypto/src/tai64n.rs`: Add nanoseconds validation in `from_bytes`: if `u32::from_be_bytes(bytes[8..12].try_into().unwrap()) > 999_999_999`, either return a `Result::Err`, clamp the field, or otherwise reject the input — to uphold the byte-wise ordering invariant documented on `from_unix` and required for correct replay-protection comparisons. [L43]
- [ ] <!-- ACT-a01ecd-1 --> **[correction · medium · small]** `rustguard-kmod/src/allowedips.rs`: Implement path compression: when creating a TrieNode during insertion, set `bit` to the actual bit index being tested at that node, and update `lookup_recursive` to jump directly to `node.bit` rather than using sequential depth. Alternatively, remove the `bit` field and update the module doc to state this is an uncompressed binary trie. [L18]
- [ ] <!-- ACT-050448-1 --> **[correction · medium · small]** `rustguard-kmod/src/timers.rs`: In is_dead() (L109), replace `self.session_established` with `self.last_received` so the dead-session check measures true inactivity (time since last received packet) rather than session age. Current code kills sessions with recent traffic once they are older than DEAD_SESSION_TIMEOUT_NS, which contradicts the documented semantics and can prematurely zero live session keys. [L109]
- [ ] <!-- ACT-050448-2 --> **[correction · medium · small]** `rustguard-kmod/src/timers.rs`: In needs_keepalive() (L121-L125), change the last_sent == 0 fallback from `now.saturating_sub(self.last_received)` to `u64::MAX`. The current fallback makes since_last_send identical to since_last_recv, making the condition `>= interval && < interval` permanently false. Using u64::MAX correctly models 'we have never sent, so infinite time has elapsed since our last send' and allows keepalives to fire as expected. [L124]
- [ ] <!-- ACT-0723df-1 --> **[correction · medium · small]** `rustguard-tun/src/uring.rs`: Replace the bare Vec<u8> backing store in BufferPool with a UnsafeCell<Vec<u8>> (or use raw allocation via alloc::alloc) so that shared mutable access by the kernel through pointers derived from &self does not violate Rust's aliasing rules. Alternatively, change slot_ptr to require &mut self to align the borrow with the mutation contract. [L55]
- [ ] <!-- ACT-0723df-2 --> **[correction · medium · small]** `rustguard-tun/src/uring.rs`: Guard pending_reads decrements with a checked subtraction or a debug_assert before subtracting 1 (e.g., `debug_assert!(self.pending_reads > 0); self.pending_reads -= 1;`) in both submit_and_wait (L194) and poll (L226) to prevent underflow from unexpected/duplicate read completions silently corrupting the refill counter. [L194]

## 🔧 Refactors

- [ ] <!-- ACT-f55ce3-1 --> **[correction · high · large]** `rustguard-core/src/replay.rs`: Fix shift_window bit-level shift: change iteration from .rev() to forward order, replace `*word >> bit_shift` with `*word << bit_shift`, and replace carry extraction `*word << (64 - bit_shift)` with `*word >> (64 - bit_shift)`. This ensures existing counter bits advance to higher-index (older) positions rather than being destroyed on every window slide. [L112]
- [ ] <!-- ACT-4d0b70-1 --> **[correction · high · large]** `rustguard-tun/src/lib.rs`: Change raw_fd() return type from i32 to BorrowedFd<'_> (std::os::unix::io::BorrowedFd) so callers cannot wrap the fd in an owning type that will close it independently, avoiding a double-close that would silently close an unrelated fd on Linux. [L60]

## 🧹 Hygiene

- [ ] <!-- ACT-310cbd-1 --> **[correction · low · trivial]** `rustguard-crypto/src/tai64n.rs`: Replace `secs + TAI64_EPOCH_OFFSET` with `secs.saturating_add(TAI64_EPOCH_OFFSET)` (or add an explicit overflow check and return an error) to prevent undefined wrapping in release builds when an out-of-range `secs` value is passed to the public `from_unix` API. [L32]
- [ ] <!-- ACT-a01ecd-2 --> **[correction · low · trivial]** `rustguard-kmod/src/allowedips.rs`: Remove the two dead assignments `node.cidr = 0; node.peer_idx = peer_idx;` at lines 116–117 inside the `cidr == 0` branch of `insert()`. Only the sentinel assignments (cidr=255) should remain to avoid confusion about the actual stored state. [L116]
- [ ] <!-- ACT-a01ecd-3 --> **[correction · low · trivial]** `rustguard-kmod/src/allowedips.rs`: Eliminate the asymmetry between the parent pre-check and the recursive-entry check in `lookup_recursive`. Remove the pre-check block entirely (lines 176–179) — it is fully subsumed by the recursive call's own entry check — or unify both checks to handle cidr==255 identically, preventing a latent incorrect-match if a non-root node ever acquires cidr==255. [L177]
- [ ] <!-- ACT-050448-3 --> **[correction · low · trivial]** `rustguard-kmod/src/timers.rs`: Change REKEY_AFTER_MESSAGES (L23) from `(1u64 << 60) - 1` to `1u64 << 60` to align with the WireGuard whitepaper Section 6 and the Linux kernel reference implementation. [L23]
- [ ] <!-- ACT-4d0b70-2 --> **[correction · low · trivial]** `rustguard-tun/src/lib.rs`: Guard the libc::close call in Drop with `if self.fd >= 0` to avoid silently swallowing an EBADF if an invalid fd sentinel ever reaches the struct (defensive correctness for error-path robustness). [L102]
- [ ] <!-- ACT-0723df-3 --> **[correction · low · trivial]** `rustguard-tun/src/uring.rs`: Add a bounds check in BufferPool::free() (`assert!(idx < NUM_BUFS, ...);`) to produce a clear panic message rather than an opaque index-out-of-bounds when callers pass a stale or corrupted buf_idx. [L71]
- [ ] <!-- ACT-0723df-4 --> **[correction · low · trivial]** `rustguard-tun/src/uring.rs`: Consider moving the fill_reads refill call (L207, L239) to after the completions are returned to the caller (i.e., have the caller trigger a separate refill pass), or pre-free error completions inside submit_and_wait/poll before calling fill_reads, so that just-completed slots are immediately available for re-submission and the read pipeline does not stall when all buffers have been handed to the caller. [L207]
