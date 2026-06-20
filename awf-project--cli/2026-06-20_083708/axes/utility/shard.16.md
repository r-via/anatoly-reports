[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 16

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/interfaces/api/handlers_history.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinterfacesapihandlershistorygo) |
| `internal/interfaces/cli/plugins.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinterfacesclipluginsgo) |
| `internal/testutil/fixtures/fixtures.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internaltestutilfixturesfixturesgo) |
| `pkg/expression/evaluator.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#pkgexpressionevaluatorgo) |
| `pkg/interpolation/errors.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#pkginterpolationerrorsgo) |
| `pkg/registry/downloader.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#pkgregistrydownloadergo) |
| `internal/interfaces/api/handlers_executions.go` | 🟡 NEEDS_REFACTOR | 5 | 93% | [details](#internalinterfacesapihandlersexecutionsgo) |
| `pkg/plugin/sdk/event.go` | 🟡 NEEDS_REFACTOR | 5 | 85% | [details](#pkgpluginsdkeventgo) |
| `internal/domain/ports/interactive.go` | 🟡 NEEDS_REFACTOR | 4 | 100% | [details](#internaldomainportsinteractivego) |
| `internal/infrastructure/agents/display_event.go` | 🟡 NEEDS_REFACTOR | 4 | 100% | [details](#internalinfrastructureagentsdisplayeventgo) |

## 🔍 Symbol Details

### `internal/interfaces/api/handlers_history.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `HistoryHandlers` | L17–L20 | 🔴 DEAD | 95% | Exported type with 0 cross-file importers. Local references only exist inside NewHistoryHandlers and RegisterHistoryRoutes, both of which are also dead. |
| `NewHistoryHandlers` | L24–L26 | 🔴 DEAD | 95% | Exported constructor with 0 importers. Not called anywhere in the file or externally. |
| `List` | L28–L52 | 🔴 DEAD | 90% | Exported method with 0 importers. Only referenced locally as h.List in RegisterHistoryRoutes (L79), which is itself dead. |
| `Stats` | L54–L66 | 🔴 DEAD | 90% | Exported method with 0 importers. Only referenced locally as h.Stats in RegisterHistoryRoutes (L82), which is itself dead. |
| `RegisterHistoryRoutes` | L69–L83 | 🔴 DEAD | 95% | Exported registration function with 0 importers and no local callers. Nothing wires these routes into the router. |

### `internal/interfaces/cli/plugins.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `PluginSystemResult` | L14–L21 | 🔴 DEAD | 85% | Exported struct with 0 external importers per exhaustive import analysis. Only local usage is as the return type and instantiation target of non-exported `initPluginSystem`, so it is unreachable from outside the package. |
| `initPluginSystem` | L29–L51 | 🔴 DEAD | 60% | Not exported and not called anywhere in this file. Likely invoked from other files in the `cli` package, but per file-scope rule it has no local callers. Confidence reduced due to strong likelihood of cross-file intra-package usage. |
| `findFirstExistingDir` | L81–L87 | 🔴 DEAD | 65% | Not exported and not called anywhere in this file. Could be used from other `cli` package files, but no local call site exists. Confidence reduced given it is a plausible intra-package utility. |
| `findPluginDir` | L93–L108 | 🔴 DEAD | 65% | Not exported and not called anywhere in this file. No local call site; likely intended for other `cli` package files (e.g., plugin install/uninstall commands). Confidence reduced accordingly. |
| `resolvePluginStateName` | L112–L122 | 🔴 DEAD | 65% | Not exported and not called anywhere in this file. No local caller; appears to be a helper for state-store lookups that may be used from other files in the `cli` package. |

### `internal/testutil/fixtures/fixtures.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `SimpleWorkflow` | L43–L77 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LinearWorkflow` | L93–L138 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ParallelWorkflow` | L157–L217 | 🔴 DEAD | 60% | Exported but imported by 0 files |
| `LoopWorkflow` | L236–L318 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `ConversationWorkflow` | L338–L402 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `pkg/expression/evaluator.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Evaluator` | L15–L20 | 🔴 DEAD | 95% | Exported interface with 0 importers; never referenced locally either. |
| `ExprEvaluator` | L23–L23 | 🔴 DEAD | 95% | Exported struct with 0 importers; only instantiated by NewExprEvaluator which is itself dead. |
| `NewExprEvaluator` | L25–L27 | 🔴 DEAD | 95% | Exported constructor with 0 importers and no local call sites. |
| `Evaluate` | L29–L67 | 🔴 DEAD | 90% | Exported method with 0 importers; no local call sites outside its own receiver. |

### `pkg/interpolation/errors.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `UndefinedVariableError` | L6–L8 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L10–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ParseError` | L15–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L20–L22 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Unwrap` | L24–L26 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `pkg/registry/downloader.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Download` | L26–L56 | 🔴 DEAD | 95% | Exported with 0 runtime and 0 type-only importers across the codebase. |
| `VerifyChecksum` | L59–L70 | 🔴 DEAD | 95% | Exported with 0 runtime and 0 type-only importers across the codebase. |
| `ExtractTarGz` | L73–L102 | 🔴 DEAD | 92% | Exported with 0 runtime and 0 type-only importers across the codebase. |
| `ExtractChecksumForAsset` | L138–L146 | 🔴 DEAD | 93% | Exported with 0 runtime and 0 type-only importers across the codebase. |

### `internal/interfaces/api/handlers_executions.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ExecutionHandlers` | L17–L21 | 🔴 DEAD | 75% | Exported struct with 0 cross-file importers. Its only constructor NewExecutionHandlers also has 0 importers and is not called locally; no external code instantiates this type. |
| `SetFacade` | L23–L23 | 🔴 DEAD | 90% | Exported setter with 0 importers and no local callers. Facade dependency injection is never exercised from outside this package. |
| `SetSessionRegistry` | L25–L25 | 🔴 DEAD | 90% | Exported setter with 0 importers and no local callers. Registry injection is never exercised from outside this package. |
| `NewExecutionHandlers` | L80–L82 | 🔴 DEAD | 90% | Exported constructor with 0 importers; not called locally. No file creates an ExecutionHandlers instance. |
| `RegisterExecutionRoutes` | L220–L257 | 🔴 DEAD | 85% | Exported function with 0 importers and no local callers. No file mounts these routes; all huma.Register calls within are never executed. |

### `pkg/plugin/sdk/event.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Event` | L15–L23 | 🔴 DEAD | 85% | 0 external importers per exhaustive analysis. Local references exist only within eventServiceServer methods, which are themselves dead — the local usage is part of an unused call chain. |
| `EventSubscriber` | L26–L29 | 🔴 DEAD | 85% | 0 external importers. Used only as a type assertion target inside eventServiceServer.HandleEvent and StreamEvents — both of which are dead. No plugin in the analyzed codebase implements this interface. |
| `eventServiceServer` | L33–L36 | 🔴 DEAD | 70% | Unexported struct; never instantiated in this file. Methods are defined on it but it is not constructed or passed to a gRPC server registration call anywhere visible. Confidence reduced because same-package usage in other files cannot be ruled out. |
| `HandleEvent` | L38–L73 | 🔴 DEAD | 80% | 0 direct importers. Implements pluginv1.EventServiceServer via gRPC interface dispatch, so it would be called by the framework only if eventServiceServer is registered with a gRPC server — no such registration is visible in the analyzed codebase. |
| `StreamEvents` | L75–L102 | 🔴 DEAD | 80% | 0 direct importers. Same reasoning as HandleEvent: gRPC interface method reachable only through framework dispatch after server registration, which is absent from the analyzed codebase. |

### `internal/domain/ports/interactive.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `StepPresenter` | L11–L23 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `StatusPresenter` | L27–L39 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `UserInteraction` | L43–L54 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `InteractivePrompt` | L59–L63 | 🔴 DEAD | 100% | Exported but imported by 0 files |

### `internal/infrastructure/agents/display_event.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `EventText` | L15–L15 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `EventToolUse` | L16–L16 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `DisplayModeDefault` | L20–L20 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `DisplayModeVerbose` | L21–L21 | 🔴 DEAD | 100% | Exported but imported by 0 files |

## ⚡ Quick Wins

- [ ] <!-- ACT-064c09-2 --> **[utility · high · trivial]** `internal/interfaces/api/handlers_executions.go`: Remove dead code: `SetFacade` is exported but unused `SetFacade`, `SetSessionRegistry`, `NewExecutionHandlers`, `RegisterExecutionRoutes` (`SetFacade, SetSessionRegistry, NewExecutionHandlers, RegisterExecutionRoutes`) [L23-L23, L25-L25, L80-L82, L220-L257]
- [ ] <!-- ACT-6f206b-1 --> **[utility · high · trivial]** `internal/interfaces/api/handlers_history.go`: Remove dead code: `HistoryHandlers` is exported but unused `HistoryHandlers`, `NewHistoryHandlers`, `List`, `Stats`, `RegisterHistoryRoutes` (`HistoryHandlers, NewHistoryHandlers, List, Stats, RegisterHistoryRoutes`) [L17-L20, L24-L26, L28-L52, L54-L66, L69-L83]
- [ ] <!-- ACT-16be32-2 --> **[utility · high · trivial]** `internal/interfaces/cli/plugins.go`: Remove dead code: `PluginSystemResult` is exported but unused (`PluginSystemResult`) [L14-L21]
- [ ] <!-- ACT-c3f659-2 --> **[utility · high · trivial]** `internal/testutil/fixtures/fixtures.go`: Remove dead code: `SimpleWorkflow` is exported but unused `SimpleWorkflow`, `LinearWorkflow`, `LoopWorkflow`, `ConversationWorkflow` (`SimpleWorkflow, LinearWorkflow, LoopWorkflow, ConversationWorkflow`) [L43-L77, L93-L138, L236-L318, L338-L402]
- [ ] <!-- ACT-0fdca5-2 --> **[utility · high · trivial]** `pkg/expression/evaluator.go`: Remove dead code: `Evaluator` is exported but unused `Evaluator`, `ExprEvaluator`, `NewExprEvaluator`, `Evaluate` (`Evaluator, ExprEvaluator, NewExprEvaluator, Evaluate`) [L15-L20, L23-L23, L25-L27, L29-L67]
- [ ] <!-- ACT-6371db-1 --> **[utility · high · trivial]** `pkg/interpolation/errors.go`: Remove dead code: `UndefinedVariableError` is exported but unused `UndefinedVariableError`, `Error`, `ParseError`, `Error`, `Unwrap` (`UndefinedVariableError, Error, ParseError, Error, Unwrap`) [L6-L8, L10-L12, L15-L18, L20-L22, L24-L26]
- [ ] <!-- ACT-b84116-2 --> **[utility · high · trivial]** `pkg/plugin/sdk/event.go`: Remove dead code: `Event` is exported but unused `Event`, `EventSubscriber`, `HandleEvent`, `StreamEvents` (`Event, EventSubscriber, HandleEvent, StreamEvents`) [L15-L23, L26-L29, L38-L73, L75-L102]
- [ ] <!-- ACT-9dff52-3 --> **[utility · high · trivial]** `pkg/registry/downloader.go`: Remove dead code: `Download` is exported but unused `Download`, `VerifyChecksum`, `ExtractTarGz`, `ExtractChecksumForAsset` (`Download, VerifyChecksum, ExtractTarGz, ExtractChecksumForAsset`) [L26-L56, L59-L70, L73-L102, L138-L146]

## 🔧 Refactors

- [ ] <!-- ACT-064c09-1 --> **[utility · medium · trivial]** `internal/interfaces/api/handlers_executions.go`: Remove dead code: `ExecutionHandlers` is exported but unused (`ExecutionHandlers`) [L17-L21]
- [ ] <!-- ACT-16be32-3 --> **[utility · medium · trivial]** `internal/interfaces/cli/plugins.go`: Remove dead code: `initPluginSystem` is exported but unused `initPluginSystem`, `findFirstExistingDir`, `findPluginDir`, `resolvePluginStateName` (`initPluginSystem, findFirstExistingDir, findPluginDir, resolvePluginStateName`) [L29-L51, L81-L87, L93-L108, L112-L122]
- [ ] <!-- ACT-c3f659-3 --> **[utility · medium · trivial]** `internal/testutil/fixtures/fixtures.go`: Remove dead code: `ParallelWorkflow` is exported but unused (`ParallelWorkflow`) [L157-L217]
- [ ] <!-- ACT-b84116-3 --> **[utility · medium · trivial]** `pkg/plugin/sdk/event.go`: Remove dead code: `eventServiceServer` is exported but unused (`eventServiceServer`) [L33-L36]
