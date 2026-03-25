# Correction — Shard 1

## Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `rustguard-core/src/replay.rs` | CRITICAL | 1 | 92% | [details](../reviews/rustguard-core-src-replay.rev.md) |
| `rustguard-cli/src/main.rs` | NEEDS_REFACTOR | 2 | 88% | [details](../reviews/rustguard-cli-src-main.rev.md) |
| `rustguard-core/src/timers.rs` | NEEDS_REFACTOR | 4 | 95% | [details](../reviews/rustguard-core-src-timers.rev.md) |
| `rustguard-core/src/cookie.rs` | NEEDS_REFACTOR | 1 | 92% | [details](../reviews/rustguard-core-src-cookie.rev.md) |

## Symbol Details

### `rustguard-core/src/replay.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `ReplayWindow` | L13–L19 | ERROR | 92% | [USED] Exported pub struct with 1 runtime importer (rustguard-core/src/sessio... |

### `rustguard-cli/src/main.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `cmd_serve` | L68–L152 | NEEDS_FIX | 88% | [USED] cmd_serve() invoked on line 14 when 'serve' command is matched in main... |
| `cmd_join` | L154–L199 | NEEDS_FIX | 88% | [USED] cmd_join() invoked on line 15 when 'join' command is matched in main's... |

### `rustguard-core/src/timers.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `REKEY_TIMEOUT` | L39–L39 | NEEDS_FIX | 55% | [USED] Exported constant with 1 runtime importer (rustguard-daemon/src/peer.r... |
| `REKEY_AFTER_MESSAGES` | L42–L42 | NEEDS_FIX | 65% | [USED] Exported constant with 1 runtime importer (rustguard-daemon/src/peer.r... |
| `DEAD_SESSION_TIMEOUT` | L54–L54 | NEEDS_FIX | 60% | [USED] Exported constant with 1 runtime importer (rustguard-daemon/src/peer.r... |
| `SessionTimers` | L57–L68 | NEEDS_FIX | 92% | [USED] Exported pub struct with 1 runtime importer (rustguard-daemon/src/peer... |

### `rustguard-core/src/cookie.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `CookieState` | L60–L65 | NEEDS_FIX | 88% | [USED] Primary client-side public API in library crate; exports cookie decryp... |

## Quick Wins

- [ ] <!-- ACT-f55ce3-1 --> **[correction · high · small]** `rustguard-core/src/replay.rs`: Fix shift_window's sub-word bit-shift loop: remove .rev() so the loop iterates from bitmap[0] to bitmap[31] (low to high), change the shift direction from `*word >> bit_shift` to `*word << bit_shift`, and change the carry computation from `*word << (64 - bit_shift)` to `*word >> (64 - bit_shift)`. The existing word-level shift (copy_within) already moves data toward higher indices (correct); the bit-level shift must do the same to avoid silently erasing bits for recently-seen counters, which would allow replay attacks. [L112]
- [ ] <!-- ACT-a4f709-1 --> **[correction · medium · small]** `rustguard-core/src/cookie.rs`: In no_std builds, CookieState::process_reply never sets self.received, so is_fresh() always returns false and compute_mac2 always returns zeros even after a valid cookie is stored. Add a #[cfg(not(feature = "std"))] block inside the successful match arm of process_reply that sets self.received = Some(0u64) (or any sentinel), and adjust is_fresh() to treat a zero-timestamp Timestamp as unconditionally fresh under no_std (consistent with elapsed_since returning Duration::ZERO). [L196]
- [ ] <!-- ACT-fb2f2d-4 --> **[correction · medium · small]** `rustguard-core/src/timers.rs`: Fix needs_keepalive: when last_sent is None and last_received is recent (elapsed_since(received) < interval), a keepalive should be sent immediately. Replace `sent.unwrap_or(received)` with a branch — e.g., `let Some(last_send_time) = sent else { return elapsed_since(received) < interval; };` — so the no-send-yet case is handled correctly instead of producing an impossible condition. [L163]
- [ ] <!-- ACT-8bab2a-1 --> **[correction · low · small]** `rustguard-cli/src/main.rs`: In cmd_serve, replace `args.get(i).cloned().unwrap_or_default()` for each value-taking flag with an explicit check: if args.get(i) is None, print a specific 'flag requires a value' error and call process::exit(1). This prevents empty strings from silently propagating as pool CIDR or auth token values. [L82]
- [ ] <!-- ACT-8bab2a-2 --> **[correction · low · small]** `rustguard-cli/src/main.rs`: In cmd_join, replace `args.get(i).cloned().unwrap_or_default()` for --token with an explicit missing-value check and early exit, preventing an empty token string from being passed to rustguard_enroll::client::run. [L163]
- [ ] <!-- ACT-fb2f2d-1 --> **[correction · low · small]** `rustguard-core/src/timers.rs`: Fix misleading REKEY_TIMEOUT doc comment: replace 'REJECT_AFTER_TIME + padding' with 'interval between handshake retries per WireGuard spec section 6.3'. [L39]
- [ ] <!-- ACT-fb2f2d-2 --> **[correction · low · small]** `rustguard-core/src/timers.rs`: Correct REKEY_AFTER_MESSAGES to match whitepaper: change `(1u64 << 60) - 1` to `1u64 << 60` (or add an explicit comment if the off-by-one is intentional for wireguard-go compatibility). [L42]
- [ ] <!-- ACT-fb2f2d-3 --> **[correction · low · small]** `rustguard-core/src/timers.rs`: Reconcile DEAD_SESSION_TIMEOUT with the standard REJECT_AFTER_TIME + REKEY_ATTEMPT_TIME + REKEY_TIMEOUT = 275s, or document why 240s is intentionally shorter to avoid premature key zeroing during an active handshake retry window. [L54]
