[← Back to Correction](./index.md) · [← Back to report](../../public_report.md)

# 🐛 Correction — Shard 7

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)

## 📊 Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `pkg/registry/downloader.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#pkgregistrydownloadergo) |
| `pkg/plugin/sdk/event.go` | 🟡 NEEDS_REFACTOR | 0 | 85% | [details](#pkgpluginsdkeventgo) |
| `internal/domain/workflow/graph.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internaldomainworkflowgraphgo) |
| `internal/domain/workflow/template_validation.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internaldomainworkflowtemplatevalidationgo) |
| `internal/infrastructure/expression/expr_evaluator.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructureexpressionexprevaluatorgo) |
| `internal/infrastructure/github/auth.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructuregithubauthgo) |
| `internal/infrastructure/roles/filesystem_repository.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructurerolesfilesystemrepositorygo) |
| `internal/infrastructure/transcript/jsonl_writer.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructuretranscriptjsonlwritergo) |
| `internal/infrastructure/workflowpkg/discoverer.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructureworkflowpkgdiscoverergo) |
| `internal/interfaces/api/sse.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinterfacesapissego) |

## 🔍 Symbol Details

### `pkg/registry/downloader.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `extractTarFile` | L158 | 🟡 NEEDS_FIX | 78% | The nolint comment claims decompression-bomb risk is mitigated by the 100 MB HTTP body limit in `Download`, but that limit applies to the *compressed* bytes. A 100 MB gzip payload can expand to many gigabytes, exhausting process memory and crashing the program. The correct mitigation is wrapping `reader` with an `io.LimitReader` sized to an acceptable decompressed-output ceiling before passing it to `io.Copy`. |

### `internal/domain/workflow/graph.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `GetTransitions` | L328–L347 | 🟡 NEEDS_FIX | 85% | No iteration over step.Transitions: states reachable only via step.Transitions[i].Goto are reported as unreachable by ValidateGraph, and cycles through those edges are missed by DetectCycles. |

### `internal/domain/workflow/template_validation.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `ValidateStep` | L143 | 🟡 NEEDS_FIX | 85% | v.stepIndex[step.Name] returns 0 when the step is not in the execution order (Go zero-value for int). validateStateRef checks currentStepIndex>=0, so it proceeds with index 0, making refIdx>=0 true for every reachable step. All state references from unreachable steps are incorrectly reported as forward references. The fix is to check whether step.Name is present in stepIndex and, when absent, use -1 as the sentinel (consistent with hook validation at line 129). |
| `validateInputRef` | L243 | 🟡 NEEDS_FIX | 68% | containsArithmeticOperator checks for '-'. ref.Path='my-input' or ref.Raw='{{inputs.my-input}}' both return true. extractInputNamesFromExpr then splits on '-' producing ['my','input'] (via the inputs.my token yielding 'my' and bare 'input'), neither of which matches the actual input name 'my-input'. Two spurious ErrUndefinedInput errors are emitted. Valid inputs with kebab-case names (common in YAML) are incorrectly rejected. |

## ⚡ Quick Wins

- [ ] <!-- ACT-5e4fda-1 --> **[correction · medium · small]** `internal/domain/workflow/graph.go`: In GetTransitions, add iteration over step.Transitions: for each tr in step.Transitions append tr.Goto to transitions (non-empty guard). This makes FindReachableStates and DetectCycles aware of all graph edges. [L328]
- [ ] <!-- ACT-5e4fda-2 --> **[correction · medium · small]** `internal/domain/workflow/graph.go`: In ValidateGraph's per-step validation loop, iterate step.Transitions and verify each tr.Goto exists in steps, emitting ErrInvalidTransition if not. [L84]
- [ ] <!-- ACT-908eaa-1 --> **[correction · medium · small]** `internal/domain/workflow/template_validation.go`: In ValidateStep, check whether step.Name is present in v.stepIndex before using its value. When absent (unreachable step), use -1 as currentIdx so validateStateRef skips forward-reference detection — consistent with the hook sentinel already defined at line 129. Change line 143 to: currentIdx, ok := v.stepIndex[step.Name]; if !ok { currentIdx = -1 }. [L143]
- [ ] <!-- ACT-908eaa-2 --> **[correction · medium · small]** `internal/domain/workflow/template_validation.go`: In validateInputRef, exclude '-' from the arithmetic-operator check (or check ref.Path/ref.Raw separately) so that hyphenated input names like 'my-input' are not misclassified as arithmetic expressions. One approach: check only ref.Path for operators excluding '-', and keep the '-' check only when the expression also contains a space or a second distinct operator, indicating a true binary minus rather than a name separator. [L243]
- [ ] <!-- ACT-5f1cd7-1 --> **[correction · medium · small]** `internal/infrastructure/expression/expr_evaluator.go`: Before `return int(v), nil` on the float64 branch in EvaluateInt, add a bounds check: if v > math.MaxFloat64 that is still finite but > float64(math.MaxInt) or < float64(math.MinInt), return an overflow error. Without it, out-of-range floats silently produce a wrong int via implementation-defined truncation. [L85]
- [ ] <!-- ACT-6e6b2d-1 --> **[correction · medium · small]** `internal/infrastructure/github/auth.go`: After `cmd.Run()` returns a non-nil error, check `ctx.Err()` and return `AuthMethod{Type:"none"}, fmt.Errorf("auth detection canceled: %w", ctx.Err())` before falling through to the GITHUB_TOKEN check, so context cancellation is not silently swallowed. [L65]
- [ ] <!-- ACT-41b9fb-1 --> **[correction · medium · small]** `internal/infrastructure/roles/filesystem_repository.go`: In LoadFromPath, replace the blanket AgentRoleNotFoundError return with an os.IsNotExist check: return AgentRoleNotFoundError only when err satisfies os.IsNotExist(err), and propagate all other errors (permission denied, I/O error) directly so callers get the correct failure reason. [L82]
- [ ] <!-- ACT-7b183c-1 --> **[correction · medium · small]** `internal/infrastructure/transcript/jsonl_writer.go`: In Close, acquire w.mu before calling w.f.Close() to prevent a data race with concurrent Write calls. E.g. wrap closeOnce.Do body with w.mu.Lock()/Unlock(), or lock around the entire closeOnce.Do call. [L67]
- [ ] <!-- ACT-7ec5ec-1 --> **[correction · medium · small]** `internal/infrastructure/workflowpkg/discoverer.go`: In DiscoverWorkflows (line 43), check ctx.Err() != nil before continuing: if DiscoverPacks returned a context error, return nil, ctx.Err() immediately instead of advancing to the next directory. [L43]
- [ ] <!-- ACT-7ec5ec-2 --> **[correction · medium · small]** `internal/infrastructure/workflowpkg/discoverer.go`: In LoadWorkflow (line 139), check ctx.Err() != nil before continuing: if repo.Load returned a context cancellation error, propagate it rather than trying subsequent directories. [L139]
- [ ] <!-- ACT-7ec5ec-3 --> **[correction · medium · small]** `internal/infrastructure/workflowpkg/discoverer.go`: In LoadWorkflow (line 133), call a.loader.LoadPackState(packDir) and skip the directory when the pack is disabled, matching the enabled-state filtering DiscoverWorkflows performs. [L133]
- [ ] <!-- ACT-a31dbf-1 --> **[correction · medium · small]** `internal/interfaces/api/sse.go`: Replace the void closure in RegisterSSERoutes with a mechanism that propagates the 404 to the HTTP client. Options: (1) pre-validate the session ID in a standard huma route that returns 404 before the SSE upgrade; (2) emit a typed error SSE event (e.g. `{"kind":"error","error":"not found"}`) inside Stream before returning, giving clients an in-band signal; (3) if huma/sse grows error-return support, remove the `_ =` discard. The current silent close gives clients no way to distinguish not-found from end-of-stream. [L219]
- [ ] <!-- ACT-b84116-1 --> **[correction · medium · small]** `pkg/plugin/sdk/event.go`: Replace `return &pluginv1.HandleEventResponse{}, nil` on error with `return nil, status.Errorf(codes.Internal, "event handler: %v", err)` so gRPC callers receive a proper error status instead of a false success. [L56]
- [ ] <!-- ACT-9dff52-1 --> **[correction · medium · small]** `pkg/registry/downloader.go`: In VerifyChecksum, compare the computed hash against `strings.ToLower(checksum)` so that uppercase or mixed-case checksum strings (valid per hex.DecodeString) are accepted when the underlying digest matches. [L66]
- [ ] <!-- ACT-9dff52-2 --> **[correction · medium · small]** `pkg/registry/downloader.go`: In extractTarFile, wrap `reader` with `io.LimitReader(reader, maxDecompressedBytes)` before `io.Copy` to enforce an upper bound on decompressed output and prevent OOM crashes from crafted gzip bombs. The existing 100 MB download limit covers only compressed data. [L158]
