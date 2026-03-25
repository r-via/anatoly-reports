# Documentation — Shard 9

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `src/utils/lock.ts` | NEEDS_REFACTOR | 1 | 92% | [details](../reviews/src-utils-lock.rev.md) |
| `src/commands/estimate.ts` | NEEDS_REFACTOR | 1 | 90% | [details](../reviews/src-commands-estimate.rev.md) |
| `src/commands/init.ts` | NEEDS_REFACTOR | 1 | 90% | [details](../reviews/src-commands-init.rev.md) |
| `src/commands/status.ts` | NEEDS_REFACTOR | 1 | 90% | [details](../reviews/src-commands-status.rev.md) |
| `src/commands/watch.ts` | NEEDS_REFACTOR | 1 | 90% | [details](../reviews/src-commands-watch.rev.md) |
| `src/commands/clean-runs.ts` | NEEDS_REFACTOR | 1 | 88% | [details](../reviews/src-commands-clean-runs.rev.md) |
| `src/commands/review.ts` | NEEDS_REFACTOR | 1 | 88% | [details](../reviews/src-commands-review.rev.md) |
| `src/commands/scan.ts` | NEEDS_REFACTOR | 1 | 88% | [details](../reviews/src-commands-scan.rev.md) |
| `src/core/docs-guard.ts` | NEEDS_REFACTOR | 1 | 88% | [details](../reviews/src-core-docs-guard.rev.md) |
| `src/utils/process.ts` | NEEDS_REFACTOR | 1 | 88% | [details](../reviews/src-utils-process.rev.md) |

## Symbol Details

### `src/utils/lock.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `acquireLock` | L20–L54 | PARTIAL | 85% | [PARTIAL] JSDoc covers the primary purpose, the LOCK_EXISTS error case, and s... |

### `src/commands/estimate.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `registerEstimateCommand` | L22–L137 | UNDOCUMENTED | 90% | [UNDOCUMENTED] Exported public function with no JSDoc/TSDoc comment whatsoeve... |

### `src/commands/init.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `registerInitCommand` | L30–L48 | UNDOCUMENTED | 90% | [UNDOCUMENTED] Exported public API function with no JSDoc comment. While the ... |

### `src/commands/status.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `registerStatusCommand` | L14–L106 | UNDOCUMENTED | 90% | [UNDOCUMENTED] Exported public function with no JSDoc/TSDoc comment. Its purp... |

### `src/commands/watch.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `registerWatchCommand` | L28–L292 | UNDOCUMENTED | 90% | [UNDOCUMENTED] No JSDoc/TSDoc comment precedes the exported function. Key beh... |

### `src/commands/clean-runs.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `registerCleanRunsCommand` | L13–L23 | UNDOCUMENTED | 88% | [UNDOCUMENTED] Exported public function with no JSDoc comment. The function n... |

### `src/commands/review.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `registerReviewCommand` | L28–L221 | UNDOCUMENTED | 88% | [UNDOCUMENTED] No JSDoc/TSDoc comment precedes this exported function. Given ... |

### `src/commands/scan.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `registerScanCommand` | L13–L51 | UNDOCUMENTED | 88% | [UNDOCUMENTED] Exported public function with no JSDoc/TSDoc comment. It regis... |

### `src/core/docs-guard.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `assertSafeOutputPath` | L24–L36 | PARTIAL | 88% | [PARTIAL] JSDoc covers purpose, @throws, and two of the three parameters (@pa... |

### `src/utils/process.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `isProcessRunning` | L8–L17 | PARTIAL | 88% | [PARTIAL] Has a one-line JSDoc describing purpose, but omits the non-trivial ... |

## Hygiene

- [ ] <!-- ACT-352c74-1 --> **[documentation · medium · trivial]** `src/commands/clean-runs.ts`: Add JSDoc documentation for exported symbol: `registerCleanRunsCommand` (`registerCleanRunsCommand`) [L13-L23]
- [ ] <!-- ACT-3c568e-1 --> **[documentation · medium · trivial]** `src/commands/estimate.ts`: Add JSDoc documentation for exported symbol: `registerEstimateCommand` (`registerEstimateCommand`) [L22-L137]
- [ ] <!-- ACT-ec8485-1 --> **[documentation · medium · trivial]** `src/commands/init.ts`: Add JSDoc documentation for exported symbol: `registerInitCommand` (`registerInitCommand`) [L30-L48]
- [ ] <!-- ACT-1dced3-1 --> **[documentation · medium · trivial]** `src/commands/review.ts`: Add JSDoc documentation for exported symbol: `registerReviewCommand` (`registerReviewCommand`) [L28-L221]
- [ ] <!-- ACT-479a90-1 --> **[documentation · medium · trivial]** `src/commands/scan.ts`: Add JSDoc documentation for exported symbol: `registerScanCommand` (`registerScanCommand`) [L13-L51]
- [ ] <!-- ACT-87bc87-1 --> **[documentation · medium · trivial]** `src/commands/status.ts`: Add JSDoc documentation for exported symbol: `registerStatusCommand` (`registerStatusCommand`) [L14-L106]
- [ ] <!-- ACT-0b5cda-1 --> **[documentation · medium · trivial]** `src/commands/watch.ts`: Add JSDoc documentation for exported symbol: `registerWatchCommand` (`registerWatchCommand`) [L28-L292]
- [ ] <!-- ACT-e54689-1 --> **[documentation · low · trivial]** `src/core/docs-guard.ts`: Complete JSDoc documentation for: `assertSafeOutputPath` (`assertSafeOutputPath`) [L24-L36]
- [ ] <!-- ACT-4b1743-1 --> **[documentation · low · trivial]** `src/utils/lock.ts`: Complete JSDoc documentation for: `acquireLock` (`acquireLock`) [L20-L54]
- [ ] <!-- ACT-b30a76-1 --> **[documentation · low · trivial]** `src/utils/process.ts`: Complete JSDoc documentation for: `isProcessRunning` (`isProcessRunning`) [L8-L17]

## Documentation Coverage

### `src/utils/lock.ts` — 20% covered

- [ ] **Lock-based concurrency control (acquireLock / releaseLock)** — MISSING: No documentation page in the docs/ tree appears to cover the lock file mechanism used to prevent concurrent Anatoly instances. This is an internal lifecycle concern, but it affects users who encounter LOCK_EXISTS errors or need to understand `anatoly reset`. The CLI reference (03-CLI-Reference/01-Commands.md) may mention `reset` but likely not the lock internals.
- [ ] **Hook coordination via isLockActive** — PARTIAL → `05-Integration/01-Claude-Code-Hooks.md`: The JSDoc for isLockActive explicitly names 'hook coordination' as the intended use-case, strongly implying this utility is referenced in the hooks integration guide. The hooks doc likely describes how hooks check for an active Anatoly run, but whether it documents the lock-check mechanism specifically (vs. just the hook behavior) is uncertain without reading the file content.

### `src/commands/init.ts` — 55% covered

- [ ] **init command** — PARTIAL → `03-CLI-Reference/01-Commands.md`: The docs directory contains '03-CLI-Reference/01-Commands.md' and '04-API-Reference/04-CLI-Reference.md', which are the expected locations for the init command. However, the actual file contents were not provided, so coverage cannot be fully verified. The presence of a CLI reference section suggests the command is at least partially documented, but the --force flag behavior and the commented-out defaults rationale may not be covered. Confidence is reduced due to inability to inspect file content.

### `src/commands/watch.ts` — 75% covered

- [ ] **CLI watch command reference (--axes option, flags)** — PARTIAL → `03-CLI-Reference/01-Commands.md`: The CLI reference page exists and likely lists the watch command, but the --axes filtering option introduced in the implementation may or may not be fully described there; without the file content this is assessed as PARTIAL with moderate confidence.

### `src/commands/review.ts` — 50% covered

- [ ] **--axes filter option** — PARTIAL → `03-CLI-Reference/02-Global-Options.md`: The --axes option is command-specific (not a global option), so 02-Global-Options.md is an imperfect fit. Coverage in 01-Commands.md is plausible but unverified. The existence of a dedicated axes system (02-Seven-Axis-System.md) suggests the concept is documented, but whether the CLI flag itself is fully described is uncertain.
- [ ] **auto-scan on empty task list** — MISSING: The implicit auto-scan behaviour (when no tasks exist, scanProject is called automatically before reviewing) is a non-obvious side effect. No doc file name in the listing suggests this fallback is explicitly described; it is not a standard CLI contract detail.
- [ ] **lock mechanism during review** — MISSING: The single-run lock (acquireLock/releaseLock/isLockActive) and the UX consequence ('A run is currently in progress') does not map to any obviously named doc page in the listing. Users hitting this error have no reference.

### `src/commands/scan.ts` — 60% covered

- [ ] **scan command** — PARTIAL → `03-CLI-Reference/01-Commands.md`: The docs directory contains a dedicated CLI reference section (03-CLI-Reference/01-Commands.md) and a second CLI reference at 04-API-Reference/04-CLI-Reference.md, making it likely the scan command is documented at the user-facing level. However, the implementation details visible here — per-file structured logging (AC 28.3.1 comment), run-ID creation, file-logger flushing, and the specific output fields (filesScanned/filesNew/filesCached) — are internal concerns not expected in end-user docs. Concept is likely covered but depth and accuracy cannot be verified from the directory listing alone; confidence is reduced accordingly.

### `src/core/docs-guard.ts` — 0% covered

- [ ] **Documentation Guard (docs-write invariant)** — MISSING: The invariant that Anatoly must never write to docs/ and the assertSafeOutputPath guard that enforces it are not covered in any listed documentation page. Neither 04-Core-Modules/, 02-Architecture/, nor 06-Development/ includes a page for this guard or the docs-write invariant.
