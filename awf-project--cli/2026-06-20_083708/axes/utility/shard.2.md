[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 2

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/infrastructure/agents/options.go` | 🟡 NEEDS_REFACTOR | 28 | 95% | [details](#internalinfrastructureagentsoptionsgo) |
| `internal/domain/workflow/context.go` | 🟡 NEEDS_REFACTOR | 27 | 95% | [details](#internaldomainworkflowcontextgo) |
| `internal/domain/workflow/conversation.go` | 🟡 NEEDS_REFACTOR | 27 | 95% | [details](#internaldomainworkflowconversationgo) |
| `internal/infrastructure/logger/hclog_adapter.go` | 🟡 NEEDS_REFACTOR | 24 | 95% | [details](#internalinfrastructureloggerhclogadaptergo) |
| `internal/infrastructure/xdg/xdg.go` | 🟡 NEEDS_REFACTOR | 23 | 95% | [details](#internalinfrastructurexdgxdggo) |
| `pkg/plugin/sdk/testing.go` | 🟡 NEEDS_REFACTOR | 23 | 95% | [details](#pkgpluginsdktestinggo) |
| `pkg/registry/version.go` | 🟡 NEEDS_REFACTOR | 22 | 95% | [details](#pkgregistryversiongo) |
| `internal/application/plugin_service.go` | 🟡 NEEDS_REFACTOR | 21 | 95% | [details](#internalapplicationpluginservicego) |
| `internal/domain/ports/facade_dto.go` | 🟡 NEEDS_REFACTOR | 21 | 95% | [details](#internaldomainportsfacadedtogo) |
| `internal/domain/ports/facade_event.go` | 🟡 NEEDS_REFACTOR | 21 | 95% | [details](#internaldomainportsfacadeeventgo) |

## 🔍 Symbol Details

### `internal/infrastructure/agents/options.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ClaudeProviderOption` | L8–L8 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithClaudeExecutor` | L10–L14 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithClaudeLogger` | L16–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithClaudeTokenizer` | L22–L26 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GeminiProviderOption` | L28–L28 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithGeminiExecutor` | L30–L34 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithGeminiLogger` | L36–L40 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithGeminiTokenizer` | L42–L46 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithGeminiDenyAllPolicy` | L48–L52 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithGeminiCommandExecutor` | L54–L58 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `CodexProviderOption` | L60–L60 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithCodexExecutor` | L62–L66 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithCodexLogger` | L68–L72 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithCodexTokenizer` | L74–L78 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `OpenCodeProviderOption` | L80–L80 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithOpenCodeExecutor` | L82–L86 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithOpenCodeLogger` | L88–L92 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithOpenCodeTokenizer` | L94–L98 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `CopilotProviderOption` | L100–L100 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithCopilotExecutor` | L102–L106 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithCopilotLogger` | L108–L112 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithCopilotTokenizer` | L114–L118 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MistralVibeProviderOption` | L120–L120 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithMistralVibeExecutor` | L122–L126 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithMistralVibeLogger` | L128–L132 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithMistralVibeTokenizer` | L134–L138 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `OpenAICompatibleProviderOption` | L140–L140 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithHTTPClient` | L142–L146 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/workflow/context.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ExecutionStatus` | L9–L9 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StatusPending` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StatusRunning` | L13–L13 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StatusCompleted` | L14–L14 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StatusFailed` | L15–L15 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StatusCancelled` | L16–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `String` | L19–L21 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepState` | L24–L53 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LoopContext` | L56–L63 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ExecutionContext` | L67–L83 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `NewExecutionContext` | L86–L98 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetInput` | L101–L106 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetInput` | L109–L114 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetStepState` | L117–L122 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DeleteStepState` | L125–L132 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetStepState` | L135–L140 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetAllStepStates` | L143–L152 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetStatus` | L155–L159 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetStatus` | L162–L167 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetCompletedAt` | L170–L174 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetCompletedAt` | L177–L182 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetCurrentStep` | L185–L189 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetUpdatedAt` | L192–L196 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `PushCallStack` | L200–L205 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `PopCallStack` | L209–L216 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `IsInCallStack` | L220–L229 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `CallStackDepth` | L233–L237 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/workflow/conversation.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ExpressionCompiler` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepTypeChecker` | L17–L17 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TurnRole` | L25–L25 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TurnRoleSystem` | L28–L28 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TurnRoleUser` | L29–L29 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TurnRoleAssistant` | L30–L30 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Turn` | L34–L38 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewTurn` | L41–L46 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Validate` | L49–L63 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ConversationConfig` | L66–L68 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Validate` | L71–L73 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StopReason` | L76–L76 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StopReasonUserExit` | L79–L79 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StopReasonError` | L80–L80 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ConversationState` | L84–L90 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewConversationState` | L93–L110 | 🔴 DEAD | 85% | Exported but imported by 0 files |
| `AddTurn` | L114–L122 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetTotalTokens` | L125–L127 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetLastTurn` | L130–L135 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetLastAssistantResponse` | L138–L146 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `IsStopped` | L149–L151 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ConversationResult` | L154–L168 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewConversationResult` | L171–L185 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Duration` | L188–L193 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Success` | L196–L198 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `HasJSONResponse` | L201–L203 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TurnCount` | L206–L211 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/logger/hclog_adapter.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `HCLogAdapter` | L16–L21 | 🔴 DEAD | 95% | 0 external importers. The compile-time interface check on L11 proves intent, but no caller ever instantiates this adapter. |
| `NewHCLogAdapter` | L24–L30 | 🔴 DEAD | 95% | 0 external importers. Sole constructor for HCLogAdapter; never called outside the package. |
| `Log` | L70–L72 | 🔴 DEAD | 85% | 0 external importers. Required by hclog.Logger interface, but HCLogAdapter itself is never imported so this is never invoked. |
| `Trace` | L74–L76 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `Debug` | L78–L80 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `Info` | L82–L84 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `Warn` | L86–L88 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `Error` | L90–L92 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `IsTrace` | L94–L96 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `IsDebug` | L98–L100 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `IsInfo` | L102–L104 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `IsWarn` | L106–L108 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `IsError` | L110–L112 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `ImpliedArgs` | L114–L114 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `With` | L116–L126 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `Name` | L128–L128 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `Named` | L130–L141 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `ResetNamed` | L143–L150 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `SetLevel` | L152–L152 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type; no-op body anyway. |
| `GetLevel` | L154–L168 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `StandardLogger` | L170–L172 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `StandardWriter` | L174–L176 | 🔴 DEAD | 85% | 0 external importers. hclog.Logger interface method on a dead type. |
| `NewLogWriter` | L186–L188 | 🔴 DEAD | 85% | 0 external importers. Called only from StandardWriter (L175), which is itself dead because HCLogAdapter is never imported. |
| `Write` | L190–L208 | 🔴 DEAD | 85% | 0 external importers. Satisfies io.Writer for logWriter, but logWriter is only reachable through the dead NewLogWriter/StandardWriter chain. |

### `internal/infrastructure/xdg/xdg.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ConfigHome` | L11–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DataHome` | L25–L34 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AWFConfigDir` | L38–L43 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `AWFDataDir` | L46–L48 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `AWFWorkflowsDir` | L51–L53 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AWFPromptsDir` | L56–L58 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AWFScriptsDir` | L61–L63 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AWFStatesDir` | L66–L68 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AWFLogsDir` | L71–L73 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LegacyDirExists` | L76–L84 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LocalWorkflowsDir` | L87–L89 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LocalPromptsDir` | L92–L94 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AWFSkillsDir` | L97–L99 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LocalSkillsDir` | L102–L104 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AWFRolesDir` | L107–L109 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LocalRolesDir` | L112–L114 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AWFPluginsDir` | L117–L119 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LocalPluginsDir` | L122–L124 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AWFWorkflowPacksDir` | L127–L129 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LocalWorkflowPacksDir` | L132–L134 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AWFPaths` | L138–L149 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `PackAWFPaths` | L152–L156 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LocalConfigPath` | L160–L165 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `pkg/plugin/sdk/testing.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `MockPlugin` | L10–L20 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `NewMockPlugin` | L22–L27 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Name` | L29–L31 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `Version` | L33–L35 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `Init` | L38–L44 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Shutdown` | L47–L52 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WasInitCalled` | L55–L59 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WasShutdownCalled` | L61–L65 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Reset` | L67–L73 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `MockOperationHandler` | L76–L83 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `NewMockOperationHandler` | L85–L89 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Handle` | L92–L105 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `GetCallCount` | L107–L111 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Reset` | L113–L118 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `MockOperationProvider` | L121–L129 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `NewMockOperationProvider` | L131–L137 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Operations` | L140–L144 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `HandleOperation` | L146–L166 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetResult` | L168–L170 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetCommandResult` | L172–L176 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetError` | L178–L182 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TestInputs` | L184–L192 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `TestConfig` | L194–L196 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `pkg/registry/version.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `OpEqual` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `OpNotEqual` | L13–L13 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `OpGreater` | L14–L14 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `OpGreaterOrEqual` | L15–L15 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `OpLess` | L16–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `OpLessOrEqual` | L17–L17 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `OpTilde` | L18–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `OpCaret` | L19–L19 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Version` | L23–L28 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `String` | L34–L40 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Compare` | L44–L83 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `IsPrerelease` | L85–L87 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Constraint` | L90–L93 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Check` | L95–L143 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Constraints` | L146–L146 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Check` | L149–L159 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `String` | L162–L171 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ParseVersion` | L175–L211 | 🔴 DEAD | 86% | Exported but imported by 0 files |
| `ParseConstraint` | L218–L245 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ParseConstraints` | L250–L285 | 🔴 DEAD | 93% | Exported but imported by 0 files |
| `CheckVersionConstraint` | L289–L308 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NormalizeTag` | L311–L313 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/application/plugin_service.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `PluginService` | L25–L30 | 🔴 DEAD | 95% | Exported struct with 0 importers across all files. |
| `NewPluginService` | L33–L44 | 🔴 DEAD | 95% | Exported constructor with 0 importers; PluginService is never instantiated externally. |
| `RegisterBuiltin` | L48–L60 | 🔴 DEAD | 95% | Exported method with 0 importers; no caller wires builtins into the service. |
| `DiscoverPlugins` | L76–L103 | 🔴 DEAD | 90% | Exported method with 0 importers; only internal caller is StartupEnabledPlugins, which is itself dead. |
| `LoadPlugin` | L107–L129 | 🔴 DEAD | 95% | Exported method with 0 importers; only internal caller is LoadAndInitPlugin, which is itself dead. |
| `InitPlugin` | L133–L156 | 🔴 DEAD | 95% | Exported method with 0 importers; only internal caller is LoadAndInitPlugin, which is itself dead. |
| `LoadAndInitPlugin` | L160–L165 | 🔴 DEAD | 95% | Exported method with 0 importers; only internal caller is StartupEnabledPlugins, which is itself dead. |
| `ShutdownPlugin` | L168–L181 | 🔴 DEAD | 95% | Exported method with 0 importers; DisablePlugin calls s.manager.Shutdown directly, not this method. |
| `ShutdownAll` | L184–L197 | 🔴 DEAD | 95% | Exported method with 0 importers and no internal callers. |
| `EnablePlugin` | L201–L218 | 🔴 DEAD | 90% | Exported method with 0 importers and no internal callers. |
| `DisablePlugin` | L221–L250 | 🔴 DEAD | 95% | Exported method with 0 importers and no internal callers. |
| `SetPluginConfig` | L253–L266 | 🔴 DEAD | 95% | Exported method with 0 importers and no internal callers. |
| `GetPluginConfig` | L269–L274 | 🔴 DEAD | 95% | Exported method with 0 importers and no internal callers. |
| `GetPlugin` | L278–L286 | 🔴 DEAD | 95% | Exported method with 0 importers and no internal callers. |
| `ListPlugins` | L289–L308 | 🔴 DEAD | 95% | Exported method with 0 importers and no internal callers. |
| `ListEnabledPlugins` | L311–L345 | 🔴 DEAD | 95% | Exported method with 0 importers and no internal callers. |
| `ListDisabledPlugins` | L348–L353 | 🔴 DEAD | 95% | Exported method with 0 importers and no internal callers. |
| `IsPluginEnabled` | L356–L361 | 🔴 DEAD | 95% | Exported method with 0 importers and no internal callers. |
| `SaveState` | L364–L377 | 🔴 DEAD | 95% | Exported method with 0 importers and no internal callers. |
| `LoadState` | L380–L393 | 🔴 DEAD | 95% | Exported method with 0 importers and no internal callers. |
| `StartupEnabledPlugins` | L397–L426 | 🔴 DEAD | 92% | Exported method with 0 importers; intended as the startup entry point but never wired into an application bootstrap. |

### `internal/domain/ports/facade_dto.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `RunState` | L13–L13 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RunStatePending` | L17–L17 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RunStateRunning` | L19–L19 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RunStateCompleted` | L21–L21 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RunStateFailed` | L23–L23 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RunStateCancelled` | L25–L25 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ValidateOptions` | L31–L34 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RunRequest` | L39–L44 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RunOptions` | L47–L51 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepStatus` | L56–L67 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Progress` | L71–L78 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RunStatus` | L85–L109 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WorkflowSummary` | L118–L125 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ValidationError` | L131–L135 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ValidationReport` | L138–L141 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `HistoryFilter` | L144–L150 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RunRecord` | L153–L162 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `InputRequest` | L165–L169 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `InputResponse` | L172–L175 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RunStepRequest` | L179–L184 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepResult` | L187–L196 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/ports/facade_event.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `EventKind` | L29–L29 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventKindUnknown` | L38–L38 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventRunStarted` | L39–L39 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventRunCompleted` | L40–L40 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventStepStarted` | L41–L41 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventStepCompleted` | L42–L42 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventStepCallWorkflowStarted` | L43–L43 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventStepCallWorkflowCompleted` | L44–L44 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventMessageUser` | L45–L45 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventMessageAssistant` | L46–L46 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventToolCall` | L47–L47 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventToolResult` | L48–L48 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventInputRequired` | L49–L49 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventWorkflowCompleted` | L50–L50 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventWorkflowFailed` | L51–L51 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `String` | L54–L85 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Event` | L113–L120 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EnrichedStepPayload` | L124–L146 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EnrichedMessagePayload` | L151–L153 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EnrichedInputRequest` | L156–L158 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EnrichedTerminal` | L162–L164 | 🔴 DEAD | 95% | Exported but imported by 0 files |

## ⚡ Quick Wins

- [ ] <!-- ACT-4ae77a-1 --> **[utility · high · trivial]** `internal/application/plugin_service.go`: Remove dead code: `PluginService` is exported but unused `PluginService`, `NewPluginService`, `RegisterBuiltin`, `DiscoverPlugins`, `LoadPlugin`, `InitPlugin`, `LoadAndInitPlugin`, `ShutdownPlugin`, `ShutdownAll`, `EnablePlugin`, `DisablePlugin`, `SetPluginConfig`, `GetPluginConfig`, `GetPlugin`, `ListPlugins`, `ListEnabledPlugins`, `ListDisabledPlugins`, `IsPluginEnabled`, `SaveState`, `LoadState`, `StartupEnabledPlugins` (`PluginService, NewPluginService, RegisterBuiltin, DiscoverPlugins, LoadPlugin, InitPlugin, LoadAndInitPlugin, ShutdownPlugin, ShutdownAll, EnablePlugin, DisablePlugin, SetPluginConfig, GetPluginConfig, GetPlugin, ListPlugins, ListEnabledPlugins, ListDisabledPlugins, IsPluginEnabled, SaveState, LoadState, StartupEnabledPlugins`) [L25-L30, L33-L44, L48-L60, L76-L103, L107-L129, L133-L156, L160-L165, L168-L181, L184-L197, L201-L218, L221-L250, L253-L266, L269-L274, L278-L286, L289-L308, L311-L345, L348-L353, L356-L361, L364-L377, L380-L393, L397-L426]
- [ ] <!-- ACT-33b392-1 --> **[utility · high · trivial]** `internal/domain/ports/facade_dto.go`: Remove dead code: `RunState` is exported but unused `RunState`, `RunStatePending`, `RunStateRunning`, `RunStateCompleted`, `RunStateFailed`, `RunStateCancelled`, `ValidateOptions`, `RunRequest`, `RunOptions`, `StepStatus`, `Progress`, `RunStatus`, `WorkflowSummary`, `ValidationError`, `ValidationReport`, `HistoryFilter`, `RunRecord`, `InputRequest`, `InputResponse`, `RunStepRequest`, `StepResult` (`RunState, RunStatePending, RunStateRunning, RunStateCompleted, RunStateFailed, RunStateCancelled, ValidateOptions, RunRequest, RunOptions, StepStatus, Progress, RunStatus, WorkflowSummary, ValidationError, ValidationReport, HistoryFilter, RunRecord, InputRequest, InputResponse, RunStepRequest, StepResult`) [L13-L13, L17-L17, L19-L19, L21-L21, L23-L23, L25-L25, L31-L34, L39-L44, L47-L51, L56-L67, L71-L78, L85-L109, L118-L125, L131-L135, L138-L141, L144-L150, L153-L162, L165-L169, L172-L175, L179-L184, L187-L196]
- [ ] <!-- ACT-e186d4-1 --> **[utility · high · trivial]** `internal/domain/ports/facade_event.go`: Remove dead code: `EventKind` is exported but unused `EventKind`, `EventKindUnknown`, `EventRunStarted`, `EventRunCompleted`, `EventStepStarted`, `EventStepCompleted`, `EventStepCallWorkflowStarted`, `EventStepCallWorkflowCompleted`, `EventMessageUser`, `EventMessageAssistant`, `EventToolCall`, `EventToolResult`, `EventInputRequired`, `EventWorkflowCompleted`, `EventWorkflowFailed`, `String`, `Event`, `EnrichedStepPayload`, `EnrichedMessagePayload`, `EnrichedInputRequest`, `EnrichedTerminal` (`EventKind, EventKindUnknown, EventRunStarted, EventRunCompleted, EventStepStarted, EventStepCompleted, EventStepCallWorkflowStarted, EventStepCallWorkflowCompleted, EventMessageUser, EventMessageAssistant, EventToolCall, EventToolResult, EventInputRequired, EventWorkflowCompleted, EventWorkflowFailed, String, Event, EnrichedStepPayload, EnrichedMessagePayload, EnrichedInputRequest, EnrichedTerminal`) [L29-L29, L38-L38, L39-L39, L40-L40, L41-L41, L42-L42, L43-L43, L44-L44, L45-L45, L46-L46, L47-L47, L48-L48, L49-L49, L50-L50, L51-L51, L54-L85, L113-L120, L124-L146, L151-L153, L156-L158, L162-L164]
- [ ] <!-- ACT-5b4667-1 --> **[utility · high · trivial]** `internal/domain/workflow/context.go`: Remove dead code: `ExecutionStatus` is exported but unused `ExecutionStatus`, `StatusPending`, `StatusRunning`, `StatusCompleted`, `StatusFailed`, `StatusCancelled`, `String`, `StepState`, `LoopContext`, `ExecutionContext`, `NewExecutionContext`, `SetInput`, `GetInput`, `SetStepState`, `DeleteStepState`, `GetStepState`, `GetAllStepStates`, `GetStatus`, `SetStatus`, `GetCompletedAt`, `SetCompletedAt`, `GetCurrentStep`, `GetUpdatedAt`, `PushCallStack`, `PopCallStack`, `IsInCallStack`, `CallStackDepth` (`ExecutionStatus, StatusPending, StatusRunning, StatusCompleted, StatusFailed, StatusCancelled, String, StepState, LoopContext, ExecutionContext, NewExecutionContext, SetInput, GetInput, SetStepState, DeleteStepState, GetStepState, GetAllStepStates, GetStatus, SetStatus, GetCompletedAt, SetCompletedAt, GetCurrentStep, GetUpdatedAt, PushCallStack, PopCallStack, IsInCallStack, CallStackDepth`) [L9-L9, L12-L12, L13-L13, L14-L14, L15-L15, L16-L16, L19-L21, L24-L53, L56-L63, L67-L83, L86-L98, L101-L106, L109-L114, L117-L122, L125-L132, L135-L140, L143-L152, L155-L159, L162-L167, L170-L174, L177-L182, L185-L189, L192-L196, L200-L205, L209-L216, L220-L229, L233-L237]
- [ ] <!-- ACT-894766-1 --> **[utility · high · trivial]** `internal/domain/workflow/conversation.go`: Remove dead code: `ExpressionCompiler` is exported but unused `ExpressionCompiler`, `StepTypeChecker`, `TurnRole`, `TurnRoleSystem`, `TurnRoleUser`, `TurnRoleAssistant`, `Turn`, `NewTurn`, `Validate`, `ConversationConfig`, `Validate`, `StopReason`, `StopReasonUserExit`, `StopReasonError`, `ConversationState`, `NewConversationState`, `AddTurn`, `GetTotalTokens`, `GetLastTurn`, `GetLastAssistantResponse`, `IsStopped`, `ConversationResult`, `NewConversationResult`, `Duration`, `Success`, `HasJSONResponse`, `TurnCount` (`ExpressionCompiler, StepTypeChecker, TurnRole, TurnRoleSystem, TurnRoleUser, TurnRoleAssistant, Turn, NewTurn, Validate, ConversationConfig, Validate, StopReason, StopReasonUserExit, StopReasonError, ConversationState, NewConversationState, AddTurn, GetTotalTokens, GetLastTurn, GetLastAssistantResponse, IsStopped, ConversationResult, NewConversationResult, Duration, Success, HasJSONResponse, TurnCount`) [L12-L12, L17-L17, L25-L25, L28-L28, L29-L29, L30-L30, L34-L38, L41-L46, L49-L63, L66-L68, L71-L73, L76-L76, L79-L79, L80-L80, L84-L90, L93-L110, L114-L122, L125-L127, L130-L135, L138-L146, L149-L151, L154-L168, L171-L185, L188-L193, L196-L198, L201-L203, L206-L211]
- [ ] <!-- ACT-e27386-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/options.go`: Remove dead code: `ClaudeProviderOption` is exported but unused `ClaudeProviderOption`, `WithClaudeExecutor`, `WithClaudeLogger`, `WithClaudeTokenizer`, `GeminiProviderOption`, `WithGeminiExecutor`, `WithGeminiLogger`, `WithGeminiTokenizer`, `WithGeminiDenyAllPolicy`, `WithGeminiCommandExecutor`, `CodexProviderOption`, `WithCodexExecutor`, `WithCodexLogger`, `WithCodexTokenizer`, `OpenCodeProviderOption`, `WithOpenCodeExecutor`, `WithOpenCodeLogger`, `WithOpenCodeTokenizer`, `CopilotProviderOption`, `WithCopilotExecutor`, `WithCopilotLogger`, `WithCopilotTokenizer`, `MistralVibeProviderOption`, `WithMistralVibeExecutor`, `WithMistralVibeLogger`, `WithMistralVibeTokenizer`, `OpenAICompatibleProviderOption`, `WithHTTPClient` (`ClaudeProviderOption, WithClaudeExecutor, WithClaudeLogger, WithClaudeTokenizer, GeminiProviderOption, WithGeminiExecutor, WithGeminiLogger, WithGeminiTokenizer, WithGeminiDenyAllPolicy, WithGeminiCommandExecutor, CodexProviderOption, WithCodexExecutor, WithCodexLogger, WithCodexTokenizer, OpenCodeProviderOption, WithOpenCodeExecutor, WithOpenCodeLogger, WithOpenCodeTokenizer, CopilotProviderOption, WithCopilotExecutor, WithCopilotLogger, WithCopilotTokenizer, MistralVibeProviderOption, WithMistralVibeExecutor, WithMistralVibeLogger, WithMistralVibeTokenizer, OpenAICompatibleProviderOption, WithHTTPClient`) [L8-L8, L10-L14, L16-L20, L22-L26, L28-L28, L30-L34, L36-L40, L42-L46, L48-L52, L54-L58, L60-L60, L62-L66, L68-L72, L74-L78, L80-L80, L82-L86, L88-L92, L94-L98, L100-L100, L102-L106, L108-L112, L114-L118, L120-L120, L122-L126, L128-L132, L134-L138, L140-L140, L142-L146]
- [ ] <!-- ACT-6b364c-1 --> **[utility · high · trivial]** `internal/infrastructure/logger/hclog_adapter.go`: Remove dead code: `HCLogAdapter` is exported but unused `HCLogAdapter`, `NewHCLogAdapter`, `Log`, `Trace`, `Debug`, `Info`, `Warn`, `Error`, `IsTrace`, `IsDebug`, `IsInfo`, `IsWarn`, `IsError`, `ImpliedArgs`, `With`, `Name`, `Named`, `ResetNamed`, `SetLevel`, `GetLevel`, `StandardLogger`, `StandardWriter`, `NewLogWriter`, `Write` (`HCLogAdapter, NewHCLogAdapter, Log, Trace, Debug, Info, Warn, Error, IsTrace, IsDebug, IsInfo, IsWarn, IsError, ImpliedArgs, With, Name, Named, ResetNamed, SetLevel, GetLevel, StandardLogger, StandardWriter, NewLogWriter, Write`) [L16-L21, L24-L30, L70-L72, L74-L76, L78-L80, L82-L84, L86-L88, L90-L92, L94-L96, L98-L100, L102-L104, L106-L108, L110-L112, L114-L114, L116-L126, L128-L128, L130-L141, L143-L150, L152-L152, L154-L168, L170-L172, L174-L176, L186-L188, L190-L208]
- [ ] <!-- ACT-b1a229-3 --> **[utility · high · trivial]** `internal/infrastructure/xdg/xdg.go`: Remove dead code: `ConfigHome` is exported but unused `ConfigHome`, `DataHome`, `AWFConfigDir`, `AWFDataDir`, `AWFWorkflowsDir`, `AWFPromptsDir`, `AWFScriptsDir`, `AWFStatesDir`, `AWFLogsDir`, `LegacyDirExists`, `LocalWorkflowsDir`, `LocalPromptsDir`, `AWFSkillsDir`, `LocalSkillsDir`, `AWFRolesDir`, `LocalRolesDir`, `AWFPluginsDir`, `LocalPluginsDir`, `AWFWorkflowPacksDir`, `LocalWorkflowPacksDir`, `AWFPaths`, `PackAWFPaths`, `LocalConfigPath` (`ConfigHome, DataHome, AWFConfigDir, AWFDataDir, AWFWorkflowsDir, AWFPromptsDir, AWFScriptsDir, AWFStatesDir, AWFLogsDir, LegacyDirExists, LocalWorkflowsDir, LocalPromptsDir, AWFSkillsDir, LocalSkillsDir, AWFRolesDir, LocalRolesDir, AWFPluginsDir, LocalPluginsDir, AWFWorkflowPacksDir, LocalWorkflowPacksDir, AWFPaths, PackAWFPaths, LocalConfigPath`) [L11-L20, L25-L34, L38-L43, L46-L48, L51-L53, L56-L58, L61-L63, L66-L68, L71-L73, L76-L84, L87-L89, L92-L94, L97-L99, L102-L104, L107-L109, L112-L114, L117-L119, L122-L124, L127-L129, L132-L134, L138-L149, L152-L156, L160-L165]
- [ ] <!-- ACT-af9ce8-1 --> **[utility · high · trivial]** `pkg/plugin/sdk/testing.go`: Remove dead code: `MockPlugin` is exported but unused `MockPlugin`, `NewMockPlugin`, `Name`, `Version`, `Init`, `Shutdown`, `WasInitCalled`, `WasShutdownCalled`, `Reset`, `MockOperationHandler`, `NewMockOperationHandler`, `Handle`, `GetCallCount`, `Reset`, `MockOperationProvider`, `NewMockOperationProvider`, `Operations`, `HandleOperation`, `SetResult`, `SetCommandResult`, `SetError`, `TestInputs`, `TestConfig` (`MockPlugin, NewMockPlugin, Name, Version, Init, Shutdown, WasInitCalled, WasShutdownCalled, Reset, MockOperationHandler, NewMockOperationHandler, Handle, GetCallCount, Reset, MockOperationProvider, NewMockOperationProvider, Operations, HandleOperation, SetResult, SetCommandResult, SetError, TestInputs, TestConfig`) [L10-L20, L22-L27, L29-L31, L33-L35, L38-L44, L47-L52, L55-L59, L61-L65, L67-L73, L76-L83, L85-L89, L92-L105, L107-L111, L113-L118, L121-L129, L131-L137, L140-L144, L146-L166, L168-L170, L172-L176, L178-L182, L184-L192, L194-L196]
- [ ] <!-- ACT-3ba903-5 --> **[utility · high · trivial]** `pkg/registry/version.go`: Remove dead code: `OpEqual` is exported but unused `OpEqual`, `OpNotEqual`, `OpGreater`, `OpGreaterOrEqual`, `OpLess`, `OpLessOrEqual`, `OpTilde`, `OpCaret`, `Version`, `String`, `Compare`, `IsPrerelease`, `Constraint`, `Check`, `Constraints`, `Check`, `String`, `ParseVersion`, `ParseConstraint`, `ParseConstraints`, `CheckVersionConstraint`, `NormalizeTag` (`OpEqual, OpNotEqual, OpGreater, OpGreaterOrEqual, OpLess, OpLessOrEqual, OpTilde, OpCaret, Version, String, Compare, IsPrerelease, Constraint, Check, Constraints, Check, String, ParseVersion, ParseConstraint, ParseConstraints, CheckVersionConstraint, NormalizeTag`) [L12-L12, L13-L13, L14-L14, L15-L15, L16-L16, L17-L17, L18-L18, L19-L19, L23-L28, L34-L40, L44-L83, L85-L87, L90-L93, L95-L143, L146-L146, L149-L159, L162-L171, L175-L211, L218-L245, L250-L285, L289-L308, L311-L313]
