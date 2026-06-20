[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 31

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/interfaces/tui/viewportscroll.go` | 🟡 NEEDS_REFACTOR | 1 | 75% | [details](#internalinterfacestuiviewportscrollgo) |
| `internal/application/prompt_file.go` | 🟡 NEEDS_REFACTOR | 1 | 72% | [details](#internalapplicationpromptfilego) |
| `internal/interfaces/cli/resume_list.go` | 🟡 NEEDS_REFACTOR | 1 | 70% | [details](#internalinterfacescliresumelistgo) |
| `internal/interfaces/cli/signal_handler.go` | 🟡 NEEDS_REFACTOR | 1 | 70% | [details](#internalinterfacesclisignalhandlergo) |
| `internal/interfaces/cli/ui/readline.go` | 🟡 NEEDS_REFACTOR | 1 | 70% | [details](#internalinterfacescliuireadlinego) |
| `internal/infrastructure/repository/yaml_mapper.go` | 🟢 CLEAN | 2 | 95% | [details](#internalinfrastructurerepositoryyamlmappergo) |
| `internal/infrastructure/mcp/mapping.go` | 🟢 CLEAN | 2 | 55% | [details](#internalinfrastructuremcpmappinggo) |

## 🔍 Symbol Details

### `internal/interfaces/tui/viewportscroll.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `viewportAutoScroll` | L9–L16 | 🔴 DEAD | 75% | Unexported function with no callers in this file. Pre-computed analysis lists no importers. Confidence reduced to 75 because Go unexported symbols are package-scoped — other files in package `tui` could call it, but the exhaustive analysis found no such references. |

### `internal/application/prompt_file.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `loadPromptFile` | L11–L18 | 🔴 DEAD | 72% | Non-exported; not called anywhere in this file. Only wraps loadExternalFile with a renamed parameter. Other files in the same `application` package are not provided, so a same-package caller cannot be ruled out — confidence reduced accordingly. |

### `internal/interfaces/cli/resume_list.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `newResumeListCommand` | L7–L23 | 🔴 DEAD | 70% | Not called anywhere in this file. Standard Cobra subcommand constructors of this pattern are typically registered from another file in the same package (e.g., a resume.go or root.go that calls AddCommand), but no cross-file usage is visible in the provided context and the pre-computed analysis found no references. |

### `internal/interfaces/cli/signal_handler.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `setupSignalHandler` | L13–L28 | 🔴 DEAD | 70% | Not called anywhere in this file. Non-exported Go symbols are package-scoped, so callers in other cli package files are possible but not visible in the provided context. Marking DEAD based on the exhaustive import analysis instruction to check local file usage only. |

### `internal/interfaces/cli/ui/readline.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `readLineWithContext` | L12–L37 | 🔴 DEAD | 70% | Not exported; no call site exists within this file. Same-package files in `ui` could reference it, but none are provided — marking DEAD per available evidence. |

### `internal/infrastructure/repository/yaml_mapper.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `mapToDomain` | L16–L70 | 🔴 DEAD | 55% | Not called anywhere in this file. Package-private entry point almost certainly called from another file in the repository package (e.g., yaml_reader.go), but per file-only analysis no local reference exists. |
| `mapTemplate` | L403–L426 | 🔴 DEAD | 55% | Not called anywhere in this file. Package-private; almost certainly invoked from another file in the repository package for template YAML parsing, but no local reference is visible. |

### `internal/infrastructure/mcp/mapping.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `toolToMCP` | L9–L19 | 🔴 DEAD | 55% | Not called within this file. Non-exported, so only same-package files can use it — but no other package files were provided. Naming convention strongly suggests it is called from another file in package mcp (e.g., a server or handler file). |
| `resultToMCP` | L21–L29 | 🔴 DEAD | 55% | Not called within this file. Same caveat as toolToMCP — almost certainly invoked from another file in package mcp that was not provided for analysis. |

## 🔧 Refactors

- [ ] <!-- ACT-6f0500-1 --> **[utility · medium · trivial]** `internal/application/prompt_file.go`: Remove dead code: `loadPromptFile` is exported but unused (`loadPromptFile`) [L11-L18]
- [ ] <!-- ACT-635b19-1 --> **[utility · medium · trivial]** `internal/infrastructure/mcp/mapping.go`: Remove dead code: `toolToMCP` is exported but unused (`toolToMCP`) [L9-L19]
- [ ] <!-- ACT-635b19-2 --> **[utility · medium · trivial]** `internal/infrastructure/mcp/mapping.go`: Remove dead code: `resultToMCP` is exported but unused (`resultToMCP`) [L21-L29]
- [ ] <!-- ACT-c81d31-1 --> **[utility · medium · trivial]** `internal/infrastructure/repository/yaml_mapper.go`: Remove dead code: `mapToDomain` is exported but unused (`mapToDomain`) [L16-L70]
- [ ] <!-- ACT-c81d31-2 --> **[utility · medium · trivial]** `internal/infrastructure/repository/yaml_mapper.go`: Remove dead code: `mapTemplate` is exported but unused (`mapTemplate`) [L403-L426]
- [ ] <!-- ACT-88ac2c-1 --> **[utility · medium · trivial]** `internal/interfaces/cli/resume_list.go`: Remove dead code: `newResumeListCommand` is exported but unused (`newResumeListCommand`) [L7-L23]
- [ ] <!-- ACT-d3b3f7-1 --> **[utility · medium · trivial]** `internal/interfaces/cli/signal_handler.go`: Remove dead code: `setupSignalHandler` is exported but unused (`setupSignalHandler`) [L13-L28]
- [ ] <!-- ACT-6aa080-1 --> **[utility · medium · trivial]** `internal/interfaces/cli/ui/readline.go`: Remove dead code: `readLineWithContext` is exported but unused (`readLineWithContext`) [L12-L37]
- [ ] <!-- ACT-ec3e48-1 --> **[utility · medium · trivial]** `internal/interfaces/tui/viewportscroll.go`: Remove dead code: `viewportAutoScroll` is exported but unused (`viewportAutoScroll`) [L9-L16]
