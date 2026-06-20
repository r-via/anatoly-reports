[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 12

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/infrastructure/tokenizer/approximation_tokenizer.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinfrastructuretokenizerapproximationtokenizergo) |
| `pkg/retry/backoff.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#pkgretrybackoffgo) |
| `internal/infrastructure/repository/yaml_repository.go` | 🟡 NEEDS_REFACTOR | 7 | 92% | [details](#internalinfrastructurerepositoryyamlrepositorygo) |
| `internal/application/execution_service_transcript.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internalapplicationexecutionservicetranscriptgo) |
| `internal/application/output_limiter.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internalapplicationoutputlimitergo) |
| `internal/application/resolver_wire.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internalapplicationresolverwirego) |
| `internal/application/session_registry.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internalapplicationsessionregistrygo) |
| `internal/domain/pluginmodel/operation.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internaldomainpluginmodeloperationgo) |
| `internal/domain/workflow/template_errors.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internaldomainworkflowtemplateerrorsgo) |
| `internal/domain/workflow/template.go` | 🟡 NEEDS_REFACTOR | 6 | 95% | [details](#internaldomainworkflowtemplatego) |

## 🔍 Symbol Details

### `internal/infrastructure/tokenizer/approximation_tokenizer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ApproximationTokenizer` | L12–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewApproximationTokenizer` | L20–L24 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewApproximationTokenizerWithRatio` | L28–L32 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `CountTokens` | L36–L49 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `CountTurnsTokens` | L53–L72 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `IsEstimate` | L75–L77 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ModelName` | L80–L82 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `pkg/retry/backoff.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Strategy` | L10–L10 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StrategyConstant` | L14–L14 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StrategyLinear` | L16–L16 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `StrategyExponential` | L18–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Valid` | L22–L29 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `CalculateDelay` | L33–L56 | 🔴 DEAD | 92% | Exported but imported by 0 files |
| `ApplyJitter` | L60–L74 | 🔴 DEAD | 75% | Exported but imported by 0 files |

### `internal/infrastructure/repository/yaml_repository.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `YAMLRepository` | L23–L26 | 🔴 DEAD | 70% | 0 cross-package importers per exhaustive analysis. Same-package usage (e.g. CompositeRepository, mentioned in comments) wouldn't appear in import counts — confidence reduced accordingly. |
| `NewYAMLRepository` | L28–L30 | 🔴 DEAD | 70% | 0 cross-package importers. Primary constructor for YAMLRepository; if only called from same-package files (e.g. composite_repository.go), import analysis would not capture it. |
| `WithSource` | L35–L37 | 🔴 DEAD | 70% | 0 cross-package importers. Designed explicitly for CompositeRepository use (per doc comment), which likely lives in the same package and wouldn't appear in import counts. |
| `Load` | L40–L137 | 🔴 DEAD | 70% | 0 cross-package importers. Core method implementing ports.WorkflowRepository; likely called from same-package composite or adapter code not captured by import analysis. |
| `List` | L140–L154 | 🔴 DEAD | 70% | 0 cross-package importers. Also delegated to by ListWithSource internally, but that call is within the same file/package and not tracked as an import. |
| `ListWithSource` | L159–L173 | 🔴 DEAD | 70% | 0 cross-package importers. Implements the extended port interface; likely consumed by same-package CompositeRepository. |
| `Exists` | L176–L189 | 🔴 DEAD | 70% | 0 cross-package importers. Implements ports.WorkflowRepository; same-package callers would not register as imports. |

### `internal/application/execution_service_transcript.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `withRecorder` | L21–L26 | 🔴 DEAD | 60% | Not called in this file. Likely called from sibling ExecutionService files in the same package (cross-file same-package usage not tracked by import analysis). |
| `emitTranscriptStep` | L47–L84 | 🔴 DEAD | 60% | Not called in this file. Expected to be called from other ExecutionService files in the same package where step execution occurs. |
| `emitTranscriptAgentMessage` | L102–L120 | 🔴 DEAD | 60% | Not called in this file. Expected to be called from sibling ExecutionService files where agent prompts are dispatched. |
| `emitTranscriptAgentResponse` | L134–L160 | 🔴 DEAD | 60% | Not called in this file. Expected to be called from sibling ExecutionService files that handle provider output. |
| `emitTranscriptCallWorkflowStarted` | L162–L175 | 🔴 DEAD | 60% | Not called in this file. Expected to be called from sibling files that invoke sub-workflows. |
| `emitTranscriptCallWorkflowCompleted` | L177–L194 | 🔴 DEAD | 60% | Not called in this file. Paired with emitTranscriptCallWorkflowStarted; expected in sibling files that complete sub-workflow calls. |

### `internal/application/output_limiter.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `OutputLimiter` | L12–L15 | 🔴 DEAD | 95% | 0 external importers per exhaustive import analysis. Type is never instantiated outside this file. |
| `NewOutputLimiter` | L18–L23 | 🔴 DEAD | 95% | 0 external importers. Constructor for a dead type; never called outside this package. |
| `LimitResult` | L26–L32 | 🔴 DEAD | 95% | 0 external importers. Only returned by Apply, which itself is never called externally. |
| `Apply` | L36–L87 | 🔴 DEAD | 92% | 0 external importers. Method on a dead type; no external caller can reach it. |
| `ShouldLimit` | L90–L98 | 🔴 DEAD | 85% | 0 external importers. Called locally by Apply (L70, L75) and Truncate (L103), but the entire OutputLimiter type is externally dead, making this transitively unreachable. |
| `Truncate` | L101–L121 | 🔴 DEAD | 85% | 0 external importers. Called locally by Apply (L71, L76), but OutputLimiter is never instantiated externally, making this transitively unreachable. |

### `internal/application/resolver_wire.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `EncodeForMCP` | L46–L46 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DecodeFromMCP` | L53–L61 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `EncodeForACP` | L68–L68 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `DecodeFromACP` | L75–L83 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ValidateAndEncodeForMCP` | L91–L96 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ValidateAndEncodeForACP` | L104–L109 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/application/session_registry.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `SessionRegistry` | L18–L21 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewSessionRegistry` | L23–L27 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Add` | L31–L40 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Get` | L42–L47 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Remove` | L49–L53 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Len` | L55–L59 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/pluginmodel/operation.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `OperationSchema` | L30–L36 | 🔴 DEAD | 85% | 0 external importers. Never instantiated within this file — only serves as method receiver. Entire struct is unreachable from outside the package. |
| `GetRequiredInputs` | L91–L101 | 🔴 DEAD | 95% | Method on *OperationSchema. 0 external importers, never called within this file. |
| `OperationResult` | L169–L173 | 🔴 DEAD | 95% | 0 external importers. Never instantiated or referenced within this file. Entire struct and all its methods are unreachable. |
| `IsSuccess` | L175–L177 | 🔴 DEAD | 95% | Method on *OperationResult. 0 external importers, never called within this file. |
| `HasError` | L179–L181 | 🔴 DEAD | 95% | Method on *OperationResult. 0 external importers, never called within this file. |
| `GetOutput` | L183–L189 | 🔴 DEAD | 95% | Method on *OperationResult. 0 external importers, never called within this file. |

### `internal/domain/workflow/template_errors.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `CircularTemplateError` | L6–L8 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L10–L12 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `MissingParameterError` | L15–L19 | 🔴 DEAD | 90% | Exported but imported by 0 files |
| `Error` | L21–L23 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TemplateNotFoundError` | L26–L29 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L31–L36 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/domain/workflow/template.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Template` | L9–L13 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `TemplateParam` | L16–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WorkflowTemplateRef` | L23–L26 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Validate` | L29–L48 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetRequiredParams` | L51–L59 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `GetDefaultValues` | L62–L70 | 🔴 DEAD | 95% | Exported but imported by 0 files |

## ⚡ Quick Wins

- [ ] <!-- ACT-460231-2 --> **[utility · high · trivial]** `internal/application/output_limiter.go`: Remove dead code: `OutputLimiter` is exported but unused `OutputLimiter`, `NewOutputLimiter`, `LimitResult`, `Apply`, `ShouldLimit`, `Truncate` (`OutputLimiter, NewOutputLimiter, LimitResult, Apply, ShouldLimit, Truncate`) [L12-L15, L18-L23, L26-L32, L36-L87, L90-L98, L101-L121]
- [ ] <!-- ACT-9a87f6-1 --> **[utility · high · trivial]** `internal/application/resolver_wire.go`: Remove dead code: `EncodeForMCP` is exported but unused `EncodeForMCP`, `DecodeFromMCP`, `EncodeForACP`, `DecodeFromACP`, `ValidateAndEncodeForMCP`, `ValidateAndEncodeForACP` (`EncodeForMCP, DecodeFromMCP, EncodeForACP, DecodeFromACP, ValidateAndEncodeForMCP, ValidateAndEncodeForACP`) [L46-L46, L53-L61, L68-L68, L75-L83, L91-L96, L104-L109]
- [ ] <!-- ACT-0706e7-1 --> **[utility · high · trivial]** `internal/application/session_registry.go`: Remove dead code: `SessionRegistry` is exported but unused `SessionRegistry`, `NewSessionRegistry`, `Add`, `Get`, `Remove`, `Len` (`SessionRegistry, NewSessionRegistry, Add, Get, Remove, Len`) [L18-L21, L23-L27, L31-L40, L42-L47, L49-L53, L55-L59]
- [ ] <!-- ACT-0be5b8-2 --> **[utility · high · trivial]** `internal/domain/pluginmodel/operation.go`: Remove dead code: `OperationSchema` is exported but unused `OperationSchema`, `GetRequiredInputs`, `OperationResult`, `IsSuccess`, `HasError`, `GetOutput` (`OperationSchema, GetRequiredInputs, OperationResult, IsSuccess, HasError, GetOutput`) [L30-L36, L91-L101, L169-L173, L175-L177, L179-L181, L183-L189]
- [ ] <!-- ACT-eba9fc-1 --> **[utility · high · trivial]** `internal/domain/workflow/template_errors.go`: Remove dead code: `CircularTemplateError` is exported but unused `CircularTemplateError`, `Error`, `MissingParameterError`, `Error`, `TemplateNotFoundError`, `Error` (`CircularTemplateError, Error, MissingParameterError, Error, TemplateNotFoundError, Error`) [L6-L8, L10-L12, L15-L19, L21-L23, L26-L29, L31-L36]
- [ ] <!-- ACT-3cad60-1 --> **[utility · high · trivial]** `internal/domain/workflow/template.go`: Remove dead code: `Template` is exported but unused `Template`, `TemplateParam`, `WorkflowTemplateRef`, `Validate`, `GetRequiredParams`, `GetDefaultValues` (`Template, TemplateParam, WorkflowTemplateRef, Validate, GetRequiredParams, GetDefaultValues`) [L9-L13, L16-L20, L23-L26, L29-L48, L51-L59, L62-L70]
- [ ] <!-- ACT-ceed2b-1 --> **[utility · high · trivial]** `internal/infrastructure/tokenizer/approximation_tokenizer.go`: Remove dead code: `ApproximationTokenizer` is exported but unused `ApproximationTokenizer`, `NewApproximationTokenizer`, `NewApproximationTokenizerWithRatio`, `CountTokens`, `CountTurnsTokens`, `IsEstimate`, `ModelName` (`ApproximationTokenizer, NewApproximationTokenizer, NewApproximationTokenizerWithRatio, CountTokens, CountTurnsTokens, IsEstimate, ModelName`) [L12-L16, L20-L24, L28-L32, L36-L49, L53-L72, L75-L77, L80-L82]
- [ ] <!-- ACT-4e977b-2 --> **[utility · high · trivial]** `pkg/retry/backoff.go`: Remove dead code: `Strategy` is exported but unused `Strategy`, `StrategyConstant`, `StrategyLinear`, `StrategyExponential`, `Valid`, `CalculateDelay` (`Strategy, StrategyConstant, StrategyLinear, StrategyExponential, Valid, CalculateDelay`) [L10-L10, L14-L14, L16-L16, L18-L18, L22-L29, L33-L56]

## 🔧 Refactors

- [ ] <!-- ACT-dc8b68-1 --> **[utility · medium · trivial]** `internal/application/execution_service_transcript.go`: Remove dead code: `withRecorder` is exported but unused `withRecorder`, `emitTranscriptStep`, `emitTranscriptAgentMessage`, `emitTranscriptAgentResponse`, `emitTranscriptCallWorkflowStarted`, `emitTranscriptCallWorkflowCompleted` (`withRecorder, emitTranscriptStep, emitTranscriptAgentMessage, emitTranscriptAgentResponse, emitTranscriptCallWorkflowStarted, emitTranscriptCallWorkflowCompleted`) [L21-L26, L47-L84, L102-L120, L134-L160, L162-L175, L177-L194]
- [ ] <!-- ACT-8c696e-1 --> **[utility · medium · trivial]** `internal/infrastructure/repository/yaml_repository.go`: Remove dead code: `YAMLRepository` is exported but unused `YAMLRepository`, `NewYAMLRepository`, `WithSource`, `Load`, `List`, `ListWithSource`, `Exists` (`YAMLRepository, NewYAMLRepository, WithSource, Load, List, ListWithSource, Exists`) [L23-L26, L28-L30, L35-L37, L40-L137, L140-L154, L159-L173, L176-L189]
- [ ] <!-- ACT-4e977b-3 --> **[utility · medium · trivial]** `pkg/retry/backoff.go`: Remove dead code: `ApplyJitter` is exported but unused (`ApplyJitter`) [L60-L74]
