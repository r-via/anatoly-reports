# Documentation — Shard 6

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `src/commands/docs.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-commands-docs.rev.md) |
| `src/commands/hook.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-commands-hook.rev.md) |
| `src/commands/rag-status.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-commands-rag-status.rev.md) |
| `src/core/axes/utility.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-core-axes-utility.rev.md) |
| `src/core/calibration.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-core-calibration.rev.md) |
| `src/core/correction-memory.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-core-correction-memory.rev.md) |
| `src/core/doc-mapping.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-core-doc-mapping.rev.md) |
| `src/core/doc-report-section.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-core-doc-report-section.rev.md) |
| `src/core/doc-sync.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-core-doc-sync.rev.md) |
| `src/core/project-tree.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-core-project-tree.rev.md) |

## Symbol Details

### `src/commands/docs.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `registerDocsCommand` | L172–L936 | UNDOCUMENTED | 95% | [UNDOCUMENTED] No JSDoc/TSDoc comment on this exported function. It registers... |

### `src/commands/hook.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `registerHookCommand` | L51–L373 | UNDOCUMENTED | 95% | [UNDOCUMENTED] The sole exported public function in the file has no JSDoc com... |

### `src/commands/rag-status.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `registerRagStatusCommand` | L52–L199 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported public API function with no JSDoc whatsoever. At ~147... |

### `src/core/axes/utility.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `buildUtilityUserMessage` | L35–L82 | PARTIAL | 85% | [PARTIAL] Exported 48-line function with no JSDoc. While the name clearly con... |

### `src/core/calibration.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `estimateCalibratedMinutes` | L170–L206 | PARTIAL | 85% | [PARTIAL] A JSDoc block reading 'Estimate total review duration in minutes fo... |

### `src/core/correction-memory.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `DeliberationMemory` | L32–L35 | PARTIAL | 82% | [PARTIAL] No top-level JSDoc comment on the interface itself. The `version: 1... |

### `src/core/doc-mapping.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `resolveOne` | L82–L112 | PARTIAL | 82% | [PARTIAL] Private function with no JSDoc. At ~30 lines it exceeds the <10-lin... |

### `src/core/doc-report-section.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `SymbolCoverage` | L16–L22 | PARTIAL | 85% | [PARTIAL] No top-level JSDoc comment. Fields are partially self-descriptive, ... |

### `src/core/doc-sync.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `syncDocs` | L58–L82 | PARTIAL | 85% | [PARTIAL] Has a JSDoc explaining the main behavior (processes recommendations... |

### `src/core/project-tree.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `collapseChain` | L94–L130 | PARTIAL | 80% | [PARTIAL] Private 36-line function with no JSDoc. An inline comment inside th... |

## Hygiene

- [ ] <!-- ACT-eaac47-1 --> **[documentation · medium · trivial]** `src/commands/docs.ts`: Add JSDoc documentation for exported symbol: `registerDocsCommand` (`registerDocsCommand`) [L172-L936]
- [ ] <!-- ACT-c84d30-1 --> **[documentation · medium · trivial]** `src/commands/hook.ts`: Add JSDoc documentation for exported symbol: `registerHookCommand` (`registerHookCommand`) [L51-L373]
- [ ] <!-- ACT-28c85f-1 --> **[documentation · medium · trivial]** `src/commands/rag-status.ts`: Add JSDoc documentation for exported symbol: `registerRagStatusCommand` (`registerRagStatusCommand`) [L52-L199]
- [ ] <!-- ACT-4e14ba-2 --> **[documentation · low · trivial]** `src/core/axes/utility.ts`: Complete JSDoc documentation for: `buildUtilityUserMessage` (`buildUtilityUserMessage`) [L35-L82]
- [ ] <!-- ACT-1ad141-1 --> **[documentation · low · trivial]** `src/core/calibration.ts`: Complete JSDoc documentation for: `estimateCalibratedMinutes` (`estimateCalibratedMinutes`) [L170-L206]
- [ ] <!-- ACT-4ec26f-1 --> **[documentation · low · trivial]** `src/core/correction-memory.ts`: Complete JSDoc documentation for: `DeliberationMemory` (`DeliberationMemory`) [L32-L35]
- [ ] <!-- ACT-693faa-1 --> **[documentation · low · trivial]** `src/core/doc-mapping.ts`: Complete JSDoc documentation for: `resolveOne` (`resolveOne`) [L82-L112]
- [ ] <!-- ACT-9e6a64-1 --> **[documentation · low · trivial]** `src/core/doc-report-section.ts`: Complete JSDoc documentation for: `SymbolCoverage` (`SymbolCoverage`) [L16-L22]
- [ ] <!-- ACT-2d2271-1 --> **[documentation · low · trivial]** `src/core/doc-sync.ts`: Complete JSDoc documentation for: `syncDocs` (`syncDocs`) [L58-L82]
- [ ] <!-- ACT-f5abd6-1 --> **[documentation · low · trivial]** `src/core/project-tree.ts`: Complete JSDoc documentation for: `collapseChain` (`collapseChain`) [L94-L130]

## Documentation Coverage

### `src/commands/hook.ts` — 70% covered

- [ ] **Anti-loop protection (stop_hook_active check, max_stop_iterations)** — PARTIAL → `05-Integration/01-Claude-Code-Hooks.md`: The hooks page likely covers the general on-stop behavior, but the specific anti-loop mechanisms (stop_hook_active payload check and max_stop_iterations config key) are implementation-level details that may not be fully documented for users who need to understand or tune this behavior.
- [ ] **Background review spawning with debounce and hash-based deduplication** — PARTIAL → `05-Integration/01-Claude-Code-Hooks.md`: The on-edit hook's non-trivial behavior — SHA-256 hash comparison to skip unchanged files, SIGTERM of a previously running review for the same file, and detached subprocess spawning — likely has only high-level coverage in the integration doc without exposing the deduplication semantics to users.

### `src/core/axes/utility.ts` — 55% covered

- [ ] **Utility axis (USED / DEAD / LOW_VALUE)** — PARTIAL → `02-Architecture/02-Seven-Axis-System.md`: The seven-axis system doc likely names the utility axis, but the specific three-value classification (USED, DEAD, LOW_VALUE) and the role of pre-computed import analysis in driving the verdict are complex enough to warrant explicit coverage. Cannot confirm full accuracy without reading the file content.
- [ ] **UtilityEvaluator / axis evaluator pattern** — PARTIAL → `04-Core-Modules/04-Axis-Evaluators.md`: The Axis-Evaluators module doc presumably covers the AxisEvaluator interface and individual evaluators. Whether UtilityEvaluator's correction-memory augmentation and transitive-usage injection are documented is uncertain without reading the file.
- [ ] **Pre-computed import analysis / usage graph injection into prompts** — PARTIAL → `02-Architecture/04-Usage-Graph.md`: The Usage-Graph doc covers the graph itself, but the utility axis specifically injects transitive-usage data into the LLM prompt as a 'Pre-computed Import Analysis' section. Whether this prompt-side integration is documented alongside the graph API is unclear from the directory listing alone.

### `src/core/correction-memory.ts` — 25% covered

- [ ] **Deliberation memory persistence (load/save)** — PARTIAL → `02-Architecture/05-Deliberation-Pass.md`: The deliberation pass architecture doc likely covers the high-level concept of persisting overturned findings, but no dedicated core-module page exists for correction-memory.ts. The load/save/atomic-write API details and legacy migration path are not covered in any visible docs page.
- [ ] **Reclassification recording and deduplication** — PARTIAL → `02-Architecture/05-Deliberation-Pass.md`: recordReclassification and its deduplication semantics (pattern+axis+dependency key) are central to the deliberation workflow. The deliberation-pass doc may describe the concept, but the deduplication contract and the deprecated recordFalsePositive alias are unlikely to be documented.
- [ ] **formatMemoryForPrompt / prompt injection of false positives** — MISSING: No docs page in the provided listing addresses how reclassification/false-positive data is injected into axis prompts. This is a key integration point between the memory store and the LLM evaluation pipeline, and it has no visible documentation coverage.
- [ ] **formatReclassificationsForAxis / per-axis prompt sections** — MISSING: The axis-specific variant of the prompt-injection function is not mentioned in any visible docs page. The 04-Core-Modules/04-Axis-Evaluators.md might touch on it but no such connection is evident from the directory listing alone.
- [ ] **Legacy correction-memory migration** — MISSING: The migration path from correction-memory.json to deliberation-memory.json (including the .bak file creation) is an operational concern not covered in any visible documentation page.

### `src/core/doc-mapping.ts` — 0% covered

- [ ] **Code → Documentation Mapping module** — MISSING: No page in docs/ covers the doc-mapping module or its resolveDocMappings API. The 04-Core-Modules/ section lists Scanner, Estimator, Triage, Axis-Evaluators, Reporter, and Worker-Pool but has no entry for doc-mapping or Story 29.5.
- [ ] **Four-strategy fallback (convention → synonym → framework → catch-all)** — MISSING: The fallback resolution strategy is the core algorithm of this file. It is documented only in the module-level source comment; no docs/ page explains it for end users or integrators.
- [ ] **LOC threshold filtering (< 200 LOC directories skipped)** — MISSING: The 200-LOC minimum threshold that gates which directories receive a doc mapping is mentioned only in the source code; no docs/ page references this behaviour or its rationale.

### `src/core/doc-report-section.ts` — 17% covered

- [ ] **Documentation Reference Section rendering** — PARTIAL → `04-Core-Modules/05-Reporter.md`: The Reporter module doc likely covers the report-generation pipeline, but the specific 'Documentation Reference' section introduced in Story 29.11 — including the page generation summary, new/refreshed/cached breakdown, and the Markdown rendering logic — is almost certainly not called out specifically in a general reporter overview.
- [ ] **Symbol-based coverage metrics (docs/ vs .anatoly/docs/)** — MISSING: The dual-coverage model (projectDocumented vs internalDocumented, with per-symbol percentages for project docs/ and .anatoly/docs/ separately) is a Story 29.20 feature. No docs page in the provided index corresponds to this concept.
- [ ] **Sync status by recommendation type (toCreate / outdated)** — MISSING: The SyncByType breakdown (pages to create vs pages outdated) surfaced in the report is not covered by any visible docs page. The closest candidate would be Reporter.md or a docs-sync guide, neither of which exists with that specific scope.

### `src/core/doc-sync.ts` — 0% covered

- [ ] **Documentation Sync Mode** — MISSING: The doc-sync module (Story 29.14) — synchronizing user docs/ from .anatoly/docs/ based on three recommendation types — is not covered in any docs/ page. The 04-Core-Modules/ section covers scanner, estimator, triage, axis-evaluators, reporter, and worker-pool but has no entry for doc-sync.
- [ ] **SyncResult / SyncReport / SyncIO public types** — MISSING: The exported public API types from doc-sync.ts are not referenced in any docs/ API reference or core-modules page.
