# Documentation — Shard 7

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `src/core/prompt-resolver.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-core-prompt-resolver.rev.md) |
| `src/core/review-writer.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-core-review-writer.rev.md) |
| `src/core/scanner.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-core-scanner.rev.md) |
| `src/core/user-doc-plan.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-core-user-doc-plan.rev.md) |
| `src/rag/docker-tei.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-rag-docker-tei.rev.md) |
| `src/rag/embeddings.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-rag-embeddings.rev.md) |
| `src/rag/hardware-detect.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-rag-hardware-detect.rev.md) |
| `src/schemas/task.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-schemas-task.rev.md) |
| `src/utils/banner.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-utils-banner.rev.md) |
| `src/utils/errors.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-utils-errors.rev.md) |

## Symbol Details

### `src/core/prompt-resolver.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `resolveSystemPrompt` | L129–L147 | PARTIAL | 85% | [PARTIAL] Has a JSDoc block describing purpose and cascade order (framework →... |

### `src/core/review-writer.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `writeTranscript` | L41–L51 | PARTIAL | 85% | [PARTIAL] JSDoc covers purpose and the runDir scoping behaviour but omits doc... |

### `src/core/scanner.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `parseFile` | L93–L121 | PARTIAL | 85% | [PARTIAL] Exported function with a JSDoc that covers purpose and fallback str... |

### `src/core/user-doc-plan.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `resolveUserDocPlan` | L51–L90 | PARTIAL | 85% | [PARTIAL] JSDoc covers purpose and the null-return condition (no docs/ direct... |

### `src/rag/docker-tei.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `startTeiContainer` | L158–L190 | PARTIAL | 85% | [PARTIAL] Single-sentence JSDoc ('Start a single TEI container (used during A... |

### `src/rag/embeddings.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `embedViaGguf` | L152–L197 | PARTIAL | 85% | [PARTIAL] Private function with no JSDoc, but ~45 lines long with retry logic... |

### `src/rag/hardware-detect.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `EmbeddingsReadyFlag` | L140–L168 | PARTIAL | 85% | [PARTIAL] No top-level JSDoc describing what this interface represents or whe... |

### `src/schemas/task.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `TaskSchema` | L37–L47 | PARTIAL | 85% | [PARTIAL] Exported public API schema with no JSDoc. Most fields are self-desc... |

### `src/utils/banner.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `printBanner` | L28–L37 | UNDOCUMENTED | 95% | [UNDOCUMENTED] Exported public function with no JSDoc at all. The optional `a... |

### `src/utils/errors.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `AnatolyError` | L35–L75 | PARTIAL | 88% | [PARTIAL] The two public methods (formatForDisplay, toLogObject) each have JS... |

## Hygiene

- [ ] <!-- ACT-cc2a64-2 --> **[documentation · medium · trivial]** `src/utils/banner.ts`: Add JSDoc documentation for exported symbol: `printBanner` (`printBanner`) [L28-L37]
- [ ] <!-- ACT-327060-1 --> **[documentation · low · trivial]** `src/core/prompt-resolver.ts`: Complete JSDoc documentation for: `resolveSystemPrompt` (`resolveSystemPrompt`) [L129-L147]
- [ ] <!-- ACT-ac11a8-1 --> **[documentation · low · trivial]** `src/core/review-writer.ts`: Complete JSDoc documentation for: `writeTranscript` (`writeTranscript`) [L41-L51]
- [ ] <!-- ACT-6f8d16-1 --> **[documentation · low · trivial]** `src/core/scanner.ts`: Complete JSDoc documentation for: `parseFile` (`parseFile`) [L93-L121]
- [ ] <!-- ACT-5f3a73-1 --> **[documentation · low · trivial]** `src/core/user-doc-plan.ts`: Complete JSDoc documentation for: `resolveUserDocPlan` (`resolveUserDocPlan`) [L51-L90]
- [ ] <!-- ACT-9f41a0-1 --> **[documentation · low · trivial]** `src/rag/docker-tei.ts`: Complete JSDoc documentation for: `startTeiContainer` (`startTeiContainer`) [L158-L190]
- [ ] <!-- ACT-e9d7b9-1 --> **[documentation · low · trivial]** `src/rag/embeddings.ts`: Complete JSDoc documentation for: `embedViaGguf` (`embedViaGguf`) [L152-L197]
- [ ] <!-- ACT-577fc3-1 --> **[documentation · low · trivial]** `src/rag/hardware-detect.ts`: Complete JSDoc documentation for: `EmbeddingsReadyFlag` (`EmbeddingsReadyFlag`) [L140-L168]
- [ ] <!-- ACT-a1323a-1 --> **[documentation · low · trivial]** `src/schemas/task.ts`: Complete JSDoc documentation for: `TaskSchema` (`TaskSchema`) [L37-L47]
- [ ] <!-- ACT-d3b5d7-1 --> **[documentation · low · trivial]** `src/utils/errors.ts`: Complete JSDoc documentation for: `AnatolyError` (`AnatolyError`) [L35-L75]

## Documentation Coverage

### `src/core/prompt-resolver.ts` — 10% covered

- [ ] **Prompt Registry (PROMPT_REGISTRY)** — MISSING: No page in docs/ describes the central prompt registry, its key naming conventions, or how prompts are loaded and keyed. The closest candidates (04-Core-Modules/04-Axis-Evaluators.md) would cover evaluator behavior but not the registry abstraction itself.
- [ ] **Cascade prompt resolution (framework → language → default)** — MISSING: The three-level cascade resolution logic (resolveSystemPrompt) is not covered in any docs/ page. The 02-Architecture/ pages may describe the pipeline but do not document this resolution strategy as an explicit concept.
- [ ] **Language and framework variant prompts** — MISSING: No docs/ page documents the convention of registering language-specific (e.g. best_practices.python) or framework-specific (e.g. best_practices.react) prompt variants, or how callers should request them.
- [ ] **Non-axis prompt domains (doc-generation, RAG, deliberation, _shared)** — PARTIAL → `02-Architecture/05-Deliberation-Pass.md`: Deliberation and RAG are covered at a high level in the architecture docs, but the prompt keys used to access those prompts (e.g. 'deliberation', 'rag.section-refiner', '_shared.guard-rails') and their composite-key resolution rules are not documented. Doc-generation prompt variants are entirely absent from docs/.

### `src/core/user-doc-plan.ts` — 0% covered

- [ ] **User Documentation Plan Resolution** — MISSING: The resolveUserDocPlan entry point and its DocPageEntry/UserDocPage/UserDocPlan types are not mentioned in any docs/ page. The 04-Core-Modules/ section lists Scanner, Estimator, Triage, Axis-Evaluators, Reporter, and Worker-Pool but has no page for this module.
- [ ] **Concept Category Mapping (CONCEPT_PATTERNS / CONTENT_PATTERNS)** — MISSING: The pattern-based classification of docs directories and file names into categories (getting-started, architecture, api-reference, etc.) is not described in any docs/ page, including the Architecture or Core-Modules sections.
- [ ] **Organizational Convention Detection (subdirectory vs flat structure)** — MISSING: The logic for handling hierarchical vs flat docs/ structures, prefix normalization (01-, a-, etc.), and H1-based fallback classification is not covered anywhere in the documentation directory.

### `src/rag/embeddings.ts` — 55% covered

- [ ] **GGUF Docker backend and ONNX fallback runtimes** — PARTIAL → `05-Modules/setup-embeddings.md`: The dual GGUF/ONNX runtime routing is a key implementation detail. setup-embeddings.md likely mentions the two runtimes but probably does not document the retry logic, multi-format response normalization, or batch-size limits in embedViaGguf. Assessed as PARTIAL.
- [ ] **Text builders (buildEmbedCode, buildEmbedNlp)** — MISSING: No doc page in the provided directory listing appears to cover the text-builder utilities for constructing embedding inputs. These are internal helpers consumed by the RAG pipeline and are unlikely to appear in any of the listed docs. Assessed as MISSING.
- [ ] **Deprecated constants (EMBEDDING_DIM, EMBEDDING_MODEL, embed)** — PARTIAL → `rag.md`: Deprecation notices with migration paths exist in JSDoc but are unlikely to be prominently covered in the docs. At best a brief migration note may exist in rag.md or 03-Guides/02-Advanced-Configuration.md. Assessed as PARTIAL.

### `src/utils/banner.ts` — 0% covered

- [ ] **Banner / MOTD startup display** — MISSING: None of the listed documentation pages (CLI Reference, Getting Started, Architecture, etc.) mention the ASCII-art banner, the MOTD_LINES array, the printBanner function, or the altMotd customisation mechanism. The concept is entirely absent from project documentation.

### `src/utils/errors.ts` — 0% covered

- [ ] **AnatolyError typed error class** — MISSING: No documentation page in docs/ covers AnatolyError, its properties (code, hint, recoverable), or the recovery-hint fallback mechanism. The internal reference in .anatoly/docs/ does not count toward coverage.
- [ ] **ERROR_CODES registry** — MISSING: No page in docs/ lists or describes the available error codes or their intended usage context. The nine codes (CONFIG_INVALID, LOCK_EXISTS, SDK_TIMEOUT, etc.) are not mentioned in any of the provided documentation files.
- [ ] **Error recovery hints** — MISSING: The DEFAULT_HINTS mechanism — surfacing actionable next steps to end users on error — is not documented anywhere in docs/. This is a user-facing feature (visible in terminal output via formatForDisplay) that would benefit from coverage in the CLI reference or a troubleshooting guide.
