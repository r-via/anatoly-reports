[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 18

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/infrastructure/config/types.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinfrastructureconfigtypesgo) |
| `internal/infrastructure/expression/expr_evaluator.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinfrastructureexpressionexprevaluatorgo) |
| `internal/infrastructure/github/auth.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinfrastructuregithubauthgo) |
| `internal/infrastructure/notify/types.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinfrastructurenotifytypesgo) |
| `internal/infrastructure/roles/filesystem_repository.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinfrastructurerolesfilesystemrepositorygo) |
| `internal/infrastructure/skills/filesystem_repository.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinfrastructureskillsfilesystemrepositorygo) |
| `internal/infrastructure/transcript/jsonl_writer.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinfrastructuretranscriptjsonlwritergo) |
| `internal/infrastructure/transcript/reader.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinfrastructuretranscriptreadergo) |
| `internal/infrastructure/workflowpkg/discoverer.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinfrastructureworkflowpkgdiscoverergo) |
| `internal/infrastructure/workflowpkg/loader.go` | 🟡 NEEDS_REFACTOR | 4 | 95% | [details](#internalinfrastructureworkflowpkgloadergo) |

## 🔍 Symbol Details

### `internal/infrastructure/config/types.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ProjectConfig` | L7–L25 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ConfigError` | L28–L33 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L36–L41 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Unwrap` | L44–L46 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/expression/expr_evaluator.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ExprEvaluator` | L16–L18 | 🔴 DEAD | 95% | 0 external importers. Only local reference is the compile-time interface assertion on L121, which is not runtime usage. |
| `NewExprEvaluator` | L21–L25 | 🔴 DEAD | 95% | 0 external importers. No file constructs an ExprEvaluator via this constructor, so the concrete type is never instantiated outside this package. |
| `EvaluateBool` | L29–L41 | 🔴 DEAD | 90% | 0 external importers. Satisfies ports.ExpressionEvaluator interface but NewExprEvaluator is itself unused, so the interface value is never created and this method is never dispatched. |
| `EvaluateInt` | L46–L95 | 🔴 DEAD | 75% | 0 external importers. Same reasoning as EvaluateBool — interface implementation is unreachable because the constructor is never called externally. |

### `internal/infrastructure/github/auth.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `AuthMethod` | L13–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `String` | L24–L35 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `IsAuthenticated` | L39–L41 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `DetectAuth` | L51–L80 | 🔴 DEAD | 88% | Exported but imported by 0 files |

### `internal/infrastructure/notify/types.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `NotificationPayload` | L8–L21 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `BackendResult` | L25–L34 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Backend` | L39–L44 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NotifyConfig` | L49–L53 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/roles/filesystem_repository.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `FilesystemAgentRoleRepository` | L16–L19 | 🔴 DEAD | 95% | Exported struct with 0 runtime and 0 type-only importers across the codebase. |
| `NewFilesystemAgentRoleRepository` | L21–L49 | 🔴 DEAD | 95% | Exported constructor with 0 importers; nothing instantiates this repository. |
| `Load` | L51–L68 | 🔴 DEAD | 90% | Exported method with 0 importers; no caller invokes it outside this package. |
| `LoadFromPath` | L70–L87 | 🔴 DEAD | 88% | Exported method with 0 importers; no caller invokes it outside this package. |

### `internal/infrastructure/skills/filesystem_repository.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `FilesystemSkillRepository` | L22–L25 | 🔴 DEAD | 95% | Exported struct with 0 external importers. Used only as a receiver type within this file; nothing outside this package constructs or references it. |
| `NewFilesystemSkillRepository` | L27–L52 | 🔴 DEAD | 95% | Exported constructor with 0 external importers. No caller wires this repository into any application component. |
| `Load` | L54–L73 | 🔴 DEAD | 95% | Exported method with 0 external importers. Likely satisfies a ports interface, but the owning struct is itself never instantiated externally. |
| `LoadFromPath` | L75–L84 | 🔴 DEAD | 95% | Exported method with 0 external importers; same dead-struct situation as Load. |

### `internal/infrastructure/transcript/jsonl_writer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `JSONLWriter` | L16–L22 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewJSONLWriter` | L26–L38 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Write` | L42–L62 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `Close` | L65–L70 | 🔴 DEAD | 92% | Exported but imported by 0 files |

### `internal/infrastructure/transcript/reader.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Reader` | L16–L19 | 🔴 DEAD | 95% | Exported struct with 0 runtime and 0 type-only importers per exhaustive import analysis. |
| `NewReader` | L25–L28 | 🔴 DEAD | 95% | Exported constructor with 0 importers; no external code instantiates Reader. |
| `Read` | L30–L61 | 🔴 DEAD | 90% | Exported method with 0 external importers. Only caller is ReadAll (itself dead). |
| `ReadAll` | L63–L75 | 🔴 DEAD | 95% | Exported method with 0 importers; entire Reader API is unreferenced externally. |

### `internal/infrastructure/workflowpkg/discoverer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `PackDiscovererAdapter` | L20–L23 | 🔴 DEAD | 95% | Exported type with 0 importers. Despite the comment claiming it implements ports.PackDiscoverer, no file imports or references this type. |
| `NewPackDiscovererAdapter` | L27–L32 | 🔴 DEAD | 95% | Exported constructor with 0 importers. No wiring site instantiates PackDiscovererAdapter through this function. |
| `DiscoverWorkflows` | L38–L114 | 🔴 DEAD | 70% | Exported method with 0 importers. Not called directly or through an interface by any file in the import analysis. |
| `LoadWorkflow` | L122–L145 | 🔴 DEAD | 65% | Exported method with 0 importers. No caller site found in the exhaustive import analysis. |

### `internal/infrastructure/workflowpkg/loader.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `PackLoader` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewPackLoader` | L15–L17 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DiscoverPacks` | L23–L90 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `LoadPackState` | L94–L108 | 🔴 DEAD | 95% | Exported but imported by 0 files |

## ⚡ Quick Wins

- [ ] <!-- ACT-67825a-1 --> **[utility · high · trivial]** `internal/infrastructure/config/types.go`: Remove dead code: `ProjectConfig` is exported but unused `ProjectConfig`, `ConfigError`, `Error`, `Unwrap` (`ProjectConfig, ConfigError, Error, Unwrap`) [L7-L25, L28-L33, L36-L41, L44-L46]
- [ ] <!-- ACT-5f1cd7-2 --> **[utility · high · trivial]** `internal/infrastructure/expression/expr_evaluator.go`: Remove dead code: `ExprEvaluator` is exported but unused `ExprEvaluator`, `NewExprEvaluator`, `EvaluateBool` (`ExprEvaluator, NewExprEvaluator, EvaluateBool`) [L16-L18, L21-L25, L29-L41]
- [ ] <!-- ACT-6e6b2d-2 --> **[utility · high · trivial]** `internal/infrastructure/github/auth.go`: Remove dead code: `AuthMethod` is exported but unused `AuthMethod`, `String`, `IsAuthenticated`, `DetectAuth` (`AuthMethod, String, IsAuthenticated, DetectAuth`) [L13-L20, L24-L35, L39-L41, L51-L80]
- [ ] <!-- ACT-a74926-1 --> **[utility · high · trivial]** `internal/infrastructure/notify/types.go`: Remove dead code: `NotificationPayload` is exported but unused `NotificationPayload`, `BackendResult`, `Backend`, `NotifyConfig` (`NotificationPayload, BackendResult, Backend, NotifyConfig`) [L8-L21, L25-L34, L39-L44, L49-L53]
- [ ] <!-- ACT-41b9fb-2 --> **[utility · high · trivial]** `internal/infrastructure/roles/filesystem_repository.go`: Remove dead code: `FilesystemAgentRoleRepository` is exported but unused `FilesystemAgentRoleRepository`, `NewFilesystemAgentRoleRepository`, `Load`, `LoadFromPath` (`FilesystemAgentRoleRepository, NewFilesystemAgentRoleRepository, Load, LoadFromPath`) [L16-L19, L21-L49, L51-L68, L70-L87]
- [ ] <!-- ACT-600759-1 --> **[utility · high · trivial]** `internal/infrastructure/skills/filesystem_repository.go`: Remove dead code: `FilesystemSkillRepository` is exported but unused `FilesystemSkillRepository`, `NewFilesystemSkillRepository`, `Load`, `LoadFromPath` (`FilesystemSkillRepository, NewFilesystemSkillRepository, Load, LoadFromPath`) [L22-L25, L27-L52, L54-L73, L75-L84]
- [ ] <!-- ACT-7b183c-2 --> **[utility · high · trivial]** `internal/infrastructure/transcript/jsonl_writer.go`: Remove dead code: `JSONLWriter` is exported but unused `JSONLWriter`, `NewJSONLWriter`, `Write`, `Close` (`JSONLWriter, NewJSONLWriter, Write, Close`) [L16-L22, L26-L38, L42-L62, L65-L70]
- [ ] <!-- ACT-fa3bc2-1 --> **[utility · high · trivial]** `internal/infrastructure/transcript/reader.go`: Remove dead code: `Reader` is exported but unused `Reader`, `NewReader`, `Read`, `ReadAll` (`Reader, NewReader, Read, ReadAll`) [L16-L19, L25-L28, L30-L61, L63-L75]
- [ ] <!-- ACT-7ec5ec-4 --> **[utility · high · trivial]** `internal/infrastructure/workflowpkg/discoverer.go`: Remove dead code: `PackDiscovererAdapter` is exported but unused (`PackDiscovererAdapter`) [L20-L23]
- [ ] <!-- ACT-7ec5ec-5 --> **[utility · high · trivial]** `internal/infrastructure/workflowpkg/discoverer.go`: Remove dead code: `NewPackDiscovererAdapter` is exported but unused (`NewPackDiscovererAdapter`) [L27-L32]
- [ ] <!-- ACT-50e064-1 --> **[utility · high · trivial]** `internal/infrastructure/workflowpkg/loader.go`: Remove dead code: `PackLoader` is exported but unused `PackLoader`, `NewPackLoader`, `DiscoverPacks`, `LoadPackState` (`PackLoader, NewPackLoader, DiscoverPacks, LoadPackState`) [L12-L12, L15-L17, L23-L90, L94-L108]

## 🔧 Refactors

- [ ] <!-- ACT-5f1cd7-3 --> **[utility · medium · trivial]** `internal/infrastructure/expression/expr_evaluator.go`: Remove dead code: `EvaluateInt` is exported but unused (`EvaluateInt`) [L46-L95]
- [ ] <!-- ACT-7ec5ec-6 --> **[utility · medium · trivial]** `internal/infrastructure/workflowpkg/discoverer.go`: Remove dead code: `DiscoverWorkflows` is exported but unused (`DiscoverWorkflows`) [L38-L114]
- [ ] <!-- ACT-7ec5ec-7 --> **[utility · medium · trivial]** `internal/infrastructure/workflowpkg/discoverer.go`: Remove dead code: `LoadWorkflow` is exported but unused (`LoadWorkflow`) [L122-L145]
