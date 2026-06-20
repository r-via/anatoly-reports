[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 25

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/infrastructure/pluginmgr/grpc_step_type.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinfrastructurepluginmgrgrpcsteptypego) |
| `internal/infrastructure/pluginmgr/system.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinfrastructurepluginmgrsystemgo) |
| `internal/infrastructure/updater/replacer.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinfrastructureupdaterreplacergo) |
| `internal/interfaces/cli/config_cmd.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacescliconfigcmdgo) |
| `internal/interfaces/cli/run_facade_projector.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinterfacesclirunfacadeprojectorgo) |
| `internal/interfaces/cli/run.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacesclirungo) |
| `internal/interfaces/cli/serve.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacescliservego) |
| `internal/interfaces/cli/ui/agent_renderer.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacescliuiagentrenderergo) |
| `internal/interfaces/tui/model.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinterfacestuimodelgo) |
| `internal/interfaces/tui/tab_history.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinterfacestuitabhistorygo) |

## 🔍 Symbol Details

### `internal/infrastructure/pluginmgr/grpc_step_type.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `newGRPCStepTypeAdapter` | L29–L40 | 🔴 DEAD | 65% | Not called anywhere in this file. Likely called from another file in the pluginmgr package (e.g. plugin init path), but no local evidence. |
| `listAndCache` | L60–L81 | 🔴 DEAD | 65% | Not called anywhere in this file. Expected to be invoked after plugin Init() in another pluginmgr file, but no local evidence. |

### `internal/infrastructure/pluginmgr/system.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `SystemResult` | L12–L19 | 🔴 DEAD | 95% | Exported struct with 0 runtime and 0 type-only importers across the codebase. Only referenced as the return type of InitSystem within the same file, which is itself dead. |
| `InitSystem` | L26–L86 | 🔴 DEAD | 90% | Exported function with 0 runtime and 0 type-only importers. The primary entrypoint for plugin infrastructure initialization is never called by any external package. |

### `internal/infrastructure/updater/replacer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ReplaceBinary` | L16–L60 | 🔴 DEAD | 92% | Exported but imported by 0 files per exhaustive import analysis. |
| `IsPackageManagerPath` | L107–L128 | 🔴 DEAD | 95% | Exported but imported by 0 files per exhaustive import analysis. |

### `internal/interfaces/cli/config_cmd.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `newConfigCommand` | L15–L27 | 🔴 DEAD | 60% | Not called within this file. In Go, unexported functions can be used across the same package, so it is likely registered in another cli package file (e.g., root_cmd.go). Marked DEAD per file-only analysis rules, but confidence is low. |

### `internal/interfaces/cli/run_facade_projector.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `RenderFacadeEventsToText` | L20–L22 | 🔴 DEAD | 95% | 0 external importers per import analysis; not called anywhere within this file. Sole purpose is to delegate to RenderFacadeEventsToTextWithOptions, but nothing invokes it. |
| `RenderFacadeEventsToTextWithOptions` | L32–L81 | 🔴 DEAD | 80% | 0 external importers. Its only caller is RenderFacadeEventsToText (L21), which is itself dead. Transitively unreachable from any live code path. |

### `internal/interfaces/cli/run.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `newRunCommand` | L37–L119 | 🔴 DEAD | 65% | Not called within this file. Almost certainly called from root.go or a sibling cli file via AddCommand, but that file is not provided — flagging at reduced confidence per local-only check rule. |

### `internal/interfaces/cli/serve.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `NewServeCommand` | L26–L47 | 🔴 DEAD | 95% | Exported cobra command constructor with 0 runtime and 0 type-only importers per exhaustive import analysis. No file in the codebase wires this command into a root command. |

### `internal/interfaces/cli/ui/agent_renderer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `RenderEvents` | L15–L31 | 🔴 DEAD | 95% | Exported but 0 runtime or type-only importers found across the codebase. |

### `internal/interfaces/tui/model.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `executionState` | L37–L41 | 🔴 DEAD | 95% | Struct is defined but never instantiated or referenced anywhere in this file. Live execution data is tracked directly on MonitoringTab fields instead. |
| `NewWithBridge` | L79–L89 | 🔴 DEAD | 70% | Not called within this file and 0 cross-package importers per exhaustive analysis. Primary package entry point for production use — low but non-zero chance the analysis misses a main/cmd package caller. |

### `internal/interfaces/tui/tab_history.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `label` | L37–L46 | 🔴 DEAD | 85% | label() is never called anywhere in this file; renderListView uses inline string literals instead of delegating to label(). |
| `newHistoryTab` | L72–L114 | 🔴 DEAD | 65% | Not called within this file. Likely called by model.go in the same package, but per local-file analysis it has no confirmed callsite. |

## ⚡ Quick Wins

- [ ] <!-- ACT-8c0adf-1 --> **[utility · high · trivial]** `internal/infrastructure/pluginmgr/system.go`: Remove dead code: `SystemResult` is exported but unused (`SystemResult`) [L12-L19]
- [ ] <!-- ACT-8c0adf-2 --> **[utility · high · trivial]** `internal/infrastructure/pluginmgr/system.go`: Remove dead code: `InitSystem` is exported but unused (`InitSystem`) [L26-L86]
- [ ] <!-- ACT-81697b-1 --> **[utility · high · trivial]** `internal/infrastructure/updater/replacer.go`: Remove dead code: `ReplaceBinary` is exported but unused (`ReplaceBinary`) [L16-L60]
- [ ] <!-- ACT-81697b-2 --> **[utility · high · trivial]** `internal/infrastructure/updater/replacer.go`: Remove dead code: `IsPackageManagerPath` is exported but unused (`IsPackageManagerPath`) [L107-L128]
- [ ] <!-- ACT-80b7e0-1 --> **[utility · high · trivial]** `internal/interfaces/cli/run_facade_projector.go`: Remove dead code: `RenderFacadeEventsToText` is exported but unused (`RenderFacadeEventsToText`) [L20-L22]
- [ ] <!-- ACT-80b7e0-2 --> **[utility · high · trivial]** `internal/interfaces/cli/run_facade_projector.go`: Remove dead code: `RenderFacadeEventsToTextWithOptions` is exported but unused (`RenderFacadeEventsToTextWithOptions`) [L32-L81]
- [ ] <!-- ACT-c74555-2 --> **[utility · high · trivial]** `internal/interfaces/cli/serve.go`: Remove dead code: `NewServeCommand` is exported but unused (`NewServeCommand`) [L26-L47]
- [ ] <!-- ACT-0c7b16-2 --> **[utility · high · trivial]** `internal/interfaces/cli/ui/agent_renderer.go`: Remove dead code: `RenderEvents` is exported but unused (`RenderEvents`) [L15-L31]
- [ ] <!-- ACT-469dff-1 --> **[utility · high · trivial]** `internal/interfaces/tui/model.go`: Remove dead code: `executionState` is exported but unused (`executionState`) [L37-L41]
- [ ] <!-- ACT-98873b-1 --> **[utility · high · trivial]** `internal/interfaces/tui/tab_history.go`: Remove dead code: `label` is exported but unused (`label`) [L37-L46]

## 🔧 Refactors

- [ ] <!-- ACT-194ca8-1 --> **[utility · medium · trivial]** `internal/infrastructure/pluginmgr/grpc_step_type.go`: Remove dead code: `newGRPCStepTypeAdapter` is exported but unused (`newGRPCStepTypeAdapter`) [L29-L40]
- [ ] <!-- ACT-194ca8-2 --> **[utility · medium · trivial]** `internal/infrastructure/pluginmgr/grpc_step_type.go`: Remove dead code: `listAndCache` is exported but unused (`listAndCache`) [L60-L81]
- [ ] <!-- ACT-f3499a-2 --> **[utility · medium · trivial]** `internal/interfaces/cli/config_cmd.go`: Remove dead code: `newConfigCommand` is exported but unused (`newConfigCommand`) [L15-L27]
- [ ] <!-- ACT-273972-3 --> **[utility · medium · trivial]** `internal/interfaces/cli/run.go`: Remove dead code: `newRunCommand` is exported but unused (`newRunCommand`) [L37-L119]
- [ ] <!-- ACT-469dff-2 --> **[utility · medium · trivial]** `internal/interfaces/tui/model.go`: Remove dead code: `NewWithBridge` is exported but unused (`NewWithBridge`) [L79-L89]
- [ ] <!-- ACT-98873b-2 --> **[utility · medium · trivial]** `internal/interfaces/tui/tab_history.go`: Remove dead code: `newHistoryTab` is exported but unused (`newHistoryTab`) [L72-L114]
