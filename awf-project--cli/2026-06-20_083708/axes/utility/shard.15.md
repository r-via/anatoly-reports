[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 15

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/infrastructure/agents/base_cli_provider.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinfrastructureagentsbasecliprovidergo) |
| `internal/infrastructure/executor/shell_executor.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinfrastructureexecutorshellexecutorgo) |
| `internal/infrastructure/logger/masker.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinfrastructureloggermaskergo) |
| `internal/infrastructure/mcp/server.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinfrastructuremcpservergo) |
| `internal/infrastructure/pluginmgr/composite.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinfrastructurepluginmgrcompositego) |
| `internal/infrastructure/repository/source.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinfrastructurerepositorysourcego) |
| `internal/infrastructure/repository/template_repository.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinfrastructurerepositorytemplaterepositorygo) |
| `internal/infrastructure/tools/plugin_adapter.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinfrastructuretoolspluginadaptergo) |
| `internal/infrastructure/transcript/nop_recorder.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinfrastructuretranscriptnoprecordergo) |
| `internal/infrastructure/workflowpkg/installer.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinfrastructureworkflowpkginstallergo) |

## 🔍 Symbol Details

### `internal/infrastructure/agents/base_cli_provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `mcpServeCommand` | L48–L50 | 🔴 DEAD | 72% | Not called anywhere in this file. May be used in sibling package files, but local-file analysis finds zero references. |
| `noopMCPCleanup` | L99–L99 | 🔴 DEAD | 72% | Only appears in a doc comment (L122); inline lambdas are used instead in execute and executeConversation. No code call site in this file. |
| `newBaseCLIProvider` | L140–L152 | 🔴 DEAD | 62% | Not called in this file. Clearly the intended constructor for use by sibling provider files in the agents package, but local-file analysis finds no call site. |
| `execute` | L196–L289 | 🔴 DEAD | 62% | Not called within this file. Core execution method intended for sibling provider files; local-file analysis finds no call site. |
| `executeConversation` | L295–L436 | 🔴 DEAD | 62% | Not called within this file. Core conversation method intended for sibling provider files; local-file analysis finds no call site. |

### `internal/infrastructure/executor/shell_executor.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ShellExecutorOption` | L21–L21 | 🔴 DEAD | 95% | Exported type alias with 0 importers outside this package. Used only within this file to type WithShell and NewShellExecutor, both of which are also dead. |
| `WithShell` | L24–L28 | 🔴 DEAD | 95% | Exported functional option with 0 importers. No external caller passes a custom shell path. |
| `ShellExecutor` | L30–L33 | 🔴 DEAD | 95% | Exported struct with 0 importers. Entire type is unreachable from outside the package. |
| `NewShellExecutor` | L36–L45 | 🔴 DEAD | 95% | Exported constructor with 0 importers. No external code instantiates ShellExecutor. |
| `Execute` | L61–L144 | 🔴 DEAD | 92% | Exported method with 0 importers. Implements ports.Command execution but no external caller invokes it. |

### `internal/infrastructure/logger/masker.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `SecretMasker` | L6–L8 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewSecretMasker` | L19–L24 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `IsSecretKey` | L27–L35 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `MaskFields` | L39–L58 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `MaskText` | L61–L105 | 🔴 DEAD | 88% | Exported but imported by 0 files |

### `internal/infrastructure/mcp/server.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Server` | L16–L19 | 🔴 DEAD | 95% | Exported struct with 0 runtime and 0 type-only importers per exhaustive import analysis. |
| `New` | L22–L31 | 🔴 DEAD | 95% | Exported constructor with 0 importers; no external callers instantiate Server. |
| `RegisterProvider` | L40–L66 | 🔴 DEAD | 95% | Exported method with 0 importers; no external caller registers tools on the server. |
| `ServeStdio` | L69–L71 | 🔴 DEAD | 95% | Exported method with 0 importers; the MCP serve command entry point is never wired up. |
| `ServeIO` | L74–L76 | 🔴 DEAD | 95% | Exported test-oriented method with 0 importers including test files per exhaustive analysis. |

### `internal/infrastructure/pluginmgr/composite.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `CompositeOperationProvider` | L14–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewCompositeOperationProvider` | L20–L24 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetOperation` | L27–L37 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ListOperations` | L40–L50 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Execute` | L53–L64 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/repository/source.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Source` | L4–L4 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SourceEnv` | L7–L7 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SourceLocal` | L8–L8 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SourceGlobal` | L9–L9 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `String` | L12–L23 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/repository/template_repository.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `YAMLTemplateRepository` | L18–L22 | 🔴 DEAD | 95% | 0 runtime and 0 type-only importers per exhaustive import analysis. |
| `NewYAMLTemplateRepository` | L25–L30 | 🔴 DEAD | 95% | 0 runtime and 0 type-only importers; no external caller constructs this repository. |
| `GetTemplate` | L33–L59 | 🔴 DEAD | 90% | 0 runtime and 0 type-only importers; method is never called externally. |
| `ListTemplates` | L62–L83 | 🔴 DEAD | 95% | 0 runtime and 0 type-only importers; method is never called externally. |
| `Exists` | L86–L94 | 🔴 DEAD | 95% | 0 runtime and 0 type-only importers; method is never called externally. |

### `internal/infrastructure/tools/plugin_adapter.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `PluginToolAdapter` | L26–L30 | 🔴 DEAD | 85% | 0 external importers. Only locally referenced in the compile-time interface assertion (L12) and as method receivers. The type is never instantiated or used outside this package. |
| `NewPluginToolAdapter` | L36–L67 | 🔴 DEAD | 85% | 0 external importers. Constructor for PluginToolAdapter, which is itself externally unused. No callers exist outside this package. |
| `ListTools` | L69–L82 | 🔴 DEAD | 80% | 0 external importers. Required only to satisfy ports.ToolProvider for the compile-time assertion at L12, which is itself dead since PluginToolAdapter has no external consumers. |
| `CallTool` | L84–L115 | 🔴 DEAD | 78% | 0 external importers. Required only to satisfy ports.ToolProvider for the compile-time assertion at L12, which is itself dead since PluginToolAdapter has no external consumers. |
| `Close` | L117–L119 | 🔴 DEAD | 80% | 0 external importers. Trivial no-op that also exists solely to satisfy ports.ToolProvider for the dead compile-time assertion. |

### `internal/infrastructure/transcript/nop_recorder.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `NopRecorder` | L14–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewNopRecorder` | L19–L23 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Record` | L27–L29 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Subscribe` | L31–L33 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Close` | L35–L35 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/workflowpkg/installer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ManifestFileName` | L17–L17 | 🔴 DEAD | 80% | 0 external importers. Referenced only at L117 inside Install, which itself has 0 callers — transitively dead. |
| `MaxManifestSize` | L20–L20 | 🔴 DEAD | 80% | 0 external importers. Referenced only at L118 inside Install, which itself has 0 callers — transitively dead. |
| `PackInstaller` | L47–L50 | 🔴 DEAD | 90% | 0 external importers. Referenced only as the receiver type and return type within this file; no external consumer constructs or uses it. |
| `NewPackInstaller` | L53–L64 | 🔴 DEAD | 95% | 0 external importers, not called anywhere in this file. Entry point for PackInstaller construction is unused. |
| `Install` | L73–L178 | 🔴 DEAD | 92% | 0 external importers, not called within this file. Core installation logic is entirely unreachable. |

## ⚡ Quick Wins

- [ ] <!-- ACT-f7dafb-1 --> **[utility · high · trivial]** `internal/infrastructure/executor/shell_executor.go`: Remove dead code: `ShellExecutorOption` is exported but unused `ShellExecutorOption`, `WithShell`, `ShellExecutor`, `NewShellExecutor`, `Execute` (`ShellExecutorOption, WithShell, ShellExecutor, NewShellExecutor, Execute`) [L21-L21, L24-L28, L30-L33, L36-L45, L61-L144]
- [ ] <!-- ACT-3b7780-2 --> **[utility · high · trivial]** `internal/infrastructure/logger/masker.go`: Remove dead code: `SecretMasker` is exported but unused `SecretMasker`, `NewSecretMasker`, `IsSecretKey`, `MaskFields`, `MaskText` (`SecretMasker, NewSecretMasker, IsSecretKey, MaskFields, MaskText`) [L6-L8, L19-L24, L27-L35, L39-L58, L61-L105]
- [ ] <!-- ACT-71d0f7-1 --> **[utility · high · trivial]** `internal/infrastructure/mcp/server.go`: Remove dead code: `Server` is exported but unused `Server`, `New`, `RegisterProvider`, `ServeStdio`, `ServeIO` (`Server, New, RegisterProvider, ServeStdio, ServeIO`) [L16-L19, L22-L31, L40-L66, L69-L71, L74-L76]
- [ ] <!-- ACT-34da5b-1 --> **[utility · high · trivial]** `internal/infrastructure/pluginmgr/composite.go`: Remove dead code: `CompositeOperationProvider` is exported but unused `CompositeOperationProvider`, `NewCompositeOperationProvider`, `GetOperation`, `ListOperations`, `Execute` (`CompositeOperationProvider, NewCompositeOperationProvider, GetOperation, ListOperations, Execute`) [L14-L16, L20-L24, L27-L37, L40-L50, L53-L64]
- [ ] <!-- ACT-38e0f8-1 --> **[utility · high · trivial]** `internal/infrastructure/repository/source.go`: Remove dead code: `Source` is exported but unused `Source`, `SourceEnv`, `SourceLocal`, `SourceGlobal`, `String` (`Source, SourceEnv, SourceLocal, SourceGlobal, String`) [L4-L4, L7-L7, L8-L8, L9-L9, L12-L23]
- [ ] <!-- ACT-a76edc-1 --> **[utility · high · trivial]** `internal/infrastructure/repository/template_repository.go`: Remove dead code: `YAMLTemplateRepository` is exported but unused `YAMLTemplateRepository`, `NewYAMLTemplateRepository`, `GetTemplate`, `ListTemplates`, `Exists` (`YAMLTemplateRepository, NewYAMLTemplateRepository, GetTemplate, ListTemplates, Exists`) [L18-L22, L25-L30, L33-L59, L62-L83, L86-L94]
- [ ] <!-- ACT-8bcf0f-2 --> **[utility · high · trivial]** `internal/infrastructure/tools/plugin_adapter.go`: Remove dead code: `PluginToolAdapter` is exported but unused `PluginToolAdapter`, `NewPluginToolAdapter`, `ListTools`, `Close` (`PluginToolAdapter, NewPluginToolAdapter, ListTools, Close`) [L26-L30, L36-L67, L69-L82, L117-L119]
- [ ] <!-- ACT-0206d2-1 --> **[utility · high · trivial]** `internal/infrastructure/transcript/nop_recorder.go`: Remove dead code: `NopRecorder` is exported but unused `NopRecorder`, `NewNopRecorder`, `Record`, `Subscribe`, `Close` (`NopRecorder, NewNopRecorder, Record, Subscribe, Close`) [L14-L16, L19-L23, L27-L29, L31-L33, L35-L35]
- [ ] <!-- ACT-9e52fe-1 --> **[utility · high · trivial]** `internal/infrastructure/workflowpkg/installer.go`: Remove dead code: `ManifestFileName` is exported but unused `ManifestFileName`, `MaxManifestSize`, `PackInstaller`, `NewPackInstaller`, `Install` (`ManifestFileName, MaxManifestSize, PackInstaller, NewPackInstaller, Install`) [L17-L17, L20-L20, L47-L50, L53-L64, L73-L178]

## 🔧 Refactors

- [ ] <!-- ACT-f43b67-3 --> **[utility · medium · trivial]** `internal/infrastructure/agents/base_cli_provider.go`: Remove dead code: `mcpServeCommand` is exported but unused `mcpServeCommand`, `noopMCPCleanup`, `newBaseCLIProvider`, `execute`, `executeConversation` (`mcpServeCommand, noopMCPCleanup, newBaseCLIProvider, execute, executeConversation`) [L48-L50, L99-L99, L140-L152, L196-L289, L295-L436]
- [ ] <!-- ACT-8bcf0f-3 --> **[utility · medium · trivial]** `internal/infrastructure/tools/plugin_adapter.go`: Remove dead code: `CallTool` is exported but unused (`CallTool`) [L84-L115]
