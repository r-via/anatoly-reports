[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 28

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/domain/ports/step_executor.go` | 🟡 NEEDS_REFACTOR | 1 | 100% | [details](#internaldomainportsstepexecutorgo) |
| `internal/domain/ports/store.go` | 🟡 NEEDS_REFACTOR | 1 | 100% | [details](#internaldomainportsstorego) |
| `internal/domain/ports/tokenizer.go` | 🟡 NEEDS_REFACTOR | 1 | 100% | [details](#internaldomainportstokenizergo) |
| `internal/domain/ports/user_input.go` | 🟡 NEEDS_REFACTOR | 1 | 100% | [details](#internaldomainportsuserinputgo) |
| `internal/application/facade_projection.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalapplicationfacadeprojectiongo) |
| `internal/domain/workflow/entry.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internaldomainworkflowentrygo) |
| `internal/domain/workflow/validation.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internaldomainworkflowvalidationgo) |
| `internal/infrastructure/agents/claude_to_content_blocks.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructureagentsclaudetocontentblocksgo) |
| `internal/infrastructure/agents/codex_to_content_blocks.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructureagentscodextocontentblocksgo) |
| `internal/infrastructure/agents/copilot_to_content_blocks.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructureagentscopilottocontentblocksgo) |

## 🔍 Symbol Details

### `internal/domain/ports/step_executor.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `StepExecutor` | L11–L20 | 🔴 DEAD | 100% | Exported but imported by 0 files |

### `internal/domain/ports/store.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `StateStore` | L10–L15 | 🔴 DEAD | 100% | Exported but imported by 0 files |

### `internal/domain/ports/tokenizer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Tokenizer` | L6–L23 | 🔴 DEAD | 100% | Exported but imported by 0 files |

### `internal/domain/ports/user_input.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `UserInputReader` | L7–L12 | 🔴 DEAD | 100% | Exported but imported by 0 files |

### `internal/application/facade_projection.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ProjectEvent` | L15–L34 | 🔴 DEAD | 92% | Exported function with 0 runtime and 0 type-only importers per pre-computed analysis. |

### `internal/domain/workflow/entry.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `WorkflowEntry` | L20–L27 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/workflow/validation.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `InputValidation` | L4–L11 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/agents/claude_to_content_blocks.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ClaudeToContentBlocks` | L25–L62 | 🔴 DEAD | 92% | Exported but imported by 0 files per pre-computed analysis. No internal callers in this file. |

### `internal/infrastructure/agents/codex_to_content_blocks.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `CodexToContentBlocks` | L26–L59 | 🔴 DEAD | 90% | Exported but imported by 0 files per pre-computed analysis. No internal callers exist in this file. |

### `internal/infrastructure/agents/copilot_to_content_blocks.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `CopilotToContentBlocks` | L16–L51 | 🔴 DEAD | 92% | Exported but imported by 0 files per exhaustive import analysis. No callers exist in the codebase. |

## ⚡ Quick Wins

- [ ] <!-- ACT-216a73-1 --> **[utility · high · trivial]** `internal/application/facade_projection.go`: Remove dead code: `ProjectEvent` is exported but unused (`ProjectEvent`) [L15-L34]
- [ ] <!-- ACT-5fbffc-1 --> **[utility · high · trivial]** `internal/domain/workflow/entry.go`: Remove dead code: `WorkflowEntry` is exported but unused (`WorkflowEntry`) [L20-L27]
- [ ] <!-- ACT-66c0b0-1 --> **[utility · high · trivial]** `internal/domain/workflow/validation.go`: Remove dead code: `InputValidation` is exported but unused (`InputValidation`) [L4-L11]
- [ ] <!-- ACT-98894a-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/claude_to_content_blocks.go`: Remove dead code: `ClaudeToContentBlocks` is exported but unused (`ClaudeToContentBlocks`) [L25-L62]
- [ ] <!-- ACT-f06b76-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/codex_to_content_blocks.go`: Remove dead code: `CodexToContentBlocks` is exported but unused (`CodexToContentBlocks`) [L26-L59]
- [ ] <!-- ACT-33792b-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/copilot_to_content_blocks.go`: Remove dead code: `CopilotToContentBlocks` is exported but unused (`CopilotToContentBlocks`) [L16-L51]
