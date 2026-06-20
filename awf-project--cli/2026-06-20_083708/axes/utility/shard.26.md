[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 26

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `pkg/httpx/response.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#pkghttpxresponsego) |
| `internal/application/skill_loader.go` | 🟡 NEEDS_REFACTOR | 2 | 92% | [details](#internalapplicationskillloadergo) |
| `internal/infrastructure/notify/webhook.go` | 🟡 NEEDS_REFACTOR | 2 | 92% | [details](#internalinfrastructurenotifywebhookgo) |
| `internal/infrastructure/pluginmgr/grpc_host.go` | 🟡 NEEDS_REFACTOR | 2 | 92% | [details](#internalinfrastructurepluginmgrgrpchostgo) |
| `internal/infrastructure/tools/builtins/write.go` | 🟡 NEEDS_REFACTOR | 1 | 92% | [details](#internalinfrastructuretoolsbuiltinswritego) |
| `internal/interfaces/tui/messages.go` | 🟡 NEEDS_REFACTOR | 1 | 85% | [details](#internalinterfacestuimessagesgo) |
| `internal/application/interpolation_helpers.go` | 🟡 NEEDS_REFACTOR | 2 | 62% | [details](#internalapplicationinterpolationhelpersgo) |
| `internal/domain/operation/operation.go` | 🟡 NEEDS_REFACTOR | 1 | 100% | [details](#internaldomainoperationoperationgo) |
| `internal/domain/ports/agent_output_normalizer.go` | 🟡 NEEDS_REFACTOR | 1 | 100% | [details](#internaldomainportsagentoutputnormalizergo) |
| `internal/domain/ports/agent_role_repository.go` | 🟡 NEEDS_REFACTOR | 1 | 100% | [details](#internaldomainportsagentrolerepositorygo) |

## 🔍 Symbol Details

### `pkg/httpx/response.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Response` | L14–L19 | 🔴 DEAD | 95% | Exported struct with 0 importers across the codebase. |
| `ReadBody` | L27–L50 | 🔴 DEAD | 95% | Exported function with 0 importers across the codebase. |

### `internal/application/skill_loader.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `FormatSkillContent` | L14–L28 | 🔴 DEAD | 85% | 0 runtime and 0 type-only importers. Called locally by ResolveAndFormatSkills, but if ResolveAndFormatSkills is itself dead, the only live usage path is internal to a dead call chain. The export provides no external value. |
| `ResolveAndFormatSkills` | L31–L52 | 🔴 DEAD | 92% | 0 runtime and 0 type-only importers. No external consumer exists. |

### `internal/infrastructure/notify/webhook.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `NewWebhookBackend` | L42–L44 | 🔴 DEAD | 90% | 0 importers per analysis. Comment claims usage in run.go but import analysis finds no consumers. Only entry point to webhookBackend from outside the package. |
| `Send` | L52–L110 | 🔴 DEAD | 85% | 0 importers per analysis. Satisfies Backend interface but since NewWebhookBackend (sole external entry point) is itself dead, Send is never reachable via interface dispatch either. |

### `internal/infrastructure/pluginmgr/grpc_host.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `convertExecuteResponse` | L12–L33 | 🔴 DEAD | 70% | Not called anywhere in this file. May be used in other files within the same pluginmgr package, but per rule: no local usage found. |
| `convertOperationSchema` | L37–L58 | 🔴 DEAD | 70% | Not called anywhere in this file. May be used in other pluginmgr package files, but no local call site is present. |

### `internal/infrastructure/tools/builtins/write.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `writeHandler` | L31–L69 | 🔴 DEAD | 65% | Not called or referenced anywhere in this file. Per rules, non-exported symbols are evaluated on local usage only. It follows the builtins handler pattern and is almost certainly wired up in another file in the same package (e.g. a Register/New function), so confidence is reduced rather than flagged at 95. |

### `internal/interfaces/tui/messages.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `facadeEventMsg` | L68–L70 | 🔴 DEAD | 85% | Non-exported type with no reference in this file. Per rule 5, local-only check applies. Comment names MonitoringTab.StartEventLoop as the producer/consumer, which would be in a sibling file — but that usage is outside the scope of local-file analysis. |

### `internal/application/interpolation_helpers.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `buildInterpolationContext` | L15–L84 | 🔴 DEAD | 62% | Not called anywhere in this file. Pre-computed analysis lists no importers. However, Go non-exported symbols are package-scoped, not file-scoped; the inline comment 'Used by both ExecutionService and InteractiveExecutor' strongly implies cross-file usage within the same `application` package that the exhaustive analysis may not have resolved. |
| `interpolateTerminalMessage` | L89–L104 | 🔴 DEAD | 62% | Not called anywhere in this file. Pre-computed analysis lists no importers. Same caveat as buildInterpolationContext: Go package-scoped visibility means sibling files (e.g. execution_service.go, interactive_executor.go) in the `application` package can call it without appearing here. |

### `internal/domain/operation/operation.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Operation` | L13–L24 | 🔴 DEAD | 100% | Exported but imported by 0 files |

### `internal/domain/ports/agent_output_normalizer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `AgentOutputNormalizer` | L12–L16 | 🔴 DEAD | 100% | Exported but imported by 0 files |

### `internal/domain/ports/agent_role_repository.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `AgentRoleRepository` | L9–L12 | 🔴 DEAD | 100% | Exported but imported by 0 files |

## ⚡ Quick Wins

- [ ] <!-- ACT-3e4c8a-1 --> **[utility · high · trivial]** `internal/application/skill_loader.go`: Remove dead code: `FormatSkillContent` is exported but unused (`FormatSkillContent`) [L14-L28]
- [ ] <!-- ACT-3e4c8a-2 --> **[utility · high · trivial]** `internal/application/skill_loader.go`: Remove dead code: `ResolveAndFormatSkills` is exported but unused (`ResolveAndFormatSkills`) [L31-L52]
- [ ] <!-- ACT-3450e3-1 --> **[utility · high · trivial]** `internal/infrastructure/notify/webhook.go`: Remove dead code: `NewWebhookBackend` is exported but unused (`NewWebhookBackend`) [L42-L44]
- [ ] <!-- ACT-3450e3-2 --> **[utility · high · trivial]** `internal/infrastructure/notify/webhook.go`: Remove dead code: `Send` is exported but unused (`Send`) [L52-L110]
- [ ] <!-- ACT-b3d574-2 --> **[utility · high · trivial]** `internal/interfaces/tui/messages.go`: Remove dead code: `facadeEventMsg` is exported but unused (`facadeEventMsg`) [L68-L70]
- [ ] <!-- ACT-b24c59-1 --> **[utility · high · trivial]** `pkg/httpx/response.go`: Remove dead code: `Response` is exported but unused (`Response`) [L14-L19]
- [ ] <!-- ACT-b24c59-2 --> **[utility · high · trivial]** `pkg/httpx/response.go`: Remove dead code: `ReadBody` is exported but unused (`ReadBody`) [L27-L50]

## 🔧 Refactors

- [ ] <!-- ACT-80714f-1 --> **[utility · medium · trivial]** `internal/application/interpolation_helpers.go`: Remove dead code: `buildInterpolationContext` is exported but unused (`buildInterpolationContext`) [L15-L84]
- [ ] <!-- ACT-80714f-2 --> **[utility · medium · trivial]** `internal/application/interpolation_helpers.go`: Remove dead code: `interpolateTerminalMessage` is exported but unused (`interpolateTerminalMessage`) [L89-L104]
- [ ] <!-- ACT-d5be4f-1 --> **[utility · medium · trivial]** `internal/infrastructure/pluginmgr/grpc_host.go`: Remove dead code: `convertExecuteResponse` is exported but unused (`convertExecuteResponse`) [L12-L33]
- [ ] <!-- ACT-d5be4f-2 --> **[utility · medium · trivial]** `internal/infrastructure/pluginmgr/grpc_host.go`: Remove dead code: `convertOperationSchema` is exported but unused (`convertOperationSchema`) [L37-L58]
- [ ] <!-- ACT-f2eaee-2 --> **[utility · medium · trivial]** `internal/infrastructure/tools/builtins/write.go`: Remove dead code: `writeHandler` is exported but unused (`writeHandler`) [L31-L69]
