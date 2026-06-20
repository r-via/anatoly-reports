[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 20

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `pkg/plugin/sdk/step_type.go` | 🟡 NEEDS_REFACTOR | 4 | 75% | [details](#pkgpluginsdksteptypego) |
| `internal/application/error_codes.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalapplicationerrorcodesgo) |
| `internal/application/hook_executor.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalapplicationhookexecutorgo) |
| `internal/application/input_collection_service.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalapplicationinputcollectionservicego) |
| `internal/application/resolver.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalapplicationresolvergo) |
| `internal/application/role_loader.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalapplicationroleloadergo) |
| `internal/application/session_input_reader.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalapplicationsessioninputreadergo) |
| `internal/application/tools/config.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalapplicationtoolsconfiggo) |
| `internal/domain/errors/catalog.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internaldomainerrorscataloggo) |
| `internal/domain/operation/validate.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internaldomainoperationvalidatego) |

## 🔍 Symbol Details

### `pkg/plugin/sdk/step_type.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `StepTypeInfo` | L12–L15 | 🔴 DEAD | 75% | 0 external importers. Locally referenced in StepTypeHandler interface (L38) and ListStepTypes (L57-61), but no package outside this file imports it — if the SDK is unused externally, this type is dead. |
| `StepExecuteRequest` | L18–L23 | 🔴 DEAD | 75% | 0 external importers. Locally used in StepTypeHandler interface (L39) and constructed in ExecuteStep (L86-91), but no external package imports it. |
| `StepExecuteResult` | L26–L30 | 🔴 DEAD | 75% | 0 external importers. Appears as return type in StepTypeHandler interface (L39) and bound as result in ExecuteStep (L86), but no external package imports it. |
| `StepTypeHandler` | L37–L40 | 🔴 DEAD | 75% | 0 external importers. Type-asserted locally in both ListStepTypes (L50) and ExecuteStep (L67), but no plugin author or other package imports or implements this interface. |

### `internal/application/error_codes.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ErrInternal` | L11–L11 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MapError` | L15–L24 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ExitCode` | L37–L56 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/application/hook_executor.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `HookExecutor` | L13–L17 | 🔴 DEAD | 95% | Exported struct with 0 runtime and 0 type-only importers across the codebase. |
| `NewHookExecutor` | L19–L29 | 🔴 DEAD | 95% | Exported constructor with 0 importers; no external caller instantiates HookExecutor. |
| `ExecuteHooks` | L34–L58 | 🔴 DEAD | 92% | Exported method with 0 importers; never called from outside this package. |

### `internal/application/input_collection_service.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `InputCollectionService` | L27–L30 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewInputCollectionService` | L33–L41 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `CollectMissingInputs` | L59–L94 | 🔴 DEAD | 92% | Exported but imported by 0 files |

### `internal/application/resolver.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Resolver` | L18–L21 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewResolver` | L23–L28 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Resolve` | L32–L109 | 🔴 DEAD | 90% | Exported but imported by 0 files |

### `internal/application/role_loader.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ResolveAgentRole` | L20–L43 | 🔴 DEAD | 88% | Exported with 0 external importers. Called locally only by BuildRoleSystemPrompt (L103), which is itself dead — no external entry point reaches this function. |
| `BuildRoleSystemPrompt` | L75–L113 | 🔴 DEAD | 78% | Exported with 0 external importers and no local callers. Root of the dead call chain in this file. |
| `ComposeSystemPrompt` | L117–L125 | 🔴 DEAD | 88% | Exported with 0 external importers. Called locally at L100, L107, L112, but all call sites are inside BuildRoleSystemPrompt, which is itself dead. |

### `internal/application/session_input_reader.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `withUserInputReader` | L20–L25 | 🔴 DEAD | 70% | Not called anywhere in this file. May be used by sibling package files, but per local-only check it has no callers. |
| `userInputReaderFrom` | L29–L34 | 🔴 DEAD | 70% | Not called anywhere in this file. May be used by sibling package files, but per local-only check it has no callers. |
| `newSessionInputReader` | L51–L53 | 🔴 DEAD | 70% | Not called anywhere in this file. May be used by sibling package files, but per local-only check it has no callers. |

### `internal/application/tools/config.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `CoexistenceProviders` | L13–L15 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ProxyConfig` | L20–L24 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `PluginToolSpec` | L31–L34 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/errors/catalog.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `CatalogEntry` | L6–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetCatalogEntry` | L120–L123 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `AllErrorCodes` | L127–L133 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/operation/validate.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ValidateInputs` | L24–L50 | 🔴 DEAD | 95% | Exported but imported by 0 files per pre-computed analysis. No internal callers either. |

## ⚡ Quick Wins

- [ ] <!-- ACT-c4463b-1 --> **[utility · high · trivial]** `internal/application/error_codes.go`: Remove dead code: `ErrInternal` is exported but unused `ErrInternal`, `MapError`, `ExitCode` (`ErrInternal, MapError, ExitCode`) [L11-L11, L15-L24, L37-L56]
- [ ] <!-- ACT-1c60b6-1 --> **[utility · high · trivial]** `internal/application/hook_executor.go`: Remove dead code: `HookExecutor` is exported but unused `HookExecutor`, `NewHookExecutor`, `ExecuteHooks` (`HookExecutor, NewHookExecutor, ExecuteHooks`) [L13-L17, L19-L29, L34-L58]
- [ ] <!-- ACT-cbb213-1 --> **[utility · high · trivial]** `internal/application/input_collection_service.go`: Remove dead code: `InputCollectionService` is exported but unused `InputCollectionService`, `NewInputCollectionService`, `CollectMissingInputs` (`InputCollectionService, NewInputCollectionService, CollectMissingInputs`) [L27-L30, L33-L41, L59-L94]
- [ ] <!-- ACT-f9f4ae-1 --> **[utility · high · trivial]** `internal/application/resolver.go`: Remove dead code: `Resolver` is exported but unused `Resolver`, `NewResolver`, `Resolve` (`Resolver, NewResolver, Resolve`) [L18-L21, L23-L28, L32-L109]
- [ ] <!-- ACT-dfb9b1-2 --> **[utility · high · trivial]** `internal/application/role_loader.go`: Remove dead code: `ResolveAgentRole` is exported but unused (`ResolveAgentRole`) [L20-L43]
- [ ] <!-- ACT-dfb9b1-3 --> **[utility · high · trivial]** `internal/application/role_loader.go`: Remove dead code: `ComposeSystemPrompt` is exported but unused (`ComposeSystemPrompt`) [L117-L125]
- [ ] <!-- ACT-3a9eb6-1 --> **[utility · high · trivial]** `internal/application/tools/config.go`: Remove dead code: `CoexistenceProviders` is exported but unused `CoexistenceProviders`, `ProxyConfig`, `PluginToolSpec` (`CoexistenceProviders, ProxyConfig, PluginToolSpec`) [L13-L15, L20-L24, L31-L34]
- [ ] <!-- ACT-3de8f7-2 --> **[utility · high · trivial]** `internal/domain/errors/catalog.go`: Remove dead code: `CatalogEntry` is exported but unused `CatalogEntry`, `GetCatalogEntry`, `AllErrorCodes` (`CatalogEntry, GetCatalogEntry, AllErrorCodes`) [L6-L18, L120-L123, L127-L133]
- [ ] <!-- ACT-92d6da-3 --> **[utility · high · trivial]** `internal/domain/operation/validate.go`: Remove dead code: `ValidateInputs` is exported but unused (`ValidateInputs`) [L24-L50]

## 🔧 Refactors

- [ ] <!-- ACT-dfb9b1-4 --> **[utility · medium · trivial]** `internal/application/role_loader.go`: Remove dead code: `BuildRoleSystemPrompt` is exported but unused (`BuildRoleSystemPrompt`) [L75-L113]
- [ ] <!-- ACT-bf30f2-1 --> **[utility · medium · trivial]** `internal/application/session_input_reader.go`: Remove dead code: `withUserInputReader` is exported but unused `withUserInputReader`, `userInputReaderFrom`, `newSessionInputReader` (`withUserInputReader, userInputReaderFrom, newSessionInputReader`) [L20-L25, L29-L34, L51-L53]
- [ ] <!-- ACT-aa4526-1 --> **[utility · medium · trivial]** `pkg/plugin/sdk/step_type.go`: Remove dead code: `StepTypeInfo` is exported but unused `StepTypeInfo`, `StepExecuteRequest`, `StepExecuteResult`, `StepTypeHandler` (`StepTypeInfo, StepExecuteRequest, StepExecuteResult, StepTypeHandler`) [L12-L15, L18-L23, L26-L30, L37-L40]
