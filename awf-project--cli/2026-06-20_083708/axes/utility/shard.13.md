[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 13

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/infrastructure/config/loader.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internalinfrastructureconfigloadergo) |
| `internal/infrastructure/diagram/dot_generator.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinfrastructurediagramdotgeneratorgo) |
| `internal/infrastructure/http/provider.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinfrastructurehttpprovidergo) |
| `internal/infrastructure/pluginmgr/grpc_validator.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internalinfrastructurepluginmgrgrpcvalidatorgo) |
| `internal/infrastructure/store/json_store.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internalinfrastructurestorejsonstorego) |
| `internal/infrastructure/workflowpkg/types.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinfrastructureworkflowpkgtypesgo) |
| `internal/interfaces/api/handlers_workflows.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internalinterfacesapihandlersworkflowsgo) |
| `internal/interfaces/api/respond_handler.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internalinterfacesapirespondhandlergo) |
| `internal/interfaces/tui/components.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internalinterfacestuicomponentsgo) |
| `internal/testutil/fixtures/cli_fixtures.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internaltestutilfixturesclifixturesgo) |

## 🔍 Symbol Details

### `internal/infrastructure/config/loader.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `WarningFunc` | L22–L22 | 🔴 DEAD | 90% | Exported type with 0 cross-file importers. Referenced only within this file as the type of YAMLConfigLoader.warnFn and WithWarningFunc parameter, both of which are themselves dead. |
| `YAMLConfigLoader` | L26–L29 | 🔴 DEAD | 95% | Exported struct with 0 importers across the codebase. |
| `NewYAMLConfigLoader` | L32–L34 | 🔴 DEAD | 95% | Exported constructor with 0 importers; no callers instantiate YAMLConfigLoader. |
| `WithWarningFunc` | L38–L41 | 🔴 DEAD | 95% | Exported method with 0 importers; no caller sets a warning callback. |
| `Path` | L44–L46 | 🔴 DEAD | 95% | Exported accessor with 0 importers; nothing reads the loader path externally. |
| `Load` | L56–L84 | 🔴 DEAD | 95% | Exported method with 0 importers; the entire loader is never invoked. |

### `internal/infrastructure/diagram/dot_generator.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Generator` | L12–L14 | 🔴 DEAD | 95% | Exported struct with 0 runtime and 0 type-only importers across the codebase. |
| `NewGenerator` | L17–L22 | 🔴 DEAD | 95% | Exported constructor with 0 importers; no external caller instantiates Generator. |
| `Generate` | L25–L35 | 🔴 DEAD | 95% | Exported method with 0 importers; public entry point never called externally. |

### `internal/infrastructure/http/provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `HTTPOperationProvider` | L29–L35 | 🔴 DEAD | 95% | Exported struct with 0 external importers. Compile-time interface check on L17 is within the same package and does not constitute external usage. |
| `NewHTTPOperationProvider` | L37–L50 | 🔴 DEAD | 95% | Exported constructor with 0 external importers. No external callers instantiate this provider. |
| `GetOperation` | L52–L55 | 🔴 DEAD | 95% | Exported method with 0 external importers. Satisfies ports.OperationProvider interface but the entire type is externally unused. |
| `ListOperations` | L57–L63 | 🔴 DEAD | 95% | Exported method with 0 external importers. Interface implementation with no external callers. |
| `Execute` | L69–L77 | 🔴 DEAD | 95% | Exported method with 0 external importers. Entry point to handleHTTPRequest but never called from outside the package. |

### `internal/infrastructure/pluginmgr/grpc_validator.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `newGRPCValidatorAdapter` | L24–L33 | 🔴 DEAD | 90% | Not exported and never called within this file. No external importers per pre-computed analysis. |
| `ValidateWorkflow` | L35–L47 | 🔴 DEAD | 88% | compositeValidatorProvider is never instantiated, so this method is never reachable. 0 external importers per pre-computed analysis. |
| `ValidateStep` | L49–L62 | 🔴 DEAD | 88% | compositeValidatorProvider is never instantiated, so this method is never reachable. 0 external importers per pre-computed analysis. |
| `compositeValidatorProvider` | L103–L105 | 🔴 DEAD | 88% | Never instantiated in this file; not exported; 0 external importers. The compile-time interface assertion at L107 is purely static and does not constitute runtime usage. |
| `ValidateWorkflow` | L109–L120 | 🔴 DEAD | 88% | compositeValidatorProvider is never instantiated, so this method is never reachable. 0 external importers per pre-computed analysis. |
| `ValidateStep` | L122–L132 | 🔴 DEAD | 88% | compositeValidatorProvider is never instantiated, so this method is never reachable. 0 external importers per pre-computed analysis. |

### `internal/infrastructure/store/json_store.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `JSONStore` | L17–L19 | 🔴 DEAD | 95% | Exported struct with 0 importers across the codebase. |
| `NewJSONStore` | L22–L24 | 🔴 DEAD | 95% | Exported constructor with 0 importers; JSONStore is never instantiated externally. |
| `Save` | L28–L71 | 🔴 DEAD | 92% | Exported method with 0 importers; never called through any interface or direct reference. |
| `Load` | L74–L91 | 🔴 DEAD | 95% | Exported method with 0 importers; never called through any interface or direct reference. |
| `Delete` | L94–L102 | 🔴 DEAD | 95% | Exported method with 0 importers; never called through any interface or direct reference. |
| `List` | L105–L119 | 🔴 DEAD | 92% | Exported method with 0 importers; never called through any interface or direct reference. |

### `internal/infrastructure/workflowpkg/types.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `PackSource` | L10–L15 | 🔴 DEAD | 95% | 0 importers per pre-computed analysis. Only referenced locally as the parameter type of SourceDataFromPackSource and return type of PackSourceFromSourceData, both of which are also dead. |
| `PackInfo` | L18–L26 | 🔴 DEAD | 95% | 0 importers per pre-computed analysis. Not referenced anywhere in this file. |
| `PackState` | L30–L34 | 🔴 DEAD | 95% | 0 importers per pre-computed analysis. Not referenced anywhere in this file. |
| `SourceDataFromPackSource` | L37–L47 | 🔴 DEAD | 95% | 0 importers per pre-computed analysis. Not called within this file. |
| `PackSourceFromSourceData` | L51–L94 | 🔴 DEAD | 90% | 0 importers per pre-computed analysis. Not called within this file. |

### `internal/interfaces/api/handlers_workflows.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `WorkflowHandlers` | L23–L26 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewWorkflowHandlers` | L31–L33 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `List` | L35–L57 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `Get` | L59–L80 | 🔴 DEAD | 93% | Exported but imported by 0 files |
| `Validate` | L82–L118 | 🔴 DEAD | 93% | Exported but imported by 0 files |
| `RegisterWorkflowRoutes` | L121–L142 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/interfaces/api/respond_handler.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `RespondInput` | L14–L19 | 🔴 DEAD | 95% | 0 external importers per pre-computed analysis. Referenced only within this file in the Respond method signature, which itself belongs to the dead entry-point chain. |
| `RespondHandler` | L22–L25 | 🔴 DEAD | 95% | 0 external importers. Struct is used only within this file; no external package instantiates or references it. |
| `NewRespondHandler` | L28–L30 | 🔴 DEAD | 95% | 0 external importers and not called anywhere within this file. No consumer constructs a RespondHandler. |
| `SetSessionLookup` | L34–L36 | 🔴 DEAD | 95% | 0 external importers and not invoked within this file. Session registry is never wired up. |
| `Respond` | L39–L56 | 🔴 DEAD | 95% | 0 external importers. Only reference is in RegisterRespondRoutes (L80) via h.Respond, but that function is itself dead (0 importers), so the route is never registered. |
| `RegisterRespondRoutes` | L72–L80 | 🔴 DEAD | 95% | 0 external importers. The route registration entry point is never called, leaving the entire handler chain unreachable at runtime. |

### `internal/interfaces/tui/components.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `StatusBadge` | L13–L28 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StatusBadgeFromString` | L31–L42 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Panel` | L45–L64 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `EmptyStateView` | L67–L77 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `HeaderBar` | L80–L95 | 🔴 DEAD | 88% | Exported but imported by 0 files |
| `Separator` | L98–L103 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/testutil/fixtures/cli_fixtures.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `SimpleWorkflowYAML` | L34–L43 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `FullWorkflowYAML` | L47–L65 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `BadWorkflowYAML` | L69–L78 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `SetupTestDir` | L103–L125 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `CreateTestWorkflow` | L146–L159 | 🔴 DEAD | 85% | Exported but imported by 0 files |
| `SetupWorkflowsDir` | L177–L189 | 🔴 DEAD | 90% | Exported but imported by 0 files |

## ⚡ Quick Wins

- [ ] <!-- ACT-c0c926-1 --> **[utility · high · trivial]** `internal/infrastructure/config/loader.go`: Remove dead code: `WarningFunc` is exported but unused `WarningFunc`, `YAMLConfigLoader`, `NewYAMLConfigLoader`, `WithWarningFunc`, `Path`, `Load` (`WarningFunc, YAMLConfigLoader, NewYAMLConfigLoader, WithWarningFunc, Path, Load`) [L22-L22, L26-L29, L32-L34, L38-L41, L44-L46, L56-L84]
- [ ] <!-- ACT-f8ba04-4 --> **[utility · high · trivial]** `internal/infrastructure/diagram/dot_generator.go`: Remove dead code: `Generator` is exported but unused `Generator`, `NewGenerator`, `Generate` (`Generator, NewGenerator, Generate`) [L12-L14, L17-L22, L25-L35]
- [ ] <!-- ACT-6ab135-2 --> **[utility · high · trivial]** `internal/infrastructure/http/provider.go`: Remove dead code: `HTTPOperationProvider` is exported but unused `HTTPOperationProvider`, `NewHTTPOperationProvider`, `GetOperation`, `ListOperations`, `Execute` (`HTTPOperationProvider, NewHTTPOperationProvider, GetOperation, ListOperations, Execute`) [L29-L35, L37-L50, L52-L55, L57-L63, L69-L77]
- [ ] <!-- ACT-d7532e-1 --> **[utility · high · trivial]** `internal/infrastructure/pluginmgr/grpc_validator.go`: Remove dead code: `newGRPCValidatorAdapter` is exported but unused `newGRPCValidatorAdapter`, `ValidateWorkflow`, `ValidateStep`, `compositeValidatorProvider`, `ValidateWorkflow`, `ValidateStep` (`newGRPCValidatorAdapter, ValidateWorkflow, ValidateStep, compositeValidatorProvider, ValidateWorkflow, ValidateStep`) [L24-L33, L35-L47, L49-L62, L103-L105, L109-L120, L122-L132]
- [ ] <!-- ACT-0d6a52-1 --> **[utility · high · trivial]** `internal/infrastructure/store/json_store.go`: Remove dead code: `JSONStore` is exported but unused `JSONStore`, `NewJSONStore`, `Save`, `Load`, `Delete`, `List` (`JSONStore, NewJSONStore, Save, Load, Delete, List`) [L17-L19, L22-L24, L28-L71, L74-L91, L94-L102, L105-L119]
- [ ] <!-- ACT-82ad65-2 --> **[utility · high · trivial]** `internal/infrastructure/workflowpkg/types.go`: Remove dead code: `PackSource` is exported but unused `PackSource`, `PackInfo`, `PackState`, `SourceDataFromPackSource`, `PackSourceFromSourceData` (`PackSource, PackInfo, PackState, SourceDataFromPackSource, PackSourceFromSourceData`) [L10-L15, L18-L26, L30-L34, L37-L47, L51-L94]
- [ ] <!-- ACT-ec95b5-1 --> **[utility · high · trivial]** `internal/interfaces/api/handlers_workflows.go`: Remove dead code: `WorkflowHandlers` is exported but unused `WorkflowHandlers`, `NewWorkflowHandlers`, `List`, `Get`, `Validate`, `RegisterWorkflowRoutes` (`WorkflowHandlers, NewWorkflowHandlers, List, Get, Validate, RegisterWorkflowRoutes`) [L23-L26, L31-L33, L35-L57, L59-L80, L82-L118, L121-L142]
- [ ] <!-- ACT-67514b-1 --> **[utility · high · trivial]** `internal/interfaces/api/respond_handler.go`: Remove dead code: `RespondInput` is exported but unused `RespondInput`, `RespondHandler`, `NewRespondHandler`, `SetSessionLookup`, `Respond`, `RegisterRespondRoutes` (`RespondInput, RespondHandler, NewRespondHandler, SetSessionLookup, Respond, RegisterRespondRoutes`) [L14-L19, L22-L25, L28-L30, L34-L36, L39-L56, L72-L80]
- [ ] <!-- ACT-86e662-1 --> **[utility · high · trivial]** `internal/interfaces/tui/components.go`: Remove dead code: `StatusBadge` is exported but unused `StatusBadge`, `StatusBadgeFromString`, `Panel`, `EmptyStateView`, `HeaderBar`, `Separator` (`StatusBadge, StatusBadgeFromString, Panel, EmptyStateView, HeaderBar, Separator`) [L13-L28, L31-L42, L45-L64, L67-L77, L80-L95, L98-L103]
- [ ] <!-- ACT-2dd90b-2 --> **[utility · high · trivial]** `internal/testutil/fixtures/cli_fixtures.go`: Remove dead code: `SimpleWorkflowYAML` is exported but unused `SimpleWorkflowYAML`, `FullWorkflowYAML`, `BadWorkflowYAML`, `SetupTestDir`, `CreateTestWorkflow`, `SetupWorkflowsDir` (`SimpleWorkflowYAML, FullWorkflowYAML, BadWorkflowYAML, SetupTestDir, CreateTestWorkflow, SetupWorkflowsDir`) [L34-L43, L47-L65, L69-L78, L103-L125, L146-L159, L177-L189]
