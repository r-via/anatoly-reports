# Documentation — Shard 3

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `src/core/docs-resolver.ts` | NEEDS_REFACTOR | 4 | 95% | [details](../reviews/src-core-docs-resolver.rev.md) |
| `src/schemas/review.ts` | NEEDS_REFACTOR | 4 | 95% | [details](../reviews/src-schemas-review.rev.md) |
| `src/cli/pipeline-runner.ts` | NEEDS_REFACTOR | 4 | 92% | [details](../reviews/src-cli-pipeline-runner.rev.md) |
| `src/cli/setup-table.ts` | NEEDS_REFACTOR | 3 | 95% | [details](../reviews/src-cli-setup-table.rev.md) |
| `src/core/axes/documentation.ts` | NEEDS_REFACTOR | 3 | 95% | [details](../reviews/src-core-axes-documentation.rev.md) |
| `src/core/axes/overengineering.ts` | NEEDS_REFACTOR | 3 | 95% | [details](../reviews/src-core-axes-overengineering.rev.md) |
| `src/core/axis-merger.ts` | NEEDS_REFACTOR | 3 | 95% | [details](../reviews/src-core-axis-merger.rev.md) |
| `src/core/deliberation.ts` | NEEDS_REFACTOR | 3 | 95% | [details](../reviews/src-core-deliberation.rev.md) |
| `src/core/doc-recommendations.ts` | NEEDS_REFACTOR | 3 | 95% | [details](../reviews/src-core-doc-recommendations.rev.md) |
| `src/core/language-detect.ts` | NEEDS_REFACTOR | 3 | 95% | [details](../reviews/src-core-language-detect.rev.md) |

## Symbol Details

### `src/core/docs-resolver.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `RAG_MAX_SECTIONS` | L16–L16 | PARTIAL | 80% | [PARTIAL] Exported constant covered only by a section banner comment '// RAG-... |
| `RAG_MAX_LINES_PER_SECTION` | L17–L17 | PARTIAL | 80% | [PARTIAL] Exported constant with only the shared section banner '// RAG-based... |
| `resolveRelevantDocs` | L114–L173 | PARTIAL | 85% | [PARTIAL] Has JSDoc describing purpose, two-phase strategy (config-driven the... |
| `resolveAllRelevantDocs` | L179–L208 | PARTIAL | 85% | [PARTIAL] Has JSDoc explaining dual-source resolution and interleaved merging... |

### `src/schemas/review.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `SymbolReviewSchema` | L17–L37 | PARTIAL | 82% | [PARTIAL] No JSDoc on the schema itself. The sentinel value '-' appearing acr... |
| `BestPracticesRuleSchema` | L77–L84 | PARTIAL | 85% | [PARTIAL] No JSDoc. The constraint rule_id: z.int().min(1).max(17) encodes a ... |
| `DocRecommendationSchema` | L113–L121 | PARTIAL | 78% | [PARTIAL] No JSDoc. The semantic distinction between path_ideal and path_user... |
| `ReviewFileSchema` | L137–L182 | PARTIAL | 87% | [PARTIAL] The section comment above is a plain comment, not JSDoc. Several v2... |

### `src/cli/pipeline-runner.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `PipelineContext` | L45–L57 | PARTIAL | 85% | [PARTIAL] No JSDoc comment. While fields like projectRoot, config, plain are ... |
| `PipelineOptions` | L59–L66 | PARTIAL | 85% | [PARTIAL] No JSDoc comment. bannerMotd is a cryptic abbreviation (MOTD = mess... |
| `runPipeline` | L75–L166 | PARTIAL | 82% | [PARTIAL] A module-level JSDoc block (lines 6–18) describes the function's pu... |
| `createExecutor` | L170–L221 | PARTIAL | 72% | [PARTIAL] Not exported (internal). Has a meaningful inline comment explaining... |

### `src/cli/setup-table.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `SetupTableData` | L7–L12 | PARTIAL | 82% | [PARTIAL] No JSDoc comment present. The interface name is reasonably descript... |
| `shortModelName` | L14–L16 | UNDOCUMENTED | 90% | [UNDOCUMENTED] Exported function with no JSDoc at all. The two regex substitu... |
| `renderSetupTable` | L18–L120 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported 103-line function with no JSDoc. Neither parameter is... |

### `src/core/axes/documentation.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `DocumentationResponseSchema` | L32–L35 | UNDOCUMENTED | 88% | [UNDOCUMENTED] Exported Zod schema with no JSDoc comment. Callers integrating... |
| `buildDocumentationUserMessage` | L49–L115 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported 67-line function with no JSDoc at all. This is the mo... |
| `DocumentationEvaluator` | L121–L185 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported class implementing AxisEvaluator with no class-level ... |

### `src/core/axes/overengineering.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `OverengineeringResponseSchema` | L20–L22 | PARTIAL | 82% | [PARTIAL] Exported Zod schema with no JSDoc. The name is self-descriptive and... |
| `buildOverengineeringUserMessage` | L34–L84 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported 50-line function with no JSDoc. It builds a multi-sec... |
| `OverengineeringEvaluator` | L90–L140 | UNDOCUMENTED | 92% | [UNDOCUMENTED] Exported class implementing AxisEvaluator with no JSDoc on the... |

### `src/core/axis-merger.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `mergeAxisResults` | L38–L95 | PARTIAL | 88% | [PARTIAL] Has a multi-line JSDoc with five behaviour bullet points, which is ... |
| `applyCoherenceRules` | L200–L219 | PARTIAL | 88% | [PARTIAL] Has a JSDoc listing two coherence rules (DEAD→tests:NONE and ERROR→... |
| `synthesizeActionsFromSymbols` | L234–L324 | PARTIAL | 85% | [PARTIAL] Has a JSDoc stating it 'Generates actions for DEAD, DUPLICATE, OVER... |

### `src/core/deliberation.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `buildDeliberationSystemPrompt` | L51–L54 | PARTIAL | 75% | [PARTIAL] Exported function with no JSDoc. 3-line body: delegates to resolveS... |
| `buildDeliberationUserMessage` | L56–L80 | UNDOCUMENTED | 87% | [UNDOCUMENTED] Exported function with no JSDoc at all. At 25 lines it embeds ... |
| `applyDeliberation` | L155–L249 | PARTIAL | 82% | [PARTIAL] Exported function with a JSDoc listing 5 bullet points (reclassify ... |

### `src/core/doc-recommendations.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `DocGap` | L30–L40 | PARTIAL | 85% | [PARTIAL] Three fields have inline JSDoc (idealPath, section, existingUserPat... |
| `DocRecommendation` | L42–L50 | PARTIAL | 88% | [PARTIAL] No JSDoc at all — neither a top-level description nor field-level c... |
| `resolveUserPath` | L124–L148 | PARTIAL | 85% | [PARTIAL] Private unexported function with no JSDoc summary. Has inline comme... |

### `src/core/language-detect.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `ProjectProfile` | L65–L74 | PARTIAL | 85% | [PARTIAL] Three of five fields have inline JSDoc (types, capabilities, primar... |
| `FrameworkDef` | L121–L131 | PARTIAL | 72% | [PARTIAL] Non-exported internal interface with no JSDoc. Most fields (id, nam... |
| `detectProjectProfile` | L279–L290 | PARTIAL | 88% | [PARTIAL] JSDoc states 'Calls detectLanguages() then reads config files for d... |

## Hygiene

- [ ] <!-- ACT-18e23f-2 --> **[documentation · medium · trivial]** `src/cli/setup-table.ts`: Add JSDoc documentation for exported symbol: `shortModelName` (`shortModelName`) [L14-L16]
- [ ] <!-- ACT-18e23f-3 --> **[documentation · medium · trivial]** `src/cli/setup-table.ts`: Add JSDoc documentation for exported symbol: `renderSetupTable` (`renderSetupTable`) [L18-L120]
- [ ] <!-- ACT-0b0956-1 --> **[documentation · medium · trivial]** `src/core/axes/documentation.ts`: Add JSDoc documentation for exported symbol: `DocumentationResponseSchema` (`DocumentationResponseSchema`) [L32-L35]
- [ ] <!-- ACT-0b0956-3 --> **[documentation · medium · trivial]** `src/core/axes/documentation.ts`: Add JSDoc documentation for exported symbol: `buildDocumentationUserMessage` (`buildDocumentationUserMessage`) [L49-L115]
- [ ] <!-- ACT-0b0956-4 --> **[documentation · medium · trivial]** `src/core/axes/documentation.ts`: Add JSDoc documentation for exported symbol: `DocumentationEvaluator` (`DocumentationEvaluator`) [L121-L185]
- [ ] <!-- ACT-8637e1-2 --> **[documentation · medium · trivial]** `src/core/axes/overengineering.ts`: Add JSDoc documentation for exported symbol: `buildOverengineeringUserMessage` (`buildOverengineeringUserMessage`) [L34-L84]
- [ ] <!-- ACT-8637e1-3 --> **[documentation · medium · trivial]** `src/core/axes/overengineering.ts`: Add JSDoc documentation for exported symbol: `OverengineeringEvaluator` (`OverengineeringEvaluator`) [L90-L140]
- [ ] <!-- ACT-75e733-2 --> **[documentation · medium · trivial]** `src/core/deliberation.ts`: Add JSDoc documentation for exported symbol: `buildDeliberationUserMessage` (`buildDeliberationUserMessage`) [L56-L80]
- [ ] <!-- ACT-2c6f93-1 --> **[documentation · low · trivial]** `src/cli/pipeline-runner.ts`: Complete JSDoc documentation for: `PipelineContext` (`PipelineContext`) [L45-L57]
- [ ] <!-- ACT-2c6f93-2 --> **[documentation · low · trivial]** `src/cli/pipeline-runner.ts`: Complete JSDoc documentation for: `PipelineOptions` (`PipelineOptions`) [L59-L66]
- [ ] <!-- ACT-2c6f93-3 --> **[documentation · low · trivial]** `src/cli/pipeline-runner.ts`: Complete JSDoc documentation for: `runPipeline` (`runPipeline`) [L75-L166]
- [ ] <!-- ACT-2c6f93-4 --> **[documentation · low · trivial]** `src/cli/pipeline-runner.ts`: Complete JSDoc documentation for: `createExecutor` (`createExecutor`) [L170-L221]
- [ ] <!-- ACT-18e23f-1 --> **[documentation · low · trivial]** `src/cli/setup-table.ts`: Complete JSDoc documentation for: `SetupTableData` (`SetupTableData`) [L7-L12]
- [ ] <!-- ACT-8637e1-1 --> **[documentation · low · trivial]** `src/core/axes/overengineering.ts`: Complete JSDoc documentation for: `OverengineeringResponseSchema` (`OverengineeringResponseSchema`) [L20-L22]
- [ ] <!-- ACT-ccf474-1 --> **[documentation · low · trivial]** `src/core/axis-merger.ts`: Complete JSDoc documentation for: `mergeAxisResults` (`mergeAxisResults`) [L38-L95]
- [ ] <!-- ACT-ccf474-2 --> **[documentation · low · trivial]** `src/core/axis-merger.ts`: Complete JSDoc documentation for: `applyCoherenceRules` (`applyCoherenceRules`) [L200-L219]
- [ ] <!-- ACT-ccf474-3 --> **[documentation · low · trivial]** `src/core/axis-merger.ts`: Complete JSDoc documentation for: `synthesizeActionsFromSymbols` (`synthesizeActionsFromSymbols`) [L234-L324]
- [ ] <!-- ACT-75e733-1 --> **[documentation · low · trivial]** `src/core/deliberation.ts`: Complete JSDoc documentation for: `buildDeliberationSystemPrompt` (`buildDeliberationSystemPrompt`) [L51-L54]
- [ ] <!-- ACT-75e733-3 --> **[documentation · low · trivial]** `src/core/deliberation.ts`: Complete JSDoc documentation for: `applyDeliberation` (`applyDeliberation`) [L155-L249]
- [ ] <!-- ACT-a98a9a-1 --> **[documentation · low · trivial]** `src/core/doc-recommendations.ts`: Complete JSDoc documentation for: `DocGap` (`DocGap`) [L30-L40]
- [ ] <!-- ACT-a98a9a-2 --> **[documentation · low · trivial]** `src/core/doc-recommendations.ts`: Complete JSDoc documentation for: `DocRecommendation` (`DocRecommendation`) [L42-L50]
- [ ] <!-- ACT-a98a9a-3 --> **[documentation · low · trivial]** `src/core/doc-recommendations.ts`: Complete JSDoc documentation for: `resolveUserPath` (`resolveUserPath`) [L124-L148]
- [ ] <!-- ACT-4bc606-1 --> **[documentation · low · trivial]** `src/core/docs-resolver.ts`: Complete JSDoc documentation for: `RAG_MAX_SECTIONS` (`RAG_MAX_SECTIONS`) [L16-L16]
- [ ] <!-- ACT-4bc606-2 --> **[documentation · low · trivial]** `src/core/docs-resolver.ts`: Complete JSDoc documentation for: `RAG_MAX_LINES_PER_SECTION` (`RAG_MAX_LINES_PER_SECTION`) [L17-L17]
- [ ] <!-- ACT-4bc606-3 --> **[documentation · low · trivial]** `src/core/docs-resolver.ts`: Complete JSDoc documentation for: `resolveRelevantDocs` (`resolveRelevantDocs`) [L114-L173]
- [ ] <!-- ACT-4bc606-4 --> **[documentation · low · trivial]** `src/core/docs-resolver.ts`: Complete JSDoc documentation for: `resolveAllRelevantDocs` (`resolveAllRelevantDocs`) [L179-L208]
- [ ] <!-- ACT-48a7fc-1 --> **[documentation · low · trivial]** `src/core/language-detect.ts`: Complete JSDoc documentation for: `ProjectProfile` (`ProjectProfile`) [L65-L74]
- [ ] <!-- ACT-48a7fc-2 --> **[documentation · low · trivial]** `src/core/language-detect.ts`: Complete JSDoc documentation for: `FrameworkDef` (`FrameworkDef`) [L121-L131]
- [ ] <!-- ACT-48a7fc-3 --> **[documentation · low · trivial]** `src/core/language-detect.ts`: Complete JSDoc documentation for: `detectProjectProfile` (`detectProjectProfile`) [L279-L290]
- [ ] <!-- ACT-30cb08-1 --> **[documentation · low · trivial]** `src/schemas/review.ts`: Complete JSDoc documentation for: `SymbolReviewSchema` (`SymbolReviewSchema`) [L17-L37]
- [ ] <!-- ACT-30cb08-2 --> **[documentation · low · trivial]** `src/schemas/review.ts`: Complete JSDoc documentation for: `BestPracticesRuleSchema` (`BestPracticesRuleSchema`) [L77-L84]
- [ ] <!-- ACT-30cb08-3 --> **[documentation · low · trivial]** `src/schemas/review.ts`: Complete JSDoc documentation for: `DocRecommendationSchema` (`DocRecommendationSchema`) [L113-L121]
- [ ] <!-- ACT-30cb08-4 --> **[documentation · low · trivial]** `src/schemas/review.ts`: Complete JSDoc documentation for: `ReviewFileSchema` (`ReviewFileSchema`) [L137-L182]

## Documentation Coverage

### `src/core/docs-resolver.ts` — 25% covered

- [ ] **RAG-based documentation resolution** — PARTIAL → `02-Architecture/03-RAG-Engine.md`: docs/rag.md and 02-Architecture/03-RAG-Engine.md likely cover the RAG engine in general, but the specific doc-resolver concern (per-file NLP vector lookup, section-level retrieval, token budget enforcement) may only be partially addressed at the architectural level without covering the implementation details in docs-resolver.ts.
- [ ] **Config-driven module-to-docs mapping (module_mapping)** — PARTIAL → `03-Guides/02-Advanced-Configuration.md`: 01-Getting-Started/02-Configuration.md and 03-Guides/02-Advanced-Configuration.md plausibly cover module_mapping config, but the two-phase resolution strategy (config-first, convention fallback) implemented in resolveRelevantDocs is a non-obvious behaviour unlikely to be fully described in configuration reference pages.
- [ ] **Doc token budget management (DOC_BUDGET_RATIO / getDocTokenBudget)** — MISSING: No documentation page in the provided tree has an obvious focus on the 20% context-window token budget for doc injection or the per-source budget splitting used in resolveRelevantDocsViaRag. This is a significant operational constraint with no apparent dedicated documentation.
- [ ] **Docs directory tree building (buildDocsTree)** — MISSING: The ASCII docs tree used as a site-map for the LLM evaluator is not addressed by any visible documentation page. It is an internal mechanism with no coverage in the provided docs tree.

### `src/cli/pipeline-runner.ts` — 20% covered

- [ ] **Pipeline runner / runPipeline abstraction** — PARTIAL → `02-Architecture/01-Pipeline-Overview.md`: The Pipeline-Overview doc likely covers the high-level pipeline architecture, but the specific shared-runner abstraction (PipelineContext, PipelineOptions, the banner+cost-tracking boilerplate pattern) has no dedicated API-reference page. The actual types and their contract are not discoverable from documentation alone.
- [ ] **PipelineContext / PipelineOptions public API types** — MISSING: No docs page in the provided directory tree covers these exported interfaces. There is no API reference entry for pipeline-runner.ts or the types it exports.
- [ ] **Cost tracking (addCost / totalCostUsd)** — MISSING: The per-command cost accumulation mechanism (addCost callback, PipelineResult.totalCostUsd) is not referenced in any visible documentation page. 07-Design-Decisions/03-Cost-Optimization.md likely covers cost philosophy but not the runtime API.
- [ ] **LLM executor / createExecutor with retryWithBackoff** — MISSING: The internal executor factory that wires the Claude Agent SDK query loop with exponential back-off retry logic is not referenced in any documentation page.

### `src/core/axes/overengineering.ts` — 40% covered

- [ ] **Overengineering axis** — PARTIAL → `02-Architecture/02-Seven-Axis-System.md`: The Seven-Axis-System doc likely names overengineering as one of the seven axes, but based on file listing alone the depth of coverage for its LEAN/OVER/ACCEPTABLE classification scheme, heuristics (directory fragmentation, nesting depth), and prompt design cannot be confirmed.
- [ ] **OverengineeringEvaluator / axis evaluator pattern** — PARTIAL → `04-Core-Modules/04-Axis-Evaluators.md`: The Axis-Evaluators doc likely covers the AxisEvaluator interface and general pattern, but whether it documents the overengineering-specific fields (defaultModel: sonnet, project-tree heuristics injected into the user message) is uncertain from the directory listing alone.

### `src/core/axis-merger.ts` — 20% covered

- [ ] **Axis result merging (mergeAxisResults)** — PARTIAL → `02-Architecture/01-Pipeline-Overview.md`: The pipeline overview likely covers the merge step at a high level, but there is no dedicated module page for axis-merger.ts in 04-Core-Modules/ (which ends at 06-Worker-Pool.md). The specific merging contract, parameter semantics, and ReviewFile v2 output are not documented anywhere visible.
- [ ] **Inter-axis coherence rules** — MISSING: No documentation page in the provided directory listing appears to cover the specific coherence rules enforced after merging (DEAD→tests:NONE, ERROR→overengineering:ACCEPTABLE, DEAD→documentation:UNDOCUMENTED). These are policy decisions with user-visible effects that are entirely absent from docs.
- [ ] **Action synthesis from symbol findings** — MISSING: The automatic synthesis of structured actions from symbol-level findings (DEAD, DUPLICATE, OVER, LOW_VALUE, UNDOCUMENTED, PARTIAL) is not represented in any visible documentation page. Callers have no way to predict which findings produce actions.
- [ ] **Contradiction detection between axes** — MISSING: The mechanism for detecting correction/best_practices contradictions and downgrading confidence to exclude findings from the verdict is a significant undocumented implementation detail. It is mentioned only in the inline JSDoc of detectContradictions itself.
- [ ] **Verdict computation (CLEAN / NEEDS_REFACTOR / CRITICAL)** — PARTIAL → `04-Core-Modules/05-Reporter.md`: Verdict outcome labels are likely described in the Reporter module docs and possibly in 03-CLI-Reference/03-Output-Formats.md, but the computation thresholds (partialDocCount>=3, VERDICT_CONFIDENCE_THRESHOLD=60, confidence gating) are not exposed in any visible documentation page.

### `src/core/deliberation.ts` — 70% covered

- [ ] **DeliberationResponse / DeliberatedSymbol schema contract** — PARTIAL → `02-Architecture/05-Deliberation-Pass.md`: Schema shapes (AxisVerdictSchema, DeliberatedSymbolSchema, DeliberationResponseSchema) may be referenced in the architecture page but are unlikely to be fully specified. Detailed schema documentation would belong in 06-Development/03-Schemas.md, which exists but may not cover deliberation schemas specifically.

### `src/core/doc-recommendations.ts` — 10% covered

- [ ] **Dual-Output Documentation Recommendations (DocGap → DocRecommendation)** — MISSING: The core concept of this module — transforming raw DocGap objects into dual-path DocRecommendation objects with path_ideal and path_user — is not covered in any page under docs/. The 04-Core-Modules directory covers Scanner, Estimator, Triage, Axis-Evaluators, Reporter, and Worker-Pool, but not the recommendation builder.
- [ ] **UserDocPlan concept-based section mappings** — MISSING: The mechanism by which UserDocPlan.sectionMappings drives user-side path resolution is not documented in any visible docs/ page. No architecture or module page references this path-mapping strategy.
- [ ] **RecommendationType values and their semantics** — MISSING: The eight recommendation types (missing_page, missing_section, outdated_content, empty_page, broken_link, missing_index_entry, missing_jsdoc, incomplete_jsdoc) are not described in any docs/ page.

### `src/core/language-detect.ts` — 0% covered

- [ ] **Language Detection (file-extension-based language distribution)** — MISSING: No documentation page in docs/ appears to cover the language detection module or its algorithm (git-tracked file scanning, extension mapping, 1% threshold filter). The 04-Core-Modules/ directory lists Scanner, Estimator, Triage, Axis-Evaluators, Reporter, and Worker-Pool — no language-detect entry is visible.
- [ ] **Framework Detection (dependency/config-file-based registry)** — MISSING: No documentation page covers the framework detection logic (JS/TS package.json deps, Python requirements.txt/pyproject.toml, Rust Cargo.toml, Go go.mod, C# .csproj, Java pom.xml, Next.js config fallback). The suppression mechanism and framework registry are entirely absent from visible docs pages.
- [ ] **ProjectProfile (aggregate project type + capabilities output)** — MISSING: The ProjectProfile type and its constituent ProjectType/ProjectCapability derivation (Monorepo from workspaces, CLI from bin field, containerized from Dockerfile presence, graphql-api from .graphql files) are not referenced in any visible documentation page.
