[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 10

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `pkg/validation/validator.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#pkgvalidationvalidatorgo) |
| `internal/application/conversation_manager.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalapplicationconversationmanagergo) |
| `internal/application/dry_run_executor.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalapplicationdryrunexecutorgo) |
| `internal/application/history_service.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalapplicationhistoryservicego) |
| `internal/domain/ports/facade.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internaldomainportsfacadego) |
| `internal/domain/ports/logger.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internaldomainportsloggergo) |
| `internal/domain/ports/repository.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internaldomainportsrepositorygo) |
| `internal/domain/workflow/workflow.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internaldomainworkflowworkflowgo) |
| `internal/infrastructure/agents/claude_provider.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinfrastructureagentsclaudeprovidergo) |
| `internal/infrastructure/agents/codex_provider.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinfrastructureagentscodexprovidergo) |

## 🔍 Symbol Details

### `pkg/validation/validator.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Input` | L15–L20 | 🔴 DEAD | 95% | Exported type with 0 importers. Entire validation package is unconsumed. |
| `Rules` | L23–L30 | 🔴 DEAD | 95% | Exported type with 0 importers. Referenced only within this dead package. |
| `ValidationError` | L33–L35 | 🔴 DEAD | 95% | Exported type with 0 importers. Only instantiated inside ValidateInputs, which is itself dead. |
| `Error` | L37–L45 | 🔴 DEAD | 95% | Method on ValidationError satisfying error interface, but no external caller exists — ValidateInputs has 0 importers so Error() is never invoked. |
| `ValidateInputs` | L49–L91 | 🔴 DEAD | 90% | Exported function with 0 importers. Entry point for the package but never called. |
| `validateFileExtension` | L337–L355 | 🔴 DEAD | 95% | Never called anywhere in the file. validateFileExtensionWithType contains its own inline logic and does not delegate here. Notably uses case-insensitive EqualFold while the live path uses case-sensitive comparison — a latent correctness divergence. |

### `internal/application/conversation_manager.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ConversationManager` | L35–L48 | 🔴 DEAD | 95% | Exported struct with 0 runtime and 0 type-only importers across the codebase. |
| `SetTurnEmitter` | L51–L53 | 🔴 DEAD | 95% | Exported method with 0 importers; no external caller wires the turn emitter. |
| `NewConversationManager` | L55–L65 | 🔴 DEAD | 95% | Exported constructor with 0 importers; ConversationManager is never instantiated externally. |
| `SetUserInputReader` | L69–L71 | 🔴 DEAD | 95% | Exported setter with 0 importers; no external caller injects a UserInputReader. |
| `SetAgentRoleRepository` | L75–L77 | 🔴 DEAD | 95% | Exported setter with 0 importers; no external caller injects an AgentRoleRepository. |
| `SetToolProxyService` | L82–L84 | 🔴 DEAD | 95% | Exported setter with 0 importers; no external caller injects the MCP tool proxy. |
| `ExecuteConversation` | L189–L329 | 🔴 DEAD | 88% | Exported method with 0 importers; the entire multi-turn conversation orchestration is never invoked externally. |

### `internal/application/dry_run_executor.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `DryRunExecutor` | L14–L21 | 🔴 DEAD | 95% | Exported struct with 0 importers across the codebase. |
| `NewDryRunExecutor` | L23–L35 | 🔴 DEAD | 95% | Exported constructor with 0 importers; no external caller instantiates DryRunExecutor. |
| `SetTemplateService` | L37–L39 | 🔴 DEAD | 95% | Exported setter with 0 importers; templateSvc injection path is never exercised externally. |
| `SetAWFPaths` | L42–L44 | 🔴 DEAD | 95% | Exported setter with 0 importers; awfPaths injection path is never exercised externally. |
| `Execute` | L46–L65 | 🔴 DEAD | 95% | Exported method with 0 importers; entry point into dry-run logic is never called. |

### `internal/application/history_service.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `retentionPeriod` | L15–L15 | 🔴 DEAD | 95% | Declared but never referenced anywhere in the file; no purge/cleanup logic exists. |
| `HistoryService` | L23–L26 | 🔴 DEAD | 95% | Exported type with 0 runtime and 0 type-only importers per import analysis. |
| `NewHistoryService` | L30–L35 | 🔴 DEAD | 95% | Exported constructor with 0 importers; no callers instantiate HistoryService. |
| `Record` | L39–L44 | 🔴 DEAD | 95% | Exported method with 0 importers; no external code persists execution records via this service. |
| `List` | L49–L65 | 🔴 DEAD | 95% | Exported method with 0 importers; history listing is never invoked externally. |
| `GetByID` | L73–L97 | 🔴 DEAD | 82% | Exported method with 0 importers; O(N) scan logic is centralized but never called. |
| `GetStats` | L101–L110 | 🔴 DEAD | 95% | Exported method with 0 importers; aggregated stats are never consumed externally. |

### `internal/domain/ports/facade.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ResumeRequest` | L40–L49 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WorkflowFacade` | L56–L66 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SingleStepRunner` | L73–L75 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WorkflowReader` | L90–L93 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RunSession` | L97–L103 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `FileValidationResult` | L108–L116 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `BatchValidator` | L138–L146 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/ports/logger.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Logger` | L3–L9 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NopLogger` | L14–L14 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Debug` | L16–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Info` | L17–L17 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Warn` | L18–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L19–L19 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithContext` | L20–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/ports/repository.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `WorkflowSource` | L14–L14 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SourceEnv` | L17–L17 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SourceLocal` | L18–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SourceGlobal` | L19–L19 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WorkflowInfo` | L25–L29 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WorkflowRepository` | L31–L39 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TemplateRepository` | L41–L45 | 🔴 DEAD | 90% | Exported but imported by 0 files |

### `internal/domain/workflow/workflow.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Input` | L10–L17 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Workflow` | L20–L33 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ResolveStepType` | L38–L47 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetStep` | L50–L53 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StateReferenceError` | L58–L63 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L65–L82 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Validate` | L89–L283 | 🔴 DEAD | 90% | Exported but imported by 0 files |

### `internal/infrastructure/agents/claude_provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ClaudeProvider` | L23–L28 | 🔴 DEAD | 85% | 0 external importers per exhaustive analysis; both constructors also have 0 importers, so no external code instantiates this type. Confidence reduced slightly because Go interface satisfaction doesn't always surface as a named symbol import. |
| `NewClaudeProvider` | L30–L43 | 🔴 DEAD | 95% | Constructor with 0 importers per exhaustive analysis. No external package creates a ClaudeProvider via this function. |
| `NewClaudeProviderWithOptions` | L45–L55 | 🔴 DEAD | 95% | Constructor with 0 importers per exhaustive analysis. No external package creates a ClaudeProvider via this function. |
| `Execute` | L74–L101 | 🔴 DEAD | 85% | 0 external importers. Since both constructors are dead, no external caller can invoke this method. Confidence reduced because Go methods aren't tracked as named symbol imports. |
| `ExecuteConversation` | L104–L120 | 🔴 DEAD | 85% | 0 external importers. Both constructors are dead, so no external caller can invoke this method. |
| `Name` | L122–L124 | 🔴 DEAD | 85% | 0 external importers. Both constructors are dead, so no external caller can invoke this method. |
| `Validate` | L126–L132 | 🔴 DEAD | 85% | 0 external importers. Both constructors are dead, so no external caller can invoke this method. |

### `internal/infrastructure/agents/codex_provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `CodexProvider` | L22–L27 | 🔴 DEAD | 95% | 0 runtime and 0 type-only importers per exhaustive analysis. No external file instantiates or references this struct type. |
| `NewCodexProvider` | L29–L31 | 🔴 DEAD | 95% | 0 importers. No external file calls this constructor, so no CodexProvider instance is ever created externally. |
| `NewCodexProviderWithOptions` | L33–L43 | 🔴 DEAD | 95% | 0 importers. Primary constructor with options is never called externally, confirming no external instantiation path. |
| `Execute` | L62–L88 | 🔴 DEAD | 90% | 0 importers. Since both constructors are dead, no CodexProvider instance exists to dispatch this method against, even via interface. |
| `ExecuteConversation` | L90–L126 | 🔴 DEAD | 90% | 0 importers. Dead for the same reason as Execute: no live CodexProvider instance exists externally. |
| `Name` | L128–L130 | 🔴 DEAD | 90% | 0 importers. Interface method unreachable without an externally created CodexProvider instance. |
| `Validate` | L132–L138 | 🔴 DEAD | 90% | 0 importers. Interface method unreachable without an externally created CodexProvider instance. |

## ⚡ Quick Wins

- [ ] <!-- ACT-ff4ae1-1 --> **[utility · high · trivial]** `internal/application/conversation_manager.go`: Remove dead code: `ConversationManager` is exported but unused `ConversationManager`, `SetTurnEmitter`, `NewConversationManager`, `SetUserInputReader`, `SetAgentRoleRepository`, `SetToolProxyService`, `ExecuteConversation` (`ConversationManager, SetTurnEmitter, NewConversationManager, SetUserInputReader, SetAgentRoleRepository, SetToolProxyService, ExecuteConversation`) [L35-L48, L51-L53, L55-L65, L69-L71, L75-L77, L82-L84, L189-L329]
- [ ] <!-- ACT-f0cee0-4 --> **[utility · high · trivial]** `internal/application/dry_run_executor.go`: Remove dead code: `DryRunExecutor` is exported but unused `DryRunExecutor`, `NewDryRunExecutor`, `SetTemplateService`, `SetAWFPaths`, `Execute` (`DryRunExecutor, NewDryRunExecutor, SetTemplateService, SetAWFPaths, Execute`) [L14-L21, L23-L35, L37-L39, L42-L44, L46-L65]
- [ ] <!-- ACT-ba7d55-2 --> **[utility · high · trivial]** `internal/application/history_service.go`: Remove dead code: `retentionPeriod` is exported but unused `retentionPeriod`, `HistoryService`, `NewHistoryService`, `Record`, `List`, `GetByID`, `GetStats` (`retentionPeriod, HistoryService, NewHistoryService, Record, List, GetByID, GetStats`) [L15-L15, L23-L26, L30-L35, L39-L44, L49-L65, L73-L97, L101-L110]
- [ ] <!-- ACT-d900b5-1 --> **[utility · high · trivial]** `internal/domain/ports/facade.go`: Remove dead code: `ResumeRequest` is exported but unused `ResumeRequest`, `WorkflowFacade`, `SingleStepRunner`, `WorkflowReader`, `RunSession`, `FileValidationResult`, `BatchValidator` (`ResumeRequest, WorkflowFacade, SingleStepRunner, WorkflowReader, RunSession, FileValidationResult, BatchValidator`) [L40-L49, L56-L66, L73-L75, L90-L93, L97-L103, L108-L116, L138-L146]
- [ ] <!-- ACT-7293ab-1 --> **[utility · high · trivial]** `internal/domain/ports/logger.go`: Remove dead code: `Logger` is exported but unused `Logger`, `NopLogger`, `Debug`, `Info`, `Warn`, `Error`, `WithContext` (`Logger, NopLogger, Debug, Info, Warn, Error, WithContext`) [L3-L9, L14-L14, L16-L16, L17-L17, L18-L18, L19-L19, L20-L20]
- [ ] <!-- ACT-df52c9-1 --> **[utility · high · trivial]** `internal/domain/ports/repository.go`: Remove dead code: `WorkflowSource` is exported but unused `WorkflowSource`, `SourceEnv`, `SourceLocal`, `SourceGlobal`, `WorkflowInfo`, `WorkflowRepository`, `TemplateRepository` (`WorkflowSource, SourceEnv, SourceLocal, SourceGlobal, WorkflowInfo, WorkflowRepository, TemplateRepository`) [L14-L14, L17-L17, L18-L18, L19-L19, L25-L29, L31-L39, L41-L45]
- [ ] <!-- ACT-837d39-2 --> **[utility · high · trivial]** `internal/domain/workflow/workflow.go`: Remove dead code: `Input` is exported but unused `Input`, `Workflow`, `ResolveStepType`, `GetStep`, `StateReferenceError`, `Error`, `Validate` (`Input, Workflow, ResolveStepType, GetStep, StateReferenceError, Error, Validate`) [L10-L17, L20-L33, L38-L47, L50-L53, L58-L63, L65-L82, L89-L283]
- [ ] <!-- ACT-4c8912-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/claude_provider.go`: Remove dead code: `ClaudeProvider` is exported but unused `ClaudeProvider`, `NewClaudeProvider`, `NewClaudeProviderWithOptions`, `Execute`, `ExecuteConversation`, `Name`, `Validate` (`ClaudeProvider, NewClaudeProvider, NewClaudeProviderWithOptions, Execute, ExecuteConversation, Name, Validate`) [L23-L28, L30-L43, L45-L55, L74-L101, L104-L120, L122-L124, L126-L132]
- [ ] <!-- ACT-663fab-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/codex_provider.go`: Remove dead code: `CodexProvider` is exported but unused `CodexProvider`, `NewCodexProvider`, `NewCodexProviderWithOptions`, `Execute`, `ExecuteConversation`, `Name`, `Validate` (`CodexProvider, NewCodexProvider, NewCodexProviderWithOptions, Execute, ExecuteConversation, Name, Validate`) [L22-L27, L29-L31, L33-L43, L62-L88, L90-L126, L128-L130, L132-L138]
- [ ] <!-- ACT-0077dc-3 --> **[utility · high · trivial]** `pkg/validation/validator.go`: Remove dead code: `Input` is exported but unused `Input`, `Rules`, `ValidationError`, `Error`, `ValidateInputs`, `validateFileExtension` (`Input, Rules, ValidationError, Error, ValidateInputs, validateFileExtension`) [L15-L20, L23-L30, L33-L35, L37-L45, L49-L91, L337-L355]
