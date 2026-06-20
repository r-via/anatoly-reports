[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 23

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/interfaces/tui/tree.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinterfacestuitreego) |
| `pkg/display/renderer_context.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#pkgdisplayrenderercontextgo) |
| `pkg/interpolation/escaping.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#pkginterpolationescapinggo) |
| `pkg/stringutil/levenshtein.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#pkgstringutillevenshteingo) |
| `internal/interfaces/cli/resume.go` | 🟡 NEEDS_REFACTOR | 1 | 92% | [details](#internalinterfacescliresumego) |
| `internal/interfaces/cli/wiring_transcript.go` | 🟡 NEEDS_REFACTOR | 3 | 92% | [details](#internalinterfacescliwiringtranscriptgo) |
| `pkg/output/format.go` | 🟡 NEEDS_REFACTOR | 3 | 90% | [details](#pkgoutputformatgo) |
| `internal/domain/ports/agent_provider.go` | 🟡 NEEDS_REFACTOR | 2 | 100% | [details](#internaldomainportsagentprovidergo) |
| `internal/domain/ports/cli_executor.go` | 🟡 NEEDS_REFACTOR | 2 | 100% | [details](#internaldomainportscliexecutorgo) |
| `internal/domain/ports/recorder.go` | 🟡 NEEDS_REFACTOR | 2 | 100% | [details](#internaldomainportsrecordergo) |

## 🔍 Symbol Details

### `internal/interfaces/tui/tree.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `StatusIcon` | L24–L39 | 🔴 DEAD | 95% | 0 external importers and never called within this file. StatusBadgeIcon is used instead in renderNode. |
| `BuildTree` | L63–L75 | 🔴 DEAD | 95% | 0 external importers and not called anywhere within this file. |
| `RenderTree` | L131–L138 | 🔴 DEAD | 95% | 0 external importers and not called anywhere within this file. |

### `pkg/display/renderer_context.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `EventRenderer` | L10–L10 | 🔴 DEAD | 95% | Exported type with 0 runtime and 0 type-only importers. No external package references it. |
| `WithRenderer` | L16–L18 | 🔴 DEAD | 95% | Exported function with 0 importers. No call site stores a renderer into context. |
| `RendererFromContext` | L22–L28 | 🔴 DEAD | 95% | Exported function with 0 importers. No execution path retrieves the renderer from context. |

### `pkg/interpolation/escaping.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ShellEscape` | L7–L16 | 🔴 DEAD | 92% | 0 runtime and 0 type-only importers. Never called outside this file. |
| `NoEscape` | L31–L33 | 🔴 DEAD | 95% | 0 runtime and 0 type-only importers. Identity function with no callers. |

### `pkg/stringutil/levenshtein.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `LevenshteinDistance` | L13–L55 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ClosestMatch` | L60–L87 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Reverse` | L90–L101 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/interfaces/cli/resume.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `newResumeCommand` | L21–L67 | 🔴 DEAD | 60% | Not called anywhere in this file. Follows the Cobra newXxxCommand pattern, making cross-file same-package usage (e.g., root.go) very likely, but per local-file-only analysis it has no in-file callers. |

### `internal/interfaces/cli/wiring_transcript.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `WireTranscript` | L16–L32 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `NewRecorderFactory` | L38–L42 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `AttachMirrorSubscriber` | L49–L51 | 🔴 DEAD | 92% | Exported but imported by 0 files |

### `pkg/output/format.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `StripCodeFences` | L20–L35 | 🔴 DEAD | 90% | 0 external importers per exhaustive analysis. Only local caller is ProcessOutputFormat, which is itself dead (0 importers, no local callers). |
| `ValidateAndParseJSON` | L39–L49 | 🔴 DEAD | 90% | 0 external importers per exhaustive analysis. Only local caller is ProcessOutputFormat, which is itself dead. |
| `ProcessOutputFormat` | L64–L84 | 🔴 DEAD | 88% | 0 external importers per exhaustive analysis and no intra-file callers. Entire public API of this package appears unused. |

### `internal/domain/ports/agent_provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `AgentProvider` | L13–L31 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `AgentRegistry` | L34–L48 | 🔴 DEAD | 100% | Exported but imported by 0 files |

### `internal/domain/ports/cli_executor.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `CLIProcess` | L14–L18 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `CLIExecutor` | L27–L47 | 🔴 DEAD | 100% | Exported but imported by 0 files |

### `internal/domain/ports/recorder.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `RecorderFactory` | L15–L15 | 🔴 DEAD | 100% | Exported but imported by 0 files |
| `Recorder` | L18–L30 | 🔴 DEAD | 100% | Exported but imported by 0 files |

## ⚡ Quick Wins

- [ ] <!-- ACT-ccfaa2-1 --> **[utility · high · trivial]** `internal/interfaces/cli/wiring_transcript.go`: Remove dead code: `WireTranscript` is exported but unused `WireTranscript`, `NewRecorderFactory`, `AttachMirrorSubscriber` (`WireTranscript, NewRecorderFactory, AttachMirrorSubscriber`) [L16-L32, L38-L42, L49-L51]
- [ ] <!-- ACT-849628-1 --> **[utility · high · trivial]** `internal/interfaces/tui/tree.go`: Remove dead code: `StatusIcon` is exported but unused `StatusIcon`, `BuildTree`, `RenderTree` (`StatusIcon, BuildTree, RenderTree`) [L24-L39, L63-L75, L131-L138]
- [ ] <!-- ACT-b4898c-1 --> **[utility · high · trivial]** `pkg/display/renderer_context.go`: Remove dead code: `EventRenderer` is exported but unused `EventRenderer`, `WithRenderer`, `RendererFromContext` (`EventRenderer, WithRenderer, RendererFromContext`) [L10-L10, L16-L18, L22-L28]
- [ ] <!-- ACT-649203-2 --> **[utility · high · trivial]** `pkg/interpolation/escaping.go`: Remove dead code: `ShellEscape` is exported but unused (`ShellEscape`) [L7-L16]
- [ ] <!-- ACT-649203-3 --> **[utility · high · trivial]** `pkg/interpolation/escaping.go`: Remove dead code: `NoEscape` is exported but unused (`NoEscape`) [L31-L33]
- [ ] <!-- ACT-8eafe3-2 --> **[utility · high · trivial]** `pkg/output/format.go`: Remove dead code: `StripCodeFences` is exported but unused `StripCodeFences`, `ValidateAndParseJSON`, `ProcessOutputFormat` (`StripCodeFences, ValidateAndParseJSON, ProcessOutputFormat`) [L20-L35, L39-L49, L64-L84]
- [ ] <!-- ACT-53ff27-1 --> **[utility · high · trivial]** `pkg/stringutil/levenshtein.go`: Remove dead code: `LevenshteinDistance` is exported but unused `LevenshteinDistance`, `ClosestMatch`, `Reverse` (`LevenshteinDistance, ClosestMatch, Reverse`) [L13-L55, L60-L87, L90-L101]

## 🔧 Refactors

- [ ] <!-- ACT-234ba5-3 --> **[utility · medium · trivial]** `internal/interfaces/cli/resume.go`: Remove dead code: `newResumeCommand` is exported but unused (`newResumeCommand`) [L21-L67]
