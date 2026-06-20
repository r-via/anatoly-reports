[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 24

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/application/drain.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalapplicationdraingo) |
| `internal/application/execution_tool_proxy.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalapplicationexecutiontoolproxygo) |
| `internal/application/external_file.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalapplicationexternalfilego) |
| `internal/application/single_step.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalapplicationsinglestepgo) |
| `internal/domain/errors/hints.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internaldomainerrorshintsgo) |
| `internal/domain/pluginmodel/event.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internaldomainpluginmodeleventgo) |
| `internal/domain/pluginmodel/state.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internaldomainpluginmodelstatego) |
| `internal/infrastructure/diagram/graphviz.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinfrastructurediagramgraphvizgo) |
| `internal/infrastructure/otel/factory.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinfrastructureotelfactorygo) |
| `internal/infrastructure/pluginmgr/grpc_event.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinfrastructurepluginmgrgrpceventgo) |

## 🔍 Symbol Details

### `internal/application/drain.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Drain` | L16–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DrainContext` | L31–L45 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/application/execution_tool_proxy.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `startToolProxy` | L30–L42 | 🔴 DEAD | 65% | Not called anywhere in this file. As an unexported method on ExecutionService it is likely invoked from another file in the application package (e.g., during provider.Execute), but that usage is not visible here. Confidence reduced accordingly. |
| `startConversationToolProxy` | L47–L59 | 🔴 DEAD | 65% | Not called anywhere in this file. Comment identifies it as the conversation-manager counterpart, implying a caller in another application-package file, but no in-file reference exists to confirm this. |

### `internal/application/external_file.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `resolveCommandAWFPaths` | L155–L193 | 🔴 DEAD | 65% | Not called anywhere in this file and not exported. No sibling file is visible in the provided context, so no caller can be confirmed. Confidence reduced because other application-package files could invoke it. |

### `internal/application/single_step.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `SingleStepResult` | L17–L26 | 🔴 DEAD | 95% | Exported struct with 0 external importers. Used only within this file as the return type of ExecuteSingleStep, which is itself dead. |
| `ExecuteSingleStep` | L32–L168 | 🔴 DEAD | 90% | Exported method with 0 runtime and 0 type-only importers across the codebase. |

### `internal/domain/errors/hints.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Hint` | L22–L26 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `HintGenerator` | L49–L49 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/pluginmodel/event.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `DomainEvent` | L9–L17 | 🔴 DEAD | 95% | Exported struct with 0 importers. Not referenced outside this file. |
| `NewDomainEvent` | L27–L36 | 🔴 DEAD | 95% | Exported constructor with 0 importers. Not referenced outside this file. |

### `internal/domain/pluginmodel/state.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `PluginState` | L3–L10 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewPluginState` | L12–L17 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/diagram/graphviz.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `CheckGraphviz` | L14–L17 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Export` | L27–L62 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/otel/factory.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `TracerConfig` | L13–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewTracerFromConfig` | L21–L61 | 🔴 DEAD | 85% | Exported but imported by 0 files |

### `internal/infrastructure/pluginmgr/grpc_event.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `newGRPCEventAdapter` | L21–L26 | 🔴 DEAD | 85% | Not exported and not called anywhere in this file. May be called elsewhere in the package, but no evidence exists in the provided context. |
| `domainEventToStreamMessage` | L52–L63 | 🔴 DEAD | 85% | Not exported and not called anywhere in this file. Likely intended for a streaming path not yet implemented or wired in this package. |

## ⚡ Quick Wins

- [ ] <!-- ACT-58aa35-1 --> **[utility · high · trivial]** `internal/application/drain.go`: Remove dead code: `Drain` is exported but unused (`Drain`) [L16-L20]
- [ ] <!-- ACT-58aa35-2 --> **[utility · high · trivial]** `internal/application/drain.go`: Remove dead code: `DrainContext` is exported but unused (`DrainContext`) [L31-L45]
- [ ] <!-- ACT-141f2c-3 --> **[utility · high · trivial]** `internal/application/single_step.go`: Remove dead code: `SingleStepResult` is exported but unused (`SingleStepResult`) [L17-L26]
- [ ] <!-- ACT-141f2c-4 --> **[utility · high · trivial]** `internal/application/single_step.go`: Remove dead code: `ExecuteSingleStep` is exported but unused (`ExecuteSingleStep`) [L32-L168]
- [ ] <!-- ACT-492ecf-1 --> **[utility · high · trivial]** `internal/domain/errors/hints.go`: Remove dead code: `Hint` is exported but unused (`Hint`) [L22-L26]
- [ ] <!-- ACT-492ecf-2 --> **[utility · high · trivial]** `internal/domain/errors/hints.go`: Remove dead code: `HintGenerator` is exported but unused (`HintGenerator`) [L49-L49]
- [ ] <!-- ACT-0311bf-1 --> **[utility · high · trivial]** `internal/domain/pluginmodel/event.go`: Remove dead code: `DomainEvent` is exported but unused (`DomainEvent`) [L9-L17]
- [ ] <!-- ACT-0311bf-2 --> **[utility · high · trivial]** `internal/domain/pluginmodel/event.go`: Remove dead code: `NewDomainEvent` is exported but unused (`NewDomainEvent`) [L27-L36]
- [ ] <!-- ACT-6d21d5-1 --> **[utility · high · trivial]** `internal/domain/pluginmodel/state.go`: Remove dead code: `PluginState` is exported but unused (`PluginState`) [L3-L10]
- [ ] <!-- ACT-6d21d5-2 --> **[utility · high · trivial]** `internal/domain/pluginmodel/state.go`: Remove dead code: `NewPluginState` is exported but unused (`NewPluginState`) [L12-L17]
- [ ] <!-- ACT-6fe950-1 --> **[utility · high · trivial]** `internal/infrastructure/diagram/graphviz.go`: Remove dead code: `CheckGraphviz` is exported but unused (`CheckGraphviz`) [L14-L17]
- [ ] <!-- ACT-6fe950-2 --> **[utility · high · trivial]** `internal/infrastructure/diagram/graphviz.go`: Remove dead code: `Export` is exported but unused (`Export`) [L27-L62]
- [ ] <!-- ACT-3418e4-2 --> **[utility · high · trivial]** `internal/infrastructure/otel/factory.go`: Remove dead code: `TracerConfig` is exported but unused (`TracerConfig`) [L13-L16]
- [ ] <!-- ACT-3418e4-3 --> **[utility · high · trivial]** `internal/infrastructure/otel/factory.go`: Remove dead code: `NewTracerFromConfig` is exported but unused (`NewTracerFromConfig`) [L21-L61]
- [ ] <!-- ACT-d439b6-1 --> **[utility · high · trivial]** `internal/infrastructure/pluginmgr/grpc_event.go`: Remove dead code: `newGRPCEventAdapter` is exported but unused (`newGRPCEventAdapter`) [L21-L26]
- [ ] <!-- ACT-d439b6-2 --> **[utility · high · trivial]** `internal/infrastructure/pluginmgr/grpc_event.go`: Remove dead code: `domainEventToStreamMessage` is exported but unused (`domainEventToStreamMessage`) [L52-L63]

## 🔧 Refactors

- [ ] <!-- ACT-0b60da-1 --> **[utility · medium · trivial]** `internal/application/execution_tool_proxy.go`: Remove dead code: `startToolProxy` is exported but unused (`startToolProxy`) [L30-L42]
- [ ] <!-- ACT-0b60da-2 --> **[utility · medium · trivial]** `internal/application/execution_tool_proxy.go`: Remove dead code: `startConversationToolProxy` is exported but unused (`startConversationToolProxy`) [L47-L59]
- [ ] <!-- ACT-57ae4f-2 --> **[utility · medium · trivial]** `internal/application/external_file.go`: Remove dead code: `resolveCommandAWFPaths` is exported but unused (`resolveCommandAWFPaths`) [L155-L193]
