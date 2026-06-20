[← Back to Correction](./index.md) · [← Back to report](../../public_report.md)

# 🐛 Correction — Shard 4

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🧹 Hygiene](#-hygiene)

## 📊 Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `internal/infrastructure/agents/mistral_vibe_provider.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructureagentsmistralvibeprovidergo) |
| `internal/infrastructure/github/provider.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinfrastructuregithubprovidergo) |
| `internal/infrastructure/pluginmgr/event_bus.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructurepluginmgreventbusgo) |
| `internal/interfaces/api/bridge.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinterfacesapibridgego) |
| `internal/interfaces/cli/ui/output_writer.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacescliuioutputwritergo) |
| `internal/interfaces/tui/tailer.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinterfacestuitailergo) |
| `pkg/validation/validator.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#pkgvalidationvalidatorgo) |
| `internal/application/dry_run_executor.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalapplicationdryrunexecutorgo) |
| `internal/application/history_service.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalapplicationhistoryservicego) |
| `internal/domain/workflow/workflow.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internaldomainworkflowworkflowgo) |

## 🔍 Symbol Details

### `internal/infrastructure/agents/mistral_vibe_provider.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `extractMistralVibeAssistantTextFromNDJSON` | L799–L800 | 🟡 NEEDS_FIX | 82% | return "", false on json.Unmarshal error throws away lastText already accumulated from prior valid NDJSON lines. A truncated or partially-malformed streaming response produces empty output instead of the last valid assistant text. Should continue to next line (and optionally reset found) rather than discarding accumulated results. |

### `internal/infrastructure/github/provider.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `handleBatch` | L464–L466 | 🟡 NEEDS_FIX | 92% | inputs["concurrency"].(int) always fails for JSON-sourced inputs (float64). Should assert float64 first, as done in formatNumber, or use a helper that handles both int and float64. |
| `fetchExistingPR` | L516–L523 | 🟡 NEEDS_FIX | 90% | Error path returns (result{Success:false}, nil). Every other handler error path returns both a non-nil result and a non-nil error. handleCreatePR does `return p.fetchExistingPR(...)`, so a runner failure here surfaces as a nil error to the caller, which will miss the failure if checking err != nil. |

### `internal/interfaces/cli/ui/output_writer.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `writePrefixedLine` | L76–L77 | 🟡 NEEDS_FIX | 95% | `_, _ = w.out.Write(...)` silently drops errors on both the prefix write and the line write. Because writePrefixedLine returns void, callers have no mechanism to receive or propagate these errors; Write therefore always returns nil error, violating the io.Writer contract. |

### `pkg/validation/validator.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `validateFileExtensionWithType` | L177 | 🟡 NEEDS_FIX | 85% | `if ext == allowed` is a byte-exact comparison. Replace with `strings.EqualFold(ext, allowed)` to match the contract established by the dead `validateFileExtension` function and to handle common mixed-case extensions (`.TXT`, `.Txt`, etc.). |
| `coerceToInt` | L247 | 🟡 NEEDS_FIX | 70% | `return int(v), nil` truncates any float64 without checking whether it represents a whole number. In a validation context, inputs like `3.7` should produce an error (e.g., `if v != math.Trunc(v) { return error }`) rather than silently coercing to `3`. |

### `internal/application/dry_run_executor.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `resolveInputs` | L250 | 🟡 NEEDS_FIX | 65% | `inputs[inputDef.Name] != nil` treats an explicit nil value the same as a missing key. Use the two-value form `val, ok := inputs[inputDef.Name]; ok` to correctly distinguish the two cases. |
| `buildHooks` | L333 | 🟡 NEEDS_FIX | 82% | `result.Pre = append(result.Pre, hook)` runs outside the if/else-if block; when an action has neither Log nor Command the empty DryRunHook{} (Type:"", Content:"") is still appended. Move the append inside each branch, or guard with `if hook.Type != ""`. |
| `buildHooks` | L348 | 🟡 NEEDS_FIX | 82% | Same unconditional append for Post hooks: `result.Post = append(result.Post, hook)` runs even when the action carries neither Log nor Command. |

## ⚡ Quick Wins

- [ ] <!-- ACT-f0cee0-2 --> **[correction · medium · small]** `internal/application/dry_run_executor.go`: In buildHooks Pre loop, move `result.Pre = append(result.Pre, hook)` inside each populated branch (Log / Command) so that actions with unrecognised or empty fields do not inject empty DryRunHook{} entries into the plan. [L333]
- [ ] <!-- ACT-f0cee0-3 --> **[correction · medium · small]** `internal/application/dry_run_executor.go`: Apply the same fix to the Post hooks loop: guard the append so an empty DryRunHook is not added when neither Log nor Command is set. [L348]
- [ ] <!-- ACT-ba7d55-1 --> **[correction · medium · small]** `internal/application/history_service.go`: Replace the fixed scanLimit sentinel in GetByID with paginated iteration over all records (fetch pages until an empty page is returned) or, preferably, add an ID-scoped lookup method to the HistoryStore port so no full scan is needed. The current cap causes GetByID to return a false ErrRecordNotFound for any record stored beyond position 10,000. [L82]
- [ ] <!-- ACT-837d39-1 --> **[correction · medium · small]** `internal/domain/workflow/workflow.go`: Inside the step-range loop, append structural reference errors (transition-target missing, OnSuccess/OnFailure missing, loop-body missing, on_complete missing, continue_from missing) to stepErrs instead of returning early, so errors.Join at the end surfaces all failures together and previously accumulated step.Validate() errors are not silently discarded. [L153]
- [ ] <!-- ACT-fc9527-1 --> **[correction · medium · small]** `internal/infrastructure/agents/mistral_vibe_provider.go`: In extractMistralVibeAssistantTextFromNDJSON, replace `return "", false` (L800) with `continue` so that a JSON parse error on one line skips that line rather than discarding all previously accumulated assistant text. If strict failure semantics are required, at minimum return `lastText, found` instead of the zero values. [L800]
- [ ] <!-- ACT-bdc0af-1 --> **[correction · medium · small]** `internal/infrastructure/github/provider.go`: In handleBatch, replace `inputs["concurrency"].(int)` with a float64-aware assertion: `if mc, ok := inputs["concurrency"].(float64); ok && mc > 0 { maxConcurrent = int(mc) }` — or handle both int and float64 cases — so user-supplied concurrency values from JSON inputs are actually applied. [L464]
- [ ] <!-- ACT-bdc0af-2 --> **[correction · medium · small]** `internal/infrastructure/github/provider.go`: In fetchExistingPR, return the error alongside the failed result (matching every other error path in this file): `return &pluginmodel.OperationResult{...}, fmt.Errorf("fetch existing PR: %w", err)`. The current nil error silently swallows the failure in handleCreatePR's caller. [L523]
- [ ] <!-- ACT-e77ff3-1 --> **[correction · medium · small]** `internal/infrastructure/pluginmgr/event_bus.go`: In Subscribe, check whether a subscription already exists for pluginName and unsubscribe it (close old sub.done, drain/wait on sub.stopped) before installing the new subscription and launching a new runDelivery goroutine. This prevents goroutine leaks on duplicate registrations. [L100]
- [ ] <!-- ACT-54aad8-1 --> **[correction · medium · small]** `internal/interfaces/api/bridge.go`: After calling ae.Cancel(), add b.activeExecutions.Delete(id) so the cancelled execution is no longer returned by GetExecution or ListExecutions. This aligns with the HTTP DELETE contract and prevents unbounded map growth. [L62]
- [ ] <!-- ACT-a21636-1 --> **[correction · medium · small]** `internal/interfaces/cli/ui/output_writer.go`: Change writePrefixedLine to return an error (combine the two w.out.Write calls, propagating the first failure). Update Write to capture this error, return it instead of nil, and adjust n accordingly when a write fails. [L74]
- [ ] <!-- ACT-571ba1-1 --> **[correction · medium · small]** `internal/interfaces/tui/tailer.go`: Fix partial-line offset overcounting in Follow: after the scan loop, update t.offset by re-seeking to determine the true current file position rather than accumulating `len(line)+1` per scanned token. Alternatively, track whether the scanner's final token was terminated by checking if the consumed byte count (tracked independently) matches file growth, and omit the +1 for an unterminated final token. [L152]
- [ ] <!-- ACT-0077dc-1 --> **[correction · medium · small]** `pkg/validation/validator.go`: Replace `if ext == allowed` with `if strings.EqualFold(ext, allowed)` in validateFileExtensionWithType to match case-insensitively, consistent with the dead validateFileExtension companion and with filesystem behavior on Windows/macOS. [L177]

## 🧹 Hygiene

- [ ] <!-- ACT-f0cee0-1 --> **[correction · low · trivial]** `internal/application/dry_run_executor.go`: In resolveInputs, replace `inputs[inputDef.Name] != nil` with the two-value map lookup `val, ok := inputs[inputDef.Name]; ok` so that an explicit nil value is accepted rather than triggering the default/required fallback. [L250]
- [ ] <!-- ACT-0077dc-2 --> **[correction · low · trivial]** `pkg/validation/validator.go`: In coerceToInt's float64 case, check `if v != math.Trunc(v)` before converting and return an error for non-integer floats, so values like 3.7 are rejected rather than silently truncated to 3. [L247]
