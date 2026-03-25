# Documentation — Shard 2

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `scripts/lib/json-helpers.sh` | NEEDS_REFACTOR | 6 | 95% | [details](../reviews/scripts-lib-json-helpers.rev.md) |
| `src/commands/run.ts` | NEEDS_REFACTOR | 6 | 95% | [details](../reviews/src-commands-run.rev.md) |
| `src/rag/orchestrator.ts` | NEEDS_REFACTOR | 6 | 92% | [details](../reviews/src-rag-orchestrator.rev.md) |
| `scripts/setup-embeddings.sh` | NEEDS_REFACTOR | 6 | 90% | [details](../reviews/scripts-setup-embeddings.rev.md) |
| `scripts/lib/logging.sh` | NEEDS_REFACTOR | 6 | 88% | [details](../reviews/scripts-lib-logging.rev.md) |
| `src/prompts/__gold-set__/mixed-lang-sql.ts` | NEEDS_REFACTOR | 5 | 95% | [details](../reviews/src-prompts-__gold-set__-mixed-lang-sql.rev.md) |
| `src/core/doc-report-aggregator.ts` | NEEDS_REFACTOR | 5 | 92% | [details](../reviews/src-core-doc-report-aggregator.rev.md) |
| `src/core/axes/tests.ts` | NEEDS_REFACTOR | 4 | 95% | [details](../reviews/src-core-axes-tests.rev.md) |
| `src/core/badge.ts` | NEEDS_REFACTOR | 4 | 95% | [details](../reviews/src-core-badge.rev.md) |
| `src/core/doc-generator.ts` | NEEDS_REFACTOR | 4 | 95% | [details](../reviews/src-core-doc-generator.rev.md) |

## Symbol Details

### `scripts/lib/json-helpers.sh`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `json_get_sample_raw` | L48–L52 | PARTIAL | 82% | [PARTIAL] Has a one-line description ('Get a sample's raw text (unquoted) for... |
| `json_get_code_label` | L55–L59 | PARTIAL | 80% | [PARTIAL] Has a brief description ('Extract label from a code sample (first f... |
| `json_get_sample_len` | L62–L66 | PARTIAL | 80% | [PARTIAL] Description ('Get character count of a sample') is brief and techni... |
| `cosine_similarity` | L105–L117 | PARTIAL | 85% | [PARTIAL] Has a purpose comment and usage example, but the two edge-case retu... |
| `write_ab_results` | L124–L130 | PARTIAL | 80% | [PARTIAL] Comment names the output file and vaguely notes it accepts a jq fil... |
| `write_embeddings_ready` | L133–L139 | PARTIAL | 80% | [PARTIAL] Only has 'Write embeddings-ready.json' — does not describe the requ... |

### `src/commands/run.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `RunContext` | L58–L124 | PARTIAL | 85% | [PARTIAL] No interface-level JSDoc. About half the fields carry inline /** */... |
| `registerRunCommand` | L126–L397 | UNDOCUMENTED | 95% | [UNDOCUMENTED] The sole exported public symbol in the file has no JSDoc comme... |
| `runDocBootstrap` | L796–L837 | PARTIAL | 85% | [PARTIAL] Has a JSDoc block describing purpose ('scaffold structure + generat... |
| `runDocUpdate` | L843–L889 | PARTIAL | 85% | [PARTIAL] Has a JSDoc block describing purpose ('regenerate only pages whose ... |
| `copyCachedReviews` | L1549–L1578 | PARTIAL | 85% | [PARTIAL] Has a meaningful JSDoc block explaining purpose and the invariant i... |
| `cacheReview` | L1584–L1594 | PARTIAL | 85% | [PARTIAL] Has a JSDoc block stating purpose and the overwrite policy ('always... |

### `src/rag/orchestrator.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `RagMode` | L21–L21 | UNDOCUMENTED | 85% | [UNDOCUMENTED] No JSDoc on the exported type. The semantic distinction betwee... |
| `RagIndexOptions` | L23–L48 | PARTIAL | 85% | [PARTIAL] No top-level JSDoc on the interface. Approximately half the fields ... |
| `RagIndexResult` | L50–L62 | PARTIAL | 80% | [PARTIAL] No top-level JSDoc on the interface. Three fields have inline JSDoc... |
| `readAndBuildCards` | L85–L114 | PARTIAL | 72% | [PARTIAL] Private unexported function with a one-liner JSDoc ('Read source an... |
| `processFileForDualIndex` | L120–L207 | PARTIAL | 88% | [PARTIAL] Exported function with a JSDoc describing overall purpose. However,... |
| `indexProject` | L219–L459 | PARTIAL | 85% | [PARTIAL] The multi-line JSDoc describing the RAG indexing phase ('Run the RA... |

### `scripts/setup-embeddings.sh`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `PROJECT_ROOT` | L18–L18 | PARTIAL | 80% | [PARTIAL] Name is clear, but the env-var override ANATOLY_PROJECT_ROOT is men... |
| `GGUF_MIN_VRAM_GB` | L35–L35 | PARTIAL | 80% | [PARTIAL] Name is clear but the magic value 12 (GB) has no comment explaining... |
| `download_gguf_model` | L81–L150 | PARTIAL | 85% | [PARTIAL] Section banner 'GGUF model download (curl with resume support)' giv... |
| `BACKEND` | L456–L456 | PARTIAL | 86% | [UNDOCUMENTED] Hardcoded to 'advanced-gguf' at this point after the smoke tes... |
| `NLP_DIM` | L457–L457 | PARTIAL | 86% | [UNDOCUMENTED] Hardcoded embedding dimension of 4096 for the NLP model; no co... |
| `CODE_DIM` | L458–L458 | PARTIAL | 86% | [UNDOCUMENTED] Fallback dimension of 3584 for the code model; no comment link... |

### `scripts/lib/logging.sh`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `_LOG_FILE` | L32–L32 | PARTIAL | 88% | [UNDOCUMENTED] Only covered by the section header comment '# State', which ap... |
| `LOG_LEVEL` | L33–L33 | PARTIAL | 80% | [PARTIAL] The file-level usage block at the top shows 'LOG_LEVEL=debug' as an... |
| `log_init` | L38–L42 | PARTIAL | 80% | [PARTIAL] Section comment 'Initialise log file (call once at script start)' d... |
| `log` | L47–L79 | PARTIAL | 80% | [PARTIAL] Section comment only says 'Core log function'. File-level header pr... |
| `log_section` | L84–L90 | PARTIAL | 75% | [PARTIAL] Covered by section comment 'Section separator (visual only)', which... |
| `log_separator` | L92–L94 | PARTIAL | 86% | [UNDOCUMENTED] No dedicated comment. Falls under the same section header as l... |

### `src/prompts/__gold-set__/mixed-lang-sql.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `getActiveUsers` | L21–L29 | PARTIAL | 88% | [UNDOCUMENTED] Exported public function with no JSDoc. While the name is desc... |
| `searchUsers` | L40–L65 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported public function with non-trivial pagination semantics... |
| `insertUser` | L67–L74 | PARTIAL | 87% | [UNDOCUMENTED] Exported public function with no JSDoc. The return value — the... |
| `deactivateUser` | L76–L84 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported public function with no JSDoc. The boolean return val... |
| `deleteInactiveUsers` | L86–L94 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported public function with no JSDoc. The return value (coun... |

### `src/core/doc-report-aggregator.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `DocReportInput` | L28–L41 | PARTIAL | 85% | [PARTIAL] No top-level JSDoc on the interface itself. Four fields carry inlin... |
| `aggregateDocReport` | L55–L77 | PARTIAL | 88% | [PARTIAL] Has a single-line JSDoc ('Aggregates all documentation data into re... |
| `buildScoringInput` | L120–L187 | PARTIAL | 85% | [PARTIAL] Has a one-line JSDoc ('Builds scoring input from reviews and doc pa... |
| `buildGapsFromReviews` | L192–L248 | PARTIAL | 85% | [PARTIAL] One-line JSDoc for a ~56-line function that implements multiple gap... |
| `buildReportStats` | L253–L291 | PARTIAL | 85% | [PARTIAL] One-line JSDoc ('Builds report stats from pipeline data.') for a ~3... |

### `src/core/axes/tests.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `TestsResponseSchema` | L21–L23 | PARTIAL | 82% | [PARTIAL] Exported Zod schema with no JSDoc. Name is descriptive and the stru... |
| `buildTestsSystemPrompt` | L31–L33 | PARTIAL | 82% | [PARTIAL] Exported 3-line wrapper delegating to resolveSystemPrompt('tests') ... |
| `buildTestsUserMessage` | L35–L119 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported 85-line function with no JSDoc whatsoever. Non-obviou... |
| `TestsEvaluator` | L125–L175 | UNDOCUMENTED | 88% | [UNDOCUMENTED] Exported class implementing AxisEvaluator with no JSDoc on the... |

### `src/core/badge.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `BadgeOptions` | L18–L23 | PARTIAL | 85% | [PARTIAL] Exported interface with no JSDoc. Fields projectRoot, link, and ver... |
| `BadgeResult` | L25–L28 | PARTIAL | 85% | [PARTIAL] Exported interface with no JSDoc. The distinction between the three... |
| `buildBadgeMarkdown` | L30–L55 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported public function with no JSDoc whatsoever. Non-trivial... |
| `injectBadge` | L57–L95 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported public function with no JSDoc. Significant undocument... |

### `src/core/doc-generator.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `DocNeighbor` | L41–L44 | PARTIAL | 88% | [PARTIAL] Has a JSDoc block immediately above it, but that comment ('Builds t... |
| `buildPagePrompt` | L46–L56 | UNDOCUMENTED | 92% | [UNDOCUMENTED] The exported main entry point has no JSDoc attached. The inten... |
| `buildUserMessage` | L77–L210 | PARTIAL | 85% | [UNDOCUMENTED] Private unexported function but 133 lines long with 6 paramete... |
| `formatSymbol` | L212–L232 | PARTIAL | 75% | [PARTIAL] Private ~21-line function. Name is self-descriptive (formats a Symb... |

## Hygiene

- [ ] <!-- ACT-e1e39d-11 --> **[documentation · medium · trivial]** `scripts/setup-embeddings.sh`: Add JSDoc documentation for exported symbol: `BACKEND` (`BACKEND`) [L456-L456]
- [ ] <!-- ACT-e1e39d-12 --> **[documentation · medium · trivial]** `scripts/setup-embeddings.sh`: Add JSDoc documentation for exported symbol: `NLP_DIM` (`NLP_DIM`) [L457-L457]
- [ ] <!-- ACT-e1e39d-13 --> **[documentation · medium · trivial]** `scripts/setup-embeddings.sh`: Add JSDoc documentation for exported symbol: `CODE_DIM` (`CODE_DIM`) [L458-L458]
- [ ] <!-- ACT-284cb7-2 --> **[documentation · medium · trivial]** `src/commands/run.ts`: Add JSDoc documentation for exported symbol: `registerRunCommand` (`registerRunCommand`) [L126-L397]
- [ ] <!-- ACT-147251-3 --> **[documentation · medium · trivial]** `src/core/axes/tests.ts`: Add JSDoc documentation for exported symbol: `buildTestsUserMessage` (`buildTestsUserMessage`) [L35-L119]
- [ ] <!-- ACT-147251-4 --> **[documentation · medium · trivial]** `src/core/axes/tests.ts`: Add JSDoc documentation for exported symbol: `TestsEvaluator` (`TestsEvaluator`) [L125-L175]
- [ ] <!-- ACT-0e9a36-3 --> **[documentation · medium · trivial]** `src/core/badge.ts`: Add JSDoc documentation for exported symbol: `buildBadgeMarkdown` (`buildBadgeMarkdown`) [L30-L55]
- [ ] <!-- ACT-0e9a36-4 --> **[documentation · medium · trivial]** `src/core/badge.ts`: Add JSDoc documentation for exported symbol: `injectBadge` (`injectBadge`) [L57-L95]
- [ ] <!-- ACT-50ae10-2 --> **[documentation · medium · trivial]** `src/core/doc-generator.ts`: Add JSDoc documentation for exported symbol: `buildPagePrompt` (`buildPagePrompt`) [L46-L56]
- [ ] <!-- ACT-ba090d-1 --> **[documentation · medium · trivial]** `src/prompts/__gold-set__/mixed-lang-sql.ts`: Add JSDoc documentation for exported symbol: `getActiveUsers` (`getActiveUsers`) [L21-L29]
- [ ] <!-- ACT-ba090d-3 --> **[documentation · medium · trivial]** `src/prompts/__gold-set__/mixed-lang-sql.ts`: Add JSDoc documentation for exported symbol: `searchUsers` (`searchUsers`) [L40-L65]
- [ ] <!-- ACT-ba090d-4 --> **[documentation · medium · trivial]** `src/prompts/__gold-set__/mixed-lang-sql.ts`: Add JSDoc documentation for exported symbol: `insertUser` (`insertUser`) [L67-L74]
- [ ] <!-- ACT-ba090d-5 --> **[documentation · medium · trivial]** `src/prompts/__gold-set__/mixed-lang-sql.ts`: Add JSDoc documentation for exported symbol: `deactivateUser` (`deactivateUser`) [L76-L84]
- [ ] <!-- ACT-ba090d-6 --> **[documentation · medium · trivial]** `src/prompts/__gold-set__/mixed-lang-sql.ts`: Add JSDoc documentation for exported symbol: `deleteInactiveUsers` (`deleteInactiveUsers`) [L86-L94]
- [ ] <!-- ACT-267589-1 --> **[documentation · medium · trivial]** `src/rag/orchestrator.ts`: Add JSDoc documentation for exported symbol: `RagMode` (`RagMode`) [L21-L21]
- [ ] <!-- ACT-cbd2bd-1 --> **[documentation · low · trivial]** `scripts/lib/json-helpers.sh`: Complete JSDoc documentation for: `json_get_sample_raw` (`json_get_sample_raw`) [L48-L52]
- [ ] <!-- ACT-cbd2bd-2 --> **[documentation · low · trivial]** `scripts/lib/json-helpers.sh`: Complete JSDoc documentation for: `json_get_code_label` (`json_get_code_label`) [L55-L59]
- [ ] <!-- ACT-cbd2bd-3 --> **[documentation · low · trivial]** `scripts/lib/json-helpers.sh`: Complete JSDoc documentation for: `json_get_sample_len` (`json_get_sample_len`) [L62-L66]
- [ ] <!-- ACT-cbd2bd-4 --> **[documentation · low · trivial]** `scripts/lib/json-helpers.sh`: Complete JSDoc documentation for: `cosine_similarity` (`cosine_similarity`) [L105-L117]
- [ ] <!-- ACT-cbd2bd-5 --> **[documentation · low · trivial]** `scripts/lib/json-helpers.sh`: Complete JSDoc documentation for: `write_ab_results` (`write_ab_results`) [L124-L130]
- [ ] <!-- ACT-cbd2bd-6 --> **[documentation · low · trivial]** `scripts/lib/json-helpers.sh`: Complete JSDoc documentation for: `write_embeddings_ready` (`write_embeddings_ready`) [L133-L139]
- [ ] <!-- ACT-e1e39d-1 --> **[documentation · low · trivial]** `scripts/setup-embeddings.sh`: Complete JSDoc documentation for: `PROJECT_ROOT` (`PROJECT_ROOT`) [L18-L18]
- [ ] <!-- ACT-e1e39d-2 --> **[documentation · low · trivial]** `scripts/setup-embeddings.sh`: Complete JSDoc documentation for: `GGUF_MIN_VRAM_GB` (`GGUF_MIN_VRAM_GB`) [L35-L35]
- [ ] <!-- ACT-e1e39d-4 --> **[documentation · low · trivial]** `scripts/setup-embeddings.sh`: Complete JSDoc documentation for: `download_gguf_model` (`download_gguf_model`) [L81-L150]
- [ ] <!-- ACT-284cb7-1 --> **[documentation · low · trivial]** `src/commands/run.ts`: Complete JSDoc documentation for: `RunContext` (`RunContext`) [L58-L124]
- [ ] <!-- ACT-284cb7-3 --> **[documentation · low · trivial]** `src/commands/run.ts`: Complete JSDoc documentation for: `runDocBootstrap` (`runDocBootstrap`) [L796-L837]
- [ ] <!-- ACT-284cb7-4 --> **[documentation · low · trivial]** `src/commands/run.ts`: Complete JSDoc documentation for: `runDocUpdate` (`runDocUpdate`) [L843-L889]
- [ ] <!-- ACT-284cb7-5 --> **[documentation · low · trivial]** `src/commands/run.ts`: Complete JSDoc documentation for: `copyCachedReviews` (`copyCachedReviews`) [L1549-L1578]
- [ ] <!-- ACT-284cb7-6 --> **[documentation · low · trivial]** `src/commands/run.ts`: Complete JSDoc documentation for: `cacheReview` (`cacheReview`) [L1584-L1594]
- [ ] <!-- ACT-147251-1 --> **[documentation · low · trivial]** `src/core/axes/tests.ts`: Complete JSDoc documentation for: `TestsResponseSchema` (`TestsResponseSchema`) [L21-L23]
- [ ] <!-- ACT-147251-2 --> **[documentation · low · trivial]** `src/core/axes/tests.ts`: Complete JSDoc documentation for: `buildTestsSystemPrompt` (`buildTestsSystemPrompt`) [L31-L33]
- [ ] <!-- ACT-0e9a36-1 --> **[documentation · low · trivial]** `src/core/badge.ts`: Complete JSDoc documentation for: `BadgeOptions` (`BadgeOptions`) [L18-L23]
- [ ] <!-- ACT-0e9a36-2 --> **[documentation · low · trivial]** `src/core/badge.ts`: Complete JSDoc documentation for: `BadgeResult` (`BadgeResult`) [L25-L28]
- [ ] <!-- ACT-50ae10-1 --> **[documentation · low · trivial]** `src/core/doc-generator.ts`: Complete JSDoc documentation for: `DocNeighbor` (`DocNeighbor`) [L41-L44]
- [ ] <!-- ACT-50ae10-3 --> **[documentation · low · trivial]** `src/core/doc-generator.ts`: Complete JSDoc documentation for: `formatSymbol` (`formatSymbol`) [L212-L232]
- [ ] <!-- ACT-b75a9a-1 --> **[documentation · low · trivial]** `src/core/doc-report-aggregator.ts`: Complete JSDoc documentation for: `DocReportInput` (`DocReportInput`) [L28-L41]
- [ ] <!-- ACT-b75a9a-2 --> **[documentation · low · trivial]** `src/core/doc-report-aggregator.ts`: Complete JSDoc documentation for: `aggregateDocReport` (`aggregateDocReport`) [L55-L77]
- [ ] <!-- ACT-b75a9a-3 --> **[documentation · low · trivial]** `src/core/doc-report-aggregator.ts`: Complete JSDoc documentation for: `buildScoringInput` (`buildScoringInput`) [L120-L187]
- [ ] <!-- ACT-b75a9a-4 --> **[documentation · low · trivial]** `src/core/doc-report-aggregator.ts`: Complete JSDoc documentation for: `buildGapsFromReviews` (`buildGapsFromReviews`) [L192-L248]
- [ ] <!-- ACT-b75a9a-5 --> **[documentation · low · trivial]** `src/core/doc-report-aggregator.ts`: Complete JSDoc documentation for: `buildReportStats` (`buildReportStats`) [L253-L291]
- [ ] <!-- ACT-267589-2 --> **[documentation · low · trivial]** `src/rag/orchestrator.ts`: Complete JSDoc documentation for: `RagIndexOptions` (`RagIndexOptions`) [L23-L48]
- [ ] <!-- ACT-267589-3 --> **[documentation · low · trivial]** `src/rag/orchestrator.ts`: Complete JSDoc documentation for: `RagIndexResult` (`RagIndexResult`) [L50-L62]
- [ ] <!-- ACT-267589-4 --> **[documentation · low · trivial]** `src/rag/orchestrator.ts`: Complete JSDoc documentation for: `readAndBuildCards` (`readAndBuildCards`) [L85-L114]
- [ ] <!-- ACT-267589-5 --> **[documentation · low · trivial]** `src/rag/orchestrator.ts`: Complete JSDoc documentation for: `processFileForDualIndex` (`processFileForDualIndex`) [L120-L207]
- [ ] <!-- ACT-267589-6 --> **[documentation · low · trivial]** `src/rag/orchestrator.ts`: Complete JSDoc documentation for: `indexProject` (`indexProject`) [L219-L459]

## Documentation Coverage

### `src/commands/run.ts` — 55% covered

- [ ] **First-run doc bootstrap (internal docs auto-generation)** — MISSING: Story 29.21 introduces automatic .anatoly/docs/ scaffolding and LLM-driven page generation on first run. No docs page in the tree clearly covers this bootstrap mechanism or the internal-docs update phase.
- [ ] **Badge injection** — MISSING: The automatic README badge injection triggered by the --badge / --badge-verdict flags after each run has no dedicated docs page and does not appear to be covered by any entry in the documentation tree.
- [ ] **Run calibration and ETA estimation** — PARTIAL → `04-Core-Modules/02-Estimator.md`: The estimator module doc likely covers token-based estimation, but the calibration feedback loop (recalibrateFromRuns, saveCalibration) that adjusts per-axis timing from historical runs is a distinct concept that may not be fully described.

### `scripts/lib/logging.sh` — 0% covered

- [ ] **scripts/lib/logging.sh — bash logging library** — MISSING: No page in the docs/ tree documents the setup-script logging library: its functions (log, log_init, log_section, log_separator), the LOG_LEVEL env variable, dual-output behavior, or color support. The closest candidates (06-Development/00-Source-Tree.md, 01-Getting-Started/01-Installation.md) do not appear to cover this utility.

### `src/core/doc-report-aggregator.ts` — 15% covered

- [ ] **Documentation Report Aggregation pipeline step** — PARTIAL → `04-Core-Modules/05-Reporter.md`: The Reporter module doc likely covers the high-level reporting step, but the specific aggregateDocReport pipeline (resolve plan → score → gaps → render) as a distinct orchestration layer is probably not documented at this granularity. Rated PARTIAL due to structural coverage in Reporter without detail on the aggregator's role.
- [ ] **5-dimension documentation scoring** — MISSING: The DocScoringInput with its five dimensions (project exports, internal exports, modules, content quality, page count) is a domain-specific concept. No docs page in the provided listing appears to cover this scoring model explicitly. The Seven-Axis-System doc covers the review axes, not the doc-quality scoring computed in doc-scoring.js.
- [ ] **Documentation gap detection from review data** — MISSING: The buildGapsFromReviews logic — deriving DocGap entries from undocumented exports and docs_coverage concepts in reviews — is not covered by any visible docs page. This is a key part of the documentation feedback loop but has no corresponding documentation entry.
- [ ] **User docs/ scanning and DocPageEntry construction** — MISSING: The scanUserDocs mechanism (recursive .md scan, headContent sampling of first 5 lines, relative path normalisation) is not referenced in any docs page. No entry in 03-Guides, 04-Core-Modules, or 05-Modules appears to describe how the user's docs/ directory is ingested.
- [ ] **Dual-output recommendations (user docs sync)** — MISSING: The concept of building dual-output recommendations (referenced in the file-level header comment as step 3) driven by gap analysis is not documented in any visible docs page. The closest candidates (05-Reporter.md, 01-Pipeline-Overview.md) do not appear to cover this by name.

### `src/core/axes/tests.ts` — 35% covered

- [ ] **Tests axis (GOOD/WEAK/NONE rating scale)** — PARTIAL → `02-Architecture/02-Seven-Axis-System.md`: The seven-axis system doc likely names 'tests' as one of the axes, but the three-value GOOD/WEAK/NONE classification scheme, the 500-line test-file truncation heuristic, and the coverage-data table format are implementation details unlikely to be fully described in architecture-level documentation.
- [ ] **TestsEvaluator class and its evaluate contract** — PARTIAL → `04-Core-Modules/04-Axis-Evaluators.md`: The Axis-Evaluators module doc presumably covers the AxisEvaluator interface and the general evaluator pattern, but the tests-specific defaultModel, semaphore usage, conversationDir/conversationFileSlug wiring, and the exact LLM response schema are unlikely to be individually documented.

### `src/core/badge.ts` — 0% covered

- [ ] **Badge injection into README.md** — MISSING: No documentation page in docs/ appears to cover the badge injection feature — neither a dedicated page nor an obvious section in the available file listing (CLI reference, integration guides, core-module pages). The closest candidates by name would be 05-Integration/02-CI-CD.md or 03-CLI-Reference/03-Output-Formats.md, but neither title suggests badge semantics. The feature's public API (buildBadgeMarkdown, injectBadge, BadgeOptions, BadgeResult) is entirely absent from documented concepts.
- [ ] **Verdict-colored badge variants** — MISSING: The color-map and label-map linking Verdict values (CLEAN/NEEDS_REFACTOR/CRITICAL) to badge colors (brightgreen/yellow/red) and human-readable labels is a user-facing feature with no corresponding documentation page in the docs/ tree.

### `src/core/doc-generator.ts` — 0% covered

- [ ] **LLM documentation generation (doc-generator module)** — MISSING: The doc-generator module — responsible for building system and user prompts for LLM-based documentation page generation — has no corresponding page in docs/. The 04-Core-Modules/ section documents Scanner, Estimator, Triage, Axis-Evaluators, Reporter, and Worker-Pool but omits doc-generator entirely.
- [ ] **Per-page-type prompt specialisation (architecture vs API-reference rules)** — MISSING: The logic of appending specialised prompt fragments for 02-Architecture/ and 04-API-Reference/ page paths (buildSystemPrompt branching) is not described anywhere in the documentation tree.
- [ ] **DocNeighbor / related-page context injection** — MISSING: The concept of injecting neighbouring page content (truncated to 80 lines) into LLM prompts for cross-reference awareness is not documented in any docs/ page.
