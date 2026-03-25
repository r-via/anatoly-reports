# Documentation — Shard 1

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `src/core/doc-gap-detection.ts` | NEEDS_REFACTOR | 12 | 95% | [details](../reviews/src-core-doc-gap-detection.rev.md) |
| `src/core/reporter.ts` | NEEDS_REFACTOR | 12 | 95% | [details](../reviews/src-core-reporter.rev.md) |
| `src/core/axes/correction.ts` | NEEDS_REFACTOR | 9 | 95% | [details](../reviews/src-core-axes-correction.rev.md) |
| `src/core/language-adapters.ts` | NEEDS_REFACTOR | 9 | 90% | [details](../reviews/src-core-language-adapters.rev.md) |
| `src/commands/clean-run.ts` | NEEDS_REFACTOR | 8 | 95% | [details](../reviews/src-commands-clean-run.rev.md) |
| `src/rag/doc-indexer.ts` | NEEDS_REFACTOR | 8 | 95% | [details](../reviews/src-rag-doc-indexer.rev.md) |
| `scripts/lib/hardware.sh` | NEEDS_REFACTOR | 8 | 90% | [details](../reviews/scripts-lib-hardware.rev.md) |
| `scripts/lib/docker-helpers.sh` | NEEDS_REFACTOR | 7 | 95% | [details](../reviews/scripts-lib-docker-helpers.rev.md) |
| `src/core/axes/best-practices.ts` | NEEDS_REFACTOR | 7 | 95% | [details](../reviews/src-core-axes-best-practices.rev.md) |
| `src/core/axis-evaluator.ts` | NEEDS_REFACTOR | 7 | 95% | [details](../reviews/src-core-axis-evaluator.rev.md) |

## Symbol Details

### `src/core/doc-gap-detection.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `GapReportV2` | L58–L78 | PARTIAL | 80% | [PARTIAL] Exported interface has inline section comments (// Strategy 1, // S... |
| `GapDetectionV2Options` | L80–L92 | PARTIAL | 80% | [PARTIAL] Inline comments document default values for threshold fields (e.g.,... |
| `classifyPage` | L98–L105 | PARTIAL | 80% | [PARTIAL] Exported function with a self-descriptive name and typed parameter.... |
| `groupByDomain` | L142–L175 | PARTIAL | 78% | [PARTIAL] Private ~33-line function with inline comments explaining the two-p... |
| `groupByPage` | L189–L219 | PARTIAL | 78% | [PARTIAL] Private ~30-line async function with no JSDoc. Inline comments note... |
| `strategy1_moduleDomains` | L225–L316 | PARTIAL | 78% | [PARTIAL] Private ~90-line function. The file-level module comment documents ... |
| `strategy2_referencePages` | L322–L378 | PARTIAL | 78% | [PARTIAL] Private ~55-line function. File header mentions Strategy 2 briefly.... |
| `strategy3_conceptualPages` | L384–L441 | PARTIAL | 78% | [PARTIAL] Private ~57-line function. File header covers Strategy 3 at a high ... |
| `detectDocGapsV2` | L447–L520 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Main exported public API with no JSDoc. Parameters (vectorStor... |
| `formatGapReportV2` | L526–L592 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported function with no JSDoc. Takes GapReportV2 and returns... |
| `GapDetectionResult` | L627–L635 | PARTIAL | 80% | [PARTIAL] Has @deprecated JSDoc pointing to GapReportV2. No description of th... |
| `detectDocGaps` | L637–L642 | PARTIAL | 80% | [PARTIAL] Has @deprecated JSDoc pointing to detectDocGapsV2. No parameter doc... |

### `src/core/reporter.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `RunStats` | L21–L28 | PARTIAL | 82% | [PARTIAL] No JSDoc at all. Most fields are self-descriptive, but axisStats is... |
| `loadReviews` | L51–L74 | PARTIAL | 82% | [PARTIAL] Has JSDoc describing purpose and the runDir parameter semantics in ... |
| `computeFileVerdict` | L109–L123 | PARTIAL | 80% | [PARTIAL] Exported function with JSDoc that covers purpose and the key behavi... |
| `aggregateReviews` | L156–L200 | PARTIAL | 85% | [PARTIAL] Exported function with brief JSDoc ('Aggregate all reviews into a R... |
| `sortFindingFiles` | L226–L242 | PARTIAL | 83% | [PARTIAL] Exported function with JSDoc describing the multi-criteria sort ord... |
| `buildShards` | L255–L275 | PARTIAL | 82% | [PARTIAL] Exported function with JSDoc noting max SHARD_SIZE per shard. Brief... |
| `hasAxisFinding` | L343–L361 | PARTIAL | 80% | [PARTIAL] Exported function with JSDoc describing purpose. Missing @param/@re... |
| `buildAxisReports` | L367–L398 | PARTIAL | 82% | [PARTIAL] Exported function with JSDoc covering purpose (filter by axis, scop... |
| `renderAxisIndex` | L509–L545 | PARTIAL | 80% | [PARTIAL] Exported function with JSDoc describing purpose. Missing @param/@re... |
| `renderAxisShard` | L674–L853 | PARTIAL | 82% | [PARTIAL] Exported function with JSDoc noting axis-scoped rendering. Missing ... |
| `renderIndex` | L1056–L1252 | PARTIAL | 83% | [PARTIAL] Exported function with JSDoc describing purpose. Has 6 parameters (... |
| `generateReport` | L1435–L1469 | PARTIAL | 82% | [PARTIAL] Exported function with JSDoc covering purpose and the runDir parame... |

### `src/core/axes/correction.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `CorrectionResponseSchema` | L28–L31 | UNDOCUMENTED | 85% | [UNDOCUMENTED] Exported Zod schema with no JSDoc. While the name is clear, it... |
| `buildCorrectionSystemPrompt` | L39–L41 | UNDOCUMENTED | 85% | [UNDOCUMENTED] Exported function with no JSDoc. Even though the function body... |
| `buildCorrectionUserMessage` | L43–L76 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported 34-line function with no JSDoc at all. It assembles a... |
| `VerificationResponseSchema` | L90–L92 | UNDOCUMENTED | 85% | [UNDOCUMENTED] Exported Zod schema with no JSDoc. It validates the pass-2 LLM... |
| `extractVerificationKeywords` | L112–L132 | PARTIAL | 85% | [PARTIAL] Exported function with a one-line JSDoc description. The descriptio... |
| `findingsNeedVerification` | L185–L194 | PARTIAL | 80% | [PARTIAL] Private function with a single-line JSDoc describing purpose. No @p... |
| `applyVerification` | L200–L248 | PARTIAL | 80% | [PARTIAL] Private 49-line function with a two-sentence JSDoc covering purpose... |
| `detectImplicatedDep` | L253–L263 | PARTIAL | 80% | [PARTIAL] Private function with a one-line JSDoc. No @param or @returns docum... |
| `CorrectionEvaluator` | L280–L393 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Primary exported class implementing AxisEvaluator with no JSDo... |

### `src/core/language-adapters.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `LanguageAdapter` | L23–L29 | PARTIAL | 85% | [PARTIAL] Core public interface with no JSDoc. The contract of extractSymbols... |
| `PythonAdapter` | L235–L331 | PARTIAL | 85% | [PARTIAL] Exported class with no class-level JSDoc. The __all__-based export ... |
| `JavaAdapter` | L498–L568 | PARTIAL | 85% | [PARTIAL] Exported class with no JSDoc. Notably, it also extracts class membe... |
| `CSharpAdapter` | L588–L656 | PARTIAL | 85% | [PARTIAL] Exported class with no JSDoc. The recursive namespace_declaration t... |
| `SqlAdapter` | L663–L696 | PARTIAL | 87% | [PARTIAL] Exported class with no JSDoc. The design split — extractSymbols alw... |
| `YamlAdapter` | L703–L740 | PARTIAL | 85% | [PARTIAL] Exported class with no JSDoc. The heuristicExtract method assigns t... |
| `JsonAdapter` | L744–L772 | PARTIAL | 85% | [PARTIAL] Exported class with no JSDoc. The extractSymbols-returns-[] / heuri... |
| `ADAPTER_REGISTRY` | L790–L790 | PARTIAL | 85% | [PARTIAL] Exported constant with no JSDoc. As the primary public registry for... |
| `heuristicParse` | L821–L852 | PARTIAL | 85% | [PARTIAL] Exported 32-line function with a JSDoc that covers the general purp... |

### `src/commands/clean-run.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `CB_NO_PROGRESS_THRESHOLD` | L25–L25 | UNDOCUMENTED | 95% | [UNDOCUMENTED] No JSDoc comment. Exported constant whose value (3) and semant... |
| `CB_SAME_ERROR_THRESHOLD` | L26–L26 | UNDOCUMENTED | 95% | [UNDOCUMENTED] No JSDoc comment. Exported constant with value 5 and no explan... |
| `isValidSha` | L28–L30 | UNDOCUMENTED | 85% | [UNDOCUMENTED] No JSDoc comment. Exported function. While the name is self-de... |
| `getGitSha` | L32–L38 | UNDOCUMENTED | 95% | [UNDOCUMENTED] No JSDoc comment. Exported function. The non-obvious behavior ... |
| `hasGitChanges` | L40–L48 | UNDOCUMENTED | 95% | [UNDOCUMENTED] No JSDoc comment. Exported function. The semantics of `sinceSh... |
| `hasUncommittedChanges` | L50–L57 | UNDOCUMENTED | 85% | [UNDOCUMENTED] No JSDoc comment. Exported function. The name is fairly self-d... |
| `rollbackToSha` | L59–L76 | UNDOCUMENTED | 95% | [UNDOCUMENTED] No JSDoc comment. Exported function with significant destructi... |
| `registerCleanRunCommand` | L117–L332 | UNDOCUMENTED | 95% | [UNDOCUMENTED] No JSDoc comment. Exported function and the primary public API... |

### `src/rag/doc-indexer.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `loadDocCacheFromRagCache` | L74–L83 | PARTIAL | 80% | [PARTIAL] Exported function with no JSDoc. The non-obvious behavior — extract... |
| `saveDocCacheToRagCache` | L85–L94 | PARTIAL | 80% | [PARTIAL] Exported function with no JSDoc. The merge behavior (reading existi... |
| `fallbackParseH2` | L240–L287 | PARTIAL | 72% | [PARTIAL] Private ~47-line function with no JSDoc. The name describes the app... |
| `stripCodeBlocks` | L293–L295 | PARTIAL | 85% | [PARTIAL] Exported function with no JSDoc. The name 'stripCodeBlocks' is misl... |
| `buildDocSectionId` | L297–L300 | PARTIAL | 82% | [PARTIAL] Exported function with no JSDoc. The ID format (first 16 hex chars ... |
| `parseDocSections` | L302–L302 | PARTIAL | 75% | [PARTIAL] Exported alias for fallbackParseH2 with no per-symbol JSDoc. The ba... |
| `collectDocSections` | L304–L318 | PARTIAL | 80% | [PARTIAL] Exported function with no JSDoc. The default docsDir='docs', the gl... |
| `DocIndexOptions` | L324–L345 | PARTIAL | 85% | [PARTIAL] Exported interface where roughly 6 of 14 fields have JSDoc (chunkMo... |

### `scripts/lib/hardware.sh`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `detect_gpu` | L11–L21 | PARTIAL | 88% | [UNDOCUMENTED] Only a section-level banner comment exists above; no per-funct... |
| `detect_vram_gb` | L24–L36 | PARTIAL | 85% | [PARTIAL] Single inline comment '# Total VRAM in GB (NVIDIA only, returns 0 o... |
| `detect_vram_used_mib` | L39–L41 | PARTIAL | 85% | [PARTIAL] Brief inline comment '# Current VRAM usage in MiB (NVIDIA only)' is... |
| `has_docker` | L46–L48 | PARTIAL | 88% | [UNDOCUMENTED] No function-level comment. Only a section banner above. Exit-c... |
| `has_nvidia_container_toolkit` | L50–L58 | UNDOCUMENTED | 90% | [UNDOCUMENTED] No function-level comment. The two-path detection logic (nvidi... |
| `select_tier` | L64–L81 | PARTIAL | 85% | [PARTIAL] Comment '# Returns: "lite" or "advanced-gguf"' documents the output... |
| `ensure_jq` | L86–L103 | UNDOCUMENTED | 90% | [UNDOCUMENTED] No function-level comment. The significant side-effect of invo... |
| `ensure_curl` | L105–L111 | UNDOCUMENTED | 90% | [UNDOCUMENTED] No comment of any kind before the function. While the name is ... |

### `scripts/lib/docker-helpers.sh`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `start_gguf_container` | L47–L68 | UNDOCUMENTED | 95% | [UNDOCUMENTED] No comment above the function. Four parameters (name, models_d... |
| `start_gguf_containers` | L70–L80 | UNDOCUMENTED | 95% | [UNDOCUMENTED] No comment above the function. Three parameters (models_dir, c... |
| `stop_gguf_containers` | L82–L86 | UNDOCUMENTED | 90% | [UNDOCUMENTED] No comment. Simple function, but the section header 'GGUF cont... |
| `start_tei_container` | L92–L110 | UNDOCUMENTED | 95% | [UNDOCUMENTED] No comment above the function. Four parameters including an op... |
| `stop_tei_containers` | L112–L116 | UNDOCUMENTED | 90% | [UNDOCUMENTED] No comment. No parameters, but the function silently assumes s... |
| `wait_for_gguf` | L143–L158 | PARTIAL | 85% | [PARTIAL] Comment 'Wait for GGUF containers (both code + NLP)' covers purpose... |
| `wait_for_tei` | L161–L210 | PARTIAL | 85% | [PARTIAL] Comment mentions purpose and log-progress feature, but all four par... |

### `src/core/axes/best-practices.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `BestPracticesRuleResponseSchema` | L15–L22 | PARTIAL | 72% | [PARTIAL] Private, unexported Zod schema with no JSDoc. The section banner '/... |
| `BestPracticesSuggestionSchema` | L24–L28 | PARTIAL | 70% | [PARTIAL] Private, unexported Zod schema with no JSDoc. The section banner co... |
| `BestPracticesResponseSchema` | L30–L34 | UNDOCUMENTED | 88% | [UNDOCUMENTED] Exported Zod schema with no JSDoc. As a public export it shoul... |
| `detectFileContext` | L44–L77 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported 33-line function with no JSDoc at all. The detection ... |
| `buildBestPracticesSystemPrompt` | L83–L85 | PARTIAL | 90% | [UNDOCUMENTED] Exported function with no JSDoc. It is a thin wrapper around r... |
| `buildBestPracticesUserMessage` | L87–L135 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported 48-line function with no JSDoc. It assembles a multi-... |
| `BestPracticesEvaluator` | L141–L186 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported class implementing AxisEvaluator with no JSDoc on the... |

### `src/core/axis-evaluator.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `PreResolvedRagEntry` | L27–L33 | PARTIAL | 80% | [PARTIAL] No interface-level JSDoc. The results field has an inline comment e... |
| `composeAxisSystemPrompt` | L45–L53 | PARTIAL | 85% | [PARTIAL] JSDoc describes purpose and composition order (json-evaluator-wrapp... |
| `AxisContext` | L68–L91 | PARTIAL | 85% | [PARTIAL] Several optional fields have inline JSDoc (testFileContent, testFil... |
| `AxisSymbolResult` | L100–L110 | PARTIAL | 85% | [PARTIAL] value field has inline JSDoc with axis-specific examples ('USED', '... |
| `AxisResult` | L112–L125 | PARTIAL | 80% | [PARTIAL] fileLevel field has inline JSDoc. Cost/token fields are self-descri... |
| `AxisEvaluator` | L127–L131 | PARTIAL | 85% | [PARTIAL] No JSDoc on the interface or any of its members. As the primary con... |
| `SingleTurnQueryParams` | L191–L203 | PARTIAL | 80% | [PARTIAL] conversationDir, conversationPrefix, and semaphore fields have inli... |

## Hygiene

- [ ] <!-- ACT-a1ad8f-1 --> **[documentation · medium · trivial]** `scripts/lib/docker-helpers.sh`: Add JSDoc documentation for exported symbol: `start_gguf_container` (`start_gguf_container`) [L47-L68]
- [ ] <!-- ACT-a1ad8f-2 --> **[documentation · medium · trivial]** `scripts/lib/docker-helpers.sh`: Add JSDoc documentation for exported symbol: `start_gguf_containers` (`start_gguf_containers`) [L70-L80]
- [ ] <!-- ACT-a1ad8f-3 --> **[documentation · medium · trivial]** `scripts/lib/docker-helpers.sh`: Add JSDoc documentation for exported symbol: `stop_gguf_containers` (`stop_gguf_containers`) [L82-L86]
- [ ] <!-- ACT-a1ad8f-4 --> **[documentation · medium · trivial]** `scripts/lib/docker-helpers.sh`: Add JSDoc documentation for exported symbol: `start_tei_container` (`start_tei_container`) [L92-L110]
- [ ] <!-- ACT-a1ad8f-5 --> **[documentation · medium · trivial]** `scripts/lib/docker-helpers.sh`: Add JSDoc documentation for exported symbol: `stop_tei_containers` (`stop_tei_containers`) [L112-L116]
- [ ] <!-- ACT-0273c8-1 --> **[documentation · medium · trivial]** `scripts/lib/hardware.sh`: Add JSDoc documentation for exported symbol: `detect_gpu` (`detect_gpu`) [L11-L21]
- [ ] <!-- ACT-0273c8-5 --> **[documentation · medium · trivial]** `scripts/lib/hardware.sh`: Add JSDoc documentation for exported symbol: `has_nvidia_container_toolkit` (`has_nvidia_container_toolkit`) [L50-L58]
- [ ] <!-- ACT-0273c8-7 --> **[documentation · medium · trivial]** `scripts/lib/hardware.sh`: Add JSDoc documentation for exported symbol: `ensure_jq` (`ensure_jq`) [L86-L103]
- [ ] <!-- ACT-0273c8-8 --> **[documentation · medium · trivial]** `scripts/lib/hardware.sh`: Add JSDoc documentation for exported symbol: `ensure_curl` (`ensure_curl`) [L105-L111]
- [ ] <!-- ACT-325771-1 --> **[documentation · medium · trivial]** `src/commands/clean-run.ts`: Add JSDoc documentation for exported symbol: `CB_NO_PROGRESS_THRESHOLD` (`CB_NO_PROGRESS_THRESHOLD`) [L25-L25]
- [ ] <!-- ACT-325771-2 --> **[documentation · medium · trivial]** `src/commands/clean-run.ts`: Add JSDoc documentation for exported symbol: `CB_SAME_ERROR_THRESHOLD` (`CB_SAME_ERROR_THRESHOLD`) [L26-L26]
- [ ] <!-- ACT-325771-3 --> **[documentation · medium · trivial]** `src/commands/clean-run.ts`: Add JSDoc documentation for exported symbol: `isValidSha` (`isValidSha`) [L28-L30]
- [ ] <!-- ACT-325771-4 --> **[documentation · medium · trivial]** `src/commands/clean-run.ts`: Add JSDoc documentation for exported symbol: `getGitSha` (`getGitSha`) [L32-L38]
- [ ] <!-- ACT-325771-5 --> **[documentation · medium · trivial]** `src/commands/clean-run.ts`: Add JSDoc documentation for exported symbol: `hasGitChanges` (`hasGitChanges`) [L40-L48]
- [ ] <!-- ACT-325771-6 --> **[documentation · medium · trivial]** `src/commands/clean-run.ts`: Add JSDoc documentation for exported symbol: `hasUncommittedChanges` (`hasUncommittedChanges`) [L50-L57]
- [ ] <!-- ACT-325771-7 --> **[documentation · medium · trivial]** `src/commands/clean-run.ts`: Add JSDoc documentation for exported symbol: `rollbackToSha` (`rollbackToSha`) [L59-L76]
- [ ] <!-- ACT-325771-8 --> **[documentation · medium · trivial]** `src/commands/clean-run.ts`: Add JSDoc documentation for exported symbol: `registerCleanRunCommand` (`registerCleanRunCommand`) [L117-L332]
- [ ] <!-- ACT-ed94c5-3 --> **[documentation · medium · trivial]** `src/core/axes/best-practices.ts`: Add JSDoc documentation for exported symbol: `BestPracticesResponseSchema` (`BestPracticesResponseSchema`) [L30-L34]
- [ ] <!-- ACT-ed94c5-4 --> **[documentation · medium · trivial]** `src/core/axes/best-practices.ts`: Add JSDoc documentation for exported symbol: `detectFileContext` (`detectFileContext`) [L44-L77]
- [ ] <!-- ACT-ed94c5-5 --> **[documentation · medium · trivial]** `src/core/axes/best-practices.ts`: Add JSDoc documentation for exported symbol: `buildBestPracticesSystemPrompt` (`buildBestPracticesSystemPrompt`) [L83-L85]
- [ ] <!-- ACT-ed94c5-6 --> **[documentation · medium · trivial]** `src/core/axes/best-practices.ts`: Add JSDoc documentation for exported symbol: `buildBestPracticesUserMessage` (`buildBestPracticesUserMessage`) [L87-L135]
- [ ] <!-- ACT-ed94c5-7 --> **[documentation · medium · trivial]** `src/core/axes/best-practices.ts`: Add JSDoc documentation for exported symbol: `BestPracticesEvaluator` (`BestPracticesEvaluator`) [L141-L186]
- [ ] <!-- ACT-5c39f0-1 --> **[documentation · medium · trivial]** `src/core/axes/correction.ts`: Add JSDoc documentation for exported symbol: `CorrectionResponseSchema` (`CorrectionResponseSchema`) [L28-L31]
- [ ] <!-- ACT-5c39f0-2 --> **[documentation · medium · trivial]** `src/core/axes/correction.ts`: Add JSDoc documentation for exported symbol: `buildCorrectionSystemPrompt` (`buildCorrectionSystemPrompt`) [L39-L41]
- [ ] <!-- ACT-5c39f0-3 --> **[documentation · medium · trivial]** `src/core/axes/correction.ts`: Add JSDoc documentation for exported symbol: `buildCorrectionUserMessage` (`buildCorrectionUserMessage`) [L43-L76]
- [ ] <!-- ACT-5c39f0-4 --> **[documentation · medium · trivial]** `src/core/axes/correction.ts`: Add JSDoc documentation for exported symbol: `VerificationResponseSchema` (`VerificationResponseSchema`) [L90-L92]
- [ ] <!-- ACT-5c39f0-10 --> **[documentation · medium · trivial]** `src/core/axes/correction.ts`: Add JSDoc documentation for exported symbol: `CorrectionEvaluator` (`CorrectionEvaluator`) [L280-L393]
- [ ] <!-- ACT-16c91e-9 --> **[documentation · medium · trivial]** `src/core/doc-gap-detection.ts`: Add JSDoc documentation for exported symbol: `detectDocGapsV2` (`detectDocGapsV2`) [L447-L520]
- [ ] <!-- ACT-16c91e-10 --> **[documentation · medium · trivial]** `src/core/doc-gap-detection.ts`: Add JSDoc documentation for exported symbol: `formatGapReportV2` (`formatGapReportV2`) [L526-L592]
- [ ] <!-- ACT-a1ad8f-6 --> **[documentation · low · trivial]** `scripts/lib/docker-helpers.sh`: Complete JSDoc documentation for: `wait_for_gguf` (`wait_for_gguf`) [L143-L158]
- [ ] <!-- ACT-a1ad8f-7 --> **[documentation · low · trivial]** `scripts/lib/docker-helpers.sh`: Complete JSDoc documentation for: `wait_for_tei` (`wait_for_tei`) [L161-L210]
- [ ] <!-- ACT-0273c8-2 --> **[documentation · low · trivial]** `scripts/lib/hardware.sh`: Complete JSDoc documentation for: `detect_vram_gb` (`detect_vram_gb`) [L24-L36]
- [ ] <!-- ACT-0273c8-3 --> **[documentation · low · trivial]** `scripts/lib/hardware.sh`: Complete JSDoc documentation for: `detect_vram_used_mib` (`detect_vram_used_mib`) [L39-L41]
- [ ] <!-- ACT-0273c8-6 --> **[documentation · low · trivial]** `scripts/lib/hardware.sh`: Complete JSDoc documentation for: `select_tier` (`select_tier`) [L64-L81]
- [ ] <!-- ACT-ed94c5-1 --> **[documentation · low · trivial]** `src/core/axes/best-practices.ts`: Complete JSDoc documentation for: `BestPracticesRuleResponseSchema` (`BestPracticesRuleResponseSchema`) [L15-L22]
- [ ] <!-- ACT-ed94c5-2 --> **[documentation · low · trivial]** `src/core/axes/best-practices.ts`: Complete JSDoc documentation for: `BestPracticesSuggestionSchema` (`BestPracticesSuggestionSchema`) [L24-L28]
- [ ] <!-- ACT-5c39f0-5 --> **[documentation · low · trivial]** `src/core/axes/correction.ts`: Complete JSDoc documentation for: `extractVerificationKeywords` (`extractVerificationKeywords`) [L112-L132]
- [ ] <!-- ACT-5c39f0-6 --> **[documentation · low · trivial]** `src/core/axes/correction.ts`: Complete JSDoc documentation for: `findingsNeedVerification` (`findingsNeedVerification`) [L185-L194]
- [ ] <!-- ACT-5c39f0-7 --> **[documentation · low · trivial]** `src/core/axes/correction.ts`: Complete JSDoc documentation for: `applyVerification` (`applyVerification`) [L200-L248]
- [ ] <!-- ACT-5c39f0-8 --> **[documentation · low · trivial]** `src/core/axes/correction.ts`: Complete JSDoc documentation for: `detectImplicatedDep` (`detectImplicatedDep`) [L253-L263]
- [ ] <!-- ACT-de128a-1 --> **[documentation · low · trivial]** `src/core/axis-evaluator.ts`: Complete JSDoc documentation for: `PreResolvedRagEntry` (`PreResolvedRagEntry`) [L27-L33]
- [ ] <!-- ACT-de128a-2 --> **[documentation · low · trivial]** `src/core/axis-evaluator.ts`: Complete JSDoc documentation for: `composeAxisSystemPrompt` (`composeAxisSystemPrompt`) [L45-L53]
- [ ] <!-- ACT-de128a-3 --> **[documentation · low · trivial]** `src/core/axis-evaluator.ts`: Complete JSDoc documentation for: `AxisContext` (`AxisContext`) [L68-L91]
- [ ] <!-- ACT-de128a-5 --> **[documentation · low · trivial]** `src/core/axis-evaluator.ts`: Complete JSDoc documentation for: `AxisSymbolResult` (`AxisSymbolResult`) [L100-L110]
- [ ] <!-- ACT-de128a-6 --> **[documentation · low · trivial]** `src/core/axis-evaluator.ts`: Complete JSDoc documentation for: `AxisResult` (`AxisResult`) [L112-L125]
- [ ] <!-- ACT-de128a-7 --> **[documentation · low · trivial]** `src/core/axis-evaluator.ts`: Complete JSDoc documentation for: `AxisEvaluator` (`AxisEvaluator`) [L127-L131]
- [ ] <!-- ACT-de128a-8 --> **[documentation · low · trivial]** `src/core/axis-evaluator.ts`: Complete JSDoc documentation for: `SingleTurnQueryParams` (`SingleTurnQueryParams`) [L191-L203]
- [ ] <!-- ACT-16c91e-1 --> **[documentation · low · trivial]** `src/core/doc-gap-detection.ts`: Complete JSDoc documentation for: `GapReportV2` (`GapReportV2`) [L58-L78]
- [ ] <!-- ACT-16c91e-2 --> **[documentation · low · trivial]** `src/core/doc-gap-detection.ts`: Complete JSDoc documentation for: `GapDetectionV2Options` (`GapDetectionV2Options`) [L80-L92]
- [ ] <!-- ACT-16c91e-3 --> **[documentation · low · trivial]** `src/core/doc-gap-detection.ts`: Complete JSDoc documentation for: `classifyPage` (`classifyPage`) [L98-L105]
- [ ] <!-- ACT-16c91e-4 --> **[documentation · low · trivial]** `src/core/doc-gap-detection.ts`: Complete JSDoc documentation for: `groupByDomain` (`groupByDomain`) [L142-L175]
- [ ] <!-- ACT-16c91e-5 --> **[documentation · low · trivial]** `src/core/doc-gap-detection.ts`: Complete JSDoc documentation for: `groupByPage` (`groupByPage`) [L189-L219]
- [ ] <!-- ACT-16c91e-6 --> **[documentation · low · trivial]** `src/core/doc-gap-detection.ts`: Complete JSDoc documentation for: `strategy1_moduleDomains` (`strategy1_moduleDomains`) [L225-L316]
- [ ] <!-- ACT-16c91e-7 --> **[documentation · low · trivial]** `src/core/doc-gap-detection.ts`: Complete JSDoc documentation for: `strategy2_referencePages` (`strategy2_referencePages`) [L322-L378]
- [ ] <!-- ACT-16c91e-8 --> **[documentation · low · trivial]** `src/core/doc-gap-detection.ts`: Complete JSDoc documentation for: `strategy3_conceptualPages` (`strategy3_conceptualPages`) [L384-L441]
- [ ] <!-- ACT-16c91e-12 --> **[documentation · low · trivial]** `src/core/doc-gap-detection.ts`: Complete JSDoc documentation for: `GapDetectionResult` (`GapDetectionResult`) [L627-L635]
- [ ] <!-- ACT-16c91e-13 --> **[documentation · low · trivial]** `src/core/doc-gap-detection.ts`: Complete JSDoc documentation for: `detectDocGaps` (`detectDocGaps`) [L637-L642]
- [ ] <!-- ACT-c741e0-1 --> **[documentation · low · trivial]** `src/core/language-adapters.ts`: Complete JSDoc documentation for: `LanguageAdapter` (`LanguageAdapter`) [L23-L29]
- [ ] <!-- ACT-c741e0-4 --> **[documentation · low · trivial]** `src/core/language-adapters.ts`: Complete JSDoc documentation for: `PythonAdapter` (`PythonAdapter`) [L235-L331]
- [ ] <!-- ACT-c741e0-7 --> **[documentation · low · trivial]** `src/core/language-adapters.ts`: Complete JSDoc documentation for: `JavaAdapter` (`JavaAdapter`) [L498-L568]
- [ ] <!-- ACT-c741e0-8 --> **[documentation · low · trivial]** `src/core/language-adapters.ts`: Complete JSDoc documentation for: `CSharpAdapter` (`CSharpAdapter`) [L588-L656]
- [ ] <!-- ACT-c741e0-9 --> **[documentation · low · trivial]** `src/core/language-adapters.ts`: Complete JSDoc documentation for: `SqlAdapter` (`SqlAdapter`) [L663-L696]
- [ ] <!-- ACT-c741e0-10 --> **[documentation · low · trivial]** `src/core/language-adapters.ts`: Complete JSDoc documentation for: `YamlAdapter` (`YamlAdapter`) [L703-L740]
- [ ] <!-- ACT-c741e0-11 --> **[documentation · low · trivial]** `src/core/language-adapters.ts`: Complete JSDoc documentation for: `JsonAdapter` (`JsonAdapter`) [L744-L772]
- [ ] <!-- ACT-c741e0-12 --> **[documentation · low · trivial]** `src/core/language-adapters.ts`: Complete JSDoc documentation for: `ADAPTER_REGISTRY` (`ADAPTER_REGISTRY`) [L790-L790]
- [ ] <!-- ACT-c741e0-14 --> **[documentation · low · trivial]** `src/core/language-adapters.ts`: Complete JSDoc documentation for: `heuristicParse` (`heuristicParse`) [L821-L852]
- [ ] <!-- ACT-a5e324-1 --> **[documentation · low · trivial]** `src/core/reporter.ts`: Complete JSDoc documentation for: `RunStats` (`RunStats`) [L21-L28]
- [ ] <!-- ACT-a5e324-2 --> **[documentation · low · trivial]** `src/core/reporter.ts`: Complete JSDoc documentation for: `loadReviews` (`loadReviews`) [L51-L74]
- [ ] <!-- ACT-a5e324-3 --> **[documentation · low · trivial]** `src/core/reporter.ts`: Complete JSDoc documentation for: `computeFileVerdict` (`computeFileVerdict`) [L109-L123]
- [ ] <!-- ACT-a5e324-5 --> **[documentation · low · trivial]** `src/core/reporter.ts`: Complete JSDoc documentation for: `aggregateReviews` (`aggregateReviews`) [L156-L200]
- [ ] <!-- ACT-a5e324-7 --> **[documentation · low · trivial]** `src/core/reporter.ts`: Complete JSDoc documentation for: `sortFindingFiles` (`sortFindingFiles`) [L226-L242]
- [ ] <!-- ACT-a5e324-8 --> **[documentation · low · trivial]** `src/core/reporter.ts`: Complete JSDoc documentation for: `buildShards` (`buildShards`) [L255-L275]
- [ ] <!-- ACT-a5e324-11 --> **[documentation · low · trivial]** `src/core/reporter.ts`: Complete JSDoc documentation for: `hasAxisFinding` (`hasAxisFinding`) [L343-L361]
- [ ] <!-- ACT-a5e324-12 --> **[documentation · low · trivial]** `src/core/reporter.ts`: Complete JSDoc documentation for: `buildAxisReports` (`buildAxisReports`) [L367-L398]
- [ ] <!-- ACT-a5e324-13 --> **[documentation · low · trivial]** `src/core/reporter.ts`: Complete JSDoc documentation for: `renderAxisIndex` (`renderAxisIndex`) [L509-L545]
- [ ] <!-- ACT-a5e324-14 --> **[documentation · low · trivial]** `src/core/reporter.ts`: Complete JSDoc documentation for: `renderAxisShard` (`renderAxisShard`) [L674-L853]
- [ ] <!-- ACT-a5e324-15 --> **[documentation · low · trivial]** `src/core/reporter.ts`: Complete JSDoc documentation for: `renderIndex` (`renderIndex`) [L1056-L1252]
- [ ] <!-- ACT-a5e324-16 --> **[documentation · low · trivial]** `src/core/reporter.ts`: Complete JSDoc documentation for: `generateReport` (`generateReport`) [L1435-L1469]
- [ ] <!-- ACT-66361e-1 --> **[documentation · low · trivial]** `src/rag/doc-indexer.ts`: Complete JSDoc documentation for: `loadDocCacheFromRagCache` (`loadDocCacheFromRagCache`) [L74-L83]
- [ ] <!-- ACT-66361e-2 --> **[documentation · low · trivial]** `src/rag/doc-indexer.ts`: Complete JSDoc documentation for: `saveDocCacheToRagCache` (`saveDocCacheToRagCache`) [L85-L94]
- [ ] <!-- ACT-66361e-3 --> **[documentation · low · trivial]** `src/rag/doc-indexer.ts`: Complete JSDoc documentation for: `fallbackParseH2` (`fallbackParseH2`) [L240-L287]
- [ ] <!-- ACT-66361e-4 --> **[documentation · low · trivial]** `src/rag/doc-indexer.ts`: Complete JSDoc documentation for: `stripCodeBlocks` (`stripCodeBlocks`) [L293-L295]
- [ ] <!-- ACT-66361e-5 --> **[documentation · low · trivial]** `src/rag/doc-indexer.ts`: Complete JSDoc documentation for: `buildDocSectionId` (`buildDocSectionId`) [L297-L300]
- [ ] <!-- ACT-66361e-6 --> **[documentation · low · trivial]** `src/rag/doc-indexer.ts`: Complete JSDoc documentation for: `parseDocSections` (`parseDocSections`) [L302-L302]
- [ ] <!-- ACT-66361e-7 --> **[documentation · low · trivial]** `src/rag/doc-indexer.ts`: Complete JSDoc documentation for: `collectDocSections` (`collectDocSections`) [L304-L318]
- [ ] <!-- ACT-66361e-8 --> **[documentation · low · trivial]** `src/rag/doc-indexer.ts`: Complete JSDoc documentation for: `DocIndexOptions` (`DocIndexOptions`) [L324-L345]

## Documentation Coverage

### `src/core/doc-gap-detection.ts` — 0% covered

- [ ] **Doc Gap Detection Module** — MISSING: No dedicated documentation page exists for the doc-gap-detection module. Neither 04-Core-Modules/ nor 05-Modules/ contains a page for this module, despite it being a significant public-facing capability.
- [ ] **Three-Strategy Architecture (module/reference/conceptual)** — MISSING: The three-strategy architecture described in the file header (domain vector matching, structural diff, concept coverage) has no coverage in any docs/ page visible in the file tree.
- [ ] **Domain Vector Matching (Strategy 1)** — MISSING: The macro + micro domain vector matching approach, including threshold semantics and the domain-splitting logic, is not documented in docs/.
- [ ] **GapReportV2 Output Format** — MISSING: The structure and interpretation of GapReportV2 (coverage scores, pagesToCreate/pagesToUpdate lists, conceptsToDocument) are not described in any docs/ page.
- [ ] **v1 Legacy API Migration** — MISSING: The deprecation of GapClassification, GapDetectionResult, detectDocGaps, and formatGapSummary and their migration path to the v2 API are not mentioned in any docs/ page.

### `src/commands/clean-run.ts` — 0% covered

- [ ] **clean-run command** — MISSING: The `clean-run` command (generate artifacts + run Ralph remediation loop) does not appear as a named entry in the docs directory structure. `03-CLI-Reference/01-Commands.md` and `04-API-Reference/04-CLI-Reference.md` exist but there is no evidence `clean-run` is described there; no dedicated page exists for it.
- [ ] **Circuit breaker mechanism** — MISSING: The circuit breaker (CB_NO_PROGRESS_THRESHOLD, CB_SAME_ERROR_THRESHOLD, CircuitBreakerState) is a safety mechanism for the Ralph loop but is not referenced in any docs page visible in the directory listing.
- [ ] **Branch isolation (clean-run branch enforcement)** — MISSING: The automatic checkout to a per-shard branch and the per-iteration protected-branch guard are undocumented in the docs tree. No docs page addresses this safety guarantee.
- [ ] **Clean artifacts generation (prd.json / CLAUDE.md)** — MISSING: The artifact generation flow (invoking `anatoly clean`, creating prd.json and CLAUDE.md under .anatoly/clean/<shard>/) is not documented in any visible docs page.

### `scripts/lib/hardware.sh` — 20% covered

- [ ] **GPU detection (cuda / metal / rocm / none)** — PARTIAL → `05-Modules/setup-embeddings.md`: setup-embeddings.md is the most likely home for hardware prerequisites. GPU type detection is a core concern of that script, but no dedicated hardware-detection reference page exists in docs/; coverage is inferred as incidental rather than explicit.
- [ ] **VRAM measurement and thresholds** — PARTIAL → `01-Getting-Started/01-Installation.md`: Installation docs likely mention VRAM requirements (e.g. 12 GB minimum for advanced-gguf tier), but the internal MiB-to-GB conversion and single-GPU query behaviour are unlikely to be described.
- [ ] **Tier selection (lite vs advanced-gguf)** — MISSING: No docs page is named or obviously scoped to tier selection logic. The 12 GB threshold and the four-parameter decision matrix implemented in select_tier have no apparent public documentation.
- [ ] **Docker and NVIDIA container toolkit detection** — MISSING: Neither has_docker nor has_nvidia_container_toolkit has a matching concept page. Prerequisites for GPU-accelerated Docker may appear in Installation docs but the detection mechanism itself is not covered.
- [ ] **Dependency auto-installation (jq, curl)** — MISSING: No docs page covers the ensure_jq / ensure_curl helpers or documents that setup scripts will invoke sudo package managers as a side effect.

### `scripts/lib/docker-helpers.sh` — 33% covered

- [ ] **Docker-based embedding backend lifecycle (start/stop/wait)** — PARTIAL → `05-Modules/setup-embeddings.md`: setup-embeddings.md likely covers container startup orchestration, but the health-check polling strategy, port-conflict resolution, and idempotent teardown are not expected to be documented given the filename alone suggests a higher-level setup guide rather than operational internals.
- [ ] **GGUF / llama.cpp embedding container** — PARTIAL → `05-Modules/setup-embeddings.md`: The module doc likely names llama.cpp as the GGUF backend and lists the image, but GPU flags (-ngl 999, --pooling last) and the dual code/NLP container split are internal details unlikely to be fully covered.
- [ ] **TEI (HuggingFace Text Embeddings Inference) container** — PARTIAL → `05-Modules/setup-embeddings.md`: TEI is likely mentioned as the dense-embedding backend, but per-container parameters (dtype=float16, HF cache mounting) and download-progress monitoring behaviour are implementation details not expected to appear in user-facing docs.
- [ ] **Embedding request API (embed_gguf / embed_tei)** — MISSING: No doc page in the provided tree is titled around the raw HTTP embedding request helpers. The /embedding and /embed endpoint details are internal plumbing not expected in any listed doc.
- [ ] **Port management and conflict resolution** — MISSING: free_port and free_all_ports are purely operational helpers. No doc page in the tree covers Anatoly's port assignments or the pre-start port-cleanup strategy.
- [ ] **GPU memory flush between container restarts** — MISSING: flush_gpu_memory and its rationale (avoiding nvidia-smi --gpu-reset) are not referenced in any listed documentation page.

### `src/core/axis-evaluator.ts` — 42% covered

- [ ] **runSingleTurnQuery / retry-on-validation-failure mechanism** — MISSING: No page in the docs tree addresses the shared single-turn query utility, its Zod validation loop, or the two-attempt retry-with-feedback pattern. This is a significant shared infrastructure piece with no docs coverage.
- [ ] **Per-axis model resolution priority (resolveAxisModel)** — PARTIAL → `03-Guides/02-Advanced-Configuration.md`: Advanced-Configuration.md may document per-axis model overrides via config.llm.axes, but the full three-level priority chain (axis override → haiku/fast_model branch → evaluator default) is likely not spelled out.
- [ ] **Conversation dump feature (conversationDir, per-attempt .md files)** — MISSING: No page in the docs tree addresses the conversation dump mechanism that writes per-attempt markdown files to conversations/. This debugging feature is invisible to users from the documentation.
- [ ] **Multi-language / framework injection (getLanguageLines, getCodeFenceTag)** — MISSING: No docs page appears to address non-TypeScript language support or the header injection mechanism for language/framework context in evaluator prompts.
- [ ] **Pre-resolved RAG entries (PreResolvedRag)** — PARTIAL → `rag.md`: rag.md or 02-Architecture/03-RAG-Engine.md may cover RAG concepts generally, but PreResolvedRag as a pre-computation optimization (resolving symbol similarity before prompt construction) is an implementation detail unlikely to be documented explicitly.
