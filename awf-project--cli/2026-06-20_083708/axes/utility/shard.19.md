[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 19

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/interfaces/api/sse.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinterfacesapissego) |
| `internal/interfaces/cli/acp_wiring.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinterfacescliacpwiringgo) |
| `internal/interfaces/cli/root.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinterfacesclirootgo) |
| `internal/interfaces/cli/ui/input_collector.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinterfacescliuiinputcollectorgo) |
| `internal/interfaces/cli/validate.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinterfacesclivalidatego) |
| `internal/interfaces/tui/tab_monitoring.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinterfacestuitabmonitoringgo) |
| `internal/interfaces/tui/tab_workflows.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinterfacestuitabworkflowsgo) |
| `pkg/interpolation/template_helpers.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#pkginterpolationtemplatehelpersgo) |
| `pkg/interpolation/template_resolver.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#pkginterpolationtemplateresolvergo) |
| `internal/infrastructure/github/client.go` | 🟡 NEEDS_REFACTOR | 4 | 92% | [details](#internalinfrastructuregithubclientgo) |

## 🔍 Symbol Details

### `internal/interfaces/api/sse.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `NewSSEHandler` | L98–L100 | 🔴 DEAD | 85% | 0 external importers; not called in this file. Constructor is never invoked per available analysis. |
| `SetSessionLookup` | L104–L106 | 🔴 DEAD | 85% | 0 external importers; not called in this file. Sessions field is never wired if this is never called. |
| `RegisterSSERoutes` | L213–L222 | 🔴 DEAD | 85% | 0 external importers; not called in this file. Route never registered into a server per available analysis. |
| `ProjectEventToSSEFrame` | L227–L231 | 🔴 DEAD | 85% | 0 external importers. Comment claims conformance test usage but no file imports it per pre-computed analysis. |

### `internal/interfaces/cli/acp_wiring.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `newACPTextWriter` | L34–L36 | 🔴 DEAD | 60% | Not called anywhere in this file. Non-exported, so can only be used within package cli — likely called from another file (e.g. acp_serve.go) but cannot confirm from this file alone. |
| `MissedEmits` | L61–L63 | 🔴 DEAD | 65% | Exported method on unexported type acpTextWriter. External packages cannot name the receiver type. Not called in this file. Not part of any interface (no compile-time assertion). Zero cross-file importers. |
| `newStreamFlaggingEmitter` | L75–L77 | 🔴 DEAD | 60% | Not called in this file. Non-exported constructor; likely used in another cli package file but cannot confirm from this file alone. |
| `sharedHistoryStore` | L96–L98 | 🔴 DEAD | 60% | Non-exported struct, not instantiated or referenced within this file. Doc references runACPServe but that is in another file; cannot confirm usage from local file content alone. |

### `internal/interfaces/cli/root.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `App` | L19–L22 | 🔴 DEAD | 95% | 0 external importers per pre-computed analysis. Only referenced as the return type of NewApp, which is itself dead. |
| `NewApp` | L24–L33 | 🔴 DEAD | 95% | 0 external importers. Not invoked anywhere in this file; subcommands receive cfg directly, not an App instance. |
| `NewRootCommand` | L35–L38 | 🔴 DEAD | 95% | 0 external importers. Thin wrapper around newRootCommand(false) with no internal callers. |
| `NewRootCommandAutoFacade` | L44–L46 | 🔴 DEAD | 95% | 0 external importers. Intended as the main entrypoint (builds facade, returns cleanup), but nothing imports or calls it. |

### `internal/interfaces/cli/ui/input_collector.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `CLIInputCollector` | L30–L34 | 🔴 DEAD | 95% | Exported struct with 0 external importers. Compile-time interface assertion on L21 is local and does not constitute external usage. |
| `NewCLIInputCollector` | L38–L44 | 🔴 DEAD | 95% | Exported constructor with 0 external importers. No file outside this package calls it. |
| `PromptForInput` | L59–L121 | 🔴 DEAD | 90% | Exported method with 0 external importers. Satisfies ports.InputCollector interface but no caller instantiates CLIInputCollector to invoke it. |

### `internal/interfaces/cli/validate.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `newValidateCommand` | L26–L72 | 🔴 DEAD | 70% | Not called anywhere in this file. Standard CLI command constructor — almost certainly registered in a sibling file (e.g. root.go), but no in-file reference exists per the provided content. |
| `validateSkillRefs` | L257–L311 | 🔴 DEAD | 70% | Not called anywhere in this file. Likely invoked from a sibling file in the same package, but no in-file reference exists. |
| `validateRoleRefs` | L313–L373 | 🔴 DEAD | 70% | Not called anywhere in this file. Likely invoked from a sibling file in the same package, but no in-file reference exists. |

### `internal/interfaces/tui/tab_monitoring.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `newMonitoringTab` | L146–L166 | 🔴 DEAD | 60% | Not called anywhere in this file. Likely called from model.go (same package), but no local evidence; cannot confirm without seeing that file. |
| `RenderEventsToTUIMsgs` | L453–L460 | 🔴 DEAD | 65% | 0 cross-package importers and not called anywhere in this file. No comments reference a caller; appears to be an unused debug/test helper. |
| `selectedStepOutput` | L877–L904 | 🔴 DEAD | 62% | Not called anywhere in this file. May be used from model.go (same package), but no local evidence; the viewport is driven by updateViewportContent/renderSelectedStepChat instead. |

### `internal/interfaces/tui/tab_workflows.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `newWorkflowsTab` | L85–L102 | 🔴 DEAD | 65% | Not called anywhere in this file. Likely called from a sibling tui package file (e.g., model initializer), but no evidence in the provided content. |
| `setWorkflows` | L104–L116 | 🔴 DEAD | 65% | Not called anywhere in this file. Pointer receiver suggests it mutates state on behalf of an external caller, but no invocation is present in the provided content. |
| `InputActive` | L393–L395 | 🔴 DEAD | 65% | Not called within this file and has 0 cross-package importers. Likely consumed by a sibling tui file to suppress global keybindings, but no evidence in the provided content. |

### `pkg/interpolation/template_helpers.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `splitTemplateFunc` | L17–L23 | 🔴 DEAD | 68% | Not called or referenced anywhere in this file. Not exported. The nolint comment claims FuncMap registration in another file (TemplateResolver), but that file is not provided — cannot confirm. Confidence reduced due to that hint. |
| `joinTemplateFunc` | L28–L30 | 🔴 DEAD | 68% | Not called or referenced in this file. Not exported. Same FuncMap-registration claim as sibling functions; unverifiable without the TemplateResolver source. |
| `readFileTemplateFunc` | L35–L61 | 🔴 DEAD | 68% | Not called or referenced in this file. Not exported. nolint comment asserts FuncMap registration elsewhere, but no confirming source is provided. |
| `trimSpaceTemplateFunc` | L66–L68 | 🔴 DEAD | 68% | Not called or referenced in this file. Not exported. Same unverifiable FuncMap-registration claim as sibling functions. |

### `pkg/interpolation/template_resolver.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `TemplateResolver` | L12–L12 | 🔴 DEAD | 95% | 0 cross-package importers. Though it implements a Resolver interface (per comment), no external package references this type. |
| `NewTemplateResolver` | L15–L17 | 🔴 DEAD | 95% | 0 cross-package importers. Constructor for TemplateResolver; no external package calls it. |
| `Resolve` | L20–L58 | 🔴 DEAD | 90% | 0 cross-package importers. Core method implementing the Resolver interface but not referenced externally. |

### `internal/infrastructure/github/client.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Client` | L16–L31 | 🔴 DEAD | 92% | Exported struct with 0 runtime and 0 type-only importers per pre-computed analysis. |
| `NewClient` | L40–L44 | 🔴 DEAD | 92% | Exported constructor with 0 importers. No file instantiates Client via this function. |
| `RunGH` | L55–L71 | 🔴 DEAD | 90% | Exported method with 0 importers. Not called from any file outside this package. |
| `DetectRepo` | L82–L99 | 🔴 DEAD | 91% | Exported method with 0 importers. Not called from any file outside this package. |

## ⚡ Quick Wins

- [ ] <!-- ACT-92adf7-2 --> **[utility · high · trivial]** `internal/infrastructure/github/client.go`: Remove dead code: `Client` is exported but unused `Client`, `NewClient`, `RunGH`, `DetectRepo` (`Client, NewClient, RunGH, DetectRepo`) [L16-L31, L40-L44, L55-L71, L82-L99]
- [ ] <!-- ACT-a31dbf-2 --> **[utility · high · trivial]** `internal/interfaces/api/sse.go`: Remove dead code: `NewSSEHandler` is exported but unused `NewSSEHandler`, `SetSessionLookup`, `RegisterSSERoutes`, `ProjectEventToSSEFrame` (`NewSSEHandler, SetSessionLookup, RegisterSSERoutes, ProjectEventToSSEFrame`) [L98-L100, L104-L106, L213-L222, L227-L231]
- [ ] <!-- ACT-560b83-1 --> **[utility · high · trivial]** `internal/interfaces/cli/root.go`: Remove dead code: `App` is exported but unused `App`, `NewApp`, `NewRootCommand`, `NewRootCommandAutoFacade` (`App, NewApp, NewRootCommand, NewRootCommandAutoFacade`) [L19-L22, L24-L33, L35-L38, L44-L46]
- [ ] <!-- ACT-8b91c6-2 --> **[utility · high · trivial]** `internal/interfaces/cli/ui/input_collector.go`: Remove dead code: `CLIInputCollector` is exported but unused `CLIInputCollector`, `NewCLIInputCollector`, `PromptForInput` (`CLIInputCollector, NewCLIInputCollector, PromptForInput`) [L30-L34, L38-L44, L59-L121]
- [ ] <!-- ACT-bfd0b7-2 --> **[utility · high · trivial]** `pkg/interpolation/template_resolver.go`: Remove dead code: `TemplateResolver` is exported but unused `TemplateResolver`, `NewTemplateResolver`, `Resolve` (`TemplateResolver, NewTemplateResolver, Resolve`) [L12-L12, L15-L17, L20-L58]

## 🔧 Refactors

- [ ] <!-- ACT-91ea58-1 --> **[utility · medium · trivial]** `internal/interfaces/cli/acp_wiring.go`: Remove dead code: `newACPTextWriter` is exported but unused `newACPTextWriter`, `MissedEmits`, `newStreamFlaggingEmitter`, `sharedHistoryStore` (`newACPTextWriter, MissedEmits, newStreamFlaggingEmitter, sharedHistoryStore`) [L34-L36, L61-L63, L75-L77, L96-L98]
- [ ] <!-- ACT-0c2be4-2 --> **[utility · medium · trivial]** `internal/interfaces/cli/validate.go`: Remove dead code: `newValidateCommand` is exported but unused `newValidateCommand`, `validateSkillRefs`, `validateRoleRefs` (`newValidateCommand, validateSkillRefs, validateRoleRefs`) [L26-L72, L257-L311, L313-L373]
- [ ] <!-- ACT-76f7c6-2 --> **[utility · medium · trivial]** `internal/interfaces/tui/tab_monitoring.go`: Remove dead code: `newMonitoringTab` is exported but unused `newMonitoringTab`, `RenderEventsToTUIMsgs`, `selectedStepOutput` (`newMonitoringTab, RenderEventsToTUIMsgs, selectedStepOutput`) [L146-L166, L453-L460, L877-L904]
- [ ] <!-- ACT-ed9b25-2 --> **[utility · medium · trivial]** `internal/interfaces/tui/tab_workflows.go`: Remove dead code: `newWorkflowsTab` is exported but unused `newWorkflowsTab`, `setWorkflows`, `InputActive` (`newWorkflowsTab, setWorkflows, InputActive`) [L85-L102, L104-L116, L393-L395]
- [ ] <!-- ACT-2ce3c2-1 --> **[utility · medium · trivial]** `pkg/interpolation/template_helpers.go`: Remove dead code: `splitTemplateFunc` is exported but unused `splitTemplateFunc`, `joinTemplateFunc`, `readFileTemplateFunc`, `trimSpaceTemplateFunc` (`splitTemplateFunc, joinTemplateFunc, readFileTemplateFunc, trimSpaceTemplateFunc`) [L17-L23, L28-L30, L35-L61, L66-L68]
