[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 17

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/application/output_streamer.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalapplicationoutputstreamergo) |
| `internal/domain/ports/acp_client.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internaldomainportsacpclientgo) |
| `internal/domain/workflow/config.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internaldomainworkflowconfiggo) |
| `internal/domain/workflow/graph.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internaldomainworkflowgraphgo) |
| `internal/domain/workflow/hooks.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internaldomainworkflowhooksgo) |
| `internal/domain/workflow/template_validation.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internaldomainworkflowtemplatevalidationgo) |
| `internal/infrastructure/acp/event_projector.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinfrastructureacpeventprojectorgo) |
| `internal/infrastructure/acp/fanout_publisher.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinfrastructureacpfanoutpublishergo) |
| `internal/infrastructure/acp/server.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinfrastructureacpservergo) |
| `internal/infrastructure/agents/openai_compatible_tools.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinfrastructureagentsopenaicompatibletoolsgo) |

## 🔍 Symbol Details

### `internal/application/output_streamer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `OutputStreamer` | L15–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewOutputStreamer` | L21–L25 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StreamOutput` | L30–L61 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StreamBoth` | L65–L83 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/ports/acp_client.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ACPClient` | L7–L9 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `PermissionRequest` | L12–L17 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `PermissionOption` | L21–L25 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `PermissionResponse` | L29–L31 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/workflow/config.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `OutputLimits` | L5–L9 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `LoopMemoryConfig` | L13–L15 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DefaultOutputLimits` | L19–L25 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DefaultLoopMemoryConfig` | L29–L33 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/workflow/graph.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `String` | L25–L27 | 🔴 DEAD | 80% | 0 external importers; no fmt.Print/Sprintf call in this file uses VisitState, so Stringer impl is never invoked. Trivial wrapper with no observable consumer. |
| `ValidateGraph` | L71–L143 | 🔴 DEAD | 80% | 0 external importers and not called anywhere within this file. Top-level entry point with no consumer. |
| `ExecutionOrder` | L264–L304 | 🔴 DEAD | 90% | 0 external importers and not called within this file. Top-level entry point with no consumer. |

### `internal/domain/workflow/hooks.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `HookAction` | L4–L7 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Hook` | L9–L9 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WorkflowHooks` | L11–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StepHooks` | L18–L21 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/workflow/template_validation.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `NewTemplateValidator` | L22–L52 | 🔴 DEAD | 70% | Not called within this file; 0 cross-package importers. Confidence reduced because Go same-package callers don't appear in import analysis. |
| `Validate` | L70–L95 | 🔴 DEAD | 70% | Not called within this file; 0 cross-package importers. Confidence reduced for same-package ambiguity. |

### `internal/infrastructure/acp/event_projector.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `WorkflowEventProjector` | L12–L23 | 🔴 DEAD | 90% | 0 files import this type. The compile-time assertion at L25 (`var _ ports.EventPublisher = (*WorkflowEventProjector)(nil)`) is a guard, not runtime usage. No external caller constructs or holds this struct. |
| `NewWorkflowEventProjector` | L27–L33 | 🔴 DEAD | 95% | 0 importers. Constructor is never called; no external code instantiates WorkflowEventProjector. |
| `Publish` | L35–L94 | 🔴 DEAD | 88% | 0 direct importers. Although Go interface dispatch can route calls without direct name references, NewWorkflowEventProjector (the only constructor) also has 0 importers, meaning no WorkflowEventProjector instance is ever created to dispatch through. |
| `Close` | L96–L98 | 🔴 DEAD | 88% | 0 importers. Trivial no-op method on a dead struct — never reachable since no caller instantiates WorkflowEventProjector. |

### `internal/infrastructure/acp/fanout_publisher.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `FanoutPublisher` | L21–L24 | 🔴 DEAD | 95% | 0 runtime and 0 type-only importers. The compile-time interface check on L26 is local and does not constitute external usage. |
| `NewFanoutPublisher` | L30–L41 | 🔴 DEAD | 95% | 0 importers. Only constructor for FanoutPublisher; unreachable from any external package. |
| `Publish` | L54–L69 | 🔴 DEAD | 92% | 0 importers. Method is only callable via FanoutPublisher instances, which are never constructed outside this package. |
| `Close` | L72–L85 | 🔴 DEAD | 95% | 0 importers. Same reachability problem as Publish — no external code constructs FanoutPublisher. |

### `internal/infrastructure/acp/server.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Conn` | L21–L23 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewConnection` | L34–L40 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Done` | L53–L58 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewEmitter` | L68–L73 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/agents/openai_compatible_tools.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ToolDefinition` | L11–L14 | 🔴 DEAD | 95% | Exported with 0 importers; not referenced anywhere in this file. |
| `ToolCallDelta` | L25–L33 | 🔴 DEAD | 95% | Exported with 0 importers. Only local reference is in assembleToolCalls, which is itself dead. |
| `ToolCall` | L36–L40 | 🔴 DEAD | 95% | Exported with 0 importers. Only local reference is in assembleToolCalls, which is itself dead. |
| `assembleToolCalls` | L46–L89 | 🔴 DEAD | 92% | Unexported; no call sites found anywhere in this file. |

## ⚡ Quick Wins

- [ ] <!-- ACT-50115e-1 --> **[utility · high · trivial]** `internal/application/output_streamer.go`: Remove dead code: `OutputStreamer` is exported but unused `OutputStreamer`, `NewOutputStreamer`, `StreamOutput`, `StreamBoth` (`OutputStreamer, NewOutputStreamer, StreamOutput, StreamBoth`) [L15-L18, L21-L25, L30-L61, L65-L83]
- [ ] <!-- ACT-40ea54-1 --> **[utility · high · trivial]** `internal/domain/ports/acp_client.go`: Remove dead code: `ACPClient` is exported but unused `ACPClient`, `PermissionRequest`, `PermissionOption`, `PermissionResponse` (`ACPClient, PermissionRequest, PermissionOption, PermissionResponse`) [L7-L9, L12-L17, L21-L25, L29-L31]
- [ ] <!-- ACT-baf4bf-1 --> **[utility · high · trivial]** `internal/domain/workflow/config.go`: Remove dead code: `OutputLimits` is exported but unused `OutputLimits`, `LoopMemoryConfig`, `DefaultOutputLimits`, `DefaultLoopMemoryConfig` (`OutputLimits, LoopMemoryConfig, DefaultOutputLimits, DefaultLoopMemoryConfig`) [L5-L9, L13-L15, L19-L25, L29-L33]
- [ ] <!-- ACT-5e4fda-3 --> **[utility · high · trivial]** `internal/domain/workflow/graph.go`: Remove dead code: `String` is exported but unused `String`, `ValidateGraph`, `ExecutionOrder` (`String, ValidateGraph, ExecutionOrder`) [L25-L27, L71-L143, L264-L304]
- [ ] <!-- ACT-bf31de-1 --> **[utility · high · trivial]** `internal/domain/workflow/hooks.go`: Remove dead code: `HookAction` is exported but unused `HookAction`, `Hook`, `WorkflowHooks`, `StepHooks` (`HookAction, Hook, WorkflowHooks, StepHooks`) [L4-L7, L9-L9, L11-L16, L18-L21]
- [ ] <!-- ACT-e2a0e8-1 --> **[utility · high · trivial]** `internal/infrastructure/acp/event_projector.go`: Remove dead code: `WorkflowEventProjector` is exported but unused `WorkflowEventProjector`, `NewWorkflowEventProjector`, `Publish`, `Close` (`WorkflowEventProjector, NewWorkflowEventProjector, Publish, Close`) [L12-L23, L27-L33, L35-L94, L96-L98]
- [ ] <!-- ACT-7cceb2-1 --> **[utility · high · trivial]** `internal/infrastructure/acp/fanout_publisher.go`: Remove dead code: `FanoutPublisher` is exported but unused `FanoutPublisher`, `NewFanoutPublisher`, `Publish`, `Close` (`FanoutPublisher, NewFanoutPublisher, Publish, Close`) [L21-L24, L30-L41, L54-L69, L72-L85]
- [ ] <!-- ACT-09fc6a-1 --> **[utility · high · trivial]** `internal/infrastructure/acp/server.go`: Remove dead code: `Conn` is exported but unused `Conn`, `NewConnection`, `Done`, `NewEmitter` (`Conn, NewConnection, Done, NewEmitter`) [L21-L23, L34-L40, L53-L58, L68-L73]
- [ ] <!-- ACT-8b4623-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/openai_compatible_tools.go`: Remove dead code: `ToolDefinition` is exported but unused `ToolDefinition`, `ToolCallDelta`, `ToolCall`, `assembleToolCalls` (`ToolDefinition, ToolCallDelta, ToolCall, assembleToolCalls`) [L11-L14, L25-L33, L36-L40, L46-L89]

## 🔧 Refactors

- [ ] <!-- ACT-908eaa-3 --> **[utility · medium · trivial]** `internal/domain/workflow/template_validation.go`: Remove dead code: `NewTemplateValidator` is exported but unused (`NewTemplateValidator`) [L22-L52]
- [ ] <!-- ACT-908eaa-4 --> **[utility · medium · trivial]** `internal/domain/workflow/template_validation.go`: Remove dead code: `Validate` is exported but unused (`Validate`) [L70-L95]
