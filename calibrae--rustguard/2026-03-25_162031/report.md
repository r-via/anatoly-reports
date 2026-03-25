<p align="center">
  <img src="https://raw.githubusercontent.com/r-via/anatoly/main/assets/imgs/logo.jpg" width="400" alt="Anatoly" />
</p>

# Anatoly Audit Report

> **41 files** reviewed in **23 min** — **$9.17** in AI analysis so you don't have to.
> Verdict: **CRITICAL** · 1 critical bug found · 84 findings in 7 files

## Axes

| Axis | Health | Findings | Details |
|------|--------|----------|---------|
| Correction | 🟩🟩🟩🟩🟩🟩🟩🟩🟩⬜ 90% OK | 5 high · 3 med | [View →](./axes/correction/index.md) |
| Utility | 🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩 100% used | All clear | — |
| Duplication | 🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩 100% unique | All clear | — |
| Overengineering | 🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩 94% lean · 6% acceptable | All clear | — |
| Tests | 🟥🟥🟥🟥🟥⬜⬜⬜⬜⬜ 46% covered | 5 high · 36 low | [View →](./axes/tests/index.md) |
| Documentation | 🟥🟥🟥🟥⬜⬜⬜⬜⬜⬜ 41% documented | 2 high · 1 med · 31 low | [View →](./axes/documentation/index.md) |
| Best Practices | 🟩🟩🟩🟩🟩🟩🟩🟩🟩⬜ avg 8.7 / 10 | 1 high | [View →](./axes/best-practices/index.md) |

## Critical Findings

- 🔴 **rustguard-core/src/replay.rs** `ReplayWindow` — shift_window (lines 109–116) applies the sub-word bit-shift in the WRONG direction with the WRONG iteration order, si...
- 🟡 **rustguard-cli/src/main.rs** `cmd_serve` — For every value-taking flag (--pool, --token, --xdp, --queues, --port), after the inner `i += 1` the code calls `args...
- 🟡 **rustguard-cli/src/main.rs** `cmd_join` — Same defect as cmd_serve: if --token is the final argument with no following value, `args.get(i).cloned().unwrap_or_d...
- 🟡 **rustguard-core/src/cookie.rs** `CookieState` — In no_std builds, process_reply (L195-L196) sets self.received = Some(now()) only under #[cfg(feature = "std")]. Ther...
- 🟡 **rustguard-core/src/timers.rs** `SessionTimers` — needs_keepalive (L154–L168) contains a self-contradictory guard when last_sent is None. The fallback `sent.unwrap_or(...
- 🟡 **rustguard-core/src/timers.rs** `REKEY_AFTER_MESSAGES` — Defined as (1u64 << 60) - 1 = 2^60 - 1, but the WireGuard whitepaper specifies REKEY-AFTER-MESSAGES = 2^60. The off-b...
- 🟡 **rustguard-core/src/timers.rs** `DEAD_SESSION_TIMEOUT` — 240 seconds is shorter than the standard derived value of REJECT_AFTER_TIME + REKEY_ATTEMPT_TIME + REKEY_TIMEOUT = 18...

> Showing top 7 of 8 correction findings. See [axes/correction/](./axes/correction/index.md) for the full list.

## Degraded Reviews

> One or more axis evaluators crashed for these files. Verdicts may be unreliable — re-run recommended.

- `rustguard-core/src/handshake.rs` — crashed: correction
- `rustguard-crypto/src/aead.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-crypto/src/blake2s.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-crypto/src/tai64n.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-crypto/src/x25519.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-daemon/src/config.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-daemon/src/peer.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-daemon/src/tunnel.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-enroll/src/client.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-enroll/src/control.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-enroll/src/fast_udp.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-enroll/src/packet.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-enroll/src/pool.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-enroll/src/protocol.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-enroll/src/server.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-enroll/src/state.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-kmod/src/allowedips.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-kmod/src/cookie.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-kmod/src/lib.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-kmod/src/noise.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-kmod/src/replay.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-kmod/src/timers.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-tun/examples/tun_echo.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-tun/src/bpf_loader.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-tun/src/lib.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-tun/src/linux_mq.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-tun/src/linux.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-tun/src/macos.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-tun/src/uring.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation
- `rustguard-tun/src/xdp.rs` — crashed: utility, duplication, correction, overengineering, tests, documentation

---

<details>
<summary><strong>Run Details</strong></summary>

Run `2026-03-25_162031` · 23.3 min · $9.17
 · 30 degraded reviews

| Axis | Calls | Duration | Cost | Tokens (in/out) |
|------|-------|----------|------|-----------------|
| utility | 9 | 3.8m | $0.34 | 81 / 35017 |
| duplication | 9 | 3.9m | $0.30 | 81 / 36263 |
| correction | 8 | 16.6m | $1.94 | 16 / 67534 |
| overengineering | 9 | 5.8m | $0.85 | 18 / 19892 |
| tests | 9 | 7.8m | $1.29 | 18 / 27958 |
| best_practices | 9 | 12.5m | $1.47 | 27 / 45407 |
| documentation | 9 | 12.1m | $2.18 | 18 / 47823 |

**Phase durations:**

| Phase | Duration |
|-------|----------|
| scan | 96ms |
| estimate | 206ms |
| triage | 3ms |
| rag-index | 828.2s |
| review | 561.3s |
| internal-docs | 3.6s |
| report | 34ms |

</details>

<details>
<summary><strong>Methodology</strong></summary>

Each file is evaluated through 7 independent axis evaluators running in parallel.
Every symbol is analysed individually with a confidence score (0–100).
Findings below 30% confidence are discarded; those below 60% are excluded from verdicts.

**Verdicts:** CLEAN (no findings) · NEEDS_REFACTOR (confirmed findings) · CRITICAL (ERROR-level bugs)

**Severity:** High = ERROR or high-confidence NEEDS_FIX/DEAD/DUPLICATE · Medium = lower confidence or OVER · Low = minor

See each axis folder for detailed rating criteria.

</details>

*Generated: 2026-03-25T16:46:15.926Z*
