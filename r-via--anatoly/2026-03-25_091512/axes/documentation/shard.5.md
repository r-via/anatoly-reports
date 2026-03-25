# Documentation — Shard 5

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `src/core/source-context.ts` | NEEDS_REFACTOR | 2 | 95% | [details](../reviews/src-core-source-context.rev.md) |
| `src/core/triage.ts` | NEEDS_REFACTOR | 2 | 95% | [details](../reviews/src-core-triage.rev.md) |
| `src/rag/types.ts` | NEEDS_REFACTOR | 2 | 95% | [details](../reviews/src-rag-types.rev.md) |
| `src/rag/vector-store.ts` | NEEDS_REFACTOR | 2 | 95% | [details](../reviews/src-rag-vector-store.rev.md) |
| `src/core/axes/duplication.ts` | NEEDS_REFACTOR | 2 | 92% | [details](../reviews/src-core-axes-duplication.rev.md) |
| `src/core/doc-llm-executor.ts` | NEEDS_REFACTOR | 2 | 92% | [details](../reviews/src-core-doc-llm-executor.rev.md) |
| `src/core/file-evaluator.ts` | NEEDS_REFACTOR | 2 | 92% | [details](../reviews/src-core-file-evaluator.rev.md) |
| `src/core/estimator.ts` | NEEDS_REFACTOR | 1 | 97% | [details](../reviews/src-core-estimator.rev.md) |
| `src/core/usage-graph.ts` | NEEDS_REFACTOR | 1 | 97% | [details](../reviews/src-core-usage-graph.rev.md) |
| `src/cli/pipeline-state.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-cli-pipeline-state.rev.md) |

## Symbol Details

### `src/core/source-context.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `buildPageContext` | L73–L120 | PARTIAL | 85% | [PARTIAL] Has JSDoc covering purpose and two key behaviors (symbol extraction... |
| `extractJsdoc` | L145–L191 | PARTIAL | 85% | [PARTIAL] Private 47-line function with no JSDoc. While the name is self-desc... |

### `src/core/triage.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `triageFile` | L36–L78 | PARTIAL | 85% | [PARTIAL] JSDoc explains the overall purpose and the axis-based pipeline cont... |
| `generateSkipReview` | L85–L121 | PARTIAL | 85% | [PARTIAL] JSDoc covers the high-level purpose, the is_generated=true flag, sk... |

### `src/rag/types.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `BehavioralProfileSchema` | L7–L14 | UNDOCUMENTED | 88% | [UNDOCUMENTED] Exported Zod enum schema with no JSDoc. The distinctions betwe... |
| `FunctionCardSchema` | L16–L28 | UNDOCUMENTED | 88% | [UNDOCUMENTED] Exported Zod object schema with no JSDoc. Non-trivial constrai... |

### `src/rag/vector-store.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `UpsertOptions` | L55–L58 | PARTIAL | 82% | [PARTIAL] Class-level JSDoc only mentions 'optional NLP embeddings', silently... |
| `VectorStore` | L60–L602 | PARTIAL | 87% | [PARTIAL] No class-level JSDoc describes the class's overall purpose, lifecyc... |

### `src/core/axes/duplication.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `buildDuplicationUserMessage` | L44–L107 | UNDOCUMENTED | 90% | [UNDOCUMENTED] Exported function (~63 lines) with no JSDoc at all. Builds a s... |
| `DuplicationEvaluator` | L144–L195 | PARTIAL | 87% | [UNDOCUMENTED] Main exported class implementing AxisEvaluator with no JSDoc. ... |

### `src/core/doc-llm-executor.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `executeOnePage` | L95–L130 | PARTIAL | 75% | [PARTIAL] Private function with a clear name, but at ~35 lines it exceeds the... |
| `reviewDocStructure` | L164–L374 | PARTIAL | 88% | [PARTIAL] JSDoc mentions it is a deterministic linter with no LLM call, which... |

### `src/core/file-evaluator.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `EvaluateFileOptions` | L35–L63 | PARTIAL | 87% | [PARTIAL] No top-level interface JSDoc explaining overall purpose. Seven fiel... |
| `evaluateFile` | L101–L325 | PARTIAL | 90% | [PARTIAL] Has a JSDoc block describing the function's purpose and enumerating... |

### `src/core/estimator.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `estimateTasksTokens` | L111–L153 | PARTIAL | 85% | [PARTIAL] JSDoc exists but is brief (2 lines) for a 42-line exported function... |

### `src/core/usage-graph.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `extractImports` | L97–L181 | PARTIAL | 87% | [PARTIAL] Has a one-line JSDoc 'Extract import relationships from a single fi... |

### `src/cli/pipeline-state.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `PipelineState` | L31–L124 | PARTIAL | 88% | [PARTIAL] No class-level JSDoc explaining the class's purpose, lifecycle, or ... |

## Hygiene

- [ ] <!-- ACT-7e9d04-2 --> **[documentation · medium · trivial]** `src/core/axes/duplication.ts`: Add JSDoc documentation for exported symbol: `buildDuplicationUserMessage` (`buildDuplicationUserMessage`) [L44-L107]
- [ ] <!-- ACT-7e9d04-3 --> **[documentation · medium · trivial]** `src/core/axes/duplication.ts`: Add JSDoc documentation for exported symbol: `DuplicationEvaluator` (`DuplicationEvaluator`) [L144-L195]
- [ ] <!-- ACT-155517-1 --> **[documentation · medium · trivial]** `src/rag/types.ts`: Add JSDoc documentation for exported symbol: `BehavioralProfileSchema` (`BehavioralProfileSchema`) [L7-L14]
- [ ] <!-- ACT-155517-2 --> **[documentation · medium · trivial]** `src/rag/types.ts`: Add JSDoc documentation for exported symbol: `FunctionCardSchema` (`FunctionCardSchema`) [L16-L28]
- [ ] <!-- ACT-39d6f8-1 --> **[documentation · low · trivial]** `src/cli/pipeline-state.ts`: Complete JSDoc documentation for: `PipelineState` (`PipelineState`) [L31-L124]
- [ ] <!-- ACT-88c6ea-2 --> **[documentation · low · trivial]** `src/core/doc-llm-executor.ts`: Complete JSDoc documentation for: `executeOnePage` (`executeOnePage`) [L95-L130]
- [ ] <!-- ACT-88c6ea-3 --> **[documentation · low · trivial]** `src/core/doc-llm-executor.ts`: Complete JSDoc documentation for: `reviewDocStructure` (`reviewDocStructure`) [L164-L374]
- [ ] <!-- ACT-0c5d31-2 --> **[documentation · low · trivial]** `src/core/estimator.ts`: Complete JSDoc documentation for: `estimateTasksTokens` (`estimateTasksTokens`) [L111-L153]
- [ ] <!-- ACT-ef74a2-1 --> **[documentation · low · trivial]** `src/core/file-evaluator.ts`: Complete JSDoc documentation for: `EvaluateFileOptions` (`EvaluateFileOptions`) [L35-L63]
- [ ] <!-- ACT-ef74a2-2 --> **[documentation · low · trivial]** `src/core/file-evaluator.ts`: Complete JSDoc documentation for: `evaluateFile` (`evaluateFile`) [L101-L325]
- [ ] <!-- ACT-acf946-1 --> **[documentation · low · trivial]** `src/core/source-context.ts`: Complete JSDoc documentation for: `buildPageContext` (`buildPageContext`) [L73-L120]
- [ ] <!-- ACT-acf946-2 --> **[documentation · low · trivial]** `src/core/source-context.ts`: Complete JSDoc documentation for: `extractJsdoc` (`extractJsdoc`) [L145-L191]
- [ ] <!-- ACT-ba7c65-1 --> **[documentation · low · trivial]** `src/core/triage.ts`: Complete JSDoc documentation for: `triageFile` (`triageFile`) [L36-L78]
- [ ] <!-- ACT-ba7c65-2 --> **[documentation · low · trivial]** `src/core/triage.ts`: Complete JSDoc documentation for: `generateSkipReview` (`generateSkipReview`) [L85-L121]
- [ ] <!-- ACT-58ba8b-1 --> **[documentation · low · trivial]** `src/core/usage-graph.ts`: Complete JSDoc documentation for: `extractImports` (`extractImports`) [L97-L181]
- [ ] <!-- ACT-5b3ff2-1 --> **[documentation · low · trivial]** `src/rag/vector-store.ts`: Complete JSDoc documentation for: `UpsertOptions` (`UpsertOptions`) [L55-L58]
- [ ] <!-- ACT-5b3ff2-2 --> **[documentation · low · trivial]** `src/rag/vector-store.ts`: Complete JSDoc documentation for: `VectorStore` (`VectorStore`) [L60-L602]

## Documentation Coverage

### `src/core/source-context.ts` — 0% covered

- [ ] **Source context extraction for documentation generation** — MISSING: The source-context module (Story 29.7) — which extracts exported symbols, re-exports, and import graphs to feed documentation scaffolding — does not appear to be covered in any of the listed docs pages. No 04-Core-Modules page exists for it, and none of the architecture pages are named for this subsystem.
- [ ] **PageContext and token-based truncation with priority ordering** — MISSING: The 4-phase truncation strategy (internals → body snippets → JSDoc → signature pruning) is non-trivial design that warrants documentation. No docs page in the listed structure covers this mechanism.
- [ ] **Import graph extraction** — MISSING: The import graph extracted for architecture pages (02-Architecture/) is a distinct subsystem concern. While 02-Architecture/03-Data-Flow.md might touch on dependency relationships conceptually, no docs page is named for this extraction mechanism, and the source-context module is not listed under 04-Core-Modules.

### `src/core/triage.ts` — 80% covered

- [ ] **Barrel export detection (isBarrelExport)** — PARTIAL → `04-Core-Modules/03-Triage.md`: Barrel export detection is an internal heuristic and may be described only superficially in the Triage module doc if at all; the doc file exists but actual coverage of this specific skip criterion cannot be confirmed from the directory listing alone.

### `src/core/doc-llm-executor.ts` — 0% covered

- [ ] **Doc generation execution pipeline (executeDocPrompts / DocExecutor)** — MISSING: No docs/ page covers the PagePrompt-to-file execution pipeline, the DocExecutor abstraction, or the semaphore-bounded concurrency model used for parallel page generation.
- [ ] **Deterministic structure linter (reviewDocStructure)** — MISSING: The seven-check linter (preamble, wrapping-fence, heading-hierarchy, numbering-gap, index completeness, broken-link, index-order) and its auto-fix behavior are not documented in any docs/ page.
- [ ] **Coherence review agent (runDocCoherenceReview)** — MISSING: The multi-turn Opus audit-fix-verify loop for cross-page coherence is not referenced in docs/02-Architecture/, docs/04-Core-Modules/, or any other docs/ section.
- [ ] **Content review agent (runDocContentReview)** — MISSING: The gap-report-driven Opus agent that fills content gaps in generated documentation is not mentioned in any docs/ page.

### `src/core/file-evaluator.ts` — 50% covered

- [ ] **File evaluation pipeline (evaluateFile orchestration)** — PARTIAL → `02-Architecture/01-Pipeline-Overview.md`: The pipeline overview and 04-Core-Modules/04-Axis-Evaluators.md likely cover the overall flow and axis execution, but no dedicated file-evaluator module page exists under 04-Core-Modules/ (Scanner, Estimator, Triage, Axis-Evaluators, Reporter, Worker-Pool are present — FileEvaluator is absent). EvaluateFileOptions and EvaluateFileResult types are probably undocumented at the reference level.
- [ ] **RAG pre-resolution (hybrid search before axis evaluation)** — PARTIAL → `02-Architecture/03-RAG-Engine.md`: RAG documentation and a top-level rag.md exist, covering general querying. However, the specific pre-resolution pattern (hybrid search keyed by buildFunctionId before axis evaluation, with codeWeight blending) is an implementation detail unlikely to be covered at the docs level.
- [ ] **Test file auto-discovery** — MISSING: evaluateFile automatically locates and injects associated test files (foo.ts → foo.test.ts / foo.spec.ts) into the axis context. No page in the provided docs tree appears to document this feature or the .test/.spec suffix resolution logic.

### `src/core/usage-graph.ts` — 65% covered

- [ ] **TypeScript vs non-TypeScript import tracking** — PARTIAL → `docs/02-Architecture/04-Usage-Graph.md`: The dual-path logic (fine-grained per-symbol TS tracking vs adapter-based bulk marking for non-TS) is an implementation detail that may be omitted or only partially covered in architecture docs.
- [ ] **Intra-file reference graph / transitive usage** — PARTIAL → `docs/02-Architecture/04-Usage-Graph.md`: The transitive-liveness concept (symbol B alive because symbol A references it and A is imported) is a subtle semantic that may not be fully explained in architecture documentation without inspection.
- [ ] **No-import-system languages (SQL, YAML, JSON)** — PARTIAL → `docs/02-Architecture/04-Usage-Graph.md`: The special-casing of SQL/YAML/JSON files to never be flagged DEAD is a policy decision that may be mentioned but is easily omitted from higher-level docs.
- [ ] **getSymbolUsage / getTypeOnlySymbolUsage / getTransitiveUsage query API** — PARTIAL → `docs/02-Architecture/04-Usage-Graph.md`: These three exported query helpers form the consumer-facing API of the module. Architecture docs may describe the graph concept without enumerating every query function's semantics.

### `src/cli/pipeline-state.ts` — 15% covered

- [ ] **PipelineState class** — MISSING: No page in docs/ documents the PipelineState class, its methods, or its lifecycle. The closest candidates (03-CLI-Reference/, 04-API-Reference/04-CLI-Reference.md) appear to cover commands and flags, not internal state infrastructure.
- [ ] **PipelinePhase (rag / review / summary pipeline phases)** — PARTIAL → `02-Architecture/01-Pipeline-Overview.md`: The three pipeline phases ('rag', 'review', 'summary') are architectural concepts that are likely described at a high level in the Pipeline Overview, but the PipelinePhase type itself and its use in PipelineState.setPhase are not expected to be covered there. Scored as PARTIAL: concept may appear but the type binding is absent.
- [ ] **TaskState / FileState / SummaryState interfaces** — MISSING: These data-shape interfaces are internal to the CLI renderer infrastructure. No public docs/ page appears to document them or their fields.
