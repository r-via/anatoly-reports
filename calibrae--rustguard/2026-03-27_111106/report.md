<p align="center">
  <img src="https://raw.githubusercontent.com/r-via/anatoly/main/assets/imgs/logo.jpg" width="400" alt="Anatoly" />
</p>

# Anatoly Audit Report

> **41 files** reviewed in **85 min** — **$68.53** in AI analysis so you don't have to.
> Verdict: **CRITICAL** · 4 critical bugs found · 472 findings in 37 files

## Axes

| Axis | Health | Findings | Details |
|------|--------|----------|---------|
| Correction | 🟩🟩🟩🟩🟩🟩🟩🟩🟩⬜ 86% OK | 39 high · 14 med | [View →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_111106/axes/correction/index.md) |
| Utility | 🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩 99% used | 3 high · 1 med | [View →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_111106/axes/utility/index.md) |
| Duplication | 🟩🟩🟩🟩🟩🟩🟩🟩🟩⬜ 94% unique | 13 high · 9 med | [View →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_111106/axes/duplication/index.md) |
| Overengineering | 🟩🟩🟩🟩🟩🟩🟩🟩🟩⬜ 93% lean | 1 high · 1 med | [View →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_111106/axes/overengineering/index.md) |
| Tests | 🟥🟥🟥🟥⬜⬜⬜⬜⬜⬜ 39% covered | 48 high · 20 med · 162 low | [View →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_111106/axes/tests/index.md) |
| Documentation | 🟨🟨🟨🟨🟨⬜⬜⬜⬜⬜ 52% documented | 24 high · 6 med · 121 low | [View →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_111106/axes/documentation/index.md) |
| Best Practices | 🟩🟩🟩🟩🟩🟩🟩🟩⬜⬜ avg 8.1 / 10 | 10 high | [View →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_111106/axes/best-practices/index.md) |

## Critical Findings

- 🔴 **rustguard-enroll/src/server.rs** `run` — Two correctness defects: (1) In the MSG_TRANSPORT handler, `decrypt_buf` is fixed at 2048 bytes, but `ct = &buf[TRANS...
- 🔴 **rustguard-kmod/src/lib.rs** `do_xmit` — wg_kfree_skb(skb) is called immediately after wg_skb_prepend_header(skb, WG_HEADER_SIZE, AEAD_TAG_SIZE) returns tx_sk...
- 🔴 **rustguard-kmod/src/replay.rs** `ReplayWindow` — Critical bug in shift_window: the bit-level carry loop iterates `for i in (0..BITMAP_LEN).rev()` — i.e. from index BI...
- 🔴 **rustguard-tun/src/xdp.rs** `XdpSocket` — Three correctness bugs found in impl methods. (1) tx_send (around L348-L352) calls copy_nonoverlapping with data.len(...
- 🟡 **rustguard-cli/src/main.rs** `cmd_serve` — Two bugs: (1) The .filter(
- 🟡 **rustguard-cli/src/main.rs** `cmd_join` — --token parsing at L172 uses the same .filter(
- 🟡 **rustguard-core/src/handshake.rs** `process_response` — The response message's mac1 field is never verified. process_initiation_with performs MAC1 verification first (before...
- 🟡 **rustguard-core/src/replay.rs** `ReplayWindow` — The bit-level shift inside shift_window (lines 112-116) is inverted in both direction and iteration order, causing al...
- 🟡 **rustguard-core/src/timers.rs** `REKEY_TIMEOUT` — The doc comment on L38 states 'Don't try to send with a keypair older than this (REJECT_AFTER_TIME + padding)' but RE...
- 🟡 **rustguard-core/src/timers.rs** `SessionTimers` — Two bugs in the impl block: (1) needs_keepalive (L160): when last_sent is None, sent.unwrap_or(received) sets last_se...

> Showing top 10 of 53 correction findings. See [axes/correction/](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_111106/axes/correction/index.md) for the full list.

## Documentation Coverage

Measures inline doc comments (`///` in Rust, `/** */` in JS/TS, docstrings in Python) on exported symbols.
Anatoly also generates reference pages in `.anatoly/docs/` for every reviewed module.

| Metric | Coverage | Description |
|--------|----------|-------------|
| Complete doc comments | 🟥⬜⬜⬜⬜⬜⬜⬜⬜⬜ 12% (16/137) | Exported symbols with a complete inline doc comment covering description, params, and return |
| Any doc comment | 🟨🟨🟨🟨🟨🟨🟨🟨⬜⬜ 77% (106/137) | Exported symbols with at least a partial inline doc comment |
| Module guides | 🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩 100% (7/7) | Modules > 200 LOC with a dedicated page in docs/ |
| Reference pages | 36 pages | Anatoly-generated module and API reference pages |

> No `docs/` directory found. Copy `.anatoly/docs/` to `docs/` to adopt the generated documentation and speed up future Anatoly runs.

**Gaps:** 1 pages to create.


## 📚 Documentation

Anatoly generated a complete documentation for this project during the audit.

**[Browse the documentation →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_111106/docs/index.md)**

---

<details>
<summary><strong>Run Details</strong></summary>

Run `2026-03-27_111106` · 85.1 min · $68.53

| Axis | Calls | Duration | Cost | Tokens (in/out) |
|------|-------|----------|------|-----------------|
| utility | 38 | 32.1m | $1.76 | 350 / 287969 |
| duplication | 38 | 21.4m | $1.48 | 350 / 193824 |
| correction | 38 | 126.2m | $15.08 | 95 / 541190 |
| overengineering | 38 | 24.2m | $3.63 | 108 / 88253 |
| tests | 38 | 27.2m | $4.21 | 79 / 98526 |
| best_practices | 38 | 71.4m | $8.15 | 114 / 269529 |
| documentation | 38 | 37.1m | $5.44 | 82 / 149903 |

**Phase durations:**

| Phase | Duration |
|-------|----------|
| scan | 215ms |
| estimate | 207ms |
| triage | 4ms |
| rag-index | 439.2s |
| review | 3460.2s |
| internal-docs | 6ms |
| report | 66ms |

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

*Generated: 2026-03-27T12:58:34.599Z*
