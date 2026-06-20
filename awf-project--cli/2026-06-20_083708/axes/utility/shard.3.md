[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 3

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)
- [🧹 Hygiene](#-hygiene)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/interfaces/tui/bridge.go` | 🟡 NEEDS_REFACTOR | 21 | 95% | [details](#internalinterfacestuibridgego) |
| `internal/testutil/facadetest/facadetest.go` | 🟡 NEEDS_REFACTOR | 20 | 95% | [details](#internaltestutilfacadetestfacadetestgo) |
| `internal/application/acp_session_service.go` | 🟡 NEEDS_REFACTOR | 20 | 95% | [details](#internalapplicationacpsessionservicego) |
| `internal/domain/workflow/parallel.go` | 🟡 NEEDS_REFACTOR | 18 | 95% | [details](#internaldomainworkflowparallelgo) |
| `internal/domain/workflow/step.go` | 🟡 NEEDS_REFACTOR | 18 | 95% | [details](#internaldomainworkflowstepgo) |
| `internal/domain/workflow/loop.go` | 🟡 NEEDS_REFACTOR | 17 | 95% | [details](#internaldomainworkflowloopgo) |
| `internal/infrastructure/logger/json_logger.go` | 🟡 NEEDS_REFACTOR | 16 | 95% | [details](#internalinfrastructureloggerjsonloggergo) |
| `internal/infrastructure/pluginmgr/state_store.go` | 🟡 NEEDS_REFACTOR | 16 | 95% | [details](#internalinfrastructurepluginmgrstatestorego) |
| `internal/interfaces/api/types.go` | 🟡 NEEDS_REFACTOR | 16 | 95% | [details](#internalinterfacesapitypesgo) |
| `internal/application/facade_adapter.go` | 🟡 NEEDS_REFACTOR | 15 | 95% | [details](#internalapplicationfacadeadaptergo) |

## 🔍 Symbol Details

### `internal/interfaces/tui/bridge.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `StreamBuffer` | L18–L21 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Write` | L23–L31 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `String` | L34–L38 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Len` | L41–L45 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Reset` | L48–L52 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WorkflowLister` | L56–L60 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `HistoryProvider` | L64–L67 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Bridge` | L75–L85 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewBridge` | L91–L97 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `SetFacade` | L102–L104 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LoadWorkflows` | L109–L135 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `LoadHistory` | L139–L157 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `RunWorkflowViaFacade` | L167–L205 | 🔴 DEAD | 93% | Exported but imported by 0 files |
| `ValidateWorkflow` | L209–L221 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `MsgSender` | L227–L227 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TUIInputReader` | L232–L236 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewTUIInputReader` | L240–L246 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetSender` | L250–L252 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ReadInput` | L257–L273 | 🔴 DEAD | 78% | Exported but imported by 0 files |
| `RequestCh` | L277–L279 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Respond` | L282–L284 | 🔴 DEAD | 90% | Exported but imported by 0 files |

### `internal/testutil/facadetest/facadetest.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Fake` | L17–L21 | 🔴 DEAD | 95% | 0 importers. Entire test-double type is unreferenced outside this file. |
| `New` | L24–L26 | 🔴 DEAD | 95% | 0 importers. Constructor for Fake, which itself has no consumers. |
| `Script` | L29–L34 | 🔴 DEAD | 95% | 0 importers. Only called internally by With* builder methods on an unused type. |
| `WithTerminalCompleted` | L37–L39 | 🔴 DEAD | 95% | 0 importers. Builder method on unreferenced Fake type. |
| `WithTerminalFailed` | L45–L54 | 🔴 DEAD | 95% | 0 importers. Builder method on unreferenced Fake type. |
| `WithInputRequired` | L58–L63 | 🔴 DEAD | 95% | 0 importers. Builder method on unreferenced Fake type. |
| `WithHistory` | L66–L71 | 🔴 DEAD | 95% | 0 importers. Builder method on unreferenced Fake type. |
| `List` | L74–L76 | 🔴 DEAD | 95% | 0 importers. ports.WorkflowFacade interface stub on unreferenced Fake type. |
| `Validate` | L79–L81 | 🔴 DEAD | 95% | 0 importers. ports.WorkflowFacade interface stub on unreferenced Fake type. |
| `Status` | L84–L86 | 🔴 DEAD | 95% | 0 importers. ports.WorkflowFacade interface stub on unreferenced Fake type. |
| `History` | L89–L95 | 🔴 DEAD | 95% | 0 importers. ports.WorkflowFacade interface stub on unreferenced Fake type. |
| `Run` | L100–L111 | 🔴 DEAD | 95% | 0 importers. Core method of unreferenced Fake; calls newFakeSession internally but no external callers. |
| `Resume` | L121–L125 | 🔴 DEAD | 95% | 0 importers. ports.WorkflowFacade interface implementation on unreferenced Fake type. |
| `FakeSession` | L130–L140 | 🔴 DEAD | 95% | 0 importers. Returned by Fake.Run/Resume, but Fake itself has no consumers. |
| `ID` | L191–L191 | 🔴 DEAD | 95% | 0 importers. ports.RunSession interface method on unreferenced FakeSession type. |
| `Events` | L195–L195 | 🔴 DEAD | 95% | 0 importers. ports.RunSession interface method on unreferenced FakeSession type. |
| `Respond` | L199–L211 | 🔴 DEAD | 95% | 0 importers. ports.RunSession interface method on unreferenced FakeSession type. |
| `Err` | L214–L214 | 🔴 DEAD | 95% | 0 importers. ports.RunSession interface stub on unreferenced FakeSession type. |
| `StatusSnapshot` | L219–L242 | 🔴 DEAD | 90% | 0 importers. Non-trivial status derivation logic on unreferenced FakeSession type. |
| `Close` | L246–L255 | 🔴 DEAD | 95% | 0 importers. ports.RunSession interface method on unreferenced FakeSession type. |

### `internal/application/acp_session_service.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `InputSpec` | L49–L53 | 🔴 DEAD | 75% | 0 external importers. Used locally as field type in WorkflowSlashCommand and by splitWorkflowInputs, but no cross-package consumers detected. |
| `WorkflowSlashCommand` | L57–L66 | 🔴 DEAD | 75% | 0 external importers. Used heavily in discoverSlashCommands, workflowCatalog, loadCommandMetadata, and HandleSessionNew internally, but no cross-package consumers. |
| `SessionUpdateEmitter` | L72–L74 | 🔴 DEAD | 75% | 0 external importers. Used as field type on ACPSessionService and checked in sendAgentText/emitAvailableCommands, but no cross-package consumers detected. |
| `WorkflowProvider` | L87–L90 | 🔴 DEAD | 75% | 0 external importers. Used as field type on ACPSessionService and dispatched in workflowCatalog, but no cross-package consumers detected. |
| `ACPSessionService` | L94–L135 | 🔴 DEAD | 75% | 0 external importers. Central service struct with all methods defined in this file; no cross-package consumers detected despite extensive internal implementation. |
| `SetSessionUpdateEmitter` | L138–L140 | 🔴 DEAD | 90% | 0 external importers. Setter method not called within this file; intended for infrastructure wiring layer that has no detected importers. |
| `SetWorkflowProvider` | L146–L148 | 🔴 DEAD | 90% | 0 external importers. Setter method not called within this file; intended for infrastructure wiring layer that has no detected importers. |
| `SetRunnerFactory` | L154–L156 | 🔴 DEAD | 90% | 0 external importers. Setter method not called within this file; intended for infrastructure wiring layer that has no detected importers. |
| `SetServerContext` | L163–L165 | 🔴 DEAD | 90% | 0 external importers. Setter method not called within this file; intended for infrastructure wiring layer that has no detected importers. |
| `SetFacade` | L171–L173 | 🔴 DEAD | 90% | 0 external importers. Setter method not called within this file; intended for infrastructure wiring layer that has no detected importers. |
| `NewACPSessionService` | L178–L191 | 🔴 DEAD | 90% | 0 external importers. Constructor not called within this file; no cross-package consumer detected. |
| `HandleSessionNew` | L314–L374 | 🔴 DEAD | 80% | 0 external importers. Core ACP handler with no detected cross-package consumer; likely unregistered in the infrastructure adapter. |
| `HandleSessionPrompt` | L465–L594 | 🔴 DEAD | 80% | 0 external importers. Core ACP handler with no detected cross-package consumer. |
| `ReadInput` | L847–L850 | 🟡 LOW_VALUE | 90% | No-op stub returning empty string; comment explicitly states it is not used in the facade path. Required only for interface compliance. |
| `SetParkHooks` | L871–L874 | 🟡 LOW_VALUE | 90% | Empty body; park accounting is handled directly in projectFacadeEvents. Required only for interface compliance; the comment confirms it is intentionally a no-op. |
| `RenderFacadeEventsToACPSessionUpdate` | L1047–L1064 | 🔴 DEAD | 80% | 0 external importers. Described as used by the conformance suite (F108 T079 golden) but no cross-package consumer detected in the analysis. |
| `HandleSessionCancel` | L1069–L1084 | 🔴 DEAD | 80% | 0 external importers. Core ACP handler with no detected cross-package consumer. |
| `Shutdown` | L1098–L1126 | 🔴 DEAD | 80% | 0 external importers. Lifecycle method not called within this file and no cross-package consumer detected. |
| `PromptResult` | L1151–L1153 | 🔴 DEAD | 75% | 0 external importers. Returned by promptStop and used as result type throughout, but no cross-package type-assert or reference detected. |
| `MaxPromptBytes` | L1165–L1165 | 🔴 DEAD | 70% | 0 external importers. Referenced locally in parseSlashCommand and explicitly documented for external use by the infrastructure adapter, but no cross-package consumer detected. |

### `internal/domain/workflow/parallel.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ParallelStrategy` | L6–L6 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StrategyAllSucceed` | L10–L10 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StrategyAnySucceed` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StrategyBestEffort` | L14–L14 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DefaultParallelStrategy` | L18–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DefaultMaxConcurrent` | L21–L21 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ParseParallelStrategy` | L25–L36 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `String` | L38–L40 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ParallelConfig` | L43–L46 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `BranchResult` | L49–L57 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Duration` | L60–L62 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `Success` | L65–L67 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ParallelResult` | L70–L77 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewParallelResult` | L80–L85 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AddResult` | L88–L98 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `Duration` | L101–L103 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `AllSucceeded` | L106–L108 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AnySucceeded` | L111–L113 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/workflow/step.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `StepType` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepTypeCommand` | L15–L15 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepTypeParallel` | L16–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepTypeTerminal` | L17–L17 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepTypeForEach` | L18–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepTypeWhile` | L19–L19 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepTypeOperation` | L20–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepTypeCallWorkflow` | L21–L21 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepTypeAgent` | L22–L22 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TerminalStatus` | L26–L26 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TerminalSuccess` | L29–L29 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TerminalFailure` | L30–L30 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `String` | L47–L49 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RetryConfig` | L52–L60 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Validate` | L63–L81 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `CaptureConfig` | L84–L89 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Step` | L92–L123 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Validate` | L131–L248 | 🔴 DEAD | 92% | Exported but imported by 0 files |

### `internal/domain/workflow/loop.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `LoopType` | L9–L9 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LoopTypeForEach` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LoopTypeWhile` | L13–L13 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `String` | L16–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DefaultMaxIterations` | L21–L21 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MaxAllowedIterations` | L24–L24 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LoopConfig` | L27–L38 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Validate` | L41–L72 | 🔴 DEAD | 85% | Exported but imported by 0 files |
| `IsMaxIterationsDynamic` | L75–L77 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `IterationResult` | L80–L87 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Duration` | L90–L92 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Success` | L95–L97 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LoopResult` | L100–L110 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewLoopResult` | L113–L119 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Duration` | L122–L124 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WasBroken` | L127–L129 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AllSucceeded` | L132–L139 | 🔴 DEAD | 80% | Exported but imported by 0 files |

### `internal/infrastructure/logger/json_logger.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Level` | L15–L15 | 🔴 DEAD | 95% | Exported type with 0 cross-file importers. Used only within this file's own switch statements. |
| `LevelDebug` | L18–L18 | 🔴 DEAD | 95% | Exported constant with 0 cross-file importers. Referenced only in local String() and toZapLevel() switch cases. |
| `LevelInfo` | L19–L19 | 🔴 DEAD | 95% | Exported constant with 0 cross-file importers. Referenced only in local String() and toZapLevel() switch cases. |
| `LevelWarn` | L20–L20 | 🔴 DEAD | 95% | Exported constant with 0 cross-file importers. Referenced only in local String() and toZapLevel() switch cases. |
| `LevelError` | L21–L21 | 🔴 DEAD | 95% | Exported constant with 0 cross-file importers. Referenced only in local String() and toZapLevel() switch cases. |
| `String` | L24–L37 | 🔴 DEAD | 95% | Exported method with 0 cross-file importers. Not called anywhere within this file either. |
| `ParseLevel` | L55–L68 | 🔴 DEAD | 95% | Exported function with 0 cross-file importers. Not called anywhere within this file. |
| `JSONLogger` | L71–L75 | 🔴 DEAD | 95% | Exported struct with 0 cross-file importers. No external consumer constructs or references this type. |
| `NewJSONLogger` | L78–L108 | 🔴 DEAD | 95% | Exported constructor with 0 cross-file importers. No external caller instantiates JSONLogger. |
| `Debug` | L110–L112 | 🔴 DEAD | 95% | Exported method with 0 cross-file importers. Not called locally within this file. |
| `Info` | L114–L116 | 🔴 DEAD | 95% | Exported method with 0 cross-file importers. Not called locally within this file. |
| `Warn` | L118–L120 | 🔴 DEAD | 95% | Exported method with 0 cross-file importers. Not called locally within this file. |
| `Error` | L122–L124 | 🔴 DEAD | 95% | Exported method with 0 cross-file importers. Not called locally within this file. |
| `WithContext` | L126–L136 | 🔴 DEAD | 90% | Exported method with 0 cross-file importers. Not called locally within this file. |
| `Sync` | L139–L144 | 🔴 DEAD | 95% | Exported method with 0 cross-file importers. Close() calls l.logger.Sync() (zap's Sync), not this method. |
| `Close` | L147–L156 | 🔴 DEAD | 95% | Exported method with 0 cross-file importers. Not called locally within this file. |

### `internal/infrastructure/pluginmgr/state_store.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `JSONPluginStateStore` | L20–L24 | 🔴 DEAD | 95% | 0 importers. The struct and its constructor are never referenced outside this package. |
| `NewJSONPluginStateStore` | L27–L32 | 🔴 DEAD | 95% | 0 importers. Constructor never called outside this package. |
| `Save` | L35–L104 | 🔴 DEAD | 90% | 0 importers. Never called outside this package; parent type itself is uninstantiated externally. |
| `Load` | L107–L147 | 🔴 DEAD | 82% | 0 importers. Never called outside this package. |
| `SetEnabled` | L150–L173 | 🔴 DEAD | 90% | 0 importers. Never called outside this package. |
| `IsEnabled` | L177–L186 | 🔴 DEAD | 90% | 0 importers. Never called outside this package. |
| `GetConfig` | L190–L199 | 🔴 DEAD | 72% | 0 importers. Never called outside this package. |
| `SetConfig` | L202–L219 | 🔴 DEAD | 72% | 0 importers. Never called outside this package. |
| `GetState` | L222–L226 | 🔴 DEAD | 82% | 0 importers. Never called outside this package. |
| `ListDisabled` | L229–L240 | 🔴 DEAD | 90% | 0 importers. Never called outside this package. |
| `BasePath` | L243–L245 | 🔴 DEAD | 95% | 0 importers. Never called outside this package. |
| `SetSourceData` | L253–L270 | 🔴 DEAD | 72% | 0 importers. Never called outside this package. |
| `GetSourceData` | L274–L283 | 🔴 DEAD | 72% | 0 importers. Never called outside this package. |
| `SetChecksum` | L287–L300 | 🔴 DEAD | 90% | 0 importers. Never called outside this package. |
| `GetChecksum` | L304–L314 | 🔴 DEAD | 90% | 0 importers. Never called outside this package. |
| `RemoveState` | L317–L328 | 🔴 DEAD | 95% | 0 importers. Never called outside this package. |

### `internal/interfaces/api/types.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `WorkflowSummary` | L17–L23 | 🔴 DEAD | 70% | 0 cross-package importers. In Go, same-package handler files would not appear in import analysis, reducing certainty. No intra-file usage found. |
| `ListWorkflowsOutput` | L29–L33 | 🔴 DEAD | 70% | 0 cross-package importers. Likely consumed by same-package handlers which Go import analysis cannot capture. |
| `GetWorkflowInput` | L37–L40 | 🔴 DEAD | 70% | 0 cross-package importers. Same-package handler usage would not appear in import analysis. |
| `GetWorkflowOutput` | L42–L46 | 🔴 DEAD | 70% | 0 cross-package importers. Same-package handler usage would not appear in import analysis. |
| `ValidateWorkflowInput` | L50–L53 | 🔴 DEAD | 70% | 0 cross-package importers. Same-package handler usage would not appear in import analysis. |
| `ValidateWorkflowOutput` | L59–L63 | 🔴 DEAD | 70% | 0 cross-package importers. Same-package handler usage would not appear in import analysis. |
| `RunWorkflowInput` | L67–L73 | 🔴 DEAD | 70% | 0 cross-package importers. Same-package handler usage would not appear in import analysis. |
| `RunWorkflowOutput` | L103–L105 | 🔴 DEAD | 70% | 0 cross-package importers. Same-package handler usage would not appear in import analysis. |
| `ListExecutionsOutput` | L113–L117 | 🔴 DEAD | 70% | 0 cross-package importers. Same-package handler usage would not appear in import analysis. |
| `GetExecutionInput` | L121–L123 | 🔴 DEAD | 70% | 0 cross-package importers. Same-package handler usage would not appear in import analysis. |
| `CancelExecutionInput` | L127–L129 | 🔴 DEAD | 70% | 0 cross-package importers. Same-package handler usage would not appear in import analysis. |
| `ResumeExecutionInput` | L133–L139 | 🔴 DEAD | 70% | 0 cross-package importers. Same-package handler usage would not appear in import analysis. |
| `ExecutionOutput` | L152–L156 | 🔴 DEAD | 70% | 0 cross-package importers. Same-package handler usage would not appear in import analysis. |
| `HistoryListInput` | L169–L175 | 🔴 DEAD | 70% | 0 cross-package importers. Same-package handler usage would not appear in import analysis. |
| `HistoryListOutput` | L181–L185 | 🔴 DEAD | 70% | 0 cross-package importers. Same-package handler usage would not appear in import analysis. |
| `HistoryStatsOutput` | L187–L191 | 🔴 DEAD | 70% | 0 cross-package importers. Same-package handler usage would not appear in import analysis. |

### `internal/application/facade_adapter.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Adapter` | L39–L64 | 🔴 DEAD | 88% | 0 external importers. The compile-time assertions at L22-27 reference *Adapter within this file to verify interface compliance, but no caller outside the package instantiates or imports the type. Confidence reduced from 95 because the interface assertions suggest intended wiring that may exist outside the import analysis scope. |
| `NewAdapter` | L68–L84 | 🔴 DEAD | 95% | 0 importers. No external file calls NewAdapter to construct an Adapter instance. |
| `SetUserInputReader` | L87–L91 | 🔴 DEAD | 95% | 0 importers. Optional edge-wiring setter never called externally. |
| `SetRunRecorderFactory` | L96–L100 | 🔴 DEAD | 95% | 0 importers. Optional per-run recorder factory setter never called externally. |
| `List` | L118–L134 | 🔴 DEAD | 95% | 0 importers. ports.WorkflowFacade implementation never invoked externally. |
| `Validate` | L138–L164 | 🔴 DEAD | 95% | 0 importers. ports.WorkflowFacade implementation never invoked externally. |
| `Status` | L171–L198 | 🔴 DEAD | 95% | 0 importers. ports.WorkflowFacade implementation never invoked externally. |
| `History` | L201–L229 | 🔴 DEAD | 95% | 0 importers. ports.WorkflowFacade implementation never invoked externally. |
| `GetWorkflow` | L237–L242 | 🔴 DEAD | 95% | 0 importers. ports.WorkflowReader implementation never invoked externally. |
| `HistoryStats` | L247–L258 | 🔴 DEAD | 95% | 0 importers. ports.WorkflowReader implementation never invoked externally. |
| `Run` | L267–L295 | 🔴 DEAD | 90% | 0 importers. ports.WorkflowFacade implementation never invoked externally. |
| `ValidateDir` | L310–L358 | 🔴 DEAD | 95% | 0 importers. ports.BatchValidator implementation never invoked externally. |
| `ValidatePack` | L370–L424 | 🔴 DEAD | 90% | 0 importers. ports.BatchValidator implementation never invoked externally. |
| `Resume` | L478–L500 | 🔴 DEAD | 95% | 0 importers. ports.WorkflowFacade implementation never invoked externally. |
| `RunStep` | L507–L525 | 🔴 DEAD | 95% | 0 importers. ports.SingleStepRunner implementation never invoked externally. |

## ⚡ Quick Wins

- [ ] <!-- ACT-9e239d-2 --> **[utility · high · trivial]** `internal/application/acp_session_service.go`: Remove dead code: `SetSessionUpdateEmitter` is exported but unused `SetSessionUpdateEmitter`, `SetWorkflowProvider`, `SetRunnerFactory`, `SetServerContext`, `SetFacade`, `NewACPSessionService`, `HandleSessionNew`, `HandleSessionPrompt`, `RenderFacadeEventsToACPSessionUpdate`, `HandleSessionCancel`, `Shutdown` (`SetSessionUpdateEmitter, SetWorkflowProvider, SetRunnerFactory, SetServerContext, SetFacade, NewACPSessionService, HandleSessionNew, HandleSessionPrompt, RenderFacadeEventsToACPSessionUpdate, HandleSessionCancel, Shutdown`) [L138-L140, L146-L148, L154-L156, L163-L165, L171-L173, L178-L191, L314-L374, L465-L594, L1047-L1064, L1069-L1084, L1098-L1126]
- [ ] <!-- ACT-ab8a80-1 --> **[utility · high · trivial]** `internal/application/facade_adapter.go`: Remove dead code: `Adapter` is exported but unused `Adapter`, `NewAdapter`, `SetUserInputReader`, `SetRunRecorderFactory`, `List`, `Validate`, `Status`, `History`, `GetWorkflow`, `HistoryStats`, `Run`, `ValidateDir`, `ValidatePack`, `Resume`, `RunStep` (`Adapter, NewAdapter, SetUserInputReader, SetRunRecorderFactory, List, Validate, Status, History, GetWorkflow, HistoryStats, Run, ValidateDir, ValidatePack, Resume, RunStep`) [L39-L64, L68-L84, L87-L91, L96-L100, L118-L134, L138-L164, L171-L198, L201-L229, L237-L242, L247-L258, L267-L295, L310-L358, L370-L424, L478-L500, L507-L525]
- [ ] <!-- ACT-ff0e03-3 --> **[utility · high · trivial]** `internal/domain/workflow/loop.go`: Remove dead code: `LoopType` is exported but unused `LoopType`, `LoopTypeForEach`, `LoopTypeWhile`, `String`, `DefaultMaxIterations`, `MaxAllowedIterations`, `LoopConfig`, `Validate`, `IsMaxIterationsDynamic`, `IterationResult`, `Duration`, `Success`, `LoopResult`, `NewLoopResult`, `Duration`, `WasBroken`, `AllSucceeded` (`LoopType, LoopTypeForEach, LoopTypeWhile, String, DefaultMaxIterations, MaxAllowedIterations, LoopConfig, Validate, IsMaxIterationsDynamic, IterationResult, Duration, Success, LoopResult, NewLoopResult, Duration, WasBroken, AllSucceeded`) [L9-L9, L12-L12, L13-L13, L16-L18, L21-L21, L24-L24, L27-L38, L41-L72, L75-L77, L80-L87, L90-L92, L95-L97, L100-L110, L113-L119, L122-L124, L127-L129, L132-L139]
- [ ] <!-- ACT-7218ad-1 --> **[utility · high · trivial]** `internal/domain/workflow/parallel.go`: Remove dead code: `ParallelStrategy` is exported but unused `ParallelStrategy`, `StrategyAllSucceed`, `StrategyAnySucceed`, `StrategyBestEffort`, `DefaultParallelStrategy`, `DefaultMaxConcurrent`, `ParseParallelStrategy`, `String`, `ParallelConfig`, `BranchResult`, `Duration`, `Success`, `ParallelResult`, `NewParallelResult`, `AddResult`, `Duration`, `AllSucceeded`, `AnySucceeded` (`ParallelStrategy, StrategyAllSucceed, StrategyAnySucceed, StrategyBestEffort, DefaultParallelStrategy, DefaultMaxConcurrent, ParseParallelStrategy, String, ParallelConfig, BranchResult, Duration, Success, ParallelResult, NewParallelResult, AddResult, Duration, AllSucceeded, AnySucceeded`) [L6-L6, L10-L10, L12-L12, L14-L14, L18-L18, L21-L21, L25-L36, L38-L40, L43-L46, L49-L57, L60-L62, L65-L67, L70-L77, L80-L85, L88-L98, L101-L103, L106-L108, L111-L113]
- [ ] <!-- ACT-8780e5-1 --> **[utility · high · trivial]** `internal/domain/workflow/step.go`: Remove dead code: `StepType` is exported but unused `StepType`, `StepTypeCommand`, `StepTypeParallel`, `StepTypeTerminal`, `StepTypeForEach`, `StepTypeWhile`, `StepTypeOperation`, `StepTypeCallWorkflow`, `StepTypeAgent`, `TerminalStatus`, `TerminalSuccess`, `TerminalFailure`, `String`, `RetryConfig`, `Validate`, `CaptureConfig`, `Step`, `Validate` (`StepType, StepTypeCommand, StepTypeParallel, StepTypeTerminal, StepTypeForEach, StepTypeWhile, StepTypeOperation, StepTypeCallWorkflow, StepTypeAgent, TerminalStatus, TerminalSuccess, TerminalFailure, String, RetryConfig, Validate, CaptureConfig, Step, Validate`) [L12-L12, L15-L15, L16-L16, L17-L17, L18-L18, L19-L19, L20-L20, L21-L21, L22-L22, L26-L26, L29-L29, L30-L30, L47-L49, L52-L60, L63-L81, L84-L89, L92-L123, L131-L248]
- [ ] <!-- ACT-8fd88e-1 --> **[utility · high · trivial]** `internal/infrastructure/logger/json_logger.go`: Remove dead code: `Level` is exported but unused `Level`, `LevelDebug`, `LevelInfo`, `LevelWarn`, `LevelError`, `String`, `ParseLevel`, `JSONLogger`, `NewJSONLogger`, `Debug`, `Info`, `Warn`, `Error`, `WithContext`, `Sync`, `Close` (`Level, LevelDebug, LevelInfo, LevelWarn, LevelError, String, ParseLevel, JSONLogger, NewJSONLogger, Debug, Info, Warn, Error, WithContext, Sync, Close`) [L15-L15, L18-L18, L19-L19, L20-L20, L21-L21, L24-L37, L55-L68, L71-L75, L78-L108, L110-L112, L114-L116, L118-L120, L122-L124, L126-L136, L139-L144, L147-L156]
- [ ] <!-- ACT-9cd1de-3 --> **[utility · high · trivial]** `internal/infrastructure/pluginmgr/state_store.go`: Remove dead code: `JSONPluginStateStore` is exported but unused `JSONPluginStateStore`, `NewJSONPluginStateStore`, `Save`, `Load`, `SetEnabled`, `IsEnabled`, `GetState`, `ListDisabled`, `BasePath`, `SetChecksum`, `GetChecksum`, `RemoveState` (`JSONPluginStateStore, NewJSONPluginStateStore, Save, Load, SetEnabled, IsEnabled, GetState, ListDisabled, BasePath, SetChecksum, GetChecksum, RemoveState`) [L20-L24, L27-L32, L35-L104, L107-L147, L150-L173, L177-L186, L222-L226, L229-L240, L243-L245, L287-L300, L304-L314, L317-L328]
- [ ] <!-- ACT-d73e13-5 --> **[utility · high · trivial]** `internal/interfaces/tui/bridge.go`: Remove dead code: `StreamBuffer` is exported but unused `StreamBuffer`, `Write`, `String`, `Len`, `Reset`, `WorkflowLister`, `HistoryProvider`, `Bridge`, `NewBridge`, `SetFacade`, `LoadWorkflows`, `LoadHistory`, `RunWorkflowViaFacade`, `ValidateWorkflow`, `MsgSender`, `TUIInputReader`, `NewTUIInputReader`, `SetSender`, `RequestCh`, `Respond` (`StreamBuffer, Write, String, Len, Reset, WorkflowLister, HistoryProvider, Bridge, NewBridge, SetFacade, LoadWorkflows, LoadHistory, RunWorkflowViaFacade, ValidateWorkflow, MsgSender, TUIInputReader, NewTUIInputReader, SetSender, RequestCh, Respond`) [L18-L21, L23-L31, L34-L38, L41-L45, L48-L52, L56-L60, L64-L67, L75-L85, L91-L97, L102-L104, L109-L135, L139-L157, L167-L205, L209-L221, L227-L227, L232-L236, L240-L246, L250-L252, L277-L279, L282-L284]
- [ ] <!-- ACT-8fa5ec-1 --> **[utility · high · trivial]** `internal/testutil/facadetest/facadetest.go`: Remove dead code: `Fake` is exported but unused `Fake`, `New`, `Script`, `WithTerminalCompleted`, `WithTerminalFailed`, `WithInputRequired`, `WithHistory`, `List`, `Validate`, `Status`, `History`, `Run`, `Resume`, `FakeSession`, `ID`, `Events`, `Respond`, `Err`, `StatusSnapshot`, `Close` (`Fake, New, Script, WithTerminalCompleted, WithTerminalFailed, WithInputRequired, WithHistory, List, Validate, Status, History, Run, Resume, FakeSession, ID, Events, Respond, Err, StatusSnapshot, Close`) [L17-L21, L24-L26, L29-L34, L37-L39, L45-L54, L58-L63, L66-L71, L74-L76, L79-L81, L84-L86, L89-L95, L100-L111, L121-L125, L130-L140, L191-L191, L195-L195, L199-L211, L214-L214, L219-L242, L246-L255]

## 🔧 Refactors

- [ ] <!-- ACT-9e239d-1 --> **[utility · medium · trivial]** `internal/application/acp_session_service.go`: Remove dead code: `InputSpec` is exported but unused `InputSpec`, `WorkflowSlashCommand`, `SessionUpdateEmitter`, `WorkflowProvider`, `ACPSessionService`, `PromptResult`, `MaxPromptBytes` (`InputSpec, WorkflowSlashCommand, SessionUpdateEmitter, WorkflowProvider, ACPSessionService, PromptResult, MaxPromptBytes`) [L49-L53, L57-L66, L72-L74, L87-L90, L94-L135, L1151-L1153, L1165-L1165]
- [ ] <!-- ACT-9cd1de-4 --> **[utility · medium · trivial]** `internal/infrastructure/pluginmgr/state_store.go`: Remove dead code: `GetConfig` is exported but unused `GetConfig`, `SetConfig`, `SetSourceData`, `GetSourceData` (`GetConfig, SetConfig, SetSourceData, GetSourceData`) [L190-L199, L202-L219, L253-L270, L274-L283]
- [ ] <!-- ACT-f033d8-1 --> **[utility · medium · trivial]** `internal/interfaces/api/types.go`: Remove dead code: `WorkflowSummary` is exported but unused `WorkflowSummary`, `ListWorkflowsOutput`, `GetWorkflowInput`, `GetWorkflowOutput`, `ValidateWorkflowInput`, `ValidateWorkflowOutput`, `RunWorkflowInput`, `RunWorkflowOutput`, `ListExecutionsOutput`, `GetExecutionInput`, `CancelExecutionInput`, `ResumeExecutionInput`, `ExecutionOutput`, `HistoryListInput`, `HistoryListOutput`, `HistoryStatsOutput` (`WorkflowSummary, ListWorkflowsOutput, GetWorkflowInput, GetWorkflowOutput, ValidateWorkflowInput, ValidateWorkflowOutput, RunWorkflowInput, RunWorkflowOutput, ListExecutionsOutput, GetExecutionInput, CancelExecutionInput, ResumeExecutionInput, ExecutionOutput, HistoryListInput, HistoryListOutput, HistoryStatsOutput`) [L17-L23, L29-L33, L37-L40, L42-L46, L50-L53, L59-L63, L67-L73, L103-L105, L113-L117, L121-L123, L127-L129, L133-L139, L152-L156, L169-L175, L181-L185, L187-L191]
- [ ] <!-- ACT-d73e13-6 --> **[utility · medium · trivial]** `internal/interfaces/tui/bridge.go`: Remove dead code: `ReadInput` is exported but unused (`ReadInput`) [L257-L273]

## 🧹 Hygiene

- [ ] <!-- ACT-9e239d-3 --> **[utility · low · trivial]** `internal/application/acp_session_service.go`: Consider removing low-value code: `ReadInput` (`ReadInput`) [L847-L850]
- [ ] <!-- ACT-9e239d-4 --> **[utility · low · trivial]** `internal/application/acp_session_service.go`: Consider removing low-value code: `SetParkHooks` (`SetParkHooks`) [L871-L874]
