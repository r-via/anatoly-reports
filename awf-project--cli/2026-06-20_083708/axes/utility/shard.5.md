[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 5

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/infrastructure/logger/multi_logger.go` | 🟡 NEEDS_REFACTOR | 13 | 95% | [details](#internalinfrastructureloggermultiloggergo) |
| `internal/interfaces/cli/config.go` | 🟡 NEEDS_REFACTOR | 13 | 95% | [details](#internalinterfacescliconfiggo) |
| `internal/infrastructure/acp/agent.go` | 🟡 NEEDS_REFACTOR | 12 | 95% | [details](#internalinfrastructureacpagentgo) |
| `internal/application/acp_session.go` | 🟡 NEEDS_REFACTOR | 11 | 95% | [details](#internalapplicationacpsessiongo) |
| `internal/application/loop_executor.go` | 🟡 NEEDS_REFACTOR | 11 | 95% | [details](#internalapplicationloopexecutorgo) |
| `internal/domain/workflow/reference.go` | 🟡 NEEDS_REFACTOR | 11 | 95% | [details](#internaldomainworkflowreferencego) |
| `internal/infrastructure/diagram/config.go` | 🟡 NEEDS_REFACTOR | 11 | 95% | [details](#internalinfrastructurediagramconfiggo) |
| `internal/infrastructure/errors/hint_generators.go` | 🟡 NEEDS_REFACTOR | 11 | 95% | [details](#internalinfrastructureerrorshintgeneratorsgo) |
| `internal/infrastructure/pluginmgr/loader.go` | 🟡 NEEDS_REFACTOR | 11 | 95% | [details](#internalinfrastructurepluginmgrloadergo) |
| `internal/infrastructure/pluginmgr/registry.go` | 🟡 NEEDS_REFACTOR | 11 | 95% | [details](#internalinfrastructurepluginmgrregistrygo) |

## 🔍 Symbol Details

### `internal/infrastructure/logger/multi_logger.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `MultiLogger` | L6–L8 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewMultiLogger` | L11–L13 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Debug` | L15–L19 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Info` | L21–L25 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Warn` | L27–L31 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L33–L37 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithContext` | L39–L45 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NopLogger` | L48–L48 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Debug` | L50–L50 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Info` | L51–L51 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Warn` | L52–L52 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L53–L53 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WithContext` | L54–L56 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/interfaces/cli/config.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `OutputMode` | L23–L23 | 🔴 DEAD | 95% | 0 cross-package importers. Within-file uses (Config field, ParseOutputMode return type, String receiver) are all themselves dead, forming an isolated cluster. |
| `OutputSilent` | L26–L26 | 🔴 DEAD | 95% | 0 cross-package importers. Only referenced inside String, ParseOutputMode, and DefaultConfig — all equally unreachable externally. |
| `OutputStreaming` | L27–L27 | 🔴 DEAD | 95% | 0 cross-package importers. Only referenced in String and ParseOutputMode switch arms. |
| `OutputBuffered` | L28–L28 | 🔴 DEAD | 95% | 0 cross-package importers. Only referenced in String and ParseOutputMode switch arms. |
| `String` | L31–L42 | 🔴 DEAD | 95% | 0 cross-package importers. OutputMode itself is dead, so this stringer is never reachable. |
| `ParseOutputMode` | L45–L56 | 🔴 DEAD | 95% | 0 cross-package importers. No call site found in this file or any external package. |
| `Config` | L59–L73 | 🔴 DEAD | 95% | 0 cross-package importers. Only used as buildFacade parameter type within this file; buildFacade is itself unexported and uncalled. |
| `DefaultConfig` | L76–L89 | 🔴 DEAD | 95% | 0 cross-package importers and not called anywhere in this file. |
| `BuildWorkflowPaths` | L95–L120 | 🔴 DEAD | 95% | 0 cross-package importers. Only called by NewWorkflowRepository (L124), which is itself dead. |
| `NewWorkflowRepository` | L123–L125 | 🔴 DEAD | 95% | 0 cross-package importers. Only called by buildFacade (L134), which is unexported and uncalled in this file. |
| `buildFacade` | L132–L168 | 🔴 DEAD | 72% | Unexported; not called anywhere in this file. Other files in package cli could reference it, but none visible here. Confidence reduced due to same-package visibility not captured by import analysis. |
| `BuildPromptPaths` | L173–L198 | 🔴 DEAD | 95% | 0 cross-package importers and not called anywhere in this file. |
| `BuildPluginPaths` | L204–L217 | 🔴 DEAD | 95% | 0 cross-package importers and not called anywhere in this file. |

### `internal/infrastructure/acp/agent.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Agent` | L24–L26 | 🔴 DEAD | 90% | 0 external importers. The compile-time assertion `var _ sdk.Agent = (*Agent)(nil)` is local to the package. No external consumer instantiates or accepts *Agent. |
| `NewAgent` | L30–L36 | 🔴 DEAD | 95% | 0 importers. Despite the comment referencing an interfaces/cli ACP server as the intended consumer, no file calls this constructor. |
| `Initialize` | L39–L51 | 🔴 DEAD | 90% | Interface method required by sdk.Agent, but *Agent has 0 external importers so it is never instantiated and this method is never invoked. |
| `NewSession` | L54–L84 | 🔴 DEAD | 90% | Interface method required by sdk.Agent; unreachable because *Agent is never constructed externally (0 importers of Agent/NewAgent). |
| `Prompt` | L87–L115 | 🔴 DEAD | 90% | Interface method required by sdk.Agent; unreachable because *Agent is never constructed externally. |
| `Cancel` | L118–L133 | 🔴 DEAD | 90% | Interface method required by sdk.Agent; unreachable because *Agent is never constructed externally. |
| `Authenticate` | L135–L137 | 🔴 DEAD | 90% | Stub satisfying sdk.Agent; always returns methodNotFoundErr. Unreachable — *Agent has 0 external importers. |
| `CloseSession` | L139–L141 | 🔴 DEAD | 90% | Stub satisfying sdk.Agent; always returns methodNotFoundErr. Unreachable — *Agent has 0 external importers. |
| `ListSessions` | L143–L145 | 🔴 DEAD | 90% | Stub satisfying sdk.Agent; always returns methodNotFoundErr. Unreachable — *Agent has 0 external importers. |
| `ResumeSession` | L147–L149 | 🔴 DEAD | 90% | Stub satisfying sdk.Agent; always returns methodNotFoundErr. Unreachable — *Agent has 0 external importers. |
| `SetSessionConfigOption` | L151–L153 | 🔴 DEAD | 90% | Stub satisfying sdk.Agent; always returns methodNotFoundErr. Unreachable — *Agent has 0 external importers. |
| `SetSessionMode` | L155–L157 | 🔴 DEAD | 90% | Stub satisfying sdk.Agent; always returns methodNotFoundErr. Unreachable — *Agent has 0 external importers. |

### `internal/application/acp_session.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `MCPEnvVariable` | L19–L22 | 🔴 DEAD | 70% | 0 cross-package importers. Referenced within this file only as the element type of MCPServerSpec.Env, which is itself dead externally. Entire chain is within-package only. |
| `MCPServerSpec` | L26–L31 | 🔴 DEAD | 70% | 0 cross-package importers. Referenced within this file as ACPSession.MCPServers value type, which is itself dead externally. Chain stays within-package. |
| `ACPInputResponder` | L44–L48 | 🔴 DEAD | 70% | 0 cross-package importers. Used within this file in inputReaderHolder.r and ACPRunnerFactory return type, both of which are also dead externally. |
| `ACPRunnerFactory` | L57–L57 | 🔴 DEAD | 75% | 0 cross-package importers. Not referenced anywhere in this file's content. Comment says injected by interfaces/cli wiring layer, but no evidence of that in visible code. |
| `ACPSession` | L96–L173 | 🔴 DEAD | 65% | 0 cross-package importers. Central session struct for the application package — almost certainly used in sibling files (e.g. acp_service.go), but that within-package usage is not visible here. |
| `getSessionCtx` | L178–L183 | 🔴 DEAD | 60% | Not called anywhere in this file. Likely called from other files in the application package (e.g. session prompt handler), but no evidence in visible scope. |
| `setCancel` | L186–L190 | 🔴 DEAD | 60% | Not called within this file. Intended to be called from the prompt handler in a sibling file when a workflow run starts, but no in-file evidence. |
| `shutdown` | L215–L220 | 🔴 DEAD | 60% | Not called within this file. Comment documents it as reserved for the service Shutdown sweep, implying usage in a sibling service file not visible here. |
| `getFacade` | L223–L227 | 🔴 DEAD | 60% | Not called within this file. Getter for sessionFacade; likely consumed by prompt-handling code in sibling package files. |
| `setFacade` | L230–L234 | 🔴 DEAD | 60% | Not called within this file. Setter populated by ensureSessionWiring in a sibling file; no in-file call site. |
| `parseInputPairs` | L239–L250 | 🔴 DEAD | 65% | Not called within this file. Non-exported utility function with no local call site; may be used in sibling application-layer files, but absent from visible scope. |

### `internal/application/loop_executor.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `StepExecutorFunc` | L19–L19 | 🔴 DEAD | 95% | Exported type with 0 external importers. Referenced only in signatures of ExecuteForEach/ExecuteWhile, which are themselves dead externally. |
| `ContextBuilderFunc` | L22–L22 | 🔴 DEAD | 95% | Exported type with 0 external importers. Referenced only in signatures of ExecuteForEach/ExecuteWhile, which are themselves dead externally. |
| `LoopExecutor` | L32–L36 | 🔴 DEAD | 95% | Exported struct with 0 external importers. No external code creates or references this type. |
| `NewLoopExecutor` | L39–L49 | 🔴 DEAD | 95% | Exported constructor with 0 external importers. No external code instantiates LoopExecutor. |
| `PushLoopContext` | L54–L70 | 🔴 DEAD | 95% | Exported method with 0 external importers. Only called from ExecuteForEach/ExecuteWhile, which are themselves externally dead. |
| `PopLoopContext` | L75–L82 | 🔴 DEAD | 95% | Exported method with 0 external importers. Only called from ExecuteForEach/ExecuteWhile, which are themselves externally dead. |
| `ExecuteForEach` | L87–L275 | 🔴 DEAD | 95% | Exported method with 0 external importers. Core forEach loop executor never called from outside this package. |
| `ExecuteWhile` | L280–L453 | 🔴 DEAD | 95% | Exported method with 0 external importers. Core while loop executor never called from outside this package. |
| `ResolveMaxIterations` | L461–L492 | 🔴 DEAD | 95% | Exported method with 0 external importers. Called only from ExecuteForEach/ExecuteWhile, which are externally dead. |
| `ParseItems` | L530–L544 | 🔴 DEAD | 95% | Exported method with 0 external importers. Called only from ExecuteForEach, which is externally dead. |
| `BuildBodyStepIndices` | L553–L564 | 🔴 DEAD | 95% | Exported for testing per inline comment, but 0 external importers including test files. Called only from externally dead ExecuteForEach/ExecuteWhile. |

### `internal/domain/workflow/reference.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ReferenceType` | L4–L4 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TypeInputs` | L8–L8 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TypeStates` | L10–L10 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TypeWorkflow` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TypeEnv` | L14–L14 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TypeError` | L16–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TypeContext` | L18–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TypeLoop` | L20–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TypeUnknown` | L22–L22 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TemplateReference` | L26–L32 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TemplateAnalyzer` | L128–L131 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/diagram/config.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Direction` | L7–L7 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DirectionTB` | L10–L10 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DirectionLR` | L11–L11 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DirectionBT` | L12–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DirectionRL` | L13–L13 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `IsValid` | L25–L27 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `String` | L30–L32 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NodeStyle` | L35–L40 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DiagramConfig` | L43–L48 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Validate` | L51–L56 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewDefaultConfig` | L59–L64 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/errors/hint_generators.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `FileNotFoundHintGenerator` | L32–L135 | 🔴 DEAD | 92% | Exported function with 0 runtime or type-only importers per pre-computed analysis. |
| `InvalidStateHintGenerator` | L154–L272 | 🔴 DEAD | 92% | Exported function with 0 runtime or type-only importers per pre-computed analysis. |
| `YAMLSyntaxHintGenerator` | L290–L369 | 🔴 DEAD | 95% | Exported function with 0 runtime or type-only importers per pre-computed analysis. |
| `MissingInputHintGenerator` | L388–L410 | 🔴 DEAD | 95% | Exported function with 0 runtime or type-only importers per pre-computed analysis. |
| `extractMissingInputs` | L413–L438 | 🔴 DEAD | 93% | Called only by MissingInputHintGenerator (L402), which is itself DEAD with 0 importers. |
| `extractRequiredInputs` | L441–L458 | 🔴 DEAD | 95% | Called only by MissingInputHintGenerator (L403), which is itself DEAD with 0 importers. |
| `buildMissingInputHints` | L461–L510 | 🔴 DEAD | 93% | Called only by MissingInputHintGenerator (L406), which is itself DEAD with 0 importers. |
| `formatInt` | L513–L545 | 🔴 DEAD | 95% | Called only by YAMLSyntaxHintGenerator (L336, L340, L344) and CommandFailureHintGenerator (L697), both of which are DEAD with 0 importers. |
| `CommandFailureHintGenerator` | L564–L720 | 🔴 DEAD | 92% | Exported function with 0 runtime or type-only importers per pre-computed analysis. |
| `PluginDisabledHintGenerator` | L732–L759 | 🔴 DEAD | 95% | Exported function with 0 runtime or type-only importers per pre-computed analysis. |
| `extractCommandName` | L763–L777 | 🔴 DEAD | 95% | Called only by CommandFailureHintGenerator (L672, L688), which is itself DEAD with 0 importers. |

### `internal/infrastructure/pluginmgr/loader.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `LoaderError` | L15–L20 | 🔴 DEAD | 70% | 0 external importers. Used only within this file as the return type of NewLoaderError and WrapLoaderError — no consumer outside the package. |
| `Error` | L23–L28 | 🔴 DEAD | 70% | 0 external importers. Implements error interface locally; only meaningful if *LoaderError escapes this package, which it does not. |
| `Unwrap` | L31–L33 | 🔴 DEAD | 70% | 0 external importers. errors.Unwrap protocol only matters if callers outside the package inspect error chains; no such callers exist. |
| `NewLoaderError` | L36–L42 | 🔴 DEAD | 70% | 0 external importers. Called heavily within this file (ValidatePlugin, LoadPlugin, DiscoverPlugins) but the package itself has no external consumers. |
| `WrapLoaderError` | L45–L52 | 🔴 DEAD | 70% | 0 external importers. Called in DiscoverPlugins and LoadPlugin within this file; package has no external consumers. |
| `ManifestFileName` | L55–L55 | 🔴 DEAD | 70% | 0 external importers. Referenced at two call sites within this file but never consumed by an external package. |
| `FileSystemLoader` | L67–L69 | 🔴 DEAD | 70% | 0 external importers. Serves as method receiver within the file; no external package instantiates or uses it. |
| `NewFileSystemLoader` | L72–L76 | 🔴 DEAD | 95% | 0 external importers and not called anywhere within this file. Pure dead constructor. |
| `DiscoverPlugins` | L80–L134 | 🔴 DEAD | 92% | 0 external importers. Primary entry-point method never called within the file or by any external package. |
| `LoadPlugin` | L138–L178 | 🔴 DEAD | 70% | 0 external importers. Called once internally by DiscoverPlugins (line 121), but since DiscoverPlugins itself is dead and the package has no external consumers, this is effectively dead. |
| `ValidatePlugin` | L182–L228 | 🔴 DEAD | 92% | 0 external importers and not called anywhere within this file. |

### `internal/infrastructure/pluginmgr/registry.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `OperationRegistry` | L20–L24 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewOperationRegistry` | L30–L35 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RegisterOperation` | L40–L56 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `UnregisterOperation` | L60–L72 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Operations` | L76–L86 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetOperation` | L90–L96 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `UnregisterPluginOperations` | L100–L119 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetPluginOperations` | L122–L136 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Count` | L139–L144 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetOperationSource` | L148–L154 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Clear` | L157–L163 | 🔴 DEAD | 95% | Exported but imported by 0 files |

## ⚡ Quick Wins

- [ ] <!-- ACT-1ad4e4-1 --> **[utility · high · trivial]** `internal/application/loop_executor.go`: Remove dead code: `StepExecutorFunc` is exported but unused `StepExecutorFunc`, `ContextBuilderFunc`, `LoopExecutor`, `NewLoopExecutor`, `PushLoopContext`, `PopLoopContext`, `ExecuteForEach`, `ExecuteWhile`, `ResolveMaxIterations`, `ParseItems`, `BuildBodyStepIndices` (`StepExecutorFunc, ContextBuilderFunc, LoopExecutor, NewLoopExecutor, PushLoopContext, PopLoopContext, ExecuteForEach, ExecuteWhile, ResolveMaxIterations, ParseItems, BuildBodyStepIndices`) [L19-L19, L22-L22, L32-L36, L39-L49, L54-L70, L75-L82, L87-L275, L280-L453, L461-L492, L530-L544, L553-L564]
- [ ] <!-- ACT-348626-1 --> **[utility · high · trivial]** `internal/domain/workflow/reference.go`: Remove dead code: `ReferenceType` is exported but unused `ReferenceType`, `TypeInputs`, `TypeStates`, `TypeWorkflow`, `TypeEnv`, `TypeError`, `TypeContext`, `TypeLoop`, `TypeUnknown`, `TemplateReference`, `TemplateAnalyzer` (`ReferenceType, TypeInputs, TypeStates, TypeWorkflow, TypeEnv, TypeError, TypeContext, TypeLoop, TypeUnknown, TemplateReference, TemplateAnalyzer`) [L4-L4, L8-L8, L10-L10, L12-L12, L14-L14, L16-L16, L18-L18, L20-L20, L22-L22, L26-L32, L128-L131]
- [ ] <!-- ACT-ca06be-1 --> **[utility · high · trivial]** `internal/infrastructure/acp/agent.go`: Remove dead code: `Agent` is exported but unused `Agent`, `NewAgent`, `Initialize`, `NewSession`, `Prompt`, `Cancel`, `Authenticate`, `CloseSession`, `ListSessions`, `ResumeSession`, `SetSessionConfigOption`, `SetSessionMode` (`Agent, NewAgent, Initialize, NewSession, Prompt, Cancel, Authenticate, CloseSession, ListSessions, ResumeSession, SetSessionConfigOption, SetSessionMode`) [L24-L26, L30-L36, L39-L51, L54-L84, L87-L115, L118-L133, L135-L137, L139-L141, L143-L145, L147-L149, L151-L153, L155-L157]
- [ ] <!-- ACT-065e81-1 --> **[utility · high · trivial]** `internal/infrastructure/diagram/config.go`: Remove dead code: `Direction` is exported but unused `Direction`, `DirectionTB`, `DirectionLR`, `DirectionBT`, `DirectionRL`, `IsValid`, `String`, `NodeStyle`, `DiagramConfig`, `Validate`, `NewDefaultConfig` (`Direction, DirectionTB, DirectionLR, DirectionBT, DirectionRL, IsValid, String, NodeStyle, DiagramConfig, Validate, NewDefaultConfig`) [L7-L7, L10-L10, L11-L11, L12-L12, L13-L13, L25-L27, L30-L32, L35-L40, L43-L48, L51-L56, L59-L64]
- [ ] <!-- ACT-7986cf-1 --> **[utility · high · trivial]** `internal/infrastructure/errors/hint_generators.go`: Remove dead code: `FileNotFoundHintGenerator` is exported but unused `FileNotFoundHintGenerator`, `InvalidStateHintGenerator`, `YAMLSyntaxHintGenerator`, `MissingInputHintGenerator`, `extractMissingInputs`, `extractRequiredInputs`, `buildMissingInputHints`, `formatInt`, `CommandFailureHintGenerator`, `PluginDisabledHintGenerator`, `extractCommandName` (`FileNotFoundHintGenerator, InvalidStateHintGenerator, YAMLSyntaxHintGenerator, MissingInputHintGenerator, extractMissingInputs, extractRequiredInputs, buildMissingInputHints, formatInt, CommandFailureHintGenerator, PluginDisabledHintGenerator, extractCommandName`) [L32-L135, L154-L272, L290-L369, L388-L410, L413-L438, L441-L458, L461-L510, L513-L545, L564-L720, L732-L759, L763-L777]
- [ ] <!-- ACT-8ad81c-1 --> **[utility · high · trivial]** `internal/infrastructure/logger/multi_logger.go`: Remove dead code: `MultiLogger` is exported but unused `MultiLogger`, `NewMultiLogger`, `Debug`, `Info`, `Warn`, `Error`, `WithContext`, `NopLogger`, `Debug`, `Info`, `Warn`, `Error`, `WithContext` (`MultiLogger, NewMultiLogger, Debug, Info, Warn, Error, WithContext, NopLogger, Debug, Info, Warn, Error, WithContext`) [L6-L8, L11-L13, L15-L19, L21-L25, L27-L31, L33-L37, L39-L45, L48-L48, L50-L50, L51-L51, L52-L52, L53-L53, L54-L56]
- [ ] <!-- ACT-9dd9a4-2 --> **[utility · high · trivial]** `internal/infrastructure/pluginmgr/loader.go`: Remove dead code: `NewFileSystemLoader` is exported but unused `NewFileSystemLoader`, `DiscoverPlugins`, `ValidatePlugin` (`NewFileSystemLoader, DiscoverPlugins, ValidatePlugin`) [L72-L76, L80-L134, L182-L228]
- [ ] <!-- ACT-24b5bc-1 --> **[utility · high · trivial]** `internal/infrastructure/pluginmgr/registry.go`: Remove dead code: `OperationRegistry` is exported but unused `OperationRegistry`, `NewOperationRegistry`, `RegisterOperation`, `UnregisterOperation`, `Operations`, `GetOperation`, `UnregisterPluginOperations`, `GetPluginOperations`, `Count`, `GetOperationSource`, `Clear` (`OperationRegistry, NewOperationRegistry, RegisterOperation, UnregisterOperation, Operations, GetOperation, UnregisterPluginOperations, GetPluginOperations, Count, GetOperationSource, Clear`) [L20-L24, L30-L35, L40-L56, L60-L72, L76-L86, L90-L96, L100-L119, L122-L136, L139-L144, L148-L154, L157-L163]
- [ ] <!-- ACT-c14c88-1 --> **[utility · high · trivial]** `internal/interfaces/cli/config.go`: Remove dead code: `OutputMode` is exported but unused `OutputMode`, `OutputSilent`, `OutputStreaming`, `OutputBuffered`, `String`, `ParseOutputMode`, `Config`, `DefaultConfig`, `BuildWorkflowPaths`, `NewWorkflowRepository`, `BuildPromptPaths`, `BuildPluginPaths` (`OutputMode, OutputSilent, OutputStreaming, OutputBuffered, String, ParseOutputMode, Config, DefaultConfig, BuildWorkflowPaths, NewWorkflowRepository, BuildPromptPaths, BuildPluginPaths`) [L23-L23, L26-L26, L27-L27, L28-L28, L31-L42, L45-L56, L59-L73, L76-L89, L95-L120, L123-L125, L173-L198, L204-L217]

## 🔧 Refactors

- [ ] <!-- ACT-06d66d-1 --> **[utility · medium · trivial]** `internal/application/acp_session.go`: Remove dead code: `MCPEnvVariable` is exported but unused `MCPEnvVariable`, `MCPServerSpec`, `ACPInputResponder`, `ACPRunnerFactory`, `ACPSession`, `getSessionCtx`, `setCancel`, `shutdown`, `getFacade`, `setFacade`, `parseInputPairs` (`MCPEnvVariable, MCPServerSpec, ACPInputResponder, ACPRunnerFactory, ACPSession, getSessionCtx, setCancel, shutdown, getFacade, setFacade, parseInputPairs`) [L19-L22, L26-L31, L44-L48, L57-L57, L96-L173, L178-L183, L186-L190, L215-L220, L223-L227, L230-L234, L239-L250]
- [ ] <!-- ACT-9dd9a4-1 --> **[utility · medium · trivial]** `internal/infrastructure/pluginmgr/loader.go`: Remove dead code: `LoaderError` is exported but unused `LoaderError`, `Error`, `Unwrap`, `NewLoaderError`, `WrapLoaderError`, `ManifestFileName`, `FileSystemLoader`, `LoadPlugin` (`LoaderError, Error, Unwrap, NewLoaderError, WrapLoaderError, ManifestFileName, FileSystemLoader, LoadPlugin`) [L15-L20, L23-L28, L31-L33, L36-L42, L45-L52, L55-L55, L67-L69, L138-L178]
- [ ] <!-- ACT-c14c88-2 --> **[utility · medium · trivial]** `internal/interfaces/cli/config.go`: Remove dead code: `buildFacade` is exported but unused (`buildFacade`) [L132-L168]
