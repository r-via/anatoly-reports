[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 29

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/infrastructure/agents/gemini_to_content_blocks.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructureagentsgeminitocontentblocksgo) |
| `internal/infrastructure/agents/mcp_proxy_purge.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructureagentsmcpproxypurgego) |
| `internal/infrastructure/agents/openai_compatible_to_content_blocks.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructureagentsopenaicompatibletocontentblocksgo) |
| `internal/infrastructure/github/operations.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructuregithuboperationsgo) |
| `internal/infrastructure/http/operations.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructurehttpoperationsgo) |
| `internal/infrastructure/mcp/handler.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructuremcphandlergo) |
| `internal/infrastructure/notify/operations.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructurenotifyoperationsgo) |
| `internal/infrastructure/pluginmgr/host_event_service.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructurepluginmgrhosteventservicego) |
| `internal/infrastructure/tools/builtins/grep.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructuretoolsbuiltinsgrepgo) |
| `internal/interfaces/api/routing.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacesapiroutinggo) |

## 🔍 Symbol Details

### `internal/infrastructure/agents/gemini_to_content_blocks.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `GeminiToContentBlocks` | L21–L46 | 🔴 DEAD | 92% | Exported but pre-computed analysis shows 0 runtime and 0 type-only importers across the codebase. |

### `internal/infrastructure/agents/mcp_proxy_purge.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `PurgeOrphanMCPRegistrations` | L45–L56 | 🔴 DEAD | 95% | Exported function with 0 runtime and 0 type-only importers per pre-computed analysis. Nothing in the project calls it. |

### `internal/infrastructure/agents/openai_compatible_to_content_blocks.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `OpenAICompatibleToContentBlocks` | L32–L75 | 🔴 DEAD | 85% | Exported but imported by 0 files. No runtime or type-only callers detected. |

### `internal/infrastructure/github/operations.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `AllOperations` | L12–L115 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/http/operations.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `AllOperations` | L6–L49 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/mcp/handler.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `handlerFor` | L13–L49 | 🔴 DEAD | 70% | Not exported; no local call site visible in this file. If called from another file in the same package (e.g., server.go), it would be USED — but no evidence of that in the provided context. |

### `internal/infrastructure/notify/operations.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `AllOperations` | L5–L40 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/pluginmgr/host_event_service.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `newHostEventService` | L22–L28 | 🔴 DEAD | 70% | Not called anywhere in this file. May be called from another file in the pluginmgr package, but per rules only local file usage is verifiable — not referenced here. |

### `internal/infrastructure/tools/builtins/grep.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `grepHandler` | L52–L112 | 🔴 DEAD | 65% | Not called anywhere in this file. As a method on *Provider it is likely registered in another file within the builtins package, but per the pre-computed analysis and local-file rule no reference is visible. |

### `internal/interfaces/api/routing.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `recomposeIdentifier` | L35–L40 | 🔴 DEAD | 60% | Not called anywhere in this file. Per rules, non-exported symbols are evaluated against file-local usage only, and no callsite exists here. However, the doc comment explicitly states it is 'shared by workflow and execution handlers', strongly implying usage in sibling files within package api that are not provided — confidence is reduced accordingly. |

## ⚡ Quick Wins

- [ ] <!-- ACT-fce6a7-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/gemini_to_content_blocks.go`: Remove dead code: `GeminiToContentBlocks` is exported but unused (`GeminiToContentBlocks`) [L21-L46]
- [ ] <!-- ACT-f3760e-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/mcp_proxy_purge.go`: Remove dead code: `PurgeOrphanMCPRegistrations` is exported but unused (`PurgeOrphanMCPRegistrations`) [L45-L56]
- [ ] <!-- ACT-2e89bb-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/openai_compatible_to_content_blocks.go`: Remove dead code: `OpenAICompatibleToContentBlocks` is exported but unused (`OpenAICompatibleToContentBlocks`) [L32-L75]
- [ ] <!-- ACT-81857d-1 --> **[utility · high · trivial]** `internal/infrastructure/github/operations.go`: Remove dead code: `AllOperations` is exported but unused (`AllOperations`) [L12-L115]
- [ ] <!-- ACT-1c94f3-1 --> **[utility · high · trivial]** `internal/infrastructure/http/operations.go`: Remove dead code: `AllOperations` is exported but unused (`AllOperations`) [L6-L49]
- [ ] <!-- ACT-fcb5ab-1 --> **[utility · high · trivial]** `internal/infrastructure/notify/operations.go`: Remove dead code: `AllOperations` is exported but unused (`AllOperations`) [L5-L40]

## 🔧 Refactors

- [ ] <!-- ACT-337d53-1 --> **[utility · medium · trivial]** `internal/infrastructure/mcp/handler.go`: Remove dead code: `handlerFor` is exported but unused (`handlerFor`) [L13-L49]
- [ ] <!-- ACT-2d60dd-1 --> **[utility · medium · trivial]** `internal/infrastructure/pluginmgr/host_event_service.go`: Remove dead code: `newHostEventService` is exported but unused (`newHostEventService`) [L22-L28]
- [ ] <!-- ACT-1aeb36-1 --> **[utility · medium · trivial]** `internal/infrastructure/tools/builtins/grep.go`: Remove dead code: `grepHandler` is exported but unused (`grepHandler`) [L52-L112]
- [ ] <!-- ACT-477dcd-1 --> **[utility · medium · trivial]** `internal/interfaces/api/routing.go`: Remove dead code: `recomposeIdentifier` is exported but unused (`recomposeIdentifier`) [L35-L40]
