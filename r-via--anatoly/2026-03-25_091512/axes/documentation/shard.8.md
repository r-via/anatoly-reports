# Documentation — Shard 8

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `src/utils/log-context.ts` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/src-utils-log-context.rev.md) |
| `src/cli/screen-renderer.ts` | NEEDS_REFACTOR | 1 | 93% | [details](../reviews/src-cli-screen-renderer.rev.md) |
| `src/cli.ts` | NEEDS_REFACTOR | 1 | 92% | [details](../reviews/src-cli.rev.md) |
| `src/commands/report.ts` | NEEDS_REFACTOR | 1 | 92% | [details](../reviews/src-commands-report.rev.md) |
| `src/commands/reset.ts` | NEEDS_REFACTOR | 1 | 92% | [details](../reviews/src-commands-reset.rev.md) |
| `src/commands/setup-embeddings.ts` | NEEDS_REFACTOR | 1 | 92% | [details](../reviews/src-commands-setup-embeddings.rev.md) |
| `src/core/doc-scaffolder.ts` | NEEDS_REFACTOR | 1 | 92% | [details](../reviews/src-core-doc-scaffolder.rev.md) |
| `src/rag/nlp-summarizer.ts` | NEEDS_REFACTOR | 1 | 92% | [details](../reviews/src-rag-nlp-summarizer.rev.md) |
| `src/rag/standalone.ts` | NEEDS_REFACTOR | 1 | 92% | [details](../reviews/src-rag-standalone.rev.md) |
| `src/schemas/progress.ts` | NEEDS_REFACTOR | 1 | 92% | [details](../reviews/src-schemas-progress.rev.md) |

## Symbol Details

### `src/utils/log-context.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `contextLogger` | L65–L83 | PARTIAL | 82% | [PARTIAL] JSDoc covers the main purpose (child logger with ALS context merged... |

### `src/cli/screen-renderer.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `ScreenRenderer` | L14–L194 | PARTIAL | 90% | [PARTIAL] The class has no class-level JSDoc explaining its purpose, lifecycl... |

### `src/cli.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `createProgram` | L34–L103 | UNDOCUMENTED | 92% | [UNDOCUMENTED] No JSDoc/TSDoc comment precedes this exported function. The na... |

### `src/commands/report.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `registerReportCommand` | L16–L69 | UNDOCUMENTED | 92% | [UNDOCUMENTED] Exported function with no JSDoc/TSDoc comment. Non-trivial beh... |

### `src/commands/reset.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `registerResetCommand` | L65–L196 | UNDOCUMENTED | 92% | [UNDOCUMENTED] No JSDoc/TSDoc comment present on this exported function. The ... |

### `src/commands/setup-embeddings.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `registerSetupEmbeddingsCommand` | L26–L40 | UNDOCUMENTED | 90% | [UNDOCUMENTED] Exported public API function with no JSDoc/TSDoc comment whats... |

### `src/core/doc-scaffolder.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `scaffoldDocs` | L125–L164 | PARTIAL | 88% | [PARTIAL] Has a prose JSDoc covering the major behaviors: base structure crea... |

### `src/rag/nlp-summarizer.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `buildUserMessage` | L44–L63 | PARTIAL | 72% | [PARTIAL] Private unexported ~20-line function with no JSDoc comment. The nam... |

### `src/rag/standalone.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `indexProjectStandalone` | L48–L110 | PARTIAL | 82% | [PARTIAL] No function-level JSDoc. The module-level comment (L5-L10) provides... |

### `src/schemas/progress.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `ProgressSchema` | L26–L30 | PARTIAL | 75% | [PARTIAL] Exported Zod object schema with no JSDoc. The version: z.literal(1)... |

## Hygiene

- [ ] <!-- ACT-fa8d4e-1 --> **[documentation · medium · trivial]** `src/cli.ts`: Add JSDoc documentation for exported symbol: `createProgram` (`createProgram`) [L34-L103]
- [ ] <!-- ACT-71c216-1 --> **[documentation · medium · trivial]** `src/commands/report.ts`: Add JSDoc documentation for exported symbol: `registerReportCommand` (`registerReportCommand`) [L16-L69]
- [ ] <!-- ACT-a78117-2 --> **[documentation · medium · trivial]** `src/commands/reset.ts`: Add JSDoc documentation for exported symbol: `registerResetCommand` (`registerResetCommand`) [L65-L196]
- [ ] <!-- ACT-ef0494-1 --> **[documentation · medium · trivial]** `src/commands/setup-embeddings.ts`: Add JSDoc documentation for exported symbol: `registerSetupEmbeddingsCommand` (`registerSetupEmbeddingsCommand`) [L26-L40]
- [ ] <!-- ACT-685f96-1 --> **[documentation · low · trivial]** `src/cli/screen-renderer.ts`: Complete JSDoc documentation for: `ScreenRenderer` (`ScreenRenderer`) [L14-L194]
- [ ] <!-- ACT-51a342-1 --> **[documentation · low · trivial]** `src/core/doc-scaffolder.ts`: Complete JSDoc documentation for: `scaffoldDocs` (`scaffoldDocs`) [L125-L164]
- [ ] <!-- ACT-64d741-1 --> **[documentation · low · trivial]** `src/rag/nlp-summarizer.ts`: Complete JSDoc documentation for: `buildUserMessage` (`buildUserMessage`) [L44-L63]
- [ ] <!-- ACT-a3d88b-1 --> **[documentation · low · trivial]** `src/rag/standalone.ts`: Complete JSDoc documentation for: `indexProjectStandalone` (`indexProjectStandalone`) [L48-L110]
- [ ] <!-- ACT-f11934-1 --> **[documentation · low · trivial]** `src/schemas/progress.ts`: Complete JSDoc documentation for: `ProgressSchema` (`ProgressSchema`) [L26-L30]
- [ ] <!-- ACT-5ad916-2 --> **[documentation · low · trivial]** `src/utils/log-context.ts`: Complete JSDoc documentation for: `contextLogger` (`contextLogger`) [L65-L83]

## Documentation Coverage

### `src/cli.ts` — 60% covered

- [ ] **createProgram programmatic factory / CLI API surface** — PARTIAL → `04-API-Reference/04-CLI-Reference.md`: The API-Reference section contains a CLI-Reference page which likely documents the entry-point, but the exported createProgram function has no JSDoc, and programmatic usage details (logger side-effect, preAction hook behaviour, return type) may be absent. Cannot confirm full coverage without page content, hence PARTIAL.
- [ ] **NO_COLOR convention support** — MISSING: The module-level guard that disables chalk when NO_COLOR is set in the environment is a user-visible behaviour, but no docs page in the provided listing is specifically dedicated to this convention. It may appear as a footnote in 03-CLI-Reference/02-Global-Options.md alongside --no-color, but no separate coverage is evident.
- [ ] **Logger initialisation via preAction hook (--log-level validation, --log-file, --verbose resolution)** — PARTIAL → `03-CLI-Reference/02-Global-Options.md`: The --log-level and --log-file flags are registered as global options and likely listed in Global-Options, but the runtime validation logic (exit on invalid level) and the interaction between --verbose and --log-level through resolveLogLevel are implementation details unlikely to be explicitly documented.

### `src/commands/reset.ts` — 50% covered

- [ ] **reset command** — PARTIAL → `03-CLI-Reference/01-Commands.md`: A CLI reference file exists at 03-CLI-Reference/01-Commands.md which is the expected location for reset command documentation. However, the specific flags --keep-rag (preserves embeddings to avoid costly rebuilds) and --keep-docs (preserves .anatoly/docs/) and the interactive confirmation flow are implementation-level details whose presence in the docs cannot be confirmed from the directory listing alone. Coverage is likely present but completeness is uncertain.

### `src/commands/setup-embeddings.ts` — 75% covered

- [ ] **Script path resolution (dev vs bundle layout)** — MISSING: The dual-layout path resolution logic in findSetupScript (handling both dist/ bundle and src/commands/ dev layouts) is an internal implementation detail not expected to be in user-facing docs, but it is also absent from any contributing/architecture docs that might help maintainers understand the packaging contract.

### `src/core/doc-scaffolder.ts` — 0% covered

- [ ] **Documentation scaffolding (scaffoldDocs)** — MISSING: The scaffoldDocs function — the primary public API of this module — is not described in any docs page. Its behaviors (generating .anatoly/docs/ structure, no-overwrite policy, index regeneration, 06→05 renumbering) have no coverage anywhere in docs/.
- [ ] **Project-type page selection (CLI / Frontend / Backend API / ORM / Monorepo pages)** — MISSING: The mechanism by which detected project types (ProjectType) map to additional scaffold page sets is not documented anywhere in docs/. No guide or architecture page explains the page-selection strategy.
- [ ] **SCAFFOLDING hints in generated pages** — MISSING: The concept of contextual SCAFFOLDING HTML comments embedded in each generated page (Story 29.3) — including the sourceHints mechanism for project-context-aware hints — is absent from all docs pages.
- [ ] **PageDef and ScaffoldResult public types** — MISSING: Both exported types are part of the public API but appear in none of the docs/04-API-Reference or docs/04-Core-Modules pages.

### `src/rag/standalone.ts` — 38% covered

- [ ] **Standalone RAG indexing** — PARTIAL → `rag.md`: rag.md and 05-Modules/setup-embeddings.md likely cover the RAG setup workflow, but the standalone callable interface (indexProjectStandalone, callable outside anatoly run) is a specific integration surface that may not be explicitly documented. File names suggest coverage of the RAG system in general but not this standalone entry point in particular.
- [ ] **Hardware detection and backend selection** — PARTIAL → `02-Architecture/03-RAG-Engine.md`: 02-Architecture/03-RAG-Engine.md is the most likely home for hardware-detect and backend determination logic (advanced-gguf vs lite). The file naming is consistent with this concept being covered, but the fallback behavior (GGUF failure → ONNX lite) may not be explicitly documented.
- [ ] **Docker container lifecycle for GGUF/TEI** — MISSING: No doc file in the provided directory listing clearly corresponds to GGUF or TEI Docker container management. The 05-Modules/setup-embeddings.md is the closest candidate but is uncertain. Container cleanup-on-failure semantics are unlikely to be covered in the architecture docs.
- [ ] **RAG table name resolution** — MISSING: resolveRagTableName and the function_cards_{mode} table naming convention do not correspond to any doc file name in the provided directory. This is an internal storage detail that is unlikely to appear in the docs as documented.
