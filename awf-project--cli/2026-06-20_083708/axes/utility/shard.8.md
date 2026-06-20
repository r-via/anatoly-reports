[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 8

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/domain/workflow/interactive.go` | 🟡 NEEDS_REFACTOR | 9 | 95% | [details](#internaldomainworkflowinteractivego) |
| `internal/domain/workflow/subworkflow.go` | 🟡 NEEDS_REFACTOR | 9 | 95% | [details](#internaldomainworkflowsubworkflowgo) |
| `internal/infrastructure/agents/cli_executor.go` | 🟡 NEEDS_REFACTOR | 8 | 95% | [details](#internalinfrastructureagentscliexecutorgo) |
| `internal/infrastructure/pluginmgr/manifest_parser.go` | 🟡 NEEDS_REFACTOR | 9 | 95% | [details](#internalinfrastructurepluginmgrmanifestparsergo) |
| `internal/infrastructure/repository/composite_repository.go` | 🟡 NEEDS_REFACTOR | 9 | 95% | [details](#internalinfrastructurerepositorycompositerepositorygo) |
| `internal/infrastructure/transcript/recorder.go` | 🟡 NEEDS_REFACTOR | 9 | 95% | [details](#internalinfrastructuretranscriptrecordergo) |
| `internal/interfaces/cli/ui/colors.go` | 🟡 NEEDS_REFACTOR | 9 | 95% | [details](#internalinterfacescliuicolorsgo) |
| `internal/interfaces/cli/ui/dry_run_formatter.go` | 🟡 NEEDS_REFACTOR | 8 | 95% | [details](#internalinterfacescliuidryrunformattergo) |
| `internal/domain/ports/plugin.go` | 🟡 NEEDS_REFACTOR | 8 | 100% | [details](#internaldomainportsplugingo) |
| `internal/application/template_service.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalapplicationtemplateservicego) |

## 🔍 Symbol Details

### `internal/domain/workflow/interactive.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `InteractiveAction` | L4–L4 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ActionContinue` | L7–L7 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ActionSkip` | L8–L8 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ActionAbort` | L9–L9 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ActionInspect` | L10–L10 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ActionEdit` | L11–L11 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ActionRetry` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `InteractiveStepInfo` | L16–L23 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `InteractiveResult` | L26–L37 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/workflow/subworkflow.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `DefaultSubWorkflowTimeout` | L9–L9 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MaxCallStackDepth` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `CallWorkflowConfig` | L15–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Validate` | L23–L31 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetTimeout` | L35–L40 | 🔴 DEAD | 72% | Exported but imported by 0 files |
| `SubWorkflowResult` | L43–L49 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewSubWorkflowResult` | L52–L58 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Duration` | L61–L63 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `Success` | L66–L68 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/agents/cli_executor.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ExecCLIExecutor` | L23–L23 | 🔴 DEAD | 95% | Exported type with 0 runtime and 0 type-only importers. Only referenced locally in method receivers and compile-time assertion. |
| `NewExecCLIExecutor` | L25–L27 | 🔴 DEAD | 95% | Exported constructor with 0 importers. No external callers instantiate ExecCLIExecutor. |
| `Run` | L29–L31 | 🔴 DEAD | 95% | Exported method with 0 importers. Delegates to RunWithEnv but is never called externally. |
| `RunWithEnv` | L33–L91 | 🔴 DEAD | 90% | Exported method with 0 importers. Called only by Run (also dead). No external consumers. |
| `Signal` | L167–L172 | 🔴 DEAD | 85% | Method on unexported type satisfying ports.CLIProcess, but never called directly in file and Start has 0 importers, so no external consumer ever calls it via the interface. |
| `Wait` | L174–L180 | 🔴 DEAD | 85% | Method on unexported type; the goroutine in Start calls sync.Once.Do directly, not this method. 0 external importers mean no interface consumer exists. |
| `Done` | L182–L184 | 🔴 DEAD | 85% | Method on unexported type satisfying ports.CLIProcess; never called locally and Start has 0 importers. |
| `Start` | L188–L209 | 🔴 DEAD | 85% | Exported method with 0 importers. No external callers use the non-blocking launch path. |

### `internal/infrastructure/pluginmgr/manifest_parser.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ManifestParseError` | L14–L19 | 🔴 DEAD | 95% | Exported struct with 0 external importers. Used only within this package by NewManifestParseError and WrapManifestParseError, which are themselves dead. |
| `Error` | L22–L27 | 🔴 DEAD | 95% | Exported method implementing error interface on ManifestParseError. 0 external importers; entire ManifestParseError chain is unused externally. |
| `Unwrap` | L30–L32 | 🔴 DEAD | 95% | Exported method supporting errors.As/Is unwrapping. 0 external importers; reachable only via ManifestParseError which has no external consumers. |
| `NewManifestParseError` | L35–L41 | 🔴 DEAD | 95% | Exported constructor with 0 external importers. Called only by validateYAMLManifest internally, which is also unreachable externally. |
| `WrapManifestParseError` | L44–L50 | 🔴 DEAD | 90% | Exported constructor with 0 external importers. Called only inside parse() within this package. |
| `ManifestParser` | L82–L82 | 🔴 DEAD | 95% | Exported struct with 0 external importers. No other package constructs or uses ManifestParser. |
| `NewManifestParser` | L85–L87 | 🔴 DEAD | 95% | Exported constructor with 0 external importers. Entry point for ManifestParser is never called outside this package. |
| `ParseFile` | L90–L103 | 🔴 DEAD | 95% | Exported method with 0 external importers. Primary public API for file-based parsing but no callers exist outside the package. |
| `Parse` | L106–L108 | 🔴 DEAD | 95% | Exported method with 0 external importers. Public reader-based parsing API with no external callers. |

### `internal/infrastructure/repository/composite_repository.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `SourcedPath` | L16–L19 | 🔴 DEAD | 95% | Exported type with 0 runtime or type-only importers. |
| `CompositeRepository` | L25–L30 | 🔴 DEAD | 95% | Exported struct with 0 importers; never instantiated outside this file. |
| `NewCompositeRepository` | L32–L41 | 🔴 DEAD | 95% | Constructor with 0 importers; CompositeRepository is never created externally. |
| `Load` | L44–L68 | 🔴 DEAD | 95% | 0 importers; internal repo.Load calls invoke YAMLRepository.Load, not this method. |
| `List` | L71–L93 | 🔴 DEAD | 90% | 0 importers; internal repo.List calls invoke YAMLRepository.List, not this method. |
| `ListWithSource` | L98–L124 | 🔴 DEAD | 90% | 0 importers; implements ports.WorkflowRepository but CompositeRepository is never used externally. |
| `Exists` | L127–L142 | 🔴 DEAD | 75% | 0 importers; never called from outside this file. |
| `SetPackPaths` | L152–L155 | 🔴 DEAD | 95% | 0 importers; packLocalDir and packGlobalDir are never populated externally. |
| `LoadPack` | L159–L188 | 🔴 DEAD | 95% | 0 importers; depends on SetPackPaths which is also dead, so pack dirs are always empty strings. |

### `internal/infrastructure/transcript/recorder.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `RecorderOption` | L16–L16 | 🔴 DEAD | 95% | Exported type with 0 external importers. Used only within this file as parameter/return types for other dead symbols. |
| `Recorder` | L18–L29 | 🔴 DEAD | 95% | Exported struct with 0 external importers. No external code constructs or references this type. |
| `NewRecorder` | L31–L39 | 🔴 DEAD | 95% | Exported constructor with 0 external importers. No external caller instantiates a Recorder. |
| `WithFanOutBufferSize` | L41–L45 | 🔴 DEAD | 95% | Exported option func with 0 external importers. |
| `WithRecorderLogger` | L47–L51 | 🔴 DEAD | 95% | Exported option func with 0 external importers. |
| `WithMasker` | L53–L57 | 🔴 DEAD | 95% | Exported option func with 0 external importers. |
| `Record` | L75–L98 | 🔴 DEAD | 90% | Exported method with 0 external importers. Likely intended to satisfy ports.Recorder interface but no external file imports or wires it. |
| `Subscribe` | L100–L102 | 🔴 DEAD | 95% | Exported method with 0 external importers. |
| `Close` | L104–L120 | 🔴 DEAD | 82% | Exported method with 0 external importers. |

### `internal/interfaces/cli/ui/colors.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Colorizer` | L6–L14 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewColorizer` | L24–L44 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Success` | L47–L49 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L52–L54 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Warning` | L57–L59 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Info` | L62–L64 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Bold` | L67–L69 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Dim` | L72–L74 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Status` | L77–L92 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/interfaces/cli/ui/dry_run_formatter.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `DryRunFormatter` | L13–L16 | 🔴 DEAD | 95% | 0 external importers. Entire type is unconsumed outside this package. |
| `NewDryRunFormatter` | L19–L24 | 🔴 DEAD | 95% | 0 external importers. No external caller can obtain a DryRunFormatter instance. |
| `Format` | L27–L40 | 🔴 DEAD | 95% | 0 external importers. Primary entry point is never called from outside this package. |
| `NewDryRunFormatterWithWriter` | L427–L432 | 🔴 DEAD | 95% | 0 external importers and body is identical to NewDryRunFormatter — duplicate with no consumers. |
| `FormatFieldIfPresent` | L435–L447 | 🔴 DEAD | 80% | 0 external importers; exported unnecessarily. Called locally at L119, L132, L140, L148, L209, L378, L380, L382 — should be unexported. |
| `FormatIntFieldIfPositive` | L450–L459 | 🔴 DEAD | 80% | 0 external importers; exported unnecessarily. Called locally at L393 — should be unexported. |
| `FormatRetry` | L462–L475 | 🔴 DEAD | 80% | 0 external importers; exported unnecessarily. Called locally at L136 — should be unexported. |
| `FormatCapture` | L478–L504 | 🔴 DEAD | 80% | 0 external importers; exported unnecessarily. Called locally at L144 — should be unexported. |

### `internal/domain/ports/plugin.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Plugin` | L10–L19 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `PluginManager` | L22–L37 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `OperationProvider` | L40–L47 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `PluginRegistry` | L50–L57 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `PluginLoader` | L60–L68 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `PluginStore` | L71–L80 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `PluginConfig` | L83–L95 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `PluginStateStore` | L99–L102 | 🔴 DEAD | 100% | Exported but imported by 0 files |

### `internal/application/template_service.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `NewTemplateService` | L19–L27 | 🔴 DEAD | 95% | 0 external importers; not called anywhere in the file. No consumer constructs a TemplateService. |
| `ExpandWorkflow` | L30–L48 | 🔴 DEAD | 90% | 0 external importers; not called locally. Public entry point for template expansion is never invoked. |
| `ExpandNestedTemplate` | L194–L207 | 🔴 DEAD | 90% | 0 external importers; not called locally. expandStep handles nesting directly at L88-L92 via s.expandStep, bypassing this wrapper entirely. |
| `ApplyTemplateFields` | L212–L297 | 🔴 DEAD | 90% | 0 external importers; not called locally. expandStep performs all field merging inline at L96-L131, making this a parallel dead implementation. |
| `ValidateTemplateRef` | L336–L344 | 🔴 DEAD | 95% | 0 external importers; not called locally. Standalone validation path has no consumer. |

## ⚡ Quick Wins

- [ ] <!-- ACT-df07ec-6 --> **[utility · high · trivial]** `internal/application/template_service.go`: Remove dead code: `NewTemplateService` is exported but unused `NewTemplateService`, `ExpandWorkflow`, `ExpandNestedTemplate`, `ApplyTemplateFields`, `ValidateTemplateRef` (`NewTemplateService, ExpandWorkflow, ExpandNestedTemplate, ApplyTemplateFields, ValidateTemplateRef`) [L19-L27, L30-L48, L194-L207, L212-L297, L336-L344]
- [ ] <!-- ACT-4f80ac-1 --> **[utility · high · trivial]** `internal/domain/workflow/interactive.go`: Remove dead code: `InteractiveAction` is exported but unused `InteractiveAction`, `ActionContinue`, `ActionSkip`, `ActionAbort`, `ActionInspect`, `ActionEdit`, `ActionRetry`, `InteractiveStepInfo`, `InteractiveResult` (`InteractiveAction, ActionContinue, ActionSkip, ActionAbort, ActionInspect, ActionEdit, ActionRetry, InteractiveStepInfo, InteractiveResult`) [L4-L4, L7-L7, L8-L8, L9-L9, L10-L10, L11-L11, L12-L12, L16-L23, L26-L37]
- [ ] <!-- ACT-b1ef22-2 --> **[utility · high · trivial]** `internal/domain/workflow/subworkflow.go`: Remove dead code: `DefaultSubWorkflowTimeout` is exported but unused `DefaultSubWorkflowTimeout`, `MaxCallStackDepth`, `CallWorkflowConfig`, `Validate`, `SubWorkflowResult`, `NewSubWorkflowResult`, `Duration`, `Success` (`DefaultSubWorkflowTimeout, MaxCallStackDepth, CallWorkflowConfig, Validate, SubWorkflowResult, NewSubWorkflowResult, Duration, Success`) [L9-L9, L12-L12, L15-L20, L23-L31, L43-L49, L52-L58, L61-L63, L66-L68]
- [ ] <!-- ACT-37898c-4 --> **[utility · high · trivial]** `internal/infrastructure/agents/cli_executor.go`: Remove dead code: `ExecCLIExecutor` is exported but unused `ExecCLIExecutor`, `NewExecCLIExecutor`, `Run`, `RunWithEnv`, `Signal`, `Wait`, `Done`, `Start` (`ExecCLIExecutor, NewExecCLIExecutor, Run, RunWithEnv, Signal, Wait, Done, Start`) [L23-L23, L25-L27, L29-L31, L33-L91, L167-L172, L174-L180, L182-L184, L188-L209]
- [ ] <!-- ACT-0e8e09-1 --> **[utility · high · trivial]** `internal/infrastructure/pluginmgr/manifest_parser.go`: Remove dead code: `ManifestParseError` is exported but unused `ManifestParseError`, `Error`, `Unwrap`, `NewManifestParseError`, `WrapManifestParseError`, `ManifestParser`, `NewManifestParser`, `ParseFile`, `Parse` (`ManifestParseError, Error, Unwrap, NewManifestParseError, WrapManifestParseError, ManifestParser, NewManifestParser, ParseFile, Parse`) [L14-L19, L22-L27, L30-L32, L35-L41, L44-L50, L82-L82, L85-L87, L90-L103, L106-L108]
- [ ] <!-- ACT-176f28-2 --> **[utility · high · trivial]** `internal/infrastructure/repository/composite_repository.go`: Remove dead code: `SourcedPath` is exported but unused `SourcedPath`, `CompositeRepository`, `NewCompositeRepository`, `Load`, `List`, `ListWithSource`, `SetPackPaths`, `LoadPack` (`SourcedPath, CompositeRepository, NewCompositeRepository, Load, List, ListWithSource, SetPackPaths, LoadPack`) [L16-L19, L25-L30, L32-L41, L44-L68, L71-L93, L98-L124, L152-L155, L159-L188]
- [ ] <!-- ACT-95e81b-2 --> **[utility · high · trivial]** `internal/infrastructure/transcript/recorder.go`: Remove dead code: `RecorderOption` is exported but unused `RecorderOption`, `Recorder`, `NewRecorder`, `WithFanOutBufferSize`, `WithRecorderLogger`, `WithMasker`, `Record`, `Subscribe`, `Close` (`RecorderOption, Recorder, NewRecorder, WithFanOutBufferSize, WithRecorderLogger, WithMasker, Record, Subscribe, Close`) [L16-L16, L18-L29, L31-L39, L41-L45, L47-L51, L53-L57, L75-L98, L100-L102, L104-L120]
- [ ] <!-- ACT-6c3ccd-1 --> **[utility · high · trivial]** `internal/interfaces/cli/ui/colors.go`: Remove dead code: `Colorizer` is exported but unused `Colorizer`, `NewColorizer`, `Success`, `Error`, `Warning`, `Info`, `Bold`, `Dim`, `Status` (`Colorizer, NewColorizer, Success, Error, Warning, Info, Bold, Dim, Status`) [L6-L14, L24-L44, L47-L49, L52-L54, L57-L59, L62-L64, L67-L69, L72-L74, L77-L92]
- [ ] <!-- ACT-ac9d6d-2 --> **[utility · high · trivial]** `internal/interfaces/cli/ui/dry_run_formatter.go`: Remove dead code: `DryRunFormatter` is exported but unused `DryRunFormatter`, `NewDryRunFormatter`, `Format`, `NewDryRunFormatterWithWriter`, `FormatFieldIfPresent`, `FormatIntFieldIfPositive`, `FormatRetry`, `FormatCapture` (`DryRunFormatter, NewDryRunFormatter, Format, NewDryRunFormatterWithWriter, FormatFieldIfPresent, FormatIntFieldIfPositive, FormatRetry, FormatCapture`) [L13-L16, L19-L24, L27-L40, L427-L432, L435-L447, L450-L459, L462-L475, L478-L504]

## 🔧 Refactors

- [ ] <!-- ACT-b1ef22-3 --> **[utility · medium · trivial]** `internal/domain/workflow/subworkflow.go`: Remove dead code: `GetTimeout` is exported but unused (`GetTimeout`) [L35-L40]
- [ ] <!-- ACT-176f28-3 --> **[utility · medium · trivial]** `internal/infrastructure/repository/composite_repository.go`: Remove dead code: `Exists` is exported but unused (`Exists`) [L127-L142]
