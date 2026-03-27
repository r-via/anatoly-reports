[← Back to Correction](./index.md) · [← Back to report](../../public_report.md)

# Correction — Shard 1

## Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `rustguard-kmod/src/lib.rs` | CRITICAL | 6 | 90% | [details](../reviews/rustguard-kmod-src-lib.rev.md) |
| `rustguard-tun/src/xdp.rs` | CRITICAL | 1 | 95% | [details](../reviews/rustguard-tun-src-xdp.rev.md) |
| `rustguard-enroll/src/server.rs` | CRITICAL | 1 | 92% | [details](../reviews/rustguard-enroll-src-server.rev.md) |
| `rustguard-kmod/src/replay.rs` | CRITICAL | 1 | 90% | [details](../reviews/rustguard-kmod-src-replay.rev.md) |
| `rustguard-kmod/src/noise.rs` | NEEDS_REFACTOR | 3 | 92% | [details](../reviews/rustguard-kmod-src-noise.rev.md) |
| `rustguard-core/src/handshake.rs` | NEEDS_REFACTOR | 1 | 88% | [details](../reviews/rustguard-core-src-handshake.rev.md) |
| `rustguard-core/src/cookie.rs` | NEEDS_REFACTOR | 4 | 92% | [details](../reviews/rustguard-core-src-cookie.rev.md) |
| `rustguard-daemon/src/tunnel.rs` | NEEDS_REFACTOR | 2 | 90% | [details](../reviews/rustguard-daemon-src-tunnel.rev.md) |
| `rustguard-core/src/timers.rs` | NEEDS_REFACTOR | 5 | 90% | [details](../reviews/rustguard-core-src-timers.rev.md) |
| `rustguard-enroll/src/control.rs` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/rustguard-enroll-src-control.rev.md) |

## Symbol Details

### `rustguard-kmod/src/lib.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `RustGuard` | L171–L171 | NEEDS_FIX | 80% | [USED] Module struct implementing kernel::Module trait; required for kernel m... |
| `cleanup_state` | L359–L376 | NEEDS_FIX | 88% | [USED] Internal function called from init error path (L212) and Drop (L354); ... |
| `do_xmit` | L386–L458 | ERROR | 75% | [USED] Internal TX implementation called directly from rustguard_xmit (L384);... |
| `handle_transport` | L676–L755 | NEEDS_FIX | 80% | [USED] Transport handler called from do_rx for MSG_TRANSPORT (L507); decrypts... |
| `handle_cookie_reply` | L758–L781 | NEEDS_FIX | 80% | [USED] Cookie handler called from do_rx for type 3 (L504); processes MAC2 coo... |
| `do_timer_tick` | L793–L844 | NEEDS_FIX | 82% | [USED] Called from rustguard_timer_tick (L791); iterates peers, checks expiry... |

### `rustguard-tun/src/xdp.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `XdpSocket` | L148–L160 | ERROR | 95% | [USED] Primary public struct returned from create(); used in all socket opera... |

### `rustguard-enroll/src/server.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `run` | L64–L532 | ERROR | 92% | [DEAD] Exported public function (main tunnel server loop); pre-computed analy... |

### `rustguard-kmod/src/replay.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `ReplayWindow` | L11–L14 | ERROR | 88% | [DEAD] Exported pub(crate) struct with 0 workspace importers per Pre-computed... |

### `rustguard-kmod/src/noise.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `hash` | L72–L94 | NEEDS_FIX | 80% | [USED] Not exported; called in initial_chain_key (L180), initial_hash (L184),... |
| `seal` | L124–L138 | NEEDS_FIX | 85% | [USED] Not exported; called in encrypt_and_hash() at L198 for AEAD encryption... |
| `open` | L141–L155 | NEEDS_FIX | 88% | [USED] Not exported; called in decrypt_and_hash() at L204 for AEAD decryption... |

### `rustguard-core/src/handshake.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `process_response` | L173–L221 | NEEDS_FIX | 80% | [USED] Exported function with 4 runtime importers; processes response msg2 an... |

### `rustguard-core/src/cookie.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `elapsed_since` | L38–L40 | NEEDS_FIX | 70% | [USED] no_std stub version called in 2 places: maybe_rotate_secret and is_fre... |
| `elapsed_since` | L51–L55 | NEEDS_FIX | 70% | [USED] no_std stub version called in 2 places: maybe_rotate_secret and is_fre... |
| `fill_random` | L270–L272 | NEEDS_FIX | 72% | [USED] no_std stub version called in 2 places: random_bytes and random_nonce ... |
| `fill_random` | L275–L280 | NEEDS_FIX | 72% | [USED] no_std stub version called in 2 places: random_bytes and random_nonce ... |

### `rustguard-daemon/src/tunnel.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `run` | L48–L489 | NEEDS_FIX | 90% | [DEAD] exported pub fn but pre-computed analysis shows 0 runtime importers | ... |
| `ctrlc_handler` | L504–L525 | NEEDS_FIX | 80% | [USED] called at L118 to setup shutdown signal handler | [UNIQUE] Platform si... |

### `rustguard-core/src/timers.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `elapsed_since` | L21–L23 | NEEDS_FIX | 75% | [USED] Non-exported no_std fallback function compiled conditionally; provides... |
| `elapsed_since` | L26–L30 | NEEDS_FIX | 75% | [USED] Non-exported no_std fallback function compiled conditionally; provides... |
| `REKEY_TIMEOUT` | L39–L39 | NEEDS_FIX | 88% | [USED] Exported constant with 1 runtime importer (rustguard-daemon/src/peer.r... |
| `REKEY_AFTER_MESSAGES` | L42–L42 | NEEDS_FIX | 72% | [USED] Exported constant with 1 runtime importer; also used locally in needs_... |
| `SessionTimers` | L54–L65 | NEEDS_FIX | 90% | [USED] Exported struct with 1 runtime importer (rustguard-daemon/src/peer.rs)... |

### `rustguard-enroll/src/control.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `start_listener` | L77–L104 | NEEDS_FIX | 78% | [USED] Exported, runtime-imported by rustguard-enroll/src/server.rs. Substant... |

## Quick Wins

- [ ] <!-- ACT-a4f709-1 --> **[correction · high · small]** `rustguard-core/src/cookie.rs`: Gate create_reply_from_bytes() with #[cfg(feature = "std")] or add a caller-supplied nonce parameter for no_std callers, so the no_std stub fill_random cannot silently produce a constant zero nonce that reuses the same XChaCha20-Poly1305 keystream for every Cookie Reply. [L275]
- [ ] <!-- ACT-fb2f2d-1 --> **[correction · high · small]** `rustguard-core/src/timers.rs`: Add checker methods that accept a current-time parameter in no_std builds (e.g., needs_rekey_at(send_counter, now_ns), is_expired_at, is_dead_at, should_retry_handshake_at) so time-based session expiry is actually usable without std. The current design leaves all time checks silently dead in no_std, including the critical is_expired path. [L26]
- [ ] <!-- ACT-a732f2-1 --> **[correction · high · small]** `rustguard-enroll/src/server.rs`: MSG_TRANSPORT handler: add a bounds check `if ct.len() > decrypt_buf.len() { continue; }` before `decrypt_buf[..ct.len()].copy_from_slice(ct)`. Without this check any UDP datagram (or XDP frame up to 4096 bytes) with payload > 2048 bytes panics the inbound thread, taking down the server. [L447]
- [ ] <!-- ACT-bb41a6-1 --> **[correction · high · small]** `rustguard-kmod/src/lib.rs`: In do_xmit, determine the ownership contract of wg_skb_prepend_header. If it operates in-place (returns same pointer), remove wg_kfree_skb(skb) after the call — the single skb is later freed as tx_skb. If it always allocates a new skb, document that the old skb is never freed internally and the explicit free is intentional. The current pattern produces either a use-after-free or double-free depending on the C implementation. [L415]
- [ ] <!-- ACT-c30836-1 --> **[correction · high · small]** `rustguard-kmod/src/replay.rs`: shift_window bit-carry loop direction is reversed. `for i in (0..BITMAP_LEN).rev()` propagates carry from higher-index words toward lower-index words, which is backwards: left-shifting sends overflow from word[i] bit 63 into word[i+1] bit 0, so carry must travel low→high. Change the loop to `for i in 0..BITMAP_LEN` (forward). The final carry value after the loop is correctly discarded — those are bits that fall off the oldest end of the window. Without this fix, 'seen' bits at the high end of any 64-bit word are silently dropped on every window advance, breaking replay-attack detection for affected counter values. [L91]
- [ ] <!-- ACT-8fc69f-1 --> **[correction · high · small]** `rustguard-tun/src/xdp.rs`: tx_send copies data.len() bytes into a UMEM frame with no bounds check against self.frame_size. If data.len() > frame_size the write overflows the allocated frame and corrupts adjacent UMEM memory visible to the kernel. Add a guard: if data.len() > self.frame_size as usize { self.free_frames.push(frame_addr); return false; } before the copy. [L348]
- [ ] <!-- ACT-a4f709-2 --> **[correction · medium · small]** `rustguard-core/src/cookie.rs`: Document or enforce via a cfg gate that CookieState::is_fresh() and CookieState::compute_mac2() must not be used in no_std mode without real timekeeping, or provide a no_std-compatible elapsed_since that takes a real clock input, since the current stub causes all stored cookies to appear permanently fresh. [L51]
- [ ] <!-- ACT-5920d4-1 --> **[correction · medium · small]** `rustguard-core/src/handshake.rs`: Add MAC1 verification in process_response before performing any DH operations. Compute expected_mac1 = compute_mac1(&our_static.public_key(), &msg.to_bytes()[..60]) and return None if it doesn't match msg.mac1 via constant_time_eq. This mirrors the guard already present in process_initiation_with and prevents a DDoS vector where an attacker triggers two DH scalar multiplications per forged response packet. [L189]
- [ ] <!-- ACT-fb2f2d-2 --> **[correction · medium · small]** `rustguard-core/src/timers.rs`: Fix needs_keepalive: replace `sent.unwrap_or(received)` with an explicit sentinel — when last_sent is None, treat elapsed time since last send as Duration::MAX so that a peer which has received data but never sent anything does trigger keepalives as expected. [L160]
- [ ] <!-- ACT-bd20cf-1 --> **[correction · medium · small]** `rustguard-daemon/src/tunnel.rs`: MSG_RESPONSE peer lookup uses 'p.endpoint.is_some() && !p.has_active_session()' as its effective matching condition, returning the first peer that has no active session rather than the peer that actually sent the handshake initiation. In any multi-peer configuration this silently assigns the new session to the wrong peer. Fix by adding the peer's index or public key as a fourth element of the pending_handshakes tuple so the response can be correlated exactly, eliminating the broad endpoint-based fallback. [L288]
- [ ] <!-- ACT-bd20cf-2 --> **[correction · medium · small]** `rustguard-daemon/src/tunnel.rs`: ctrlc_handler calls sigprocmask(SIG_BLOCK) only inside the spawned handler thread. All other threads (main, outbound, inbound, timer) inherit an unblocked signal mask from the main thread, so SIGINT/SIGTERM delivered to the process can be consumed by signal_noop in any of those threads before sigwait ever observes it, silently preventing clean shutdown. Fix by calling pthread_sigmask/sigprocmask in the main thread before any threads are spawned so the block is inherited by all children. [L519]
- [ ] <!-- ACT-18d76d-1 --> **[correction · medium · small]** `rustguard-enroll/src/control.rs`: Set a read timeout on the accepted UnixStream before passing it to handle_client (e.g., `stream.set_read_timeout(Some(Duration::from_secs(30)))?`) or spawn a new thread per connection. Without this, a client that connects but never writes (or never closes) will permanently block the single listener thread, making the control socket unresponsive to all subsequent commands for the life of the server process. [L99]
- [ ] <!-- ACT-a732f2-2 --> **[correction · medium · small]** `rustguard-enroll/src/server.rs`: MSG_INITIATION handler: store the returned TAI64N timestamp `_ts` in `PeerState` (e.g., `ps.timers.latest_handshake_ts`) and reject initiations whose timestamp is not strictly greater than the stored value. Discarding `_ts` removes WireGuard's mandated replay protection for handshake initiation messages. [L413]
- [ ] <!-- ACT-bb41a6-2 --> **[correction · medium · small]** `rustguard-kmod/src/lib.rs`: In RustGuard::init, add wg_crypto_exit() to the wg_queue_init failure path, and add both wg_queue_destroy() and wg_crypto_exit() to the KBox allocation failure, wg_create_device failure, and wg_socket_create failure paths to prevent subsystem resource leaks on module load failure. [L196]
- [ ] <!-- ACT-bb41a6-3 --> **[correction · medium · small]** `rustguard-kmod/src/lib.rs`: In do_timer_tick, drop the outer 'peer: &mut Peer' borrow before calling initiate_rekey() and send_keepalive(), then re-acquire it afterward if needed. Both callees create a second &mut Peer alias to the same slot, which is undefined behavior under Rust's aliasing rules. [L819]
- [ ] <!-- ACT-bb41a6-4 --> **[correction · medium · small]** `rustguard-kmod/src/lib.rs`: In handle_cookie_reply, store the MAC1 computed for the last outgoing handshake initiation per peer and pass it instead of [0u8;16]. Without the correct MAC1 the cookie reply cannot be authenticated, breaking DoS protection and causing every MAC2 retry to fail. [L777]
- [ ] <!-- ACT-58e5ff-2 --> **[correction · medium · small]** `rustguard-kmod/src/noise.rs`: In open(): add a guard `if ciphertext.len() < AEAD_TAG_SIZE { return None; }` before computing `ciphertext.len() - AEAD_TAG_SIZE`. Without it, an undersized ciphertext causes a usize underflow: panic (kernel oops) in debug builds, or a wrap-around length returned to the caller in release builds. [L151]
- [ ] <!-- ACT-58e5ff-3 --> **[correction · medium · small]** `rustguard-kmod/src/noise.rs`: In seal(): add a bounds check `if dst.len() < plaintext.len() + AEAD_TAG_SIZE { return None; }` before entering the unsafe block. Without it, passing an undersized dst causes the C function to write beyond the slice boundary, corrupting adjacent kernel memory. [L125]
- [ ] <!-- ACT-8fc69f-2 --> **[correction · medium · small]** `rustguard-tun/src/xdp.rs`: Drop only calls munmap on self.umem. The four ring mmap regions (rx, tx, fill, completion) returned from mmap_ring are leaked on every drop because make_ring discards the base pointer and no size is retained. Store the base pointer and size for each of the four rings in XdpSocket and unmap them in Drop. [L400]
- [ ] <!-- ACT-8fc69f-3 --> **[correction · medium · small]** `rustguard-tun/src/xdp.rs`: The four setsockopt calls at lines 217-220 that configure ring sizes silently discard their i32 return values. A failure leaves the kernel-side ring sizes at their defaults while the code proceeds to mmap rings using offsets from getsockopt, resulting in mismatched ring geometry and silent memory corruption. Each call must be checked and the error propagated with fd/umem cleanup. [L217]
- [ ] <!-- ACT-8fc69f-4 --> **[correction · medium · small]** `rustguard-tun/src/xdp.rs`: In create_inner, if the second, third, or fourth mmap_ring call fails (lines 241-243), all previously successful ring mmaps are leaked because no cleanup is performed before the ? operator returns the error. Introduce a cleanup guard or explicit munmap calls for each successfully mapped ring on all error paths. [L241]
- [ ] <!-- ACT-fb2f2d-3 --> **[correction · low · small]** `rustguard-core/src/timers.rs`: Reset self.last_sent to None inside session_started and session_started_at so stale send timestamps from the previous session do not carry over and suppress keepalive generation in the new session. [L80]
- [ ] <!-- ACT-fb2f2d-4 --> **[correction · low · small]** `rustguard-core/src/timers.rs`: Correct the doc comment on REKEY_TIMEOUT (L38): the constant is the delay between handshake retry attempts, not a keypair age limit. The current comment referencing REJECT_AFTER_TIME is factually wrong and misleading. [L38]
- [ ] <!-- ACT-fb2f2d-5 --> **[correction · low · small]** `rustguard-core/src/timers.rs`: Change REKEY_AFTER_MESSAGES to (1u64 << 60) to match the WireGuard spec, or add an explicit comment documenting the intentional one-message-early conservative deviation. [L42]
- [ ] <!-- ACT-bd20cf-3 --> **[correction · low · small]** `rustguard-daemon/src/tunnel.rs`: The UDP recv_from error handler only continues (suppresses) io::ErrorKind::WouldBlock. On Windows, a read timeout set via set_read_timeout fires with io::ErrorKind::TimedOut, causing the else branch to log a spurious 'UDP recv error' on every 500 ms poll cycle. Fix: add TimedOut to the continue arm: 'Err(ref e) if e.kind() == io::ErrorKind::WouldBlock || e.kind() == io::ErrorKind::TimedOut => continue'. [L232]
- [ ] <!-- ACT-bb41a6-5 --> **[correction · low · small]** `rustguard-kmod/src/lib.rs`: In cleanup_state, add zeroization of peer.prev_session.key_send and peer.prev_session.key_recv for each peer, and zeroize ephemeral key material in peer.pending_handshake before the KBox is dropped. [L369]
- [ ] <!-- ACT-bb41a6-6 --> **[correction · low · small]** `rustguard-kmod/src/lib.rs`: In RustGuard::init, check the return value of wg_timer_start(); if it fails, log an error (or return Err) since all rekeying and keepalive processing will be silently disabled, causing sessions to expire irreversibly. [L289]
- [ ] <!-- ACT-bb41a6-7 --> **[correction · low · small]** `rustguard-kmod/src/lib.rs`: In handle_transport, replace the hard-coded 2048-byte stack buffer with a heap allocation sized to the net_device MTU plus AEAD_TAG_SIZE, or raise the static limit and enforce it against the configured MTU. Packets with plaintext larger than 2032 bytes are currently silently dropped. [L708]
- [ ] <!-- ACT-58e5ff-1 --> **[correction · low · small]** `rustguard-kmod/src/noise.rs`: In hash(): when chunks.len() > 8, the function silently drops excess chunks and returns a wrong hash. Add `debug_assert!(chunks.len() <= 8)` or return an explicit error/panic so callers discover misuse immediately rather than receiving a silently incorrect hash value. [L74]
