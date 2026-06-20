<p align="center">
  <img src="https://raw.githubusercontent.com/r-via/anatoly/main/assets/imgs/logo.jpg" width="400" alt="Anatoly" />
</p>

# Anatoly Audit Report

> **369 files** reviewed in **137 min** — **$52.00** in AI analysis so you don't have to.
> Verdict: **CRITICAL** · 2 critical bugs found · 2292 findings in 314 files · ⚠️ 2 axis evaluators crashed (Duplication, Best Practices) — re-run recommended · ⚠️ partial run: 6 axis skipped via --axes (Duplication, Overengineering, Tests, Documentation, Best Practices, Flow) — re-run without filter for full coverage

## Axes

| Axis | Health | Findings | Details |
|------|--------|----------|---------|
| Correction | 🟨🟨🟨🟨🟨🟨🟨🟨🟨🟨 98% OK | 2 critical · 52 high · 24 med | [View →](./axes/correction/index.md) |
| Utility | 🟥🟥🟥🟥⬜⬜⬜⬜⬜⬜ 37% used | 2013 high · 201 med | [View →](./axes/utility/index.md) |
| Duplication | ⬜⬜⬜⬜⬜⬜⬜⬜⬜⬜ not in this run | — | — |
| Overengineering | ⬜⬜⬜⬜⬜⬜⬜⬜⬜⬜ not in this run | — | — |
| Tests | ⬜⬜⬜⬜⬜⬜⬜⬜⬜⬜ not in this run | — | — |
| Documentation | ⬜⬜⬜⬜⬜⬜⬜⬜⬜⬜ not in this run | — | — |
| Best Practices | ⬜⬜⬜⬜⬜⬜⬜⬜⬜⬜ not in this run | — | — |
| Flow | ⬜⬜⬜⬜⬜⬜⬜⬜⬜⬜ not in this run | — | — |

## Top Findings

### 🐛 Correction

> Showing top 5 of 78 findings. [View all →](./axes/correction/index.md)

- 🔴 **internal/infrastructure/audit/file_writer.go** `truncateInputs` — Infinite loop: keys are never deleted from the map so len(event.Inputs) never reaches 0; once all values are '…' (3 b...
- 🔴 **internal/interfaces/tui/command.go** `buildBridge` — Two early-exit paths return nil *TUIInputReader with nil error, causing a nil-pointer panic in runTUI at inputReader....
- 🟡 **internal/application/template_service.go** `SelectPrimaryStep` — Non-deterministic primary step selection: falls back to an arbitrary map element when no state matches the template n...
- 🟡 **internal/infrastructure/agents/helpers.go** `truncateArg` — truncLen=37 + one ellipsis rune = 38 visible chars, contradicting the documented contract of exactly 40; truncLen sho...
- 🟡 **internal/infrastructure/workflowpkg/types.go** `parseTimeValue` — Uses time.RFC3339 to parse strings produced by time.Time.MarshalJSON, which emits RFC3339Nano; parse silently returns...

### ♻️ Utility

> Showing top 5 of 2216 findings. [View all →](./axes/utility/index.md)

- 🔴 **internal/domain/operation/operation.go** `Operation` — Exported but imported by 0 files
- 🔴 **internal/domain/ports/agent_output_normalizer.go** `AgentOutputNormalizer` — Exported but imported by 0 files
- 🔴 **internal/domain/ports/agent_provider.go** `AgentProvider` — Exported but imported by 0 files
- 🔴 **internal/domain/ports/agent_provider.go** `AgentRegistry` — Exported but imported by 0 files
- 🔴 **internal/domain/ports/agent_role_repository.go** `AgentRoleRepository` — Exported but imported by 0 files

### 📋 Duplication

⚪ **NOT TESTED** — axis skipped via --axes filter. Re-run without --axes to evaluate.

### 🏗️ Overengineering

⚪ **NOT TESTED** — axis skipped via --axes filter. Re-run without --axes to evaluate.

### 🧪 Tests

⚪ **NOT TESTED** — axis skipped via --axes filter. Re-run without --axes to evaluate.

### 📝 Documentation

⚪ **NOT TESTED** — axis skipped via --axes filter. Re-run without --axes to evaluate.

### ✅ Best Practices

⚪ **NOT TESTED** — axis skipped via --axes filter. Re-run without --axes to evaluate.

### 🔀 Flow

⚪ **NOT TESTED** — axis skipped via --axes filter. Re-run without --axes to evaluate.

## Documentation Coverage

Measures inline doc comments (`///` in Rust, `/** */` in JS/TS, docstrings in Python) on exported symbols.

**Project docs (docs/):**

| Metric | Coverage | Description |
|--------|----------|-------------|
| Complete doc comments | ⬜⬜⬜⬜⬜⬜⬜⬜⬜⬜ 0% (0/2305) | Exported symbols with a complete inline doc comment covering description, params, and return |
| Any doc comment | ⬜⬜⬜⬜⬜⬜⬜⬜⬜⬜ 0% (0/2305) | Exported symbols with at least a partial inline doc comment |
| Module guides | ⬜⬜⬜⬜⬜⬜⬜⬜⬜⬜ 0% (0/2) | Modules > 200 LOC with a dedicated page in docs/ |

**Internal docs (.anatoly/docs/):**

| Metric | Coverage | Description |
|--------|----------|-------------|
| Reference pages | 0 pages | Anatoly-generated module and API reference pages |
| Module guides | ⬜⬜⬜⬜⬜⬜⬜⬜⬜⬜ 0% (0/2) | Modules > 200 LOC with a page in .anatoly/docs/ |

> Check the internal Anatoly docs in `.anatoly/docs/` or simply replace your current `docs/` with the internal docs to speed up future Anatoly runs.


## Degraded Reviews

> One or more axis evaluators crashed for these files. Verdicts may be unreliable — re-run recommended.

- `internal/application/acp_session_service.go` — crashed: duplication
- `internal/application/execution_service.go` — crashed: duplication
- `internal/application/loop_executor.go` — crashed: duplication

---

<details>
<summary><strong>Run Details</strong></summary>

Run `2026-06-20_083708` · 136.9 min · $52.00
 · 3 degraded reviews

| Axis | Calls | Duration | Cost | Tokens (in/out) |
|------|-------|----------|------|-----------------|
| utility | 345 | 135.6m | $12.87 | 600 / 594819 |
| correction | 342 | 317.3m | $24.32 | 1029 / 1194298 |

**Phase durations:**

| Phase | Duration |
|-------|----------|
| scan | 4.9s |
| estimate | 953ms |
| triage | 14ms |
| rag-index | 2072.9s |
| flows | 333ms |
| flow-doc | 29.7s |
| review | 3854.5s |
| invariants | 73.4s |
| refinement | 2170.9s |

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

*Generated: 2026-06-20T08:54:06.751Z*
