[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 22

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/infrastructure/errors/json_formatter.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinfrastructureerrorsjsonformattergo) |
| `internal/infrastructure/expression/expr_validator.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinfrastructureexpressionexprvalidatorgo) |
| `internal/infrastructure/notify/desktop.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinfrastructurenotifydesktopgo) |
| `internal/infrastructure/otel/provider.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinfrastructureotelprovidergo) |
| `internal/infrastructure/repository/yaml_types.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinfrastructurerepositoryyamltypesgo) |
| `internal/infrastructure/workflowpkg/manifest.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinfrastructureworkflowpkgmanifestgo) |
| `internal/interfaces/cli/pack_resolver.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinterfacesclipackresolvergo) |
| `internal/interfaces/cli/ui/stdin_input_reader.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinterfacescliuistdininputreadergo) |
| `internal/interfaces/cli/ui/sync_writer.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinterfacescliuisyncwritergo) |
| `internal/interfaces/tui/tab_logs.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacestuitablogsgo) |

## 🔍 Symbol Details

### `internal/infrastructure/errors/json_formatter.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `JSONErrorFormatter` | L31–L34 | 🔴 DEAD | 95% | Exported type with 0 runtime and 0 type-only importers. Only referenced within this file (compile-time assertion and method receivers). |
| `NewJSONErrorFormatter` | L52–L57 | 🔴 DEAD | 95% | Exported constructor with 0 importers. No external package instantiates JSONErrorFormatter through this function. |
| `FormatError` | L85–L115 | 🔴 DEAD | 95% | Exported method with 0 importers. The compile-time assertion (var _ ports.ErrorFormatter) verifies interface satisfaction but no caller invokes this method. |

### `internal/infrastructure/expression/expr_validator.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ExprValidator` | L13–L13 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewExprValidator` | L16–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Compile` | L23–L34 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/notify/desktop.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `NewDesktopBackend` | L49–L51 | 🔴 DEAD | 95% | 0 external importers. Comment claims CLI wiring in run.go but no file imports it per import analysis. |
| `Send` | L61–L142 | 🔴 DEAD | 85% | Implements Backend interface but 0 files import this package. NewDesktopBackend (the only way to obtain a desktopBackend) is itself dead, making Send unreachable. |

### `internal/infrastructure/otel/provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Config` | L20–L24 | 🔴 DEAD | 90% | 0 external importers. Only referenced locally as the parameter type of NewTracerProvider, which itself has 0 importers — no external consumer can ever construct or pass a Config. |
| `TracerProvider` | L28–L32 | 🔴 DEAD | 85% | 0 external importers. The compile-time assertion `var _ ports.Tracer = (*TracerProvider)(nil)` is local and keeps the type structurally valid, but no external code ever instantiates or holds a *TracerProvider. |
| `NewTracerProvider` | L58–L92 | 🔴 DEAD | 90% | 0 external importers. The sole constructor for TracerProvider is never called from outside this package. |

### `internal/infrastructure/repository/yaml_types.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `yamlTemplate` | L168–L172 | 🔴 DEAD | 95% | Maintainer-annotated //nolint:unused stub for F017; no active consumer exists yet (template_repository.go not present). |
| `yamlTemplateParam` | L177–L181 | 🔴 DEAD | 95% | Maintainer-annotated //nolint:unused stub; only referenced from the equally-dead yamlTemplate. |

### `internal/infrastructure/workflowpkg/manifest.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Manifest` | L15–L24 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ParseManifest` | L27–L38 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Validate` | L45–L87 | 🔴 DEAD | 92% | Exported but imported by 0 files |

### `internal/interfaces/cli/pack_resolver.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `parseWorkflowNamespace` | L24–L30 | 🔴 DEAD | 65% | Not called anywhere in this file. The comment acknowledges a duplicate exists in the application layer; this copy may be called from another file in the cli package, but no evidence exists in the provided content. |
| `resolvePackWorkflow` | L123–L145 | 🔴 DEAD | 65% | Not called within this file. As the primary pack-resolution entry point it is likely called from a command handler elsewhere in the cli package, but no evidence exists in the provided content. |

### `internal/interfaces/cli/ui/stdin_input_reader.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `StdinInputReader` | L15–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewStdinInputReader` | L21–L26 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ReadInput` | L31–L34 | 🔴 DEAD | 90% | Exported but imported by 0 files |

### `internal/interfaces/cli/ui/sync_writer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `SyncWriter` | L21–L24 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewSyncWriter` | L28–L30 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Write` | L34–L38 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/interfaces/tui/tab_logs.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `newLogsTab` | L51–L60 | 🔴 DEAD | 65% | Not called anywhere in this file. Non-exported constructor; may be called in a sibling file of the same package, but no local evidence of use. |

## ⚡ Quick Wins

- [ ] <!-- ACT-fd81be-1 --> **[utility · high · trivial]** `internal/infrastructure/errors/json_formatter.go`: Remove dead code: `JSONErrorFormatter` is exported but unused `JSONErrorFormatter`, `NewJSONErrorFormatter`, `FormatError` (`JSONErrorFormatter, NewJSONErrorFormatter, FormatError`) [L31-L34, L52-L57, L85-L115]
- [ ] <!-- ACT-954fb7-1 --> **[utility · high · trivial]** `internal/infrastructure/expression/expr_validator.go`: Remove dead code: `ExprValidator` is exported but unused `ExprValidator`, `NewExprValidator`, `Compile` (`ExprValidator, NewExprValidator, Compile`) [L13-L13, L16-L18, L23-L34]
- [ ] <!-- ACT-9d9e68-2 --> **[utility · high · trivial]** `internal/infrastructure/notify/desktop.go`: Remove dead code: `NewDesktopBackend` is exported but unused (`NewDesktopBackend`) [L49-L51]
- [ ] <!-- ACT-9d9e68-3 --> **[utility · high · trivial]** `internal/infrastructure/notify/desktop.go`: Remove dead code: `Send` is exported but unused (`Send`) [L61-L142]
- [ ] <!-- ACT-c9aa2e-1 --> **[utility · high · trivial]** `internal/infrastructure/otel/provider.go`: Remove dead code: `Config` is exported but unused `Config`, `TracerProvider`, `NewTracerProvider` (`Config, TracerProvider, NewTracerProvider`) [L20-L24, L28-L32, L58-L92]
- [ ] <!-- ACT-3c4111-2 --> **[utility · high · trivial]** `internal/infrastructure/repository/yaml_types.go`: Remove dead code: `yamlTemplate` is exported but unused (`yamlTemplate`) [L168-L172]
- [ ] <!-- ACT-3c4111-3 --> **[utility · high · trivial]** `internal/infrastructure/repository/yaml_types.go`: Remove dead code: `yamlTemplateParam` is exported but unused (`yamlTemplateParam`) [L177-L181]
- [ ] <!-- ACT-cfddcd-1 --> **[utility · high · trivial]** `internal/infrastructure/workflowpkg/manifest.go`: Remove dead code: `Manifest` is exported but unused `Manifest`, `ParseManifest`, `Validate` (`Manifest, ParseManifest, Validate`) [L15-L24, L27-L38, L45-L87]
- [ ] <!-- ACT-250f5f-1 --> **[utility · high · trivial]** `internal/interfaces/cli/ui/stdin_input_reader.go`: Remove dead code: `StdinInputReader` is exported but unused `StdinInputReader`, `NewStdinInputReader`, `ReadInput` (`StdinInputReader, NewStdinInputReader, ReadInput`) [L15-L18, L21-L26, L31-L34]
- [ ] <!-- ACT-2c37f5-1 --> **[utility · high · trivial]** `internal/interfaces/cli/ui/sync_writer.go`: Remove dead code: `SyncWriter` is exported but unused `SyncWriter`, `NewSyncWriter`, `Write` (`SyncWriter, NewSyncWriter, Write`) [L21-L24, L28-L30, L34-L38]

## 🔧 Refactors

- [ ] <!-- ACT-37a14b-3 --> **[utility · medium · trivial]** `internal/interfaces/cli/pack_resolver.go`: Remove dead code: `parseWorkflowNamespace` is exported but unused (`parseWorkflowNamespace`) [L24-L30]
- [ ] <!-- ACT-37a14b-4 --> **[utility · medium · trivial]** `internal/interfaces/cli/pack_resolver.go`: Remove dead code: `resolvePackWorkflow` is exported but unused (`resolvePackWorkflow`) [L123-L145]
- [ ] <!-- ACT-14e37d-3 --> **[utility · medium · trivial]** `internal/interfaces/tui/tab_logs.go`: Remove dead code: `newLogsTab` is exported but unused (`newLogsTab`) [L51-L60]
