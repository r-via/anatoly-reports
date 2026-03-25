<p align="center">
  <img src="https://raw.githubusercontent.com/r-via/anatoly/main/assets/imgs/logo.jpg" width="400" alt="Anatoly" />
</p>

# Anatoly Audit Report

## Executive Summary

- **Files reviewed:** 41
- **Global verdict:** CRITICAL
- **Clean files:** 34
- **Files with findings:** 7
- **Degraded reviews (axis crashes):** 30

| Category | High | Medium | Low | Total |
|----------|------|--------|-----|-------|
| Correction errors | 5 | 3 | 0 | 8 |
| Test coverage gaps | 5 | 0 | 36 | 41 |
| Best practices | 1 | 0 | 0 | 1 |
| Documentation gaps | 2 | 1 | 31 | 34 |

## Documentation Reference

.anatoly/docs/ updated: 18 pages (18 refreshed)

Documentation coverage:
  Project docs (docs/): 77% (106/137 symbols)
  Internal ref (.anatoly/docs/): 91% (125/137 symbols)
  Modules: 100% (0/0 modules > 200 LOC in project docs)

Sync status: 17 pages to create

## Axes

| Axis | Files | Shards | Link |
|------|-------|--------|------|
| Correction | 4 | 1 | [axes/correction/index.md](./axes/correction/index.md) |
| Tests | 7 | 1 | [axes/tests/index.md](./axes/tests/index.md) |
| Documentation | 5 | 1 | [axes/documentation/index.md](./axes/documentation/index.md) |
| Best Practices | 1 | 1 | [axes/best-practices/index.md](./axes/best-practices/index.md) |

## Degraded Reviews

> One or more axis evaluators crashed for these files. Verdicts may be unreliable — re-run recommended.

- `rustguard-core/src/handshake.rs`
- `rustguard-crypto/src/aead.rs`
- `rustguard-crypto/src/blake2s.rs`
- `rustguard-crypto/src/tai64n.rs`
- `rustguard-crypto/src/x25519.rs`
- `rustguard-daemon/src/config.rs`
- `rustguard-daemon/src/peer.rs`
- `rustguard-daemon/src/tunnel.rs`
- `rustguard-enroll/src/client.rs`
- `rustguard-enroll/src/control.rs`
- `rustguard-enroll/src/fast_udp.rs`
- `rustguard-enroll/src/packet.rs`
- `rustguard-enroll/src/pool.rs`
- `rustguard-enroll/src/protocol.rs`
- `rustguard-enroll/src/server.rs`
- `rustguard-enroll/src/state.rs`
- `rustguard-kmod/src/allowedips.rs`
- `rustguard-kmod/src/cookie.rs`
- `rustguard-kmod/src/lib.rs`
- `rustguard-kmod/src/noise.rs`
- `rustguard-kmod/src/replay.rs`
- `rustguard-kmod/src/timers.rs`
- `rustguard-tun/examples/tun_echo.rs`
- `rustguard-tun/src/bpf_loader.rs`
- `rustguard-tun/src/lib.rs`
- `rustguard-tun/src/linux_mq.rs`
- `rustguard-tun/src/linux.rs`
- `rustguard-tun/src/macos.rs`
- `rustguard-tun/src/uring.rs`
- `rustguard-tun/src/xdp.rs`

## Performance & Triage

| Tier | Files | % |
|------|-------|---|
| Skip | 3 | 7% |
| Evaluate | 38 | 93% |

Estimated time saved: **0.3 min**

## Run Statistics

| Metric | Value |
|--------|-------|
| Run ID | `2026-03-25_162031` |
| Duration | 23.3 min |
| API cost | $9.17 |
| Degraded reviews | 30 |

**Phase durations:**

| Phase | Duration |
|-------|----------|
| scan | 96ms |
| estimate | 206ms |
| triage | 3ms |
| rag-index | 828.2s |
| review | 561.3s |
| internal-docs | 3.6s |

**Per-axis breakdown:**

| Axis | Calls | Duration | Cost | Tokens (in/out) |
|------|-------|----------|------|-----------------|
| utility | 9 | 3.8m | $0.34 | 81 / 35017 |
| duplication | 9 | 3.9m | $0.30 | 81 / 36263 |
| correction | 8 | 16.6m | $1.94 | 16 / 67534 |
| overengineering | 9 | 5.8m | $0.85 | 18 / 19892 |
| tests | 9 | 7.8m | $1.29 | 18 / 27958 |
| best_practices | 9 | 12.5m | $1.47 | 27 / 45407 |
| documentation | 9 | 12.1m | $2.18 | 18 / 47823 |

## Axis Summary

**Utility** — 80 symbols evaluated

| Verdict | Count | % |
|---------|-------|---|
| USED | 80 | 100% |

**Duplication** — 80 symbols evaluated

| Verdict | Count | % |
|---------|-------|---|
| UNIQUE | 80 | 100% |

**Correction** — 80 symbols evaluated

| Verdict | Count | % |
|---------|-------|---|
| OK | 72 | 90% |
| NEEDS_FIX | 7 | 9% |
| ERROR | 1 | 1% |

**Overengineering** — 80 symbols evaluated

| Verdict | Count | % |
|---------|-------|---|
| LEAN | 75 | 94% |
| ACCEPTABLE | 5 | 6% |

**Tests** — 76 symbols evaluated

| Verdict | Count | % |
|---------|-------|---|
| GOOD | 35 | 46% |
| WEAK | 28 | 37% |
| NONE | 13 | 17% |

**Documentation** — 76 symbols evaluated

| Verdict | Count | % |
|---------|-------|---|
| DOCUMENTED | 31 | 41% |
| PARTIAL | 26 | 34% |
| UNDOCUMENTED | 19 | 25% |

**Best Practices** — 9 files evaluated

| Metric | Value |
|--------|-------|
| Average score | 8.7/10 |
| Min / Max | 6.0 / 10.0 |

## Deliberation

| Metric | Value |
|--------|-------|
| Files deliberated | 5 |
| Symbols reclassified | 5 |
| Actions removed | 0 |
| Verdict changes | 1 |

**Verdict changes:**

- `rustguard-core/src/session.rs`: CLEAN → NEEDS_REFACTOR

**Reclassified files:**

- `rustguard-core/src/cookie.rs`: 5 symbol(s) — Overall assessment: The file is a well-structured WireGuard cookie implementation with one genuine correctness issue. CookieState's process_reply has a no_std bug where self.received is never set, causing compute_mac2 to silently return zeros even after successful cookie decryption — this is the primary NEEDS_FIX driving the NEEDS_REFACTOR verdict. Multiple UNDOCUMENTED findings on private helper functions were reclassified to DOCUMENTED under private-item leniency: these are trivial wrappers with self-descriptive names (now, elapsed_since, random_bytes, constant_time_eq_16) or already have doc comments that were overlooked (fill_random std variant). WEAK test findings are preserved where justified — verify_mac1 has zero coverage, IPv6 encoding is untested, and the no_std cookie path has no tests. All five actions remain valid: the correction action for CookieState no_std and four documentation hygiene actions for public API items.

---

## Methodology

Each file is evaluated through 7 independent axis evaluators running in parallel.
Every symbol (function, class, variable, type) is analysed individually and receives a rating per axis along with a confidence score (0–100).
Findings with confidence < 30 are discarded; those with confidence < 60 are excluded from verdict computation.

| Axis | Model | Ratings | Description |
|------|-------|---------|-------------|
| Utility | haiku | USED / DEAD / LOW_VALUE | Is this symbol actually used in the codebase? |
| Duplication | haiku | UNIQUE / DUPLICATE | Is this symbol a copy of logic that exists elsewhere? |
| Correction | sonnet | OK / NEEDS_FIX / ERROR | Does this symbol contain bugs or correctness issues? |
| Overengineering | haiku | LEAN / OVER / ACCEPTABLE | Is the implementation unnecessarily complex? |
| Tests | haiku | GOOD / WEAK / NONE | Does this symbol have adequate test coverage? |
| Best Practices | sonnet | Score 0–10 | Does the file follow Rust best practices? |
| Documentation | haiku | DOCUMENTED / PARTIAL / UNDOCUMENTED | Are exported symbols properly documented with `///` doc comments? |

See each axis folder for detailed rating criteria and methodology.

### Severity Classification

- **High**: ERROR corrections, or NEEDS_FIX / DEAD / DUPLICATE with confidence >= 80%.
- **Medium**: NEEDS_FIX / DEAD / DUPLICATE with confidence < 80%, or OVER (any confidence).
- **Low**: LOW_VALUE utility or remaining minor findings.

### Verdict Rules

- **CLEAN**: No actionable findings with confidence >= 60%.
- **NEEDS_REFACTOR**: At least one confirmed finding (DEAD, DUPLICATE, OVER, or NEEDS_FIX) with confidence >= 60%.
- **CRITICAL**: At least one ERROR correction found.

### Inter-axis Coherence

After individual evaluation, coherence rules reconcile contradictions:

- If utility = DEAD, tests is forced to NONE (no point testing dead code).
- If utility = DEAD, documentation is forced to UNDOCUMENTED (no point documenting dead code).
- If correction = ERROR, overengineering is forced to ACCEPTABLE (complexity is secondary to correctness).

*Generated: 2026-03-25T15:43:47.011Z*
