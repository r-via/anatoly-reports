[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 21

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/domain/pluginmodel/manifest.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internaldomainpluginmodelmanifestgo) |
| `internal/domain/ports/executor.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internaldomainportsexecutorgo) |
| `internal/domain/transcript/payload.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internaldomaintranscriptpayloadgo) |
| `internal/domain/workflow/execution_record.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internaldomainworkflowexecutionrecordgo) |
| `internal/infrastructure/acp/emitter.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinfrastructureacpemittergo) |
| `internal/infrastructure/acp/errors.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinfrastructureacperrorsgo) |
| `internal/infrastructure/acp/permission.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinfrastructureacppermissiongo) |
| `internal/infrastructure/agents/content_block_normalizer.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinfrastructureagentscontentblocknormalizergo) |
| `internal/infrastructure/analyzer/template_analyzer.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinfrastructureanalyzertemplateanalyzergo) |
| `internal/infrastructure/errors/human_formatter.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinfrastructureerrorshumanformattergo) |

## 🔍 Symbol Details

### `internal/domain/pluginmodel/manifest.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Manifest` | L46–L57 | 🔴 DEAD | 85% | Primary exported type of package with 0 external importers. No external package consumes this struct; entire pluginmodel package appears unimported. |
| `Validate` | L67–L96 | 🔴 DEAD | 95% | 0 external callers and not invoked anywhere within this file. |
| `HasCapability` | L140–L147 | 🔴 DEAD | 95% | 0 external callers and not invoked anywhere within this file. |

### `internal/domain/ports/executor.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Command` | L20–L27 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `CommandResult` | L29–L33 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `CommandExecutor` | L35–L37 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/transcript/payload.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `MessagePayload` | L3–L6 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepPayload` | L8–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ToolPayload` | L22–L29 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/workflow/execution_record.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ExecutionRecord` | L7–L17 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `HistoryFilter` | L20–L26 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `HistoryStats` | L29–L35 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/acp/emitter.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Emitter` | L25–L28 | 🔴 DEAD | 90% | 0 external importers. The compile-time assertion at L11 (`var _ application.SessionUpdateEmitter = (*Emitter)(nil)`) is file-local and does not constitute external usage. No file constructs or receives this type. |
| `NewEmitter` | L33–L38 | 🔴 DEAD | 95% | 0 external importers. The only constructor for Emitter is never called from any other file. |
| `EmitSessionUpdate` | L40–L83 | 🔴 DEAD | 90% | 0 external importers. Satisfies application.SessionUpdateEmitter interface but since Emitter itself is never instantiated externally (NewEmitter has 0 importers), this method is unreachable at runtime. |

### `internal/infrastructure/acp/errors.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `invalidParamsErr` | L34–L36 | 🔴 DEAD | 70% | Not called anywhere in this file. May be used by other files in package acp, but no evidence in the provided context. |
| `internalErr` | L38–L40 | 🔴 DEAD | 70% | Not called anywhere in this file. May be used by other files in package acp, but no evidence in the provided context. |
| `methodNotFoundErr` | L42–L44 | 🔴 DEAD | 70% | Not called anywhere in this file. May be used by other files in package acp, but no evidence in the provided context. |

### `internal/infrastructure/acp/permission.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `PermissionClient` | L26–L29 | 🔴 DEAD | 95% | Exported struct with 0 external importers per pre-computed analysis. The local compile-time assertion at L11 and NewPermissionClient's return type are intra-package references only — no external caller wires this adapter. |
| `NewPermissionClient` | L34–L39 | 🔴 DEAD | 95% | Exported constructor with 0 external importers. PermissionClient is never wired from outside this package, so this constructor is unreachable in practice. |
| `RequestPermission` | L49–L89 | 🔴 DEAD | 95% | Exported method with 0 external importers. Satisfies ports.ACPClient locally via the L11 compile-time assertion, but since PermissionClient is never imported, this method is never dispatched through the interface externally. |

### `internal/infrastructure/agents/content_block_normalizer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ContentBlockNormalizer` | L22–L22 | 🔴 DEAD | 95% | Exported type with 0 external importers. NewContentBlockNormalizer and Normalize are also unimported, so the entire struct is unreachable from outside this package. |
| `NewContentBlockNormalizer` | L26–L28 | 🔴 DEAD | 95% | Exported constructor with 0 external importers. No call site exists outside the package. |
| `Normalize` | L34–L51 | 🔴 DEAD | 95% | Exported method with 0 external importers. The receiver type ContentBlockNormalizer is itself dead, so this method is never called. |

### `internal/infrastructure/analyzer/template_analyzer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `InterpolationAnalyzer` | L11–L11 | 🔴 DEAD | 90% | 0 external importers. The only local reference is the compile-time interface assertion on ~L61, which itself is unreachable from outside the package. |
| `NewInterpolationAnalyzer` | L14–L16 | 🔴 DEAD | 95% | 0 external importers. Constructor is never called outside this package. |
| `ExtractReferences` | L19–L37 | 🔴 DEAD | 90% | 0 external importers. Implements workflow.TemplateAnalyzer but no caller wires up InterpolationAnalyzer to that interface externally. |

### `internal/infrastructure/errors/human_formatter.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `HumanErrorFormatter` | L31–L35 | 🔴 DEAD | 95% | Exported struct with 0 external importers. Compile-time assertion at L37 is in the same file and does not constitute external usage. |
| `NewHumanErrorFormatter` | L54–L60 | 🔴 DEAD | 95% | Exported constructor with 0 external importers. No call sites outside this file. |
| `FormatError` | L89–L140 | 🔴 DEAD | 92% | Exported method with 0 external importers. Satisfies ports.ErrorFormatter interface but the type itself is never injected or used externally. |

## ⚡ Quick Wins

- [ ] <!-- ACT-436bb2-2 --> **[utility · high · trivial]** `internal/domain/pluginmodel/manifest.go`: Remove dead code: `Manifest` is exported but unused `Manifest`, `Validate`, `HasCapability` (`Manifest, Validate, HasCapability`) [L46-L57, L67-L96, L140-L147]
- [ ] <!-- ACT-b6c24e-1 --> **[utility · high · trivial]** `internal/domain/ports/executor.go`: Remove dead code: `Command` is exported but unused `Command`, `CommandResult`, `CommandExecutor` (`Command, CommandResult, CommandExecutor`) [L20-L27, L29-L33, L35-L37]
- [ ] <!-- ACT-400139-1 --> **[utility · high · trivial]** `internal/domain/transcript/payload.go`: Remove dead code: `MessagePayload` is exported but unused `MessagePayload`, `StepPayload`, `ToolPayload` (`MessagePayload, StepPayload, ToolPayload`) [L3-L6, L8-L20, L22-L29]
- [ ] <!-- ACT-f77716-1 --> **[utility · high · trivial]** `internal/domain/workflow/execution_record.go`: Remove dead code: `ExecutionRecord` is exported but unused `ExecutionRecord`, `HistoryFilter`, `HistoryStats` (`ExecutionRecord, HistoryFilter, HistoryStats`) [L7-L17, L20-L26, L29-L35]
- [ ] <!-- ACT-e9fb6e-1 --> **[utility · high · trivial]** `internal/infrastructure/acp/emitter.go`: Remove dead code: `Emitter` is exported but unused `Emitter`, `NewEmitter`, `EmitSessionUpdate` (`Emitter, NewEmitter, EmitSessionUpdate`) [L25-L28, L33-L38, L40-L83]
- [ ] <!-- ACT-0bfe43-1 --> **[utility · high · trivial]** `internal/infrastructure/acp/permission.go`: Remove dead code: `PermissionClient` is exported but unused `PermissionClient`, `NewPermissionClient`, `RequestPermission` (`PermissionClient, NewPermissionClient, RequestPermission`) [L26-L29, L34-L39, L49-L89]
- [ ] <!-- ACT-4961f5-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/content_block_normalizer.go`: Remove dead code: `ContentBlockNormalizer` is exported but unused `ContentBlockNormalizer`, `NewContentBlockNormalizer`, `Normalize` (`ContentBlockNormalizer, NewContentBlockNormalizer, Normalize`) [L22-L22, L26-L28, L34-L51]
- [ ] <!-- ACT-7c977b-1 --> **[utility · high · trivial]** `internal/infrastructure/analyzer/template_analyzer.go`: Remove dead code: `InterpolationAnalyzer` is exported but unused `InterpolationAnalyzer`, `NewInterpolationAnalyzer`, `ExtractReferences` (`InterpolationAnalyzer, NewInterpolationAnalyzer, ExtractReferences`) [L11-L11, L14-L16, L19-L37]
- [ ] <!-- ACT-141fd7-1 --> **[utility · high · trivial]** `internal/infrastructure/errors/human_formatter.go`: Remove dead code: `HumanErrorFormatter` is exported but unused `HumanErrorFormatter`, `NewHumanErrorFormatter`, `FormatError` (`HumanErrorFormatter, NewHumanErrorFormatter, FormatError`) [L31-L35, L54-L60, L89-L140]

## 🔧 Refactors

- [ ] <!-- ACT-ac78aa-1 --> **[utility · medium · trivial]** `internal/infrastructure/acp/errors.go`: Remove dead code: `invalidParamsErr` is exported but unused `invalidParamsErr`, `internalErr`, `methodNotFoundErr` (`invalidParamsErr, internalErr, methodNotFoundErr`) [L34-L36, L38-L40, L42-L44]
