[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 9

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/application/tools/router.go` | 🟡 NEEDS_REFACTOR | 8 | 95% | [details](#internalapplicationtoolsroutergo) |
| `internal/domain/workflow/runtime_context.go` | 🟡 NEEDS_REFACTOR | 8 | 95% | [details](#internaldomainworkflowruntimecontextgo) |
| `internal/infrastructure/agents/mistral_vibe_provider.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinfrastructureagentsmistralvibeprovidergo) |
| `internal/infrastructure/github/provider.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internalinfrastructuregithubprovidergo) |
| `internal/infrastructure/pluginmgr/event_bus.go` | 🟡 NEEDS_REFACTOR | 8 | 95% | [details](#internalinfrastructurepluginmgreventbusgo) |
| `internal/infrastructure/pluginmgr/installer.go` | 🟡 NEEDS_REFACTOR | 8 | 95% | [details](#internalinfrastructurepluginmgrinstallergo) |
| `internal/interfaces/api/bridge.go` | 🟡 NEEDS_REFACTOR | 8 | 95% | [details](#internalinterfacesapibridgego) |
| `internal/interfaces/cli/ui/output_writer.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinterfacescliuioutputwritergo) |
| `internal/interfaces/tui/tailer.go` | 🟡 NEEDS_REFACTOR | 8 | 95% | [details](#internalinterfacestuitailergo) |
| `pkg/retry/retryer.go` | 🟡 NEEDS_REFACTOR | 8 | 95% | [details](#pkgretryretryergo) |

## 🔍 Symbol Details

### `internal/application/tools/router.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Router` | L25–L34 | 🔴 DEAD | 95% | 0 runtime and type-only importers. The compile-time interface check at L16 is a local assertion only; no external file instantiates or references this type. |
| `SetRecorder` | L37–L39 | 🔴 DEAD | 95% | 0 importers. Not part of the ports.ToolRouter interface (only the concrete *Router type exposes it), and never called externally. |
| `SetRunID` | L43–L45 | 🔴 DEAD | 95% | 0 importers. Concrete-type-only method not included in ports.ToolRouter; no external caller exists. |
| `NewRouter` | L47–L53 | 🔴 DEAD | 95% | 0 importers. Constructor for Router is never called outside this package. |
| `Register` | L58–L84 | 🔴 DEAD | 92% | 0 importers. Router itself is dead, so all its methods are unreachable. |
| `ListTools` | L86–L93 | 🔴 DEAD | 95% | 0 importers. Implements ports.ToolRouter but Router is never instantiated externally, making this unreachable. |
| `CallTool` | L95–L169 | 🔴 DEAD | 95% | 0 importers. Core dispatch method is unreachable because no external code holds a *Router or ports.ToolRouter backed by this type. |
| `Close` | L171–L183 | 🔴 DEAD | 92% | 0 importers. Cleanup method is unreachable for the same reason as all other Router methods. |

### `internal/domain/workflow/runtime_context.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `RuntimeContext` | L7–L15 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RuntimeStepState` | L18–L23 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RuntimeWorkflowData` | L26–L31 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Duration` | L34–L36 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RuntimeContextData` | L39–L43 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RuntimeErrorData` | L46–L51 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RuntimeLoopData` | L54–L61 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Index1` | L64–L66 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/agents/mistral_vibe_provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `MistralVibeProvider` | L25–L30 | 🔴 DEAD | 95% | 0 external importers. No file outside this package references the type, making the entire provider unreachable from any call graph entry point. |
| `NewMistralVibeProvider` | L32–L34 | 🔴 DEAD | 95% | 0 external importers. No file instantiates MistralVibeProvider through this constructor. |
| `NewMistralVibeProviderWithOptions` | L36–L46 | 🔴 DEAD | 95% | 0 external importers. Only caller is NewMistralVibeProvider, which is itself dead. |
| `Execute` | L63–L83 | 🔴 DEAD | 95% | 0 external importers. Satisfies ports.AgentProvider interface but the type itself has no external importers, so the method is unreachable. |
| `ExecuteConversation` | L85–L110 | 🔴 DEAD | 95% | 0 external importers. Interface method unreachable because MistralVibeProvider has no external importers. |
| `Name` | L112–L114 | 🔴 DEAD | 95% | 0 external importers. Interface method on a type with no external consumers. |
| `Validate` | L116–L122 | 🔴 DEAD | 95% | 0 external importers. Interface method on a type with no external consumers. |

### `internal/infrastructure/github/provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `GHRunner` | L16–L18 | 🔴 DEAD | 95% | Exported interface with 0 runtime and 0 type-only importers from other packages. Referenced only within this package as a field/parameter type. |
| `GitHubOperationProvider` | L26–L32 | 🔴 DEAD | 95% | Exported struct with 0 importers. Used only as a receiver type within this package; no external consumer instantiates or references it. |
| `NewGitHubOperationProvider` | L34–L47 | 🔴 DEAD | 95% | Exported constructor with 0 importers. No external package calls this to obtain a GitHubOperationProvider. |
| `GetOperation` | L49–L52 | 🔴 DEAD | 95% | Exported method with 0 importers. Not called within this file or from any other package. |
| `ListOperations` | L54–L60 | 🔴 DEAD | 95% | Exported method with 0 importers. Not called within this file or from any other package. |
| `Execute` | L66–L95 | 🔴 DEAD | 95% | Exported method with 0 importers. No external caller dispatches operations through this entry point. |

### `internal/infrastructure/pluginmgr/event_bus.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `EventDeliverer` | L18–L20 | 🔴 DEAD | 95% | 0 external importers. No other package implements or references this interface; Subscribe (its only consumer) is itself dead. |
| `EventBus` | L36–L41 | 🔴 DEAD | 95% | 0 external importers. No other package instantiates or references this type. |
| `NewEventBus` | L46–L48 | 🔴 DEAD | 95% | 0 external importers. No other package calls this constructor. |
| `NewEventBusWithBufferSize` | L51–L57 | 🔴 DEAD | 95% | 0 external importers. Called only by NewEventBus (L47) within the same file, which is itself dead. |
| `Subscribe` | L92–L107 | 🔴 DEAD | 92% | 0 external importers. No other package registers a plugin subscriber. |
| `Unsubscribe` | L141–L153 | 🔴 DEAD | 95% | 0 external importers. Only called internally by Close (L186), which is itself dead. |
| `Publish` | L155–L175 | 🔴 DEAD | 95% | 0 external importers. Also called internally by runDelivery for event re-propagation, but the entire EventBus is unreachable from outside the package. |
| `Close` | L177–L189 | 🔴 DEAD | 90% | 0 external importers. No other package calls this teardown method. |

### `internal/infrastructure/pluginmgr/installer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `PluginSource` | L18–L23 | 🔴 DEAD | 95% | Exported struct with 0 external importers. Locally referenced only as parameter/return type in SourceDataFromPluginSource and PluginSourceFromSourceData, which are themselves dead. |
| `PluginInstaller` | L27–L29 | 🔴 DEAD | 95% | Exported struct with 0 external importers. Used only as a receiver type for methods within this file; no external consumer creates or uses this type. |
| `NewPluginInstaller` | L33–L40 | 🔴 DEAD | 95% | Exported constructor with 0 external importers and no local callers within the file. |
| `AtomicInstall` | L46–L61 | 🔴 DEAD | 90% | Exported method with 0 external importers. Called only by Install (L163), which is itself dead. |
| `ValidateManifest` | L66–L75 | 🔴 DEAD | 95% | Exported method with 0 external importers. Called only by Install (L152), which is itself dead. |
| `Install` | L121–L164 | 🔴 DEAD | 92% | Exported method with 0 external importers and no local callers within the file. |
| `SourceDataFromPluginSource` | L182–L188 | 🔴 DEAD | 95% | Exported function with 0 external importers and no local callers within the file. |
| `PluginSourceFromSourceData` | L191–L200 | 🔴 DEAD | 95% | Exported function with 0 external importers and no local callers within the file. |

### `internal/interfaces/api/bridge.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ActiveExecution` | L14–L24 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Bridge` | L33–L35 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewBridge` | L40–L42 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetExecution` | L46–L52 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `CancelExecution` | L56–L63 | 🔴 DEAD | 82% | Exported but imported by 0 files |
| `ListExecutions` | L67–L74 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `TrackFacadeSession` | L90–L111 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `Shutdown` | L117–L123 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/interfaces/cli/ui/output_writer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `PrefixType` | L10–L10 | 🔴 DEAD | 95% | 0 external importers. Used only as a local field type, but the entire package is unreferenced externally. |
| `PrefixStdout` | L13–L13 | 🔴 DEAD | 95% | 0 external importers. Referenced only in local buildPrefix comparisons within an otherwise-dead package. |
| `PrefixStderr` | L14–L14 | 🔴 DEAD | 95% | 0 external importers and never directly referenced locally — only implied by else branches in buildPrefix. |
| `PrefixedWriter` | L18–L25 | 🔴 DEAD | 95% | 0 external importers. No file outside this package constructs or references this type. |
| `NewPrefixedWriter` | L28–L35 | 🔴 DEAD | 95% | 0 external importers. Constructor for a dead type; no call sites exist outside this file. |
| `Write` | L38–L60 | 🔴 DEAD | 92% | 0 external importers. Implements io.Writer but PrefixedWriter is never passed as io.Writer by any external caller. |
| `Flush` | L63–L72 | 🔴 DEAD | 85% | 0 external importers. No caller outside this package invokes Flush. |

### `internal/interfaces/tui/tailer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `LogEntry` | L18–L28 | 🔴 DEAD | 95% | Exported type with 0 external importers. Package tui has no consumers; the struct is used only within this file. |
| `LogLineMsg` | L31–L33 | 🔴 DEAD | 95% | Exported type with 0 external importers and no internal references in this file — never instantiated or returned anywhere. |
| `LogBatchMsg` | L36–L38 | 🔴 DEAD | 95% | Exported type with 0 external importers. Returned by Tail/Follow internally, but those methods are also dead externally. |
| `Tailer` | L49–L53 | 🔴 DEAD | 95% | Exported struct with 0 external importers; NewTailer (the constructor) is also dead, so the type is never instantiated outside this file. |
| `NewTailer` | L56–L58 | 🔴 DEAD | 95% | Exported constructor with 0 external importers and no internal call sites — the only entry point into Tailer is never reached. |
| `Tail` | L99–L124 | 🔴 DEAD | 92% | Exported method with 0 external importers. Called only from Next (L172), which is itself dead externally. |
| `Follow` | L128–L166 | 🔴 DEAD | 88% | Exported method with 0 external importers. Called only from Next (L173), which is itself dead externally. |
| `Next` | L169–L174 | 🔴 DEAD | 95% | Exported method with 0 external importers and no internal call sites — the intended public entry point is never invoked. |

### `pkg/retry/retryer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Config` | L11–L19 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Logger` | L22–L25 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Retryer` | L28–L32 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewRetryer` | L35–L41 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ShouldRetry` | L44–L54 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `NextDelay` | L57–L67 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `Wait` | L70–L90 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `IsRetryableExitCode` | L93–L109 | 🔴 DEAD | 95% | Exported but imported by 0 files |

## ⚡ Quick Wins

- [ ] <!-- ACT-c357ae-3 --> **[utility · high · trivial]** `internal/application/tools/router.go`: Remove dead code: `Router` is exported but unused `Router`, `SetRecorder`, `SetRunID`, `NewRouter`, `Register`, `ListTools`, `CallTool`, `Close` (`Router, SetRecorder, SetRunID, NewRouter, Register, ListTools, CallTool, Close`) [L25-L34, L37-L39, L43-L45, L47-L53, L58-L84, L86-L93, L95-L169, L171-L183]
- [ ] <!-- ACT-30384c-1 --> **[utility · high · trivial]** `internal/domain/workflow/runtime_context.go`: Remove dead code: `RuntimeContext` is exported but unused `RuntimeContext`, `RuntimeStepState`, `RuntimeWorkflowData`, `Duration`, `RuntimeContextData`, `RuntimeErrorData`, `RuntimeLoopData`, `Index1` (`RuntimeContext, RuntimeStepState, RuntimeWorkflowData, Duration, RuntimeContextData, RuntimeErrorData, RuntimeLoopData, Index1`) [L7-L15, L18-L23, L26-L31, L34-L36, L39-L43, L46-L51, L54-L61, L64-L66]
- [ ] <!-- ACT-fc9527-2 --> **[utility · high · trivial]** `internal/infrastructure/agents/mistral_vibe_provider.go`: Remove dead code: `MistralVibeProvider` is exported but unused `MistralVibeProvider`, `NewMistralVibeProvider`, `NewMistralVibeProviderWithOptions`, `Execute`, `ExecuteConversation`, `Name`, `Validate` (`MistralVibeProvider, NewMistralVibeProvider, NewMistralVibeProviderWithOptions, Execute, ExecuteConversation, Name, Validate`) [L25-L30, L32-L34, L36-L46, L63-L83, L85-L110, L112-L114, L116-L122]
- [ ] <!-- ACT-bdc0af-3 --> **[utility · high · trivial]** `internal/infrastructure/github/provider.go`: Remove dead code: `GHRunner` is exported but unused `GHRunner`, `GitHubOperationProvider`, `NewGitHubOperationProvider`, `GetOperation`, `ListOperations`, `Execute` (`GHRunner, GitHubOperationProvider, NewGitHubOperationProvider, GetOperation, ListOperations, Execute`) [L16-L18, L26-L32, L34-L47, L49-L52, L54-L60, L66-L95]
- [ ] <!-- ACT-e77ff3-2 --> **[utility · high · trivial]** `internal/infrastructure/pluginmgr/event_bus.go`: Remove dead code: `EventDeliverer` is exported but unused `EventDeliverer`, `EventBus`, `NewEventBus`, `NewEventBusWithBufferSize`, `Subscribe`, `Unsubscribe`, `Publish`, `Close` (`EventDeliverer, EventBus, NewEventBus, NewEventBusWithBufferSize, Subscribe, Unsubscribe, Publish, Close`) [L18-L20, L36-L41, L46-L48, L51-L57, L92-L107, L141-L153, L155-L175, L177-L189]
- [ ] <!-- ACT-8c8e6a-1 --> **[utility · high · trivial]** `internal/infrastructure/pluginmgr/installer.go`: Remove dead code: `PluginSource` is exported but unused `PluginSource`, `PluginInstaller`, `NewPluginInstaller`, `AtomicInstall`, `ValidateManifest`, `Install`, `SourceDataFromPluginSource`, `PluginSourceFromSourceData` (`PluginSource, PluginInstaller, NewPluginInstaller, AtomicInstall, ValidateManifest, Install, SourceDataFromPluginSource, PluginSourceFromSourceData`) [L18-L23, L27-L29, L33-L40, L46-L61, L66-L75, L121-L164, L182-L188, L191-L200]
- [ ] <!-- ACT-54aad8-2 --> **[utility · high · trivial]** `internal/interfaces/api/bridge.go`: Remove dead code: `ActiveExecution` is exported but unused `ActiveExecution`, `Bridge`, `NewBridge`, `GetExecution`, `CancelExecution`, `ListExecutions`, `TrackFacadeSession`, `Shutdown` (`ActiveExecution, Bridge, NewBridge, GetExecution, CancelExecution, ListExecutions, TrackFacadeSession, Shutdown`) [L14-L24, L33-L35, L40-L42, L46-L52, L56-L63, L67-L74, L90-L111, L117-L123]
- [ ] <!-- ACT-a21636-2 --> **[utility · high · trivial]** `internal/interfaces/cli/ui/output_writer.go`: Remove dead code: `PrefixType` is exported but unused `PrefixType`, `PrefixStdout`, `PrefixStderr`, `PrefixedWriter`, `NewPrefixedWriter`, `Write`, `Flush` (`PrefixType, PrefixStdout, PrefixStderr, PrefixedWriter, NewPrefixedWriter, Write, Flush`) [L10-L10, L13-L13, L14-L14, L18-L25, L28-L35, L38-L60, L63-L72]
- [ ] <!-- ACT-571ba1-2 --> **[utility · high · trivial]** `internal/interfaces/tui/tailer.go`: Remove dead code: `LogEntry` is exported but unused `LogEntry`, `LogLineMsg`, `LogBatchMsg`, `Tailer`, `NewTailer`, `Tail`, `Follow`, `Next` (`LogEntry, LogLineMsg, LogBatchMsg, Tailer, NewTailer, Tail, Follow, Next`) [L18-L28, L31-L33, L36-L38, L49-L53, L56-L58, L99-L124, L128-L166, L169-L174]
- [ ] <!-- ACT-e84021-1 --> **[utility · high · trivial]** `pkg/retry/retryer.go`: Remove dead code: `Config` is exported but unused `Config`, `Logger`, `Retryer`, `NewRetryer`, `ShouldRetry`, `NextDelay`, `Wait`, `IsRetryableExitCode` (`Config, Logger, Retryer, NewRetryer, ShouldRetry, NextDelay, Wait, IsRetryableExitCode`) [L11-L19, L22-L25, L28-L32, L35-L41, L44-L54, L57-L67, L70-L90, L93-L109]
