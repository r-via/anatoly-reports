[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 4

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/domain/pluginmodel/info.go` | 🟡 NEEDS_REFACTOR | 15 | 95% | [details](#internaldomainpluginmodelinfogo) |
| `internal/domain/transcript/content.go` | 🟡 NEEDS_REFACTOR | 15 | 95% | [details](#internaldomaintranscriptcontentgo) |
| `pkg/interpolation/reference.go` | 🟡 NEEDS_REFACTOR | 14 | 95% | [details](#pkginterpolationreferencego) |
| `internal/interfaces/tui/keys.go` | 🟡 NEEDS_REFACTOR | 15 | 70% | [details](#internalinterfacestuikeysgo) |
| `internal/domain/transcript/event.go` | 🟡 NEEDS_REFACTOR | 14 | 95% | [details](#internaldomaintranscripteventgo) |
| `internal/domain/workflow/agent_config.go` | 🟡 NEEDS_REFACTOR | 14 | 95% | [details](#internaldomainworkflowagentconfiggo) |
| `internal/interfaces/cli/ui/formatter.go` | 🟡 NEEDS_REFACTOR | 14 | 95% | [details](#internalinterfacescliuiformattergo) |
| `internal/interfaces/cli/ui/interactive_prompt.go` | 🟡 NEEDS_REFACTOR | 13 | 95% | [details](#internalinterfacescliuiinteractivepromptgo) |
| `internal/application/service.go` | 🟡 NEEDS_REFACTOR | 12 | 95% | [details](#internalapplicationservicego) |
| `internal/domain/errors/structured_error.go` | 🟡 NEEDS_REFACTOR | 13 | 95% | [details](#internaldomainerrorsstructurederrorgo) |

## 🔍 Symbol Details

### `internal/domain/pluginmodel/info.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `PluginStatus` | L4–L4 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StatusDiscovered` | L7–L7 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StatusLoaded` | L8–L8 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StatusInitialized` | L9–L9 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StatusRunning` | L10–L10 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StatusStopped` | L11–L11 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StatusFailed` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StatusDisabled` | L13–L13 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StatusBuiltin` | L14–L14 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `PluginType` | L18–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `PluginTypeBuiltin` | L21–L21 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `PluginTypeExternal` | L22–L22 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `PluginInfo` | L25–L35 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `IsActive` | L37–L39 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `CanLoad` | L41–L43 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/transcript/content.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `BlockType` | L9–L9 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `BlockTypeText` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `BlockTypeThinking` | L13–L13 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `BlockTypeToolUse` | L14–L14 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `BlockTypeToolResult` | L15–L15 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `BlockTypeCommand` | L16–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `BlockTypeStream` | L17–L17 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Fidelity` | L20–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `FidelityRouter` | L23–L23 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `FidelityAgentEmitted` | L24–L24 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ContentBlock` | L29–L43 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MarshalJSON` | L45–L64 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `UnmarshalJSON` | L66–L77 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ValidBlockType` | L79–L86 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ValidFidelity` | L88–L95 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `pkg/interpolation/reference.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ReferenceType` | L6–L6 | 🔴 DEAD | 95% | 0 external importers; only used internally as the type for constants and struct fields within this package. |
| `TypeInputs` | L10–L10 | 🔴 DEAD | 95% | 0 external importers; referenced only inside ParseReference and CategorizeNamespace within this file. |
| `TypeStates` | L12–L12 | 🔴 DEAD | 95% | 0 external importers; referenced only inside ParseReference and CategorizeNamespace within this file. |
| `TypeWorkflow` | L14–L14 | 🔴 DEAD | 95% | 0 external importers; referenced only inside ParseReference and CategorizeNamespace within this file. |
| `TypeEnv` | L16–L16 | 🔴 DEAD | 95% | 0 external importers; referenced only inside ParseReference and CategorizeNamespace within this file. |
| `TypeError` | L18–L18 | 🔴 DEAD | 95% | 0 external importers; referenced only inside ParseReference and CategorizeNamespace within this file. |
| `TypeContext` | L20–L20 | 🔴 DEAD | 95% | 0 external importers; referenced only inside ParseReference and CategorizeNamespace within this file. |
| `TypeLoop` | L22–L22 | 🔴 DEAD | 95% | 0 external importers; referenced only inside ParseReference and CategorizeNamespace within this file. |
| `TypeUnknown` | L24–L24 | 🔴 DEAD | 95% | 0 external importers; referenced only inside ParseReference within this file. |
| `Reference` | L28–L34 | 🔴 DEAD | 95% | 0 external importers; returned by ParseReference and ExtractReferences but neither is called outside the package. |
| `ExtractReferences` | L77–L117 | 🔴 DEAD | 95% | 0 external importers; the primary entry point for template parsing is unused outside this package. |
| `ExtractRefPaths` | L136–L138 | 🔴 DEAD | 95% | 0 external importers; trivial one-line public wrapper around unexported extractRefPaths with no callers. |
| `ParseReference` | L235–L281 | 🔴 DEAD | 95% | 0 external importers; called only at L111 inside ExtractReferences, which itself has 0 external importers. |
| `CategorizeNamespace` | L284–L303 | 🔴 DEAD | 95% | 0 external importers; called only at L253 inside ParseReference, which itself has 0 external importers. |

### `internal/interfaces/tui/keys.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `globalHelpKeys` | L31–L31 | 🔴 DEAD | 70% | Unexported struct not instantiated anywhere in this file. May be used in sibling tui package files not visible here, but no evidence of usage. |
| `ShortHelp` | L33–L35 | 🔴 DEAD | 70% | Method on logsHelpKeys; 0 cross-package importers. Dead if parent type is never instantiated. |
| `FullHelp` | L37–L42 | 🔴 DEAD | 70% | Method on logsHelpKeys; 0 cross-package importers. Dead if parent type is never instantiated. |
| `workflowsHelpKeys` | L45–L45 | 🔴 DEAD | 70% | Unexported struct not instantiated in this file. Likely intended for use in a sibling tui file but no evidence confirms it. |
| `ShortHelp` | L47–L49 | 🔴 DEAD | 70% | Method on logsHelpKeys; 0 cross-package importers. Dead if parent type is never instantiated. |
| `FullHelp` | L51–L57 | 🔴 DEAD | 70% | Method on logsHelpKeys; 0 cross-package importers. Dead if parent type is never instantiated. |
| `monitoringHelpKeys` | L60–L60 | 🔴 DEAD | 70% | Unexported struct not instantiated in this file. No confirmed usage. |
| `ShortHelp` | L62–L64 | 🔴 DEAD | 70% | Method on logsHelpKeys; 0 cross-package importers. Dead if parent type is never instantiated. |
| `FullHelp` | L66–L72 | 🔴 DEAD | 70% | Method on logsHelpKeys; 0 cross-package importers. Dead if parent type is never instantiated. |
| `historyHelpKeys` | L75–L75 | 🔴 DEAD | 70% | Unexported struct not instantiated in this file. No confirmed usage. |
| `ShortHelp` | L77–L79 | 🔴 DEAD | 70% | Method on logsHelpKeys; 0 cross-package importers. Dead if parent type is never instantiated. |
| `FullHelp` | L81–L87 | 🔴 DEAD | 70% | Method on logsHelpKeys; 0 cross-package importers. Dead if parent type is never instantiated. |
| `logsHelpKeys` | L90–L90 | 🔴 DEAD | 70% | Unexported struct not instantiated in this file. No confirmed usage. |
| `ShortHelp` | L92–L94 | 🔴 DEAD | 70% | Method on logsHelpKeys; 0 cross-package importers. Dead if parent type is never instantiated. |
| `FullHelp` | L96–L102 | 🔴 DEAD | 70% | Method on logsHelpKeys; 0 cross-package importers. Dead if parent type is never instantiated. |

### `internal/domain/transcript/event.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `EventType` | L10–L10 | 🔴 DEAD | 95% | 0 external importers. Used only within this file for field typing and constant declarations. |
| `EventTypeRunStarted` | L13–L13 | 🔴 DEAD | 95% | 0 external importers. Referenced only in local validEventType switch. |
| `EventTypeRunCompleted` | L14–L14 | 🔴 DEAD | 95% | 0 external importers. Referenced only in local validEventType switch. |
| `EventTypeStepStarted` | L15–L15 | 🔴 DEAD | 95% | 0 external importers. Referenced only in local validEventType switch. |
| `EventTypeStepCompleted` | L16–L16 | 🔴 DEAD | 95% | 0 external importers. Referenced only in local validEventType switch. |
| `EventTypeStepCallWorkflowStarted` | L17–L17 | 🔴 DEAD | 95% | 0 external importers. Referenced only in local validEventType switch. |
| `EventTypeStepCallWorkflowCompleted` | L18–L18 | 🔴 DEAD | 95% | 0 external importers. Referenced only in local validEventType switch. |
| `EventTypeMessageUser` | L19–L19 | 🔴 DEAD | 95% | 0 external importers. Referenced only in local validEventType switch. |
| `EventTypeMessageAssistant` | L20–L20 | 🔴 DEAD | 95% | 0 external importers. Referenced only in local validEventType switch. |
| `EventTypeToolCall` | L21–L21 | 🔴 DEAD | 95% | 0 external importers. Referenced only in local validEventType switch. |
| `EventTypeToolResult` | L22–L22 | 🔴 DEAD | 95% | 0 external importers. Referenced only in local validEventType switch. |
| `ExchangeEvent` | L27–L37 | 🔴 DEAD | 95% | 0 external importers. No external package constructs or receives ExchangeEvent values. |
| `MarshalJSON` | L39–L57 | 🔴 DEAD | 95% | Implements json.Marshaler via interface dispatch, but ExchangeEvent has 0 importers so this method is never reachable at runtime. |
| `UnmarshalJSON` | L59–L101 | 🔴 DEAD | 95% | Implements json.Unmarshaler via interface dispatch, but ExchangeEvent has 0 importers so this method is never reachable at runtime. |

### `internal/domain/workflow/agent_config.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `DefaultAgentTimeout` | L10–L10 | 🔴 DEAD | 95% | 0 external importers. Used locally only in GetTimeout, which is itself dead. |
| `OutputFormat` | L13–L13 | 🔴 DEAD | 95% | 0 external importers. Used only within this file's constants and AgentConfig struct, both dead. |
| `OutputFormatNone` | L16–L16 | 🔴 DEAD | 95% | 0 external importers. Referenced only in validOutputFormats map within this file. |
| `OutputFormatJSON` | L17–L17 | 🔴 DEAD | 95% | 0 external importers. Referenced only in validOutputFormats map within this file. |
| `OutputFormatText` | L18–L18 | 🔴 DEAD | 95% | 0 external importers. Referenced only in validOutputFormats map within this file. |
| `AgentConfig` | L28–L39 | 🔴 DEAD | 95% | 0 external importers. Central config struct with no external consumers. |
| `Validate` | L43–L112 | 🔴 DEAD | 90% | 0 external importers. Method on dead AgentConfig type. |
| `GetTimeout` | L120–L125 | 🔴 DEAD | 95% | 0 external importers. Method on dead AgentConfig type. |
| `IsConversationMode` | L128–L130 | 🔴 DEAD | 95% | 0 external importers. Method on dead AgentConfig type. |
| `AgentResult` | L133–L145 | 🔴 DEAD | 95% | 0 external importers. Result struct with no external consumers. |
| `NewAgentResult` | L148–L154 | 🔴 DEAD | 95% | 0 external importers. Constructor for dead AgentResult type. |
| `Duration` | L157–L159 | 🔴 DEAD | 90% | 0 external importers. Method on dead AgentResult type. |
| `Success` | L162–L164 | 🔴 DEAD | 95% | 0 external importers. Method on dead AgentResult type. |
| `HasJSONResponse` | L167–L169 | 🔴 DEAD | 90% | 0 external importers. Method on dead AgentResult type. |

### `internal/interfaces/cli/ui/formatter.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `FormatOptions` | L11–L15 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Formatter` | L18–L22 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewFormatter` | L25–L31 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Printf` | L34–L36 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Println` | L39–L41 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Info` | L44–L49 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Debug` | L52–L57 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Success` | L60–L62 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L65–L67 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Warning` | L70–L75 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepSuccess` | L79–L85 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Table` | L88–L108 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `StatusLine` | L111–L114 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Colorizer` | L117–L119 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/interfaces/cli/ui/interactive_prompt.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `CLIPrompt` | L25–L29 | 🔴 DEAD | 90% | 0 external importers. Interface compliance assertions at L18–22 reference it but only as compile-time checks within the same package; no runtime instantiation occurs outside this file. |
| `NewCLIPrompt` | L32–L38 | 🔴 DEAD | 95% | 0 importers. Only constructor for CLIPrompt; never called externally or within this file. |
| `ShowHeader` | L41–L45 | 🔴 DEAD | 92% | 0 importers. Interface method on an uninstantiated struct. |
| `ShowStepDetails` | L48–L74 | 🔴 DEAD | 90% | 0 importers. Interface method on an uninstantiated struct. |
| `PromptAction` | L77–L102 | 🔴 DEAD | 90% | 0 importers. Interface method on an uninstantiated struct. |
| `ShowExecuting` | L105–L108 | 🔴 DEAD | 95% | 0 importers. Interface method on an uninstantiated struct. |
| `ShowStepResult` | L111–L137 | 🔴 DEAD | 90% | 0 importers. Interface method on an uninstantiated struct. |
| `ShowContext` | L140–L174 | 🔴 DEAD | 92% | 0 importers. Interface method on an uninstantiated struct. |
| `EditInput` | L177–L195 | 🔴 DEAD | 93% | 0 importers. Interface method on an uninstantiated struct. |
| `ShowAborted` | L198–L200 | 🔴 DEAD | 95% | 0 importers. Interface method on an uninstantiated struct. |
| `ShowSkipped` | L203–L208 | 🔴 DEAD | 95% | 0 importers. Interface method on an uninstantiated struct. |
| `ShowCompleted` | L211–L217 | 🔴 DEAD | 95% | 0 importers. Interface method on an uninstantiated struct. |
| `ShowError` | L220–L222 | 🔴 DEAD | 95% | 0 importers. Interface method on an uninstantiated struct. |

### `internal/application/service.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `WorkflowService` | L19–L31 | 🔴 DEAD | 95% | Exported struct with 0 runtime and 0 type-only importers per exhaustive import analysis. |
| `NewWorkflowService` | L33–L47 | 🔴 DEAD | 95% | Exported constructor with 0 runtime and 0 type-only importers; no external callers found. |
| `SetValidatorProvider` | L49–L51 | 🔴 DEAD | 95% | Exported setter with 0 importers; wires an optional dependency that no caller configures. |
| `SetPackDiscoverer` | L53–L55 | 🔴 DEAD | 95% | Exported setter with 0 importers; optional dependency wired by no caller. |
| `SetPluginOperationProvider` | L57–L59 | 🔴 DEAD | 95% | Exported setter with 0 importers; optional dependency wired by no caller. |
| `SetTemplateAnalyzer` | L66–L68 | 🔴 DEAD | 95% | Exported setter with 0 importers; optional dependency wired by no caller. |
| `SetSkillRepository` | L70–L72 | 🔴 DEAD | 95% | Exported setter with 0 importers; optional dependency wired by no caller. |
| `LastValidationWarnings` | L78–L80 | 🔴 DEAD | 95% | Exported accessor with 0 importers; warnings it exposes are never consumed externally. |
| `ListAllWorkflows` | L82–L112 | 🔴 DEAD | 90% | Exported method with 0 importers; no external caller invokes it. |
| `GetWorkflow` | L114–L128 | 🔴 DEAD | 95% | Exported method with 0 importers; only called internally from ValidateWorkflow, which is itself dead. |
| `ValidateWorkflow` | L130–L136 | 🔴 DEAD | 95% | Exported method with 0 importers; thin load-then-validate wrapper never called externally. |
| `ValidateLoadedWorkflow` | L146–L188 | 🔴 DEAD | 95% | Exported method with 0 importers; only called from ValidateWorkflow, which is itself dead. |

### `internal/domain/errors/structured_error.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `StructuredError` | L32–L50 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewStructuredError` | L52–L60 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L62–L64 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Unwrap` | L68–L70 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewUserError` | L72–L74 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `NewWorkflowError` | L76–L78 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `NewExecutionError` | L80–L82 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `NewSystemError` | L84–L86 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `ExitCode` | L94–L96 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithDetails` | L104–L121 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Is` | L125–L131 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `As` | L135–L141 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `Format` | L151–L180 | 🔴 DEAD | 90% | Exported but imported by 0 files |

## ⚡ Quick Wins

- [ ] <!-- ACT-9df798-2 --> **[utility · high · trivial]** `internal/application/service.go`: Remove dead code: `WorkflowService` is exported but unused `WorkflowService`, `NewWorkflowService`, `SetValidatorProvider`, `SetPackDiscoverer`, `SetPluginOperationProvider`, `SetTemplateAnalyzer`, `SetSkillRepository`, `LastValidationWarnings`, `ListAllWorkflows`, `GetWorkflow`, `ValidateWorkflow`, `ValidateLoadedWorkflow` (`WorkflowService, NewWorkflowService, SetValidatorProvider, SetPackDiscoverer, SetPluginOperationProvider, SetTemplateAnalyzer, SetSkillRepository, LastValidationWarnings, ListAllWorkflows, GetWorkflow, ValidateWorkflow, ValidateLoadedWorkflow`) [L19-L31, L33-L47, L49-L51, L53-L55, L57-L59, L66-L68, L70-L72, L78-L80, L82-L112, L114-L128, L130-L136, L146-L188]
- [ ] <!-- ACT-0c1499-1 --> **[utility · high · trivial]** `internal/domain/errors/structured_error.go`: Remove dead code: `StructuredError` is exported but unused `StructuredError`, `NewStructuredError`, `Error`, `Unwrap`, `NewUserError`, `NewWorkflowError`, `NewExecutionError`, `NewSystemError`, `ExitCode`, `WithDetails`, `Is`, `As`, `Format` (`StructuredError, NewStructuredError, Error, Unwrap, NewUserError, NewWorkflowError, NewExecutionError, NewSystemError, ExitCode, WithDetails, Is, As, Format`) [L32-L50, L52-L60, L62-L64, L68-L70, L72-L74, L76-L78, L80-L82, L84-L86, L94-L96, L104-L121, L125-L131, L135-L141, L151-L180]
- [ ] <!-- ACT-bdd138-1 --> **[utility · high · trivial]** `internal/domain/pluginmodel/info.go`: Remove dead code: `PluginStatus` is exported but unused `PluginStatus`, `StatusDiscovered`, `StatusLoaded`, `StatusInitialized`, `StatusRunning`, `StatusStopped`, `StatusFailed`, `StatusDisabled`, `StatusBuiltin`, `PluginType`, `PluginTypeBuiltin`, `PluginTypeExternal`, `PluginInfo`, `IsActive`, `CanLoad` (`PluginStatus, StatusDiscovered, StatusLoaded, StatusInitialized, StatusRunning, StatusStopped, StatusFailed, StatusDisabled, StatusBuiltin, PluginType, PluginTypeBuiltin, PluginTypeExternal, PluginInfo, IsActive, CanLoad`) [L4-L4, L7-L7, L8-L8, L9-L9, L10-L10, L11-L11, L12-L12, L13-L13, L14-L14, L18-L18, L21-L21, L22-L22, L25-L35, L37-L39, L41-L43]
- [ ] <!-- ACT-260275-1 --> **[utility · high · trivial]** `internal/domain/transcript/content.go`: Remove dead code: `BlockType` is exported but unused `BlockType`, `BlockTypeText`, `BlockTypeThinking`, `BlockTypeToolUse`, `BlockTypeToolResult`, `BlockTypeCommand`, `BlockTypeStream`, `Fidelity`, `FidelityRouter`, `FidelityAgentEmitted`, `ContentBlock`, `MarshalJSON`, `UnmarshalJSON`, `ValidBlockType`, `ValidFidelity` (`BlockType, BlockTypeText, BlockTypeThinking, BlockTypeToolUse, BlockTypeToolResult, BlockTypeCommand, BlockTypeStream, Fidelity, FidelityRouter, FidelityAgentEmitted, ContentBlock, MarshalJSON, UnmarshalJSON, ValidBlockType, ValidFidelity`) [L9-L9, L12-L12, L13-L13, L14-L14, L15-L15, L16-L16, L17-L17, L20-L20, L23-L23, L24-L24, L29-L43, L45-L64, L66-L77, L79-L86, L88-L95]
- [ ] <!-- ACT-eb2f4f-1 --> **[utility · high · trivial]** `internal/domain/transcript/event.go`: Remove dead code: `EventType` is exported but unused `EventType`, `EventTypeRunStarted`, `EventTypeRunCompleted`, `EventTypeStepStarted`, `EventTypeStepCompleted`, `EventTypeStepCallWorkflowStarted`, `EventTypeStepCallWorkflowCompleted`, `EventTypeMessageUser`, `EventTypeMessageAssistant`, `EventTypeToolCall`, `EventTypeToolResult`, `ExchangeEvent`, `MarshalJSON`, `UnmarshalJSON` (`EventType, EventTypeRunStarted, EventTypeRunCompleted, EventTypeStepStarted, EventTypeStepCompleted, EventTypeStepCallWorkflowStarted, EventTypeStepCallWorkflowCompleted, EventTypeMessageUser, EventTypeMessageAssistant, EventTypeToolCall, EventTypeToolResult, ExchangeEvent, MarshalJSON, UnmarshalJSON`) [L10-L10, L13-L13, L14-L14, L15-L15, L16-L16, L17-L17, L18-L18, L19-L19, L20-L20, L21-L21, L22-L22, L27-L37, L39-L57, L59-L101]
- [ ] <!-- ACT-518390-1 --> **[utility · high · trivial]** `internal/domain/workflow/agent_config.go`: Remove dead code: `DefaultAgentTimeout` is exported but unused `DefaultAgentTimeout`, `OutputFormat`, `OutputFormatNone`, `OutputFormatJSON`, `OutputFormatText`, `AgentConfig`, `Validate`, `GetTimeout`, `IsConversationMode`, `AgentResult`, `NewAgentResult`, `Duration`, `Success`, `HasJSONResponse` (`DefaultAgentTimeout, OutputFormat, OutputFormatNone, OutputFormatJSON, OutputFormatText, AgentConfig, Validate, GetTimeout, IsConversationMode, AgentResult, NewAgentResult, Duration, Success, HasJSONResponse`) [L10-L10, L13-L13, L16-L16, L17-L17, L18-L18, L28-L39, L43-L112, L120-L125, L128-L130, L133-L145, L148-L154, L157-L159, L162-L164, L167-L169]
- [ ] <!-- ACT-b580da-2 --> **[utility · high · trivial]** `internal/interfaces/cli/ui/formatter.go`: Remove dead code: `FormatOptions` is exported but unused `FormatOptions`, `Formatter`, `NewFormatter`, `Printf`, `Println`, `Info`, `Debug`, `Success`, `Error`, `Warning`, `StepSuccess`, `Table`, `StatusLine`, `Colorizer` (`FormatOptions, Formatter, NewFormatter, Printf, Println, Info, Debug, Success, Error, Warning, StepSuccess, Table, StatusLine, Colorizer`) [L11-L15, L18-L22, L25-L31, L34-L36, L39-L41, L44-L49, L52-L57, L60-L62, L65-L67, L70-L75, L79-L85, L88-L108, L111-L114, L117-L119]
- [ ] <!-- ACT-0865af-2 --> **[utility · high · trivial]** `internal/interfaces/cli/ui/interactive_prompt.go`: Remove dead code: `CLIPrompt` is exported but unused `CLIPrompt`, `NewCLIPrompt`, `ShowHeader`, `ShowStepDetails`, `PromptAction`, `ShowExecuting`, `ShowStepResult`, `ShowContext`, `EditInput`, `ShowAborted`, `ShowSkipped`, `ShowCompleted`, `ShowError` (`CLIPrompt, NewCLIPrompt, ShowHeader, ShowStepDetails, PromptAction, ShowExecuting, ShowStepResult, ShowContext, EditInput, ShowAborted, ShowSkipped, ShowCompleted, ShowError`) [L25-L29, L32-L38, L41-L45, L48-L74, L77-L102, L105-L108, L111-L137, L140-L174, L177-L195, L198-L200, L203-L208, L211-L217, L220-L222]
- [ ] <!-- ACT-8cee72-2 --> **[utility · high · trivial]** `pkg/interpolation/reference.go`: Remove dead code: `ReferenceType` is exported but unused `ReferenceType`, `TypeInputs`, `TypeStates`, `TypeWorkflow`, `TypeEnv`, `TypeError`, `TypeContext`, `TypeLoop`, `TypeUnknown`, `Reference`, `ExtractReferences`, `ExtractRefPaths`, `ParseReference`, `CategorizeNamespace` (`ReferenceType, TypeInputs, TypeStates, TypeWorkflow, TypeEnv, TypeError, TypeContext, TypeLoop, TypeUnknown, Reference, ExtractReferences, ExtractRefPaths, ParseReference, CategorizeNamespace`) [L6-L6, L10-L10, L12-L12, L14-L14, L16-L16, L18-L18, L20-L20, L22-L22, L24-L24, L28-L34, L77-L117, L136-L138, L235-L281, L284-L303]

## 🔧 Refactors

- [ ] <!-- ACT-d4506d-1 --> **[utility · medium · trivial]** `internal/interfaces/tui/keys.go`: Remove dead code: `globalHelpKeys` is exported but unused `globalHelpKeys`, `ShortHelp`, `FullHelp`, `workflowsHelpKeys`, `ShortHelp`, `FullHelp`, `monitoringHelpKeys`, `ShortHelp`, `FullHelp`, `historyHelpKeys`, `ShortHelp`, `FullHelp`, `logsHelpKeys`, `ShortHelp`, `FullHelp` (`globalHelpKeys, ShortHelp, FullHelp, workflowsHelpKeys, ShortHelp, FullHelp, monitoringHelpKeys, ShortHelp, FullHelp, historyHelpKeys, ShortHelp, FullHelp, logsHelpKeys, ShortHelp, FullHelp`) [L31-L31, L33-L35, L37-L42, L45-L45, L47-L49, L51-L57, L60-L60, L62-L64, L66-L72, L75-L75, L77-L79, L81-L87, L90-L90, L92-L94, L96-L102]
