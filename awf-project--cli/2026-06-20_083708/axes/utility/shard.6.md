[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 6

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/infrastructure/pluginmgr/rpc_manager.go` | 🟡 NEEDS_REFACTOR | 9 | 95% | [details](#internalinfrastructurepluginmgrrpcmanagergo) |
| `internal/infrastructure/transcript/fanout.go` | 🟡 NEEDS_REFACTOR | 11 | 95% | [details](#internalinfrastructuretranscriptfanoutgo) |
| `internal/interfaces/api/server.go` | 🟡 NEEDS_REFACTOR | 11 | 95% | [details](#internalinterfacesapiservergo) |
| `pkg/httpx/client.go` | 🟡 NEEDS_REFACTOR | 11 | 95% | [details](#pkghttpxclientgo) |
| `pkg/plugin/sdk/validator.go` | 🟡 NEEDS_REFACTOR | 11 | 95% | [details](#pkgpluginsdkvalidatorgo) |
| `pkg/registry/github_client.go` | 🟡 NEEDS_REFACTOR | 11 | 95% | [details](#pkgregistrygithubclientgo) |
| `internal/application/acp_errors.go` | 🟡 NEEDS_REFACTOR | 10 | 95% | [details](#internalapplicationacperrorsgo) |
| `internal/domain/workflow/condition.go` | 🟡 NEEDS_REFACTOR | 10 | 95% | [details](#internaldomainworkflowconditiongo) |
| `internal/domain/workflow/dry_run.go` | 🟡 NEEDS_REFACTOR | 10 | 95% | [details](#internaldomainworkflowdryrungo) |
| `internal/infrastructure/agents/helpers.go` | 🟡 NEEDS_REFACTOR | 9 | 95% | [details](#internalinfrastructureagentshelpersgo) |

## 🔍 Symbol Details

### `internal/infrastructure/pluginmgr/rpc_manager.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `DefaultPluginsDir` | L44–L44 | 🔴 DEAD | 95% | 0 external importers; not referenced anywhere in this file. |
| `NewRPCPluginManager` | L154–L161 | 🔴 DEAD | 90% | Constructor not called anywhere in this file; 0 external importers per exhaustive analysis. |
| `SetPluginsDir` | L222–L226 | 🔴 DEAD | 85% | Not called in this file; 0 external importers. SetPluginsDirs supersedes it for multi-dir configs. |
| `SetPluginsDirs` | L230–L234 | 🔴 DEAD | 85% | Not called in this file; 0 external importers per exhaustive analysis. |
| `SetHostVersion` | L956–L960 | 🔴 DEAD | 85% | Not called in this file; 0 external importers per exhaustive analysis. |
| `SetEventBus` | L964–L968 | 🔴 DEAD | 85% | Not called in this file; 0 external importers per exhaustive analysis. |
| `SetStreamManager` | L972–L976 | 🔴 DEAD | 85% | Not called in this file; 0 external importers per exhaustive analysis. |
| `SetStateStore` | L1026–L1030 | 🔴 DEAD | 85% | Not called in this file; 0 external importers per exhaustive analysis. |
| `SetZapLogger` | L1034–L1038 | 🔴 DEAD | 85% | Not called in this file; 0 external importers per exhaustive analysis. |

### `internal/infrastructure/transcript/fanout.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `FanOutOption` | L16–L16 | 🔴 DEAD | 95% | 0 external importers. Used only within this file as the variadic parameter type for NewFanOut and return type of WithBufferSize/WithLogger, but all three of those are also dead. |
| `FanOutStats` | L18–L21 | 🔴 DEAD | 95% | 0 external importers. Only returned by Stats(), which is itself dead. |
| `FanOut` | L30–L37 | 🔴 DEAD | 95% | 0 external importers. The entire struct and its method set are unreachable from outside this package. |
| `NewFanOut` | L39–L51 | 🔴 DEAD | 95% | 0 external importers. Constructor for FanOut, which itself has 0 importers. |
| `WithBufferSize` | L53–L57 | 🔴 DEAD | 95% | 0 external importers. Functional option that can never be passed to NewFanOut from outside the package. |
| `WithLogger` | L59–L63 | 🔴 DEAD | 95% | 0 external importers. Functional option with no callers anywhere in the codebase. |
| `Subscribe` | L65–L94 | 🔴 DEAD | 95% | 0 external importers. FanOut may satisfy ports.Recorder but no file instantiates FanOut or calls this method. |
| `Publish` | L96–L118 | 🔴 DEAD | 95% | 0 external importers. No callers in the codebase; FanOut itself is unreachable from outside the package. |
| `Stats` | L120–L129 | 🔴 DEAD | 95% | 0 external importers. Returns FanOutStats, which is also dead. No callers anywhere. |
| `MirrorToFile` | L137–L163 | 🔴 DEAD | 95% | 0 external importers. Package-level function with no callers despite being exported. |
| `Close` | L165–L183 | 🔴 DEAD | 95% | 0 external importers. FanOut is never instantiated outside this package, so Close is never invoked. |

### `internal/interfaces/api/server.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Option` | L31–L31 | 🔴 DEAD | 95% | 0 external importers. All With* functions that produce Option values also have 0 importers, so the entire functional-options API is externally unreachable. |
| `WithShutdownTimeout` | L34–L38 | 🔴 DEAD | 95% | 0 external importers; not called anywhere in this file. |
| `WithFacade` | L41–L45 | 🔴 DEAD | 95% | 0 external importers; not called anywhere in this file. |
| `WithWorkflowReader` | L50–L54 | 🔴 DEAD | 95% | 0 external importers; not called anywhere in this file. |
| `WithSessionRegistry` | L59–L63 | 🔴 DEAD | 95% | 0 external importers; not called anywhere in this file. |
| `WithRegistryImpl` | L67–L72 | 🔴 DEAD | 95% | 0 external importers; not called anywhere in this file. |
| `Server` | L75–L86 | 🔴 DEAD | 85% | 0 external importers per exhaustive analysis. Used internally as receiver type but no external caller constructs or holds a *Server. |
| `NewServer` | L89–L137 | 🔴 DEAD | 85% | 0 external importers; not called within this file. Likely intended for a cmd/main package absent from the import graph. |
| `Start` | L141–L147 | 🔴 DEAD | 85% | 0 external importers; not called within this file. |
| `Shutdown` | L162–L185 | 🔴 DEAD | 85% | 0 external importers; not called within this file. |
| `Handler` | L188–L190 | 🔴 DEAD | 85% | 0 external importers; not called within this file. Comment references httptest use, but no test file is in the import graph. |

### `pkg/httpx/client.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `HTTPDoer` | L14–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Client` | L19–L22 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Option` | L25–L25 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithDoer` | L28–L32 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithTimeout` | L35–L39 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewClient` | L43–L54 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `Do` | L60–L105 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `Get` | L108–L110 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Post` | L113–L115 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Put` | L118–L120 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Delete` | L123–L125 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `pkg/plugin/sdk/validator.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Severity` | L13–L13 | 🔴 DEAD | 95% | Exported type with 0 external importers. Used only within this file as the underlying type for the severity constants. |
| `SeverityError` | L16–L16 | 🔴 DEAD | 95% | Exported constant with 0 external importers. Not referenced by name within the file (severityToProto uses a default: branch, not SeverityError explicitly). |
| `SeverityWarning` | L17–L17 | 🔴 DEAD | 95% | Exported constant with 0 external importers. Referenced only in the file-internal severityToProto switch. |
| `SeverityInfo` | L18–L18 | 🔴 DEAD | 95% | Exported constant with 0 external importers. Referenced only in the file-internal severityToProto switch. |
| `ValidationIssue` | L22–L27 | 🔴 DEAD | 95% | Exported struct with 0 external importers. Used only within this file as the return element type of the Validator interface and input to toProtoIssues. |
| `WorkflowDefinition` | L31–L39 | 🔴 DEAD | 95% | Exported struct with 0 external importers. Used only within this file as the deserialization target and Validator method parameter. |
| `StepDefinition` | L42–L51 | 🔴 DEAD | 95% | Exported struct with 0 external importers. Referenced only within this file as the value type of WorkflowDefinition.Steps. |
| `Validator` | L58–L61 | 🔴 DEAD | 95% | Exported interface with 0 external importers. Used only within this file for type-asserting s.impl in validatorServiceServer methods. |
| `validatorServiceServer` | L65–L68 | 🔴 DEAD | 70% | Unexported struct not instantiated anywhere in this file. May be constructed in another file of the same sdk package (not visible here), reducing confidence; no local evidence of use. |
| `ValidateWorkflow` | L70–L89 | 🔴 DEAD | 70% | Method on unexported struct; 0 importers per analysis. Implements pluginv1.ValidatorServiceServer interface so the gRPC framework calls it at runtime, but no registration site is visible in the provided context. |
| `ValidateStep` | L91–L110 | 🔴 DEAD | 70% | Method on unexported struct; 0 importers per analysis. Implements pluginv1.ValidatorServiceServer interface so the gRPC framework calls it at runtime, but no registration site is visible in the provided context. |

### `pkg/registry/github_client.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Release` | L18–L23 | 🔴 DEAD | 95% | 0 external importers. Used only within this file by other dead exported symbols (ListReleases, ResolveVersion). |
| `Asset` | L26–L30 | 🔴 DEAD | 95% | 0 external importers. Referenced only in Release.Assets field and FindPlatformAsset, both also dead. |
| `GitHubReleaseClient` | L33–L35 | 🔴 DEAD | 95% | 0 external importers. Acts as receiver for ListReleases, ResolveVersion, and buildGitHubHeaders, but no external caller constructs one. |
| `ValidateOwnerRepo` | L38–L63 | 🔴 DEAD | 90% | 0 external importers. Only called internally by ListReleases (L76), which is itself dead. |
| `NewGitHubReleaseClient` | L67–L72 | 🔴 DEAD | 95% | 0 external importers. Constructor with no local callers and no external consumers. |
| `ListReleases` | L75–L108 | 🔴 DEAD | 95% | 0 external importers. Called only by ResolveVersion (L113), which is also dead. |
| `ResolveVersion` | L111–L160 | 🔴 DEAD | 90% | 0 external importers and no local callers. |
| `FindPlatformAsset` | L163–L189 | 🔴 DEAD | 90% | 0 external importers and no local callers. |
| `GitHubAPIDoer` | L193–L196 | 🔴 DEAD | 95% | 0 external importers. Only instantiated by NewGitHubAPIDoer (L200), which is also dead. |
| `NewGitHubAPIDoer` | L200–L202 | 🔴 DEAD | 95% | 0 external importers and no local callers. |
| `Do` | L204–L217 | 🔴 DEAD | 90% | 0 external importers. Satisfies httpx.HTTPDoer interface but GitHubAPIDoer is never constructed externally, so this method is unreachable. |

### `internal/application/acp_errors.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ACPErrorKind` | L14–L14 | 🔴 DEAD | 95% | Exported type with 0 cross-package importers. Only referenced locally as the type of ACPHandlerError.Kind, itself dead. |
| `ACPErrInvalidParams` | L19–L19 | 🔴 DEAD | 95% | Exported constant, 0 cross-package importers. Referenced only by acpInvalidParams and acpInvalidParamsWithData, which are themselves unused outside this file. |
| `ACPErrInternal` | L22–L22 | 🔴 DEAD | 95% | Exported constant, 0 cross-package importers. Referenced only by acpInternal, which is unused outside this file. |
| `ACPErrMethodNotFound` | L25–L25 | 🔴 DEAD | 95% | Exported constant, 0 cross-package importers. Referenced only by acpMethodNotFound, which is unused outside this file. |
| `ACPHandlerError` | L33–L39 | 🔴 DEAD | 95% | Exported struct, 0 cross-package importers. Returned only by the four unexported helper constructors, none of which are called outside this file. |
| `Error` | L41–L41 | 🔴 DEAD | 85% | Implements the error interface on ACPHandlerError, but ACPHandlerError has 0 cross-package importers and no intra-package callers visible in this file. Confidence reduced slightly because interface dispatch is invisible to static import analysis. |
| `acpInvalidParamsWithData` | L45–L47 | 🔴 DEAD | 65% | Unexported constructor not called within this file. May be used by other files in the application package, but no evidence present here. |
| `acpInvalidParams` | L50–L52 | 🔴 DEAD | 65% | Unexported constructor not called within this file. May be used by other files in the application package, but no evidence present here. |
| `acpInternal` | L55–L57 | 🔴 DEAD | 65% | Unexported constructor not called within this file. May be used by other files in the application package, but no evidence present here. |
| `acpMethodNotFound` | L62–L64 | 🔴 DEAD | 65% | Unexported constructor not called within this file. May be used by other files in the application package, but no evidence present here. |

### `internal/domain/workflow/condition.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Transition` | L8–L11 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Transitions` | L15–L15 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `HasDefault` | L17–L24 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Validate` | L26–L31 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `String` | L33–L38 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Validate` | L40–L47 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetTargetStates` | L49–L55 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DefaultIndex` | L57–L64 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EvaluatorFunc` | L66–L66 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EvaluateFirstMatch` | L68–L85 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/workflow/dry_run.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `DryRunPlan` | L3–L8 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DryRunInput` | L10–L15 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DryRunStep` | L17–L36 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DryRunHooks` | L38–L41 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DryRunHook` | L43–L46 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DryRunTransition` | L48–L52 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DryRunRetry` | L54–L62 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DryRunCapture` | L64–L68 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DryRunLoop` | L70–L78 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DryRunAgent` | L80–L87 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/agents/helpers.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `cloneState` | L11–L26 | 🔴 DEAD | 70% | No call site in this file. Go package scope means other files in `agents` could call it, but none visible here. |
| `buildFirstTurnPrompt` | L36–L41 | 🔴 DEAD | 70% | No call site in this file. Could be used in another file in the `agents` package. |
| `getBoolOption` | L43–L49 | 🔴 DEAD | 70% | No call site in this file. Could be used in another file in the `agents` package. |
| `reverseString` | L51–L53 | 🔴 DEAD | 70% | No call site in this file. Also a trivial one-line wrapper over stringutil.Reverse with no added logic. |
| `tryParseJSONResponse` | L55–L65 | 🔴 DEAD | 70% | No call site in this file. Could be used in another file in the `agents` package. |
| `findFirstNDJSONEvent` | L69–L84 | 🔴 DEAD | 70% | No call site in this file. Could be used in another file in the `agents` package. |
| `findLastNDJSONEvent` | L86–L102 | 🔴 DEAD | 70% | No call site in this file. Could be used in another file in the `agents` package. |
| `intFromMap` | L104–L109 | 🔴 DEAD | 70% | No call site in this file. Could be used in another file in the `agents` package. |
| `extractArgPreview` | L146–L155 | 🔴 DEAD | 70% | No call site in this file. Entry point of the argPreview chain; if unused in other package files the whole chain is dead. |

## ⚡ Quick Wins

- [ ] <!-- ACT-b8ac1c-1 --> **[utility · high · trivial]** `internal/application/acp_errors.go`: Remove dead code: `ACPErrorKind` is exported but unused `ACPErrorKind`, `ACPErrInvalidParams`, `ACPErrInternal`, `ACPErrMethodNotFound`, `ACPHandlerError`, `Error` (`ACPErrorKind, ACPErrInvalidParams, ACPErrInternal, ACPErrMethodNotFound, ACPHandlerError, Error`) [L14-L14, L19-L19, L22-L22, L25-L25, L33-L39, L41-L41]
- [ ] <!-- ACT-94797d-1 --> **[utility · high · trivial]** `internal/domain/workflow/condition.go`: Remove dead code: `Transition` is exported but unused `Transition`, `Transitions`, `HasDefault`, `Validate`, `String`, `Validate`, `GetTargetStates`, `DefaultIndex`, `EvaluatorFunc`, `EvaluateFirstMatch` (`Transition, Transitions, HasDefault, Validate, String, Validate, GetTargetStates, DefaultIndex, EvaluatorFunc, EvaluateFirstMatch`) [L8-L11, L15-L15, L17-L24, L26-L31, L33-L38, L40-L47, L49-L55, L57-L64, L66-L66, L68-L85]
- [ ] <!-- ACT-cd6984-1 --> **[utility · high · trivial]** `internal/domain/workflow/dry_run.go`: Remove dead code: `DryRunPlan` is exported but unused `DryRunPlan`, `DryRunInput`, `DryRunStep`, `DryRunHooks`, `DryRunHook`, `DryRunTransition`, `DryRunRetry`, `DryRunCapture`, `DryRunLoop`, `DryRunAgent` (`DryRunPlan, DryRunInput, DryRunStep, DryRunHooks, DryRunHook, DryRunTransition, DryRunRetry, DryRunCapture, DryRunLoop, DryRunAgent`) [L3-L8, L10-L15, L17-L36, L38-L41, L43-L46, L48-L52, L54-L62, L64-L68, L70-L78, L80-L87]
- [ ] <!-- ACT-e3cecf-3 --> **[utility · high · trivial]** `internal/infrastructure/pluginmgr/rpc_manager.go`: Remove dead code: `DefaultPluginsDir` is exported but unused `DefaultPluginsDir`, `NewRPCPluginManager`, `SetPluginsDir`, `SetPluginsDirs`, `SetHostVersion`, `SetEventBus`, `SetStreamManager`, `SetStateStore`, `SetZapLogger` (`DefaultPluginsDir, NewRPCPluginManager, SetPluginsDir, SetPluginsDirs, SetHostVersion, SetEventBus, SetStreamManager, SetStateStore, SetZapLogger`) [L44-L44, L154-L161, L222-L226, L230-L234, L956-L960, L964-L968, L972-L976, L1026-L1030, L1034-L1038]
- [ ] <!-- ACT-69ac39-1 --> **[utility · high · trivial]** `internal/infrastructure/transcript/fanout.go`: Remove dead code: `FanOutOption` is exported but unused `FanOutOption`, `FanOutStats`, `FanOut`, `NewFanOut`, `WithBufferSize`, `WithLogger`, `Subscribe`, `Publish`, `Stats`, `MirrorToFile`, `Close` (`FanOutOption, FanOutStats, FanOut, NewFanOut, WithBufferSize, WithLogger, Subscribe, Publish, Stats, MirrorToFile, Close`) [L16-L16, L18-L21, L30-L37, L39-L51, L53-L57, L59-L63, L65-L94, L96-L118, L120-L129, L137-L163, L165-L183]
- [ ] <!-- ACT-4898fe-1 --> **[utility · high · trivial]** `internal/interfaces/api/server.go`: Remove dead code: `Option` is exported but unused `Option`, `WithShutdownTimeout`, `WithFacade`, `WithWorkflowReader`, `WithSessionRegistry`, `WithRegistryImpl`, `Server`, `NewServer`, `Start`, `Shutdown`, `Handler` (`Option, WithShutdownTimeout, WithFacade, WithWorkflowReader, WithSessionRegistry, WithRegistryImpl, Server, NewServer, Start, Shutdown, Handler`) [L31-L31, L34-L38, L41-L45, L50-L54, L59-L63, L67-L72, L75-L86, L89-L137, L141-L147, L162-L185, L188-L190]
- [ ] <!-- ACT-187c7b-2 --> **[utility · high · trivial]** `pkg/httpx/client.go`: Remove dead code: `HTTPDoer` is exported but unused `HTTPDoer`, `Client`, `Option`, `WithDoer`, `WithTimeout`, `NewClient`, `Do`, `Get`, `Post`, `Put`, `Delete` (`HTTPDoer, Client, Option, WithDoer, WithTimeout, NewClient, Do, Get, Post, Put, Delete`) [L14-L16, L19-L22, L25-L25, L28-L32, L35-L39, L43-L54, L60-L105, L108-L110, L113-L115, L118-L120, L123-L125]
- [ ] <!-- ACT-eaadfc-1 --> **[utility · high · trivial]** `pkg/plugin/sdk/validator.go`: Remove dead code: `Severity` is exported but unused `Severity`, `SeverityError`, `SeverityWarning`, `SeverityInfo`, `ValidationIssue`, `WorkflowDefinition`, `StepDefinition`, `Validator` (`Severity, SeverityError, SeverityWarning, SeverityInfo, ValidationIssue, WorkflowDefinition, StepDefinition, Validator`) [L13-L13, L16-L16, L17-L17, L18-L18, L22-L27, L31-L39, L42-L51, L58-L61]
- [ ] <!-- ACT-de2f90-2 --> **[utility · high · trivial]** `pkg/registry/github_client.go`: Remove dead code: `Release` is exported but unused `Release`, `Asset`, `GitHubReleaseClient`, `ValidateOwnerRepo`, `NewGitHubReleaseClient`, `ListReleases`, `ResolveVersion`, `FindPlatformAsset`, `GitHubAPIDoer`, `NewGitHubAPIDoer`, `Do` (`Release, Asset, GitHubReleaseClient, ValidateOwnerRepo, NewGitHubReleaseClient, ListReleases, ResolveVersion, FindPlatformAsset, GitHubAPIDoer, NewGitHubAPIDoer, Do`) [L18-L23, L26-L30, L33-L35, L38-L63, L67-L72, L75-L108, L111-L160, L163-L189, L193-L196, L200-L202, L204-L217]

## 🔧 Refactors

- [ ] <!-- ACT-b8ac1c-2 --> **[utility · medium · trivial]** `internal/application/acp_errors.go`: Remove dead code: `acpInvalidParamsWithData` is exported but unused `acpInvalidParamsWithData`, `acpInvalidParams`, `acpInternal`, `acpMethodNotFound` (`acpInvalidParamsWithData, acpInvalidParams, acpInternal, acpMethodNotFound`) [L45-L47, L50-L52, L55-L57, L62-L64]
- [ ] <!-- ACT-63e31e-2 --> **[utility · medium · trivial]** `internal/infrastructure/agents/helpers.go`: Remove dead code: `cloneState` is exported but unused `cloneState`, `buildFirstTurnPrompt`, `getBoolOption`, `reverseString`, `tryParseJSONResponse`, `findFirstNDJSONEvent`, `findLastNDJSONEvent`, `intFromMap`, `extractArgPreview` (`cloneState, buildFirstTurnPrompt, getBoolOption, reverseString, tryParseJSONResponse, findFirstNDJSONEvent, findLastNDJSONEvent, intFromMap, extractArgPreview`) [L11-L26, L36-L41, L43-L49, L51-L53, L55-L65, L69-L84, L86-L102, L104-L109, L146-L155]
- [ ] <!-- ACT-eaadfc-2 --> **[utility · medium · trivial]** `pkg/plugin/sdk/validator.go`: Remove dead code: `validatorServiceServer` is exported but unused `validatorServiceServer`, `ValidateWorkflow`, `ValidateStep` (`validatorServiceServer, ValidateWorkflow, ValidateStep`) [L65-L68, L70-L89, L91-L110]
