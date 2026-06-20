[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 14

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `pkg/plugin/sdk/host_client.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#pkgpluginsdkhostclientgo) |
| `internal/interfaces/cli/exitcodes.go` | 🟡 NEEDS_REFACTOR | 5 | 100% | [details](#internalinterfacescliexitcodesgo) |
| `internal/application/parallel_executor.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalapplicationparallelexecutorgo) |
| `internal/application/run_session.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalapplicationrunsessiongo) |
| `internal/application/tools/proxy_service.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalapplicationtoolsproxyservicego) |
| `internal/domain/ports/tool_provider.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internaldomainportstoolprovidergo) |
| `internal/domain/workflow/agent_role.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internaldomainworkflowagentrolego) |
| `internal/domain/workflow/mcp_proxy.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internaldomainworkflowmcpproxygo) |
| `internal/domain/workflow/skill.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internaldomainworkflowskillgo) |
| `internal/infrastructure/acp/renderer.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinfrastructureacprenderergo) |

## 🔍 Symbol Details

### `pkg/plugin/sdk/host_client.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `HostEventServiceID` | L15–L15 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EventEmitter` | L18–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `BrokerAwarePlugin` | L24–L26 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `HostClient` | L29–L32 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewHostClient` | L36–L48 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `Emit` | L51–L69 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/interfaces/cli/exitcodes.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ExitSuccess` | L5–L5 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `ExitUser` | L6–L6 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `ExitWorkflow` | L7–L7 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `ExitExecution` | L8–L8 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `ExitSystem` | L9–L9 | 🔴 DEAD | 100% | Exported but imported by 0 files |

### `internal/application/parallel_executor.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ParallelExecutor` | L17–L19 | 🔴 DEAD | 95% | 0 external importers. Compile-time interface assertion on L357 is local and not a runtime caller. No external code constructs or references this type. |
| `NewParallelExecutor` | L22–L26 | 🔴 DEAD | 95% | 0 external importers. Constructor is never called outside this file; no wiring point found. |
| `Execute` | L29–L73 | 🔴 DEAD | 90% | 0 external importers. Implements ports.ParallelExecutor interface but no caller injects or invokes this method anywhere in the import graph. |

### `internal/application/run_session.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `newRunSession` | L52–L66 | 🔴 DEAD | 65% | Not called anywhere in this file. Comments reference it being called by Adapter.Run in the same package, but cross-file same-package usage is not visible in provided content. |
| `CancelWithEvent` | L132–L144 | 🔴 DEAD | 75% | Exported with 0 importers. Not part of ports.RunSession interface (no compile-time constraint). Comment references interface callers but no evidence in provided code. |
| `ReplayFromSeq` | L181–L183 | 🔴 DEAD | 65% | Exported with 0 importers. Comment claims SSE handler type-asserts it, but no such assertion is visible in provided code. Confidence reduced because type-assertion usage is plausible. |
| `StatusSnapshot` | L223–L263 | 🔴 DEAD | 70% | Exported with 0 importers. Comments reference HTTP and CLI callers, but no usage is visible in the provided code. Confidence reduced because the interface for this method may exist outside the provided files. |
| `setInputs` | L361–L365 | 🔴 DEAD | 65% | Not called anywhere in this file. Comment explicitly states it is called by Adapter.Run after newSession, but that cross-file same-package call is not visible in provided content. |

### `internal/application/tools/proxy_service.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ProviderFactory` | L17–L17 | 🔴 DEAD | 88% | 0 external importers per pre-computed analysis. Referenced locally as a struct field type and parameter type, but those referencing symbols (ProxyService, NewProxyService) are themselves externally dead, making the whole cluster unreachable from outside this package. |
| `ProxyService` | L24–L29 | 🔴 DEAD | 95% | 0 external importers. No internal call site instantiates or references the type outside of method receivers in this same file. |
| `NewProxyService` | L32–L39 | 🔴 DEAD | 95% | 0 external importers and not called within this file. Only constructor for ProxyService, making the entire ProxyService cluster externally unreachable. |
| `StartForStdio` | L52–L120 | 🔴 DEAD | 92% | 0 external importers; not called internally. Receiver type ProxyService is itself externally dead. |
| `StartForHTTP` | L124–L157 | 🔴 DEAD | 78% | 0 external importers; not called internally. Receiver type ProxyService is itself externally dead. |

### `internal/domain/ports/tool_provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ToolDefinition` | L5–L10 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ToolContent` | L12–L15 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ToolResult` | L17–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ToolProvider` | L32–L36 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ToolRouter` | L44–L47 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/workflow/agent_role.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `AgentRoleSizeWarnBytes` | L11–L11 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AgentRole` | L14–L22 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AgentRoleNotFoundError` | L25–L35 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L37–L39 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Unwrap` | L41–L43 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/workflow/mcp_proxy.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `MCPProxyConfigPathKey` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MCPProxyConfigKey` | L16–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `PluginToolExpose` | L19–L22 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MCPProxyConfig` | L25–L29 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Validate` | L33–L63 | 🔴 DEAD | 85% | Exported but imported by 0 files |

### `internal/domain/workflow/skill.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Skill` | L9–L14 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SkillReference` | L18–L21 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `IsPathBased` | L24–L26 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SkillNotFoundError` | L29–L32 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L34–L36 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/acp/renderer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `SecretMasker` | L16–L18 | 🔴 DEAD | 90% | 0 external importers. Used within this file only (Renderer.masker field type and NewRenderer param type), but the entire Renderer cluster is externally unreachable, making intra-file usage moot. |
| `Renderer` | L43–L56 | 🔴 DEAD | 95% | 0 external importers. No file outside this package references the type. |
| `NewRenderer` | L60–L70 | 🔴 DEAD | 95% | 0 external importers. Constructor for a dead type; never called from outside the package. |
| `Render` | L82–L149 | 🔴 DEAD | 85% | 0 external importers. Called only by RenderFunc within this file, which is itself dead — transitively unreachable from outside the package. |
| `RenderFunc` | L162–L173 | 🔴 DEAD | 90% | 0 external importers. Entry point for the rendering pipeline but never called from outside the package. |

## ⚡ Quick Wins

- [ ] <!-- ACT-206272-3 --> **[utility · high · trivial]** `internal/application/parallel_executor.go`: Remove dead code: `ParallelExecutor` is exported but unused `ParallelExecutor`, `NewParallelExecutor`, `Execute` (`ParallelExecutor, NewParallelExecutor, Execute`) [L17-L19, L22-L26, L29-L73]
- [ ] <!-- ACT-2a2ca9-2 --> **[utility · high · trivial]** `internal/application/tools/proxy_service.go`: Remove dead code: `ProviderFactory` is exported but unused `ProviderFactory`, `ProxyService`, `NewProxyService`, `StartForStdio` (`ProviderFactory, ProxyService, NewProxyService, StartForStdio`) [L17-L17, L24-L29, L32-L39, L52-L120]
- [ ] <!-- ACT-f8fa6f-1 --> **[utility · high · trivial]** `internal/domain/ports/tool_provider.go`: Remove dead code: `ToolDefinition` is exported but unused `ToolDefinition`, `ToolContent`, `ToolResult`, `ToolProvider`, `ToolRouter` (`ToolDefinition, ToolContent, ToolResult, ToolProvider, ToolRouter`) [L5-L10, L12-L15, L17-L20, L32-L36, L44-L47]
- [ ] <!-- ACT-dcf269-1 --> **[utility · high · trivial]** `internal/domain/workflow/agent_role.go`: Remove dead code: `AgentRoleSizeWarnBytes` is exported but unused `AgentRoleSizeWarnBytes`, `AgentRole`, `AgentRoleNotFoundError`, `Error`, `Unwrap` (`AgentRoleSizeWarnBytes, AgentRole, AgentRoleNotFoundError, Error, Unwrap`) [L11-L11, L14-L22, L25-L35, L37-L39, L41-L43]
- [ ] <!-- ACT-c93571-3 --> **[utility · high · trivial]** `internal/domain/workflow/mcp_proxy.go`: Remove dead code: `MCPProxyConfigPathKey` is exported but unused `MCPProxyConfigPathKey`, `MCPProxyConfigKey`, `PluginToolExpose`, `MCPProxyConfig`, `Validate` (`MCPProxyConfigPathKey, MCPProxyConfigKey, PluginToolExpose, MCPProxyConfig, Validate`) [L12-L12, L16-L16, L19-L22, L25-L29, L33-L63]
- [ ] <!-- ACT-5f8460-1 --> **[utility · high · trivial]** `internal/domain/workflow/skill.go`: Remove dead code: `Skill` is exported but unused `Skill`, `SkillReference`, `IsPathBased`, `SkillNotFoundError`, `Error` (`Skill, SkillReference, IsPathBased, SkillNotFoundError, Error`) [L9-L14, L18-L21, L24-L26, L29-L32, L34-L36]
- [ ] <!-- ACT-6a0ebf-1 --> **[utility · high · trivial]** `internal/infrastructure/acp/renderer.go`: Remove dead code: `SecretMasker` is exported but unused `SecretMasker`, `Renderer`, `NewRenderer`, `Render`, `RenderFunc` (`SecretMasker, Renderer, NewRenderer, Render, RenderFunc`) [L16-L18, L43-L56, L60-L70, L82-L149, L162-L173]
- [ ] <!-- ACT-fcbea1-2 --> **[utility · high · trivial]** `pkg/plugin/sdk/host_client.go`: Remove dead code: `HostEventServiceID` is exported but unused `HostEventServiceID`, `EventEmitter`, `BrokerAwarePlugin`, `HostClient`, `NewHostClient`, `Emit` (`HostEventServiceID, EventEmitter, BrokerAwarePlugin, HostClient, NewHostClient, Emit`) [L15-L15, L18-L20, L24-L26, L29-L32, L36-L48, L51-L69]

## 🔧 Refactors

- [ ] <!-- ACT-324d2a-1 --> **[utility · medium · trivial]** `internal/application/run_session.go`: Remove dead code: `newRunSession` is exported but unused `newRunSession`, `CancelWithEvent`, `ReplayFromSeq`, `StatusSnapshot`, `setInputs` (`newRunSession, CancelWithEvent, ReplayFromSeq, StatusSnapshot, setInputs`) [L52-L66, L132-L144, L181-L183, L223-L263, L361-L365]
- [ ] <!-- ACT-2a2ca9-3 --> **[utility · medium · trivial]** `internal/application/tools/proxy_service.go`: Remove dead code: `StartForHTTP` is exported but unused (`StartForHTTP`) [L124-L157]
