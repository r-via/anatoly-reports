[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 7

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/infrastructure/agents/openai_compatible_provider.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinfrastructureagentsopenaicompatibleprovidergo) |
| `internal/infrastructure/pluginmgr/stream_manager.go` | 🟡 NEEDS_REFACTOR | 9 | 95% | [details](#internalinfrastructurepluginmgrstreammanagergo) |
| `internal/infrastructure/tools/builtins/provider.go` | 🟡 NEEDS_REFACTOR | 10 | 95% | [details](#internalinfrastructuretoolsbuiltinsprovidergo) |
| `pkg/display/event.go` | 🟡 NEEDS_REFACTOR | 10 | 95% | [details](#pkgdisplayeventgo) |
| `pkg/interpolation/resolver.go` | 🟡 NEEDS_REFACTOR | 10 | 95% | [details](#pkginterpolationresolvergo) |
| `internal/application/interactive_executor.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalapplicationinteractiveexecutorgo) |
| `internal/domain/operation/registry.go` | 🟡 NEEDS_REFACTOR | 9 | 95% | [details](#internaldomainoperationregistrygo) |
| `internal/domain/ports/plugin_validator.go` | 🟡 NEEDS_REFACTOR | 9 | 95% | [details](#internaldomainportspluginvalidatorgo) |
| `internal/domain/ports/tracer.go` | 🟡 NEEDS_REFACTOR | 9 | 95% | [details](#internaldomainportstracergo) |
| `internal/domain/workflow/audit_event.go` | 🟡 NEEDS_REFACTOR | 8 | 95% | [details](#internaldomainworkflowauditeventgo) |

## 🔍 Symbol Details

### `internal/infrastructure/agents/openai_compatible_provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `OpenAICompatibleProvider` | L43–L46 | 🔴 DEAD | 75% | 0 importers per exhaustive analysis. Compile-time interface guard at L22 confirms intent, but NewOpenAICompatibleProvider is also unimported, so the type is never instantiated. Possibly not yet wired into a provider registry. |
| `NewOpenAICompatibleProvider` | L105–L113 | 🔴 DEAD | 75% | 0 importers per exhaustive analysis. The provider is never instantiated anywhere in the codebase. |
| `SetToolRouter` | L115–L117 | 🔴 DEAD | 75% | 0 importers. Interface method on an uninstantiated type; never called externally. |
| `Name` | L119–L121 | 🔴 DEAD | 75% | 0 importers. Interface method on an uninstantiated type. |
| `Validate` | L123–L125 | 🔴 DEAD | 75% | 0 importers. Trivially returns nil and is never called externally. |
| `Execute` | L131–L179 | 🔴 DEAD | 75% | 0 importers. Interface method on an uninstantiated type; never invoked externally. |
| `ExecuteConversation` | L417–L488 | 🔴 DEAD | 75% | 0 importers. Interface method on an uninstantiated type; never invoked externally. |

### `internal/infrastructure/pluginmgr/stream_manager.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `EventStreamSender` | L18–L20 | 🔴 DEAD | 95% | Exported with 0 external importers. Used only within this file as a field type and parameter type, but no consumer outside the package depends on it. |
| `StreamManager` | L28–L33 | 🔴 DEAD | 95% | Exported with 0 external importers. Entire type is unrooted from any external consumer. |
| `NewStreamManager` | L38–L44 | 🔴 DEAD | 95% | Exported constructor with 0 external importers; StreamManager is never instantiated outside this package. |
| `RegisterStream` | L47–L51 | 🔴 DEAD | 90% | Exported method with 0 external importers; no caller outside the package populates the stream registry. |
| `UnregisterStream` | L54–L58 | 🔴 DEAD | 90% | Exported with 0 external importers. Called internally at L107 inside DeliverEvent, but that entire call chain is unreachable externally. |
| `HasStream` | L61–L66 | 🔴 DEAD | 95% | Exported with 0 external importers and no internal call sites in this file. |
| `GetDeliverer` | L69–L82 | 🔴 DEAD | 90% | Exported with 0 external importers and no internal call sites in this file. |
| `Close` | L85–L89 | 🔴 DEAD | 95% | Exported with 0 external importers and no internal call sites in this file. |
| `DeliverEvent` | L98–L111 | 🔴 DEAD | 88% | Exported method with 0 external importers. Satisfies EventDeliverer locally (L35 assertion), but since GetDeliverer (the only factory) is itself dead, this method is never reached at runtime. |

### `internal/infrastructure/tools/builtins/provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `MaxReadBytes` | L22–L22 | 🔴 DEAD | 65% | 0 cross-package importers. Not referenced in this file. Likely consumed by readHandler/editHandler in sibling package files, but those files are absent from this analysis context. |
| `Provider` | L39–L49 | 🔴 DEAD | 75% | 0 cross-package importers. Interface compliance assertion at L12 proves intent, but no external caller constructs or references Provider. |
| `Option` | L52–L52 | 🔴 DEAD | 75% | 0 cross-package importers. Used within this file as the return type of WithExecutor/WithRootDir and parameter type of NewProvider, but the entire construction chain is externally dead. |
| `WithExecutor` | L55–L59 | 🔴 DEAD | 80% | 0 cross-package importers. No external caller injects a CommandExecutor via this option. |
| `WithRootDir` | L70–L82 | 🔴 DEAD | 80% | 0 cross-package importers. No external caller sets root-dir restriction through this option. |
| `NewProvider` | L85–L111 | 🔴 DEAD | 80% | 0 cross-package importers. Primary construction entry point for Provider, but nothing calls it outside this package. |
| `ListTools` | L128–L135 | 🔴 DEAD | 80% | 0 cross-package importers. Satisfies ports.ToolProvider interface (L12), but no external caller invokes it. |
| `CallTool` | L141–L150 | 🔴 DEAD | 80% | 0 cross-package importers. Satisfies ports.ToolProvider interface (L12), but no external caller invokes it. |
| `Close` | L153–L155 | 🔴 DEAD | 80% | 0 cross-package importers. Satisfies ports.ToolProvider interface (L12) as a no-op, but no external caller invokes it. |
| `resolvePath` | L165–L192 | 🔴 DEAD | 65% | Non-exported; not called anywhere in this file. Almost certainly called by readHandler, writeHandler, etc. in sibling files, but those are not in scope for this analysis. |

### `pkg/display/event.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `EventKind` | L6–L6 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventText` | L9–L9 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventToolUse` | L10–L10 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventReasoning` | L11–L11 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DisplayEvent` | L18–L26 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DisplayMode` | L29–L29 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DisplayModeDefault` | L32–L32 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DisplayModeVerbose` | L33–L33 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DisplayEventParser` | L39–L39 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RenderFunc` | L43–L43 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `pkg/interpolation/resolver.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Resolver` | L6–L8 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Context` | L11–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LoopData` | L23–L30 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Index1` | L33–L35 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepStateData` | L38–L50 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WorkflowData` | L53–L58 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Duration` | L61–L63 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ContextData` | L66–L70 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ErrorData` | L73–L78 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewContext` | L81–L88 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/application/interactive_executor.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `InteractiveExecutor` | L18–L34 | 🔴 DEAD | 70% | 0 external importers per analysis. However, it is instantiated by NewInteractiveExecutor (L55) and used throughout the file as receiver type — the struct itself is internally alive. DEAD only from external package perspective. |
| `NewInteractiveExecutor` | L37–L59 | 🔴 DEAD | 70% | 0 external importers. Not called anywhere in this file. Entry point for constructing InteractiveExecutor — dead if no external caller wires it up. |
| `SetTemplateService` | L62–L64 | 🔴 DEAD | 70% | 0 external importers. Not called within this file. templateSvc is read in Run (L100), but the setter itself is never invoked locally, so templateSvc remains nil unless externally called. |
| `SetAWFPaths` | L68–L70 | 🔴 DEAD | 70% | 0 external importers. Not called within this file. awfPaths is referenced in buildInterpolationContext and executeStep, but the setter is never locally invoked. |
| `SetBreakpoints` | L74–L83 | 🔴 DEAD | 70% | 0 external importers. Not called within this file. breakpoints map is read in shouldPause (L267), but SetBreakpoints is never locally invoked. |
| `SetOutputWriters` | L86–L89 | 🔴 DEAD | 70% | 0 external importers. Not called within this file. stdoutWriter/stderrWriter are used in executeStep (L460), but the setter is never locally invoked. |
| `Run` | L95–L263 | 🔴 DEAD | 70% | 0 external importers. Not called within this file. Core execution method that drives the entire interactive loop — dead only from external package perspective. |

### `internal/domain/operation/registry.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `OperationRegistry` | L17–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewOperationRegistry` | L22–L26 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Register` | L31–L45 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Unregister` | L49–L59 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Get` | L63–L69 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `List` | L71–L84 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetOperation` | L86–L92 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ListOperations` | L94–L105 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Execute` | L107–L125 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/ports/plugin_validator.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Severity` | L7–L7 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SeverityError` | L10–L10 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SeverityWarning` | L11–L11 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SeverityInfo` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ValidationResult` | L16–L21 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepExecuteRequest` | L24–L29 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepExecuteResult` | L32–L36 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WorkflowValidatorProvider` | L40–L43 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepTypeProvider` | L47–L50 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/ports/tracer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Span` | L5–L10 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Tracer` | L12–L14 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NopSpan` | L16–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `End` | L18–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetAttribute` | L19–L19 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RecordError` | L20–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AddEvent` | L21–L21 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NopTracer` | L23–L23 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Start` | L25–L27 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/workflow/audit_event.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `EventWorkflowFailed` | L12–L12 | 🔴 DEAD | 95% | 0 external importers and never referenced anywhere in this file. |
| `EventStepStarted` | L13–L13 | 🔴 DEAD | 95% | 0 external importers and never referenced anywhere in this file. |
| `EventStepCompleted` | L14–L14 | 🔴 DEAD | 95% | 0 external importers and never referenced anywhere in this file. |
| `EventStepFailed` | L15–L15 | 🔴 DEAD | 95% | 0 external importers and never referenced anywhere in this file. |
| `EventStepRetrying` | L16–L16 | 🔴 DEAD | 95% | 0 external importers and never referenced anywhere in this file. |
| `NewStartedEvent` | L40–L50 | 🔴 DEAD | 95% | 0 external importers and never called within this file. |
| `NewCompletedEvent` | L53–L74 | 🔴 DEAD | 90% | 0 external importers and never called within this file. |
| `MarshalJSON` | L79–L112 | 🔴 DEAD | 85% | Implements json.Marshaler but AuditEvent is never marshaled in this file and has 0 external importers, so this interface method is never invoked. |

## ⚡ Quick Wins

- [ ] <!-- ACT-bac80d-1 --> **[utility · high · trivial]** `internal/domain/operation/registry.go`: Remove dead code: `OperationRegistry` is exported but unused `OperationRegistry`, `NewOperationRegistry`, `Register`, `Unregister`, `Get`, `List`, `GetOperation`, `ListOperations`, `Execute` (`OperationRegistry, NewOperationRegistry, Register, Unregister, Get, List, GetOperation, ListOperations, Execute`) [L17-L20, L22-L26, L31-L45, L49-L59, L63-L69, L71-L84, L86-L92, L94-L105, L107-L125]
- [ ] <!-- ACT-dd25d1-1 --> **[utility · high · trivial]** `internal/domain/ports/plugin_validator.go`: Remove dead code: `Severity` is exported but unused `Severity`, `SeverityError`, `SeverityWarning`, `SeverityInfo`, `ValidationResult`, `StepExecuteRequest`, `StepExecuteResult`, `WorkflowValidatorProvider`, `StepTypeProvider` (`Severity, SeverityError, SeverityWarning, SeverityInfo, ValidationResult, StepExecuteRequest, StepExecuteResult, WorkflowValidatorProvider, StepTypeProvider`) [L7-L7, L10-L10, L11-L11, L12-L12, L16-L21, L24-L29, L32-L36, L40-L43, L47-L50]
- [ ] <!-- ACT-5e521d-1 --> **[utility · high · trivial]** `internal/domain/ports/tracer.go`: Remove dead code: `Span` is exported but unused `Span`, `Tracer`, `NopSpan`, `End`, `SetAttribute`, `RecordError`, `AddEvent`, `NopTracer`, `Start` (`Span, Tracer, NopSpan, End, SetAttribute, RecordError, AddEvent, NopTracer, Start`) [L5-L10, L12-L14, L16-L16, L18-L18, L19-L19, L20-L20, L21-L21, L23-L23, L25-L27]
- [ ] <!-- ACT-1a334c-2 --> **[utility · high · trivial]** `internal/domain/workflow/audit_event.go`: Remove dead code: `EventWorkflowFailed` is exported but unused `EventWorkflowFailed`, `EventStepStarted`, `EventStepCompleted`, `EventStepFailed`, `EventStepRetrying`, `NewStartedEvent`, `NewCompletedEvent`, `MarshalJSON` (`EventWorkflowFailed, EventStepStarted, EventStepCompleted, EventStepFailed, EventStepRetrying, NewStartedEvent, NewCompletedEvent, MarshalJSON`) [L12-L12, L13-L13, L14-L14, L15-L15, L16-L16, L40-L50, L53-L74, L79-L112]
- [ ] <!-- ACT-688884-4 --> **[utility · high · trivial]** `internal/infrastructure/pluginmgr/stream_manager.go`: Remove dead code: `EventStreamSender` is exported but unused `EventStreamSender`, `StreamManager`, `NewStreamManager`, `RegisterStream`, `UnregisterStream`, `HasStream`, `GetDeliverer`, `Close`, `DeliverEvent` (`EventStreamSender, StreamManager, NewStreamManager, RegisterStream, UnregisterStream, HasStream, GetDeliverer, Close, DeliverEvent`) [L18-L20, L28-L33, L38-L44, L47-L51, L54-L58, L61-L66, L69-L82, L85-L89, L98-L111]
- [ ] <!-- ACT-7f8988-2 --> **[utility · high · trivial]** `internal/infrastructure/tools/builtins/provider.go`: Remove dead code: `WithExecutor` is exported but unused `WithExecutor`, `WithRootDir`, `NewProvider`, `ListTools`, `CallTool`, `Close` (`WithExecutor, WithRootDir, NewProvider, ListTools, CallTool, Close`) [L55-L59, L70-L82, L85-L111, L128-L135, L141-L150, L153-L155]
- [ ] <!-- ACT-e4e075-1 --> **[utility · high · trivial]** `pkg/display/event.go`: Remove dead code: `EventKind` is exported but unused `EventKind`, `EventText`, `EventToolUse`, `EventReasoning`, `DisplayEvent`, `DisplayMode`, `DisplayModeDefault`, `DisplayModeVerbose`, `DisplayEventParser`, `RenderFunc` (`EventKind, EventText, EventToolUse, EventReasoning, DisplayEvent, DisplayMode, DisplayModeDefault, DisplayModeVerbose, DisplayEventParser, RenderFunc`) [L6-L6, L9-L9, L10-L10, L11-L11, L18-L26, L29-L29, L32-L32, L33-L33, L39-L39, L43-L43]
- [ ] <!-- ACT-3cf3e6-1 --> **[utility · high · trivial]** `pkg/interpolation/resolver.go`: Remove dead code: `Resolver` is exported but unused `Resolver`, `Context`, `LoopData`, `Index1`, `StepStateData`, `WorkflowData`, `Duration`, `ContextData`, `ErrorData`, `NewContext` (`Resolver, Context, LoopData, Index1, StepStateData, WorkflowData, Duration, ContextData, ErrorData, NewContext`) [L6-L8, L11-L20, L23-L30, L33-L35, L38-L50, L53-L58, L61-L63, L66-L70, L73-L78, L81-L88]

## 🔧 Refactors

- [ ] <!-- ACT-f18b00-4 --> **[utility · medium · trivial]** `internal/application/interactive_executor.go`: Remove dead code: `InteractiveExecutor` is exported but unused `InteractiveExecutor`, `NewInteractiveExecutor`, `SetTemplateService`, `SetAWFPaths`, `SetBreakpoints`, `SetOutputWriters`, `Run` (`InteractiveExecutor, NewInteractiveExecutor, SetTemplateService, SetAWFPaths, SetBreakpoints, SetOutputWriters, Run`) [L18-L34, L37-L59, L62-L64, L68-L70, L74-L83, L86-L89, L95-L263]
- [ ] <!-- ACT-d48717-4 --> **[utility · medium · trivial]** `internal/infrastructure/agents/openai_compatible_provider.go`: Remove dead code: `OpenAICompatibleProvider` is exported but unused `OpenAICompatibleProvider`, `NewOpenAICompatibleProvider`, `SetToolRouter`, `Name`, `Validate`, `Execute`, `ExecuteConversation` (`OpenAICompatibleProvider, NewOpenAICompatibleProvider, SetToolRouter, Name, Validate, Execute, ExecuteConversation`) [L43-L46, L105-L113, L115-L117, L119-L121, L123-L125, L131-L179, L417-L488]
- [ ] <!-- ACT-7f8988-1 --> **[utility · medium · trivial]** `internal/infrastructure/tools/builtins/provider.go`: Remove dead code: `MaxReadBytes` is exported but unused `MaxReadBytes`, `Provider`, `Option`, `resolvePath` (`MaxReadBytes, Provider, Option, resolvePath`) [L22-L22, L39-L49, L52-L52, L165-L192]
