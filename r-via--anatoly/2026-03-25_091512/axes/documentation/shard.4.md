# Documentation — Shard 4

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `src/rag/docker-gguf.ts` | NEEDS_REFACTOR | 3 | 95% | [details](../reviews/src-rag-docker-gguf.rev.md) |
| `src/schemas/config.ts` | NEEDS_REFACTOR | 3 | 95% | [details](../reviews/src-schemas-config.rev.md) |
| `src/utils/run-id.ts` | NEEDS_REFACTOR | 3 | 95% | [details](../reviews/src-utils-run-id.rev.md) |
| `src/utils/schema-example.ts` | NEEDS_REFACTOR | 3 | 95% | [details](../reviews/src-utils-schema-example.rev.md) |
| `src/core/doc-pipeline.ts` | NEEDS_REFACTOR | 3 | 92% | [details](../reviews/src-core-doc-pipeline.rev.md) |
| `src/utils/format.ts` | NEEDS_REFACTOR | 2 | 97% | [details](../reviews/src-utils-format.rev.md) |
| `src/commands/clean-sync.ts` | NEEDS_REFACTOR | 2 | 95% | [details](../reviews/src-commands-clean-sync.rev.md) |
| `src/commands/clean.ts` | NEEDS_REFACTOR | 2 | 95% | [details](../reviews/src-commands-clean.rev.md) |
| `src/core/doc-scoring.ts` | NEEDS_REFACTOR | 2 | 95% | [details](../reviews/src-core-doc-scoring.rev.md) |
| `src/core/grammar-manager.ts` | NEEDS_REFACTOR | 2 | 95% | [details](../reviews/src-core-grammar-manager.rev.md) |

## Symbol Details

### `src/rag/docker-gguf.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `runContainer` | L53–L71 | PARTIAL | 78% | [PARTIAL] Private helper, 18 lines — above the <10-line leniency threshold. N... |
| `waitForContainer` | L73–L96 | PARTIAL | 75% | [PARTIAL] Private helper, 23 lines. No JSDoc. The return type Promise<boolean... |
| `startGgufContainers` | L168–L224 | PARTIAL | 88% | [PARTIAL] Exported function with JSDoc describing prerequisite validation and... |

### `src/schemas/config.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `LlmConfigSchema` | L44–L66 | PARTIAL | 90% | [PARTIAL] Several fields have non-obvious semantics without any JSDoc: sdk_co... |
| `BadgeConfigSchema` | L78–L82 | PARTIAL | 78% | [PARTIAL] The verdict: boolean field is non-obvious in a badge context — it i... |
| `DocumentationConfigSchema` | L90–L93 | PARTIAL | 85% | [PARTIAL] docs_path is self-descriptive, but module_mapping: z.record(z.strin... |

### `src/utils/run-id.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `isValidRunId` | L25–L27 | PARTIAL | 85% | [PARTIAL] JSDoc says only 'Validate a user-provided run ID.' but omits the ac... |
| `MiniRunPaths` | L51–L56 | PARTIAL | 88% | [PARTIAL] The JSDoc comment above the interface describes createMiniRun's beh... |
| `createMiniRun` | L58–L67 | UNDOCUMENTED | 90% | [UNDOCUMENTED] No JSDoc is attached to this exported function. The comment th... |

### `src/utils/schema-example.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `generateSchemaExample` | L54–L111 | PARTIAL | 85% | [PARTIAL] Exported public function with a section banner '// generateSchemaEx... |
| `collectEnums` | L122–L162 | PARTIAL | 78% | [PARTIAL] Private unexported function at ~40 lines — above the <10-line autom... |
| `formatSchemaExample` | L168–L196 | PARTIAL | 85% | [PARTIAL] Exported public function with a section banner '// formatSchemaExam... |

### `src/core/doc-pipeline.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `runDocScaffold` | L63–L90 | PARTIAL | 85% | [PARTIAL] Has JSDoc listing the 4 numbered steps clearly. However, none of th... |
| `runDocGeneration` | L106–L196 | PARTIAL | 85% | [PARTIAL] Has thorough JSDoc covering the 5 steps and — critically — the impo... |
| `buildPageMappings` | L257–L311 | PARTIAL | 80% | [PARTIAL] Private function with a minimal one-line JSDoc ('Builds page-to-sou... |

### `src/utils/format.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `formatTokenSummary` | L48–L61 | PARTIAL | 87% | [PARTIAL] JSDoc describes purpose and mentions cache hit rate, but does not d... |
| `countReviewFindings` | L63–L79 | UNDOCUMENTED | 97% | [UNDOCUMENTED] No JSDoc comment whatsoever. This exported function implements... |

### `src/commands/clean-sync.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `syncAxis` | L64–L115 | PARTIAL | 85% | [PARTIAL] JSDoc is minimal: one sentence of purpose and a bare listing of the... |
| `registerCleanSyncCommand` | L117–L178 | UNDOCUMENTED | 95% | [UNDOCUMENTED] No JSDoc comment whatsoever on the primary exported entry-poin... |

### `src/commands/clean.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `parseUncheckedActions` | L40–L76 | PARTIAL | 85% | [PARTIAL] JSDoc describes purpose and return filter condition, but has no @pa... |
| `registerCleanCommand` | L242–L318 | UNDOCUMENTED | 90% | [UNDOCUMENTED] Exported public function with no JSDoc. As the entry point tha... |

### `src/core/doc-scoring.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `computeStructural` | L122–L139 | PARTIAL | 85% | [PARTIAL] Private function under the '// --- Structural score ---' section ba... |
| `computeWeights` | L167–L204 | PARTIAL | 90% | [PARTIAL] Private function but 37 lines with non-trivial logic: bonus accumul... |

### `src/core/grammar-manager.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `GrammarManager` | L36–L39 | PARTIAL | 85% | [PARTIAL] Exported interface with no JSDoc. The resolve(languageId) return ty... |
| `createGrammarManager` | L70–L126 | UNDOCUMENTED | 90% | [UNDOCUMENTED] Primary exported factory function with no JSDoc at all. The op... |

## Hygiene

- [ ] <!-- ACT-723e57-5 --> **[documentation · medium · trivial]** `src/commands/clean-sync.ts`: Add JSDoc documentation for exported symbol: `registerCleanSyncCommand` (`registerCleanSyncCommand`) [L117-L178]
- [ ] <!-- ACT-95a961-3 --> **[documentation · medium · trivial]** `src/commands/clean.ts`: Add JSDoc documentation for exported symbol: `registerCleanCommand` (`registerCleanCommand`) [L242-L318]
- [ ] <!-- ACT-2a7d50-2 --> **[documentation · medium · trivial]** `src/core/grammar-manager.ts`: Add JSDoc documentation for exported symbol: `createGrammarManager` (`createGrammarManager`) [L70-L126]
- [ ] <!-- ACT-3964b7-4 --> **[documentation · medium · trivial]** `src/utils/format.ts`: Add JSDoc documentation for exported symbol: `countReviewFindings` (`countReviewFindings`) [L63-L79]
- [ ] <!-- ACT-e485d2-3 --> **[documentation · medium · trivial]** `src/utils/run-id.ts`: Add JSDoc documentation for exported symbol: `createMiniRun` (`createMiniRun`) [L58-L67]
- [ ] <!-- ACT-723e57-4 --> **[documentation · low · trivial]** `src/commands/clean-sync.ts`: Complete JSDoc documentation for: `syncAxis` (`syncAxis`) [L64-L115]
- [ ] <!-- ACT-95a961-1 --> **[documentation · low · trivial]** `src/commands/clean.ts`: Complete JSDoc documentation for: `parseUncheckedActions` (`parseUncheckedActions`) [L40-L76]
- [ ] <!-- ACT-1db759-1 --> **[documentation · low · trivial]** `src/core/doc-pipeline.ts`: Complete JSDoc documentation for: `runDocScaffold` (`runDocScaffold`) [L63-L90]
- [ ] <!-- ACT-1db759-2 --> **[documentation · low · trivial]** `src/core/doc-pipeline.ts`: Complete JSDoc documentation for: `runDocGeneration` (`runDocGeneration`) [L106-L196]
- [ ] <!-- ACT-1db759-3 --> **[documentation · low · trivial]** `src/core/doc-pipeline.ts`: Complete JSDoc documentation for: `buildPageMappings` (`buildPageMappings`) [L257-L311]
- [ ] <!-- ACT-c457c3-2 --> **[documentation · low · trivial]** `src/core/doc-scoring.ts`: Complete JSDoc documentation for: `computeStructural` (`computeStructural`) [L122-L139]
- [ ] <!-- ACT-c457c3-4 --> **[documentation · low · trivial]** `src/core/doc-scoring.ts`: Complete JSDoc documentation for: `computeWeights` (`computeWeights`) [L167-L204]
- [ ] <!-- ACT-2a7d50-1 --> **[documentation · low · trivial]** `src/core/grammar-manager.ts`: Complete JSDoc documentation for: `GrammarManager` (`GrammarManager`) [L36-L39]
- [ ] <!-- ACT-ed143f-1 --> **[documentation · low · trivial]** `src/rag/docker-gguf.ts`: Complete JSDoc documentation for: `runContainer` (`runContainer`) [L53-L71]
- [ ] <!-- ACT-ed143f-2 --> **[documentation · low · trivial]** `src/rag/docker-gguf.ts`: Complete JSDoc documentation for: `waitForContainer` (`waitForContainer`) [L73-L96]
- [ ] <!-- ACT-ed143f-3 --> **[documentation · low · trivial]** `src/rag/docker-gguf.ts`: Complete JSDoc documentation for: `startGgufContainers` (`startGgufContainers`) [L168-L224]
- [ ] <!-- ACT-f3933d-1 --> **[documentation · low · trivial]** `src/schemas/config.ts`: Complete JSDoc documentation for: `LlmConfigSchema` (`LlmConfigSchema`) [L44-L66]
- [ ] <!-- ACT-f3933d-3 --> **[documentation · low · trivial]** `src/schemas/config.ts`: Complete JSDoc documentation for: `BadgeConfigSchema` (`BadgeConfigSchema`) [L78-L82]
- [ ] <!-- ACT-f3933d-4 --> **[documentation · low · trivial]** `src/schemas/config.ts`: Complete JSDoc documentation for: `DocumentationConfigSchema` (`DocumentationConfigSchema`) [L90-L93]
- [ ] <!-- ACT-3964b7-3 --> **[documentation · low · trivial]** `src/utils/format.ts`: Complete JSDoc documentation for: `formatTokenSummary` (`formatTokenSummary`) [L48-L61]
- [ ] <!-- ACT-e485d2-1 --> **[documentation · low · trivial]** `src/utils/run-id.ts`: Complete JSDoc documentation for: `isValidRunId` (`isValidRunId`) [L25-L27]
- [ ] <!-- ACT-e485d2-2 --> **[documentation · low · trivial]** `src/utils/run-id.ts`: Complete JSDoc documentation for: `MiniRunPaths` (`MiniRunPaths`) [L51-L56]
- [ ] <!-- ACT-9adf61-1 --> **[documentation · low · trivial]** `src/utils/schema-example.ts`: Complete JSDoc documentation for: `generateSchemaExample` (`generateSchemaExample`) [L54-L111]
- [ ] <!-- ACT-9adf61-2 --> **[documentation · low · trivial]** `src/utils/schema-example.ts`: Complete JSDoc documentation for: `collectEnums` (`collectEnums`) [L122-L162]
- [ ] <!-- ACT-9adf61-3 --> **[documentation · low · trivial]** `src/utils/schema-example.ts`: Complete JSDoc documentation for: `formatSchemaExample` (`formatSchemaExample`) [L168-L196]

## Documentation Coverage

### `src/rag/docker-gguf.ts` — 38% covered

- [ ] **GGUF Docker container lifecycle (start/stop/health-check)** — PARTIAL → `rag.md`: rag.md and 02-Architecture/03-RAG-Engine.md likely mention GGUF containers as part of the embedding pipeline, but the sequential lifecycle API (startGgufContainers, stopGgufContainers, areGgufContainersRunning) has no dedicated module reference page. The module-level header comment describes the strategy but it lives in source, not docs.
- [ ] **Sequential/swap model loading strategy (VRAM optimisation)** — PARTIAL → `07-Design-Decisions/01-Why-Local-RAG.md`: The swap strategy (only one model loaded at a time, ~10 GB vs ~20 GB) is a significant architectural decision documented in the file-level JSDoc. The design-decisions docs may address local RAG motivation but likely do not detail the swap-cost trade-off (30–120 s startup cost).
- [ ] **ensureModel swap API** — MISSING: No doc page in the provided directory appears to cover the ensureModel function or the code/NLP model-switching contract that callers must honour. This is a key integration point for any consumer of the GGUF embedding subsystem.

### `src/schemas/config.ts` — 55% covered

- [ ] **Configuration Schemas (overall)** — PARTIAL → `01-Getting-Started/02-Configuration.md`: A configuration guide exists and a dedicated Schemas dev doc (06-Development/03-Schemas.md) is present, suggesting the overall config structure is covered. However, the non-obvious fields in LlmConfigSchema (sdk_concurrency, agentic_tools, max_stop_iterations, deliberation) and the module_mapping semantics in DocumentationConfigSchema are likely underdocumented given no inline JSDoc compensates.
- [ ] **LLM Configuration (LlmConfigSchema)** — PARTIAL → `03-Guides/02-Advanced-Configuration.md`: An advanced configuration guide exists. The standard fields (model, concurrency, max_retries) are likely covered, but non-obvious settings such as the sdk_concurrency/concurrency split, agentic_tools toggle, and the deliberation pass are unlikely to be fully explained without dedicated sections.
- [ ] **Documentation Concept Mapping (DocumentationConfigSchema)** — MISSING: No documentation file explicitly targets the DocumentationConfigSchema or explains the module_mapping field semantics. The docs_path default is likely mentioned in configuration guides, but the purpose and format of module_mapping appears undocumented.

### `src/utils/run-id.ts` — 0% covered

- [ ] **Run lifecycle (ID generation, .anatoly/runs/ directory structure, latest pointer)** — MISSING: No page in the provided docs tree explicitly covers run ID generation, the on-disk .anatoly/runs/<runId>/ layout, or the latest symlink mechanism. Candidates like 06-Development/00-Source-Tree.md or 02-Architecture/03-Data-Flow.md may mention the directory in passing, but no dedicated coverage is apparent from the listing.
- [ ] **MiniRun concept for standalone commands** — MISSING: The MiniRun abstraction — a lightweight run directory created for scan, estimate, review, and watch — is not documented in any visible docs page. The CLI reference likely documents the commands themselves but not the underlying run directory they create.
- [ ] **Run purging and retention policy** — MISSING: The purgeRuns function implements a keep-N-most-recent retention policy, but no docs page exposes this behaviour or its configuration surface to users.

### `src/core/doc-pipeline.ts` — 15% covered

- [ ] **Documentation Pipeline Orchestration (runDocScaffold / runDocGeneration)** — MISSING: No entry in 04-Core-Modules/ for the doc-pipeline module. The 04-Core-Modules/ section covers Scanner, Estimator, Triage, Axis-Evaluators, Reporter, and Worker-Pool — but not the doc-pipeline orchestrator which wires Stories 29.1–29.14 together.
- [ ] **Incremental Doc Cache (loadDocCache / checkDocCache / saveDocCache)** — MISSING: The SHA-256 source-hash-based incremental caching strategy used by runDocGeneration is not mentioned in any listed documentation page. No cache-related doc exists in 02-Architecture/ or 04-Core-Modules/.
- [ ] **Doc Scaffolding Phase** — MISSING: The scaffolding phase (resolveModuleGranularity → resolveDocMappings → scaffoldDocs) has no dedicated doc page. The 05-Modules/ section contains only example module pages (monolith-500-lines.md, setup-embeddings.md), not documentation of the scaffolding system itself.
- [ ] **Pipeline Overview** — PARTIAL → `docs/02-Architecture/01-Pipeline-Overview.md`: 02-Architecture/01-Pipeline-Overview.md likely describes the overall Anatoly pipeline, which may include the doc generation phase at a high level. However, the two-phase doc-specific flow (scaffold then generation) and the LLM-delegation contract (prompts returned, not executed) are probably not covered in detail there.

### `src/core/doc-scoring.ts` — 0% covered

- [ ] **5-dimension weighted documentation scoring** — MISSING: No docs/ page describes the five scoring dimensions (structural, API coverage, module coverage, content quality, navigation) or their base weights (25/25/20/15/15). The closest candidates (02-Architecture/02-Seven-Axis-System.md, 04-Core-Modules/04-Axis-Evaluators.md) do not appear to cover the scoring formula by their titles.
- [ ] **Project-type weight adjustments** — MISSING: The bonus system (+10% structural for Backend API/ORM/Frontend/Monorepo, +15% API coverage for Library) with proportional pool reduction from smaller dimensions is not reflected in any visible docs/ page.
- [ ] **DocVerdict thresholds (DOCUMENTED ≥80, PARTIAL ≥50)** — MISSING: The verdict thresholds (overall ≥80 → DOCUMENTED, ≥50 → PARTIAL, else UNDOCUMENTED) are a user-facing scoring contract with no visible documentation in docs/.
- [ ] **Structural presence scoring (required section patterns)** — MISSING: The required-section regex patterns (index.md, getting-started, architecture, api-reference, development plus per-type extras) define what counts as structurally present, but no docs/ page explains this requirement set.

### `src/core/grammar-manager.ts` — 0% covered

- [ ] **Dynamic WASM grammar downloading and caching** — MISSING: No page in docs/ mentions the GrammarManager, the WASM download-on-demand strategy, the .anatoly/grammars cache directory, or the manifest integrity tracking. The 04-Core-Modules/ section covers Scanner, Estimator, Triage, Axis-Evaluators, Reporter, and Worker-Pool but not the grammar subsystem.
- [ ] **GRAMMAR_REGISTRY and supported language tiers** — MISSING: The registry listing (bash, python, rust, go, java, csharp, sql, yaml, json) and the concept of 'Tier 1' languages downloaded via CDN are not documented in any docs/ page. Users have no way to discover which languages are supported or how to add new ones.
