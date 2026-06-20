[← Back to Correction](./index.md) · [← Back to report](../../public_report.md)

# 🐛 Correction — Shard 2

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)
- [🧹 Hygiene](#-hygiene)

## 📊 Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `internal/infrastructure/pluginmgr/state_store.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructurepluginmgrstatestorego) |
| `pkg/interpolation/reference.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#pkginterpolationreferencego) |
| `internal/interfaces/cli/ui/formatter.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinterfacescliuiformattergo) |
| `internal/interfaces/cli/ui/interactive_prompt.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacescliuiinteractivepromptgo) |
| `internal/application/service.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalapplicationservicego) |
| `internal/infrastructure/pluginmgr/rpc_manager.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinfrastructurepluginmgrrpcmanagergo) |
| `pkg/httpx/client.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#pkghttpxclientgo) |
| `pkg/registry/github_client.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#pkgregistrygithubclientgo) |
| `internal/infrastructure/agents/helpers.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructureagentshelpersgo) |
| `internal/infrastructure/agents/openai_compatible_provider.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinfrastructureagentsopenaicompatibleprovidergo) |

## 🔍 Symbol Details

### `pkg/interpolation/reference.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `stripFuncName` | L199–L201 | 🟡 NEEDS_FIX | 92% | After rest = strings.TrimPrefix/TrimSuffix strips parens, rest = 'trimSpace .states.step.output'. firstDotPath(rest) takes the first token 'trimSpace', finds it in templateFuncNames, and returns ''. The actual reference is never extracted. Fix: replace firstDotPath(rest) with stripFuncName(rest) so the nested function prefix is also stripped. |

### `internal/interfaces/cli/ui/interactive_prompt.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `truncateString` | L256 | 🟡 NEEDS_FIX | 92% | When maxLen is 0, 1, or 2, maxLen-3 is negative; Go panics on a negative slice index. The function must guard: if maxLen <= 3, clamp the slice to s[:maxLen] (no ellipsis) or return an empty string. |

### `internal/application/service.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `warnIfUnsupportedProvider` | L414–L416 | 🟡 NEEDS_FIX | 80% | `if step.Agent == nil \|\| s.logger == nil { return nil }` short-circuits before the ValidationError is constructed. When s.logger is nil, no *workflow.ValidationError is returned, so validateMCPProxy never appends the warning to s.lastValidationWarnings. Callers using LastValidationWarnings() will silently miss UNSUPPORTED_PROVIDER warnings. Fix: guard only the s.logger.Warn() call with a nil check, and always build and return the ValidationError when an unsupported provider is detected. |

### `internal/infrastructure/pluginmgr/rpc_manager.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `Load` | L308–L315 | 🟡 NEEDS_FIX | 85% | `existing.Status` is read after `m.mu.RUnlock()`. The Status field is written by Init and Shutdown under the write lock, so this is an unsynchronised read — a data race the Go race detector will flag. |
| `Load` | L350–L353 | 🟡 NEEDS_FIX | 85% | Between releasing RLock (~L308) and acquiring Lock for the write, another goroutine can call Init and advance the plugin to StatusRunning. The unconditional `m.plugins[name] = info` then overwrites the running plugin's info with stale StatusLoaded data, silently discarding the initialised state. |
| `Init` | L401–L402 | 🟡 NEEDS_FIX | 85% | `queryOperationNames` and `queryStepTypeNames` each create a 5 s timeout context and issue a blocking gRPC call. Both are invoked inside the `m.mu.Lock()/defer m.mu.Unlock()` section, holding the write lock for up to 10 s and blocking every concurrent Get, List, GetOperation, ListOperations, Execute, Shutdown, and ShutdownAll call. The comment on connectionsSnapshot (line ~737) explicitly warns: 'Callers must not hold the lock when making gRPC calls (which can block for seconds).' |

### `internal/infrastructure/agents/helpers.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `truncateArg` | L120–L121 | 🟡 NEEDS_FIX | 95% | const truncLen = 37 produces string(runes[:37]) + "…" = 38 runes total. The function's own doc states "yielding exactly 40 visible characters". To match the contract, truncLen must be 39 (39 + 1 ellipsis = 40). |

### `internal/infrastructure/agents/openai_compatible_provider.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `chatMessage` | L61–L66 | 🟡 NEEDS_FIX | 65% | Content is `string` not `*string`: API-returned `"content":null` is coerced to `""` and re-serialized as `"content":""` when the assistant tool-call message is appended to history and re-sent in the next loop iteration. The OpenAI spec permits null content on assistant messages that carry tool_calls; stricter OpenAI-compatible backends may reject the empty-string substitution. |
| `dispatchToolCall` | L324–L350 | 🟡 NEEDS_FIX | 65% | `json.Unmarshal([]byte(""), &args)` returns `unexpected end of JSON input` when a model emits an empty-string arguments field for a no-parameter tool call. The error propagates to the model as "error: invalid tool arguments for <name>" instead of dispatching with empty args. |
| `parseMaxCompletionTokensOption` | L571–L595 | 🟡 NEEDS_FIX | 65% | Non-int/non-float64 values (e.g., string, bool) fall through the switch and silently return `nil, nil`, dropping the caller's token limit with no error. Both parseTemperatureOption and parseTopPOption return errors for unrecognized types; the inconsistency here means an invalid max_completion_tokens value is silently ignored rather than rejected. (deliberated: confirmed — Confirmed. Lines 571-595 in openai_compatible_provider.go: the type switch handles int (L582) and float64 (L587) but has no default case — non-numeric types (string, bool) fall through to `return nil, nil` at L594, silently ignoring the value. Sibling functions parseTemperatureOption (L563) and parseTopPOption (L609) both have a default case returning fmt.Errorf with the unexpected type. This is a genuine inconsistency: a user passing max_completion_tokens: "4096" (string) gets no error and the value is silently dropped. Adding a default error case matching the siblings would fix it.) |

## ⚡ Quick Wins

- [ ] <!-- ACT-d48717-1 --> **[correction · medium · small]** `internal/infrastructure/agents/openai_compatible_provider.go`: Change `Content string` to `Content *string` in chatMessage (with json tag `"content"`) so that null content from API responses round-trips as null rather than empty string when assistant tool-call messages are re-sent in the multi-turn loop. [L63]
- [ ] <!-- ACT-e3cecf-1 --> **[correction · medium · small]** `internal/infrastructure/pluginmgr/rpc_manager.go`: In Init, compute queryOperationNames and queryStepTypeNames before acquiring m.mu.Lock (store results in local variables), then assign them to pluginInfo under the lock. This prevents holding the write lock during blocking gRPC calls that can take up to 5 s each. [L401]
- [ ] <!-- ACT-e3cecf-2 --> **[correction · medium · small]** `internal/infrastructure/pluginmgr/rpc_manager.go`: In Load, read existing.Status inside the m.mu.RLock section (not after RUnlock) and perform the unconditional m.plugins[name] = info write only if the plugin is still in a non-running state at lock-acquisition time, to eliminate the data race and the TOCTOU overwrite. [L312]
- [ ] <!-- ACT-9cd1de-2 --> **[correction · medium · small]** `internal/infrastructure/pluginmgr/state_store.go`: GetState must return a deep copy of *PluginState rather than the raw internal pointer so callers cannot mutate internal fields without holding the mutex. [L225]
- [ ] <!-- ACT-b580da-1 --> **[correction · medium · small]** `internal/interfaces/cli/ui/formatter.go`: Apply f.color.Bold() to each header cell individually before joining, or strip ANSI codes before writing to the tabwriter and re-apply color after flushing. This ensures tabwriter measures only printable character widths for column alignment. [L93]
- [ ] <!-- ACT-0865af-1 --> **[correction · medium · small]** `internal/interfaces/cli/ui/interactive_prompt.go`: Guard against maxLen < 3 before slicing: add `if maxLen <= 3 { if len(s) > maxLen { return s[:maxLen] }; return s }` before the truncation return, or use `max(0, maxLen-3)` as the slice index with a conditional ellipsis. [L256]
- [ ] <!-- ACT-187c7b-1 --> **[correction · medium · small]** `pkg/httpx/client.go`: In NewClient, after applying all options, update the underlying *http.Client timeout to match c.timeout when the doer is still the default *http.Client, or document that WithTimeout only affects the context deadline and not the transport-level timeout. Otherwise callers using WithTimeout(5s) still get a 30s transport timeout and callers using WithTimeout(60s) get silently capped at 30s by the transport. [L47]
- [ ] <!-- ACT-8cee72-1 --> **[correction · medium · small]** `pkg/interpolation/reference.go`: In stripFuncName, replace the final call to firstDotPath(rest) (after stripping parens) with a recursive call to stripFuncName(rest), so that nested calls like 'json (trimSpace .ref)' correctly extract '.ref' instead of returning empty. [L201]
- [ ] <!-- ACT-de2f90-1 --> **[correction · medium · small]** `pkg/registry/github_client.go`: Remove or guard the X-RateLimit-Remaining == "0" branch so a successful 200 response with an exhausted rate-limit counter is not discarded. Either rely solely on status 429/403, or split into two separate checks: `resp.StatusCode == 429` and a separate check for the header only when status is non-2xx. [L93]

## 🔧 Refactors

- [ ] <!-- ACT-9cd1de-1 --> **[correction · high · large]** `internal/infrastructure/pluginmgr/state_store.go`: In Load, after json.Unmarshal succeeds, iterate loadedStates and delete (or replace with pluginmodel.NewPluginState()) any nil-pointer entries before assigning to s.states, preventing nil-pointer panics across all methods that dereference stored state. [L140]

## 🧹 Hygiene

- [ ] <!-- ACT-9df798-1 --> **[correction · low · trivial]** `internal/application/service.go`: Decouple the logger-nil guard from the ValidationError construction. Move `if s.logger != nil { s.logger.Warn(...) }` inside the provider-check block, and always build and return the *workflow.ValidationError when the provider is unsupported. This ensures LastValidationWarnings() is populated regardless of whether a logger is wired. [L414]
- [ ] <!-- ACT-63e31e-1 --> **[correction · low · trivial]** `internal/infrastructure/agents/helpers.go`: Change const truncLen from 37 to 39 so that string(runes[:39])+"…" yields exactly 40 visible characters, matching the documented contract. [L121]
- [ ] <!-- ACT-d48717-2 --> **[correction · low · trivial]** `internal/infrastructure/agents/openai_compatible_provider.go`: In dispatchToolCall, normalize empty-string Arguments to `"{}"` before unmarshaling: `if tc.Function.Arguments == "" { tc.Function.Arguments = "{}" }`. Prevents a spurious parse error for no-parameter tool calls from models that omit the field. [L330]
- [ ] <!-- ACT-d48717-3 --> **[correction · low · trivial]** `internal/infrastructure/agents/openai_compatible_provider.go`: In parseMaxCompletionTokensOption, add a default switch case that returns an error for non-int/non-float64 types, consistent with parseTemperatureOption and parseTopPOption. [L594]
