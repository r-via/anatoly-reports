[← Back to Correction](./index.md) · [← Back to report](../../public_report.md)

# 🐛 Correction — Shard 3

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🧹 Hygiene](#-hygiene)

## 📊 Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `internal/infrastructure/pluginmgr/stream_manager.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructurepluginmgrstreammanagergo) |
| `internal/application/interactive_executor.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalapplicationinteractiveexecutorgo) |
| `internal/domain/workflow/audit_event.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internaldomainworkflowauditeventgo) |
| `internal/domain/workflow/subworkflow.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internaldomainworkflowsubworkflowgo) |
| `internal/infrastructure/agents/cli_executor.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructureagentscliexecutorgo) |
| `internal/infrastructure/repository/composite_repository.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructurerepositorycompositerepositorygo) |
| `internal/infrastructure/transcript/recorder.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructuretranscriptrecordergo) |
| `internal/interfaces/cli/ui/dry_run_formatter.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacescliuidryrunformattergo) |
| `internal/application/template_service.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalapplicationtemplateservicego) |
| `internal/application/tools/router.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalapplicationtoolsroutergo) |

## 🔍 Symbol Details

### `internal/infrastructure/pluginmgr/stream_manager.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `sendWithTimeout` | L116–L119 | 🟡 NEEDS_FIX | 82% | When timer.C or ctx.Done() wins the select, sendWithTimeout returns an error and DeliverEvent proceeds to the unary fallback. The goroutine on L117 is still executing send() against the stream. If it eventually succeeds, the event is delivered via both the stream and the unary fallback. The buffered channel (capacity 1) prevents goroutine blocking, but not duplicate delivery. Fix: pass a derived context to send() so it can be cancelled when a timeout or parent-ctx cancellation is detected. |

### `internal/application/interactive_executor.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `executeStep` | L481 | 🟡 NEEDS_FIX | 82% | After the execErr != nil early-return (L471-L479), the code accesses result.ExitCode directly. The earlier block at L465-L469 explicitly guards 'if result != nil' before reading result fields, confirming the developer knows result can be nil. If the executor returns (nil, nil), this line panics. Add a nil guard: if result == nil { treat as success with exit code 0 } or enforce that the executor contract guarantees non-nil result on nil error. |
| `executeLoopStep` | L803 | 🟡 NEEDS_FIX | 85% | return step.Loop.OnComplete, nil — step.Loop is never nil-checked before this access. formatTransitions (L327) correctly guards with 'if step.Loop != nil', but executeLoopStep omits the same guard. A ForEach/While step with a missing Loop field (misconfigured or partially constructed) will panic here. Add: if step.Loop == nil { return "", fmt.Errorf("loop step %s missing loop configuration", step.Name) }. |

### `internal/domain/workflow/audit_event.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `writeJSONField` | L114–L123 | 🟡 NEEDS_FIX | 90% | json.Marshal(value) errors are silently discarded. When value is a map[string]any containing non-serializable types (chan, func, etc.), valBytes is nil, buf.Write(nil) emits nothing, and the caller's MarshalJSON returns malformed JSON — e.g. '"inputs":' with no value — while still returning a nil error. Audit records are silently corrupted. |

### `internal/infrastructure/agents/cli_executor.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `findChildPIDs` | L123–L132 | 🟡 NEEDS_FIX | 70% | closeParenIdx is initialized to len(statStr)-1. If the backward search finds no ')', the value is never updated. statStr[closeParenIdx+2:] then becomes statStr[len(statStr)+1:], which panics (index > len). The same panic fires on empty statStr: closeParenIdx=-1, statStr[1:] on a zero-length string panics. A found-flag guard with continue on failure fixes both cases. |

### `internal/interfaces/cli/ui/dry_run_formatter.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `formatStepHeader` | L162–L183 | 🟡 NEEDS_FIX | 72% | switch covers StepTypeTerminal, StepTypeParallel, StepTypeForEach, StepTypeWhile, but omits StepTypeAgent. stepTypeLabel returns "[AGENT]" for it, matching the named-label pattern of the other explicit cases. The default branch ignores label entirely, so agent step headers appear as "[N] stepName" rather than "[AGENT] stepName". |

### `internal/application/template_service.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `expandStep` | L101 | 🟡 NEEDS_FIX | 88% | step.Command is unconditionally overwritten with the template-expanded command, discarding any existing command already set on the workflow step. ApplyTemplateFields — the exported equivalent — correctly guards with `if step.Command == ""` so the step's own value takes precedence. A workflow step that legitimately carries both TemplateRef and Command will silently lose its command at runtime. |
| `expandStep` | L114–L116 | 🟡 NEEDS_FIX | 88% | Retry merge is all-or-nothing: when step.Retry != nil the entire template Retry block is skipped. ApplyTemplateFields merges MaxAttempts, InitialDelayMs, and Backoff individually, filling in only the fields the step left at their zero values. A step with only MaxAttempts configured will never receive the template's Backoff or InitialDelayMs through expandStep. |
| `expandStep` | L119–L121 | 🟡 NEEDS_FIX | 88% | Capture merge has the same all-or-nothing defect as Retry: when step.Capture != nil the template's Capture is ignored wholesale, while ApplyTemplateFields merges Stdout and Stderr field-by-field. A step with only Stdout set will not inherit the template's Stderr. |
| `ValidateAndLoadTemplate` | L148–L150 | 🟡 NEEDS_FIX | 90% | `for k := range visited` produces a different key order on every run. A cycle A→B→A can report Chain as [B,A,A] or [A,B,A] arbitrarily. The error becomes useless for diagnosing which template introduced the cycle. Fix: thread an ordered []string call-chain through expandStep/ValidateAndLoadTemplate instead of reconstructing order from the unordered map. |
| `SelectPrimaryStep` | L184–L185 | 🟡 NEEDS_FIX | 95% | Go randomises map iteration order per run; returning the first element of `tmpl.States` is non-deterministic. Templates with multiple states and no state whose key matches tmpl.Name will expand to a different step on different runs. The inline comment acknowledges the issue but accepts it — correct fix is a deterministic tie-breaking policy (e.g., lexicographically smallest state name) or an explicit primary-step annotation in the template schema. |

## ⚡ Quick Wins

- [ ] <!-- ACT-f18b00-1 --> **[correction · medium · small]** `internal/application/interactive_executor.go`: In Run's ActionSkip case, replace 'currentStep = step.OnSuccess' with a call to resolveNextStep(step, interpCtx, true) so steps using Transitions are routed correctly and steps with no OnSuccess do not produce a 'step not found:' error. [L205]
- [ ] <!-- ACT-f18b00-2 --> **[correction · medium · small]** `internal/application/interactive_executor.go`: In executeStep, add a nil check for result after the execErr != nil early-return: if result == nil { treat as exit code 0 / completed } to prevent panic on a (nil, nil) return from the executor. [L481]
- [ ] <!-- ACT-f18b00-3 --> **[correction · medium · small]** `internal/application/interactive_executor.go`: In executeLoopStep, nil-check step.Loop before accessing step.Loop.OnComplete on the success return path, returning an explicit error if Loop is nil. [L803]
- [ ] <!-- ACT-df07ec-1 --> **[correction · medium · small]** `internal/application/template_service.go`: Guard step.Command assignment with `if step.Command == ""` so the workflow step's existing command takes precedence over the template command, matching ApplyTemplateFields semantics. [L101]
- [ ] <!-- ACT-df07ec-2 --> **[correction · medium · small]** `internal/application/template_service.go`: Replace all-or-nothing Retry merge with field-level merge (MaxAttempts, InitialDelayMs, Backoff) mirroring ApplyTemplateFields, so a step with partial Retry config receives missing fields from the template. [L114]
- [ ] <!-- ACT-df07ec-3 --> **[correction · medium · small]** `internal/application/template_service.go`: Replace all-or-nothing Capture merge with field-level merge (Stdout, Stderr) mirroring ApplyTemplateFields. [L119]
- [ ] <!-- ACT-df07ec-5 --> **[correction · medium · small]** `internal/application/template_service.go`: Replace non-deterministic map-iteration fallback with a deterministic tie-breaking policy (e.g., sort state names and take the first) so SelectPrimaryStep returns the same step on every run. [L184]
- [ ] <!-- ACT-c357ae-1 --> **[correction · medium · small]** `internal/application/tools/router.go`: Acquire r.mu.Lock() in SetRecorder before writing r.recorder (and RLock in CallTool when reading it, or snapshot recorder+runID immediately after the existing RLock at L102 before releasing at L104). [L38]
- [ ] <!-- ACT-c357ae-2 --> **[correction · medium · small]** `internal/application/tools/router.go`: Acquire r.mu.Lock() in SetRunID before writing r.runID; consistent with the fix above. [L44]
- [ ] <!-- ACT-1a334c-1 --> **[correction · medium · small]** `internal/domain/workflow/audit_event.go`: Change writeJSONField signature to return error. Propagate json.Marshal(value) errors to callers, and update MarshalJSON to check and return those errors rather than always returning nil. [L117]
- [ ] <!-- ACT-b1ef22-1 --> **[correction · medium · small]** `internal/domain/workflow/subworkflow.go`: GetTimeout should return 0 (or a separate bool/sentinel) when c.Timeout==0 so callers can honour the 'inherit from step' contract documented on the Timeout field, rather than silently substituting DefaultSubWorkflowTimeout. [L38]
- [ ] <!-- ACT-37898c-1 --> **[correction · medium · small]** `internal/infrastructure/agents/cli_executor.go`: Move descendant cleanup before cmd.Wait(): scan and kill /proc descendants immediately after sending SIGKILL to the process group (e.g., inside cmd.Cancel or right after), before waiting for the main process to exit. After cmd.Wait() returns, orphaned grandchildren have already been reparented to init and cannot be found via ppid scan. [L81]
- [ ] <!-- ACT-37898c-3 --> **[correction · medium · small]** `internal/infrastructure/agents/cli_executor.go`: In Start(), set cmd.Cancel = func() error { if cmd.Process == nil { return nil }; return syscall.Kill(-cmd.Process.Pid, syscall.SIGKILL) } immediately after creating the command, to kill the entire process group on context cancellation — consistent with RunWithEnv. [L190]
- [ ] <!-- ACT-688884-1 --> **[correction · medium · small]** `internal/infrastructure/pluginmgr/stream_manager.go`: In DeliverEvent, check if the sendWithTimeout error wraps ctx.Err() before calling UnregisterStream; skip unregistration when the error is due to context cancellation rather than a stream fault. [L106]
- [ ] <!-- ACT-688884-2 --> **[correction · medium · small]** `internal/infrastructure/pluginmgr/stream_manager.go`: In DeliverEvent, acquire the write lock and compare the current map entry against d.entry before deleting; only call delete() when the map still holds the stale entry, not a newly registered one. [L107]
- [ ] <!-- ACT-688884-3 --> **[correction · medium · small]** `internal/infrastructure/pluginmgr/stream_manager.go`: In sendWithTimeout, create a child context with cancel and pass it to send(); on timer or parent-ctx expiry, call cancel() so the in-flight stream send is interrupted and cannot succeed after the caller has already fallen back, preventing duplicate event delivery. [L116]
- [ ] <!-- ACT-176f28-1 --> **[correction · medium · small]** `internal/infrastructure/repository/composite_repository.go`: In Exists, mirror Load's error-discrimination pattern: only continue on ErrorCodeUserInputMissingFile; propagate all other errors so callers receive (false, err) instead of a silent false negative. [L133]
- [ ] <!-- ACT-95e81b-1 --> **[correction · medium · small]** `internal/infrastructure/transcript/recorder.go`: In Close()'s closeOnce.Do body, call r.writerOnce.Do(func(){}) before reading r.writer to ensure any in-flight initWriter completes and the file descriptor is not leaked. [L112]

## 🧹 Hygiene

- [ ] <!-- ACT-df07ec-4 --> **[correction · low · trivial]** `internal/application/template_service.go`: Thread an ordered call-chain slice through expandStep/ValidateAndLoadTemplate so CircularTemplateError.Chain reports the actual traversal order instead of an unordered map snapshot. [L148]
- [ ] <!-- ACT-37898c-2 --> **[correction · low · trivial]** `internal/infrastructure/agents/cli_executor.go`: In findChildPIDs, track whether ')' was actually found in the backward search. If no ')' was found, or if closeParenIdx+2 > len(statStr), continue to the next /proc entry instead of panicking with an out-of-bounds slice. [L132]
- [ ] <!-- ACT-ac9d6d-1 --> **[correction · low · trivial]** `internal/interfaces/cli/ui/dry_run_formatter.go`: Add a case for workflow.StepTypeAgent in formatStepHeader's switch so it uses fmt.Sprintf("\n%s %s", label, step.Name) like the other named-label types, instead of falling through to the numeric-index default. [L162]
