[← Back to Correction](./index.md) · [← Back to report](../../public_report.md)

# 🐛 Correction — Shard 1

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)
- [🧹 Hygiene](#-hygiene)

## 📊 Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `internal/infrastructure/audit/file_writer.go` | 🔴 CRITICAL | 1 | 95% | [details](#internalinfrastructureauditfilewritergo) |
| `internal/interfaces/tui/command.go` | 🔴 CRITICAL | 1 | 95% | [details](#internalinterfacestuicommandgo) |
| `internal/testutil/mocks/mocks.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internaltestutilmocksmocksgo) |
| `internal/testutil/builders/builders.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internaltestutilbuildersbuildersgo) |
| `internal/interfaces/cli/ui/output.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacescliuioutputgo) |
| `pkg/plugin/sdk/sdk.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#pkgpluginsdksdkgo) |
| `internal/infrastructure/xdg/xdg.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructurexdgxdggo) |
| `pkg/registry/version.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#pkgregistryversiongo) |
| `internal/interfaces/tui/bridge.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinterfacestuibridgego) |
| `internal/domain/workflow/loop.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internaldomainworkflowloopgo) |

## 🔍 Symbol Details

### `internal/infrastructure/audit/file_writer.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `truncateInputs` | L108–L110 | 🔴 ERROR | 95% | The termination guard checks len(event.Inputs) == 0, but the loop never removes keys — it only replaces values. With enough keys the serialized JSON with all-'…' values still exceeds posixPipeBuf, yet len(event.Inputs) stays constant. Fix: call delete(event.Inputs, longestKey) instead of assigning '…', or break when the selected key's current value is already the placeholder. |
| `truncateInputs` | L113–L114 | 🔴 ERROR | 95% | When all inputs are non-string (valueLength returns 0 for every entry), longestInputKey returns the lex-first key on every iteration; that key gets set to '…' (a string, length 3), but on the next pass it wins again with length 3 vs 0 for remaining keys. The loop never advances past the first key, producing the same infinite-loop scenario. |

### `internal/interfaces/tui/command.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `buildBridge` | L139–L140 | 🔴 ERROR | 95% | histErr path: returns nil *TUIInputReader with nil error before inputReader is ever created. Caller runTUI checks only err, finds nil, then calls inputReader.SetSender(p.Send) → nil pointer panic. |
| `buildBridge` | L195–L197 | 🔴 ERROR | 95% | buildErr path: inputReader was created at line 185 but the return still passes nil in its slot. Same nil-deref panic in caller; also leaks the allocated TUIInputReader. |

### `internal/interfaces/cli/ui/output.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `writeValidationResultTable` | L1254–L1300 | 🟡 NEEDS_FIX | 72% | If len(result.Inputs)==0 && len(result.Steps)==0 neither the inputs block nor the steps block is entered, so renderStatusHeader is never called. The function then only prints raw ERROR:/WARNING: lines with no workflow name or valid/invalid status, making the output uninterpretable for the caller. |

### `pkg/plugin/sdk/sdk.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `sprintfImpl` | L310–L317 | 🟡 NEEDS_FIX | 90% | Each iteration passes the already-modified `result` to replaceFirst. If a replacement string itself contains %s/%v/%d, the NEXT iteration finds that injected specifier before the next real placeholder — the next arg replaces the wrong slot and the intended placeholder is left unreplaced. Example: sprintfImpl("%s %s", "hello %s", "world") yields "hello world %s" instead of "hello %s world". |
| `intToString` | L335–L336 | 🟡 NEEDS_FIX | 95% | When n == math.MinInt (e.g. -9223372036854775808 on 64-bit), `n = -n` overflows to math.MinInt again. `for n > 0` is immediately false; `digits` stays empty; the function returns "-" — a silent wrong result. |

## ⚡ Quick Wins

- [ ] <!-- ACT-ff0e03-1 --> **[correction · medium · small]** `internal/domain/workflow/loop.go`: In Validate, when MaxIterations==0 and !MaxIterationsExplicitlySet and !IsMaxIterationsDynamic(), set c.MaxIterations=DefaultMaxIterations (or return an error requiring callers to supply a non-zero value) to prevent a zero iteration limit from silently disabling the safety cap. [L60]
- [ ] <!-- ACT-ff0e03-2 --> **[correction · medium · small]** `internal/domain/workflow/loop.go`: In AllSucceeded, change the empty-slice guard to `len(r.Iterations) > 0 || r.PrunedCount > 0` so that a fully-pruned-but-successful loop returns true rather than false. [L138]
- [ ] <!-- ACT-1ab882-2 --> **[correction · medium · small]** `internal/infrastructure/audit/file_writer.go`: In Write (L62), add an explicit nil check on w.file inside the mutex-protected block and return a descriptive 'audit writer is closed' error rather than allowing os.File.Write to surface os.ErrInvalid. [L62]
- [ ] <!-- ACT-1ab882-3 --> **[correction · medium · small]** `internal/infrastructure/audit/file_writer.go`: In Write (L53) or at the top of truncateInputs, shallow-copy event.Inputs into a new map before modifying it so the caller's AuditEvent is not permanently mutated. [L53]
- [ ] <!-- ACT-b1a229-1 --> **[correction · medium · small]** `internal/infrastructure/xdg/xdg.go`: AWFConfigDir: guard against ConfigHome() returning ""; either return "" early when ConfigHome() returns "", or propagate the error so callers can distinguish a missing home directory from a valid path. [L42]
- [ ] <!-- ACT-b1a229-2 --> **[correction · medium · small]** `internal/infrastructure/xdg/xdg.go`: AWFDataDir: same fix — return "" when DataHome() returns "" rather than producing the relative path "awf". [L48]
- [ ] <!-- ACT-de0f12-1 --> **[correction · medium · small]** `internal/interfaces/cli/ui/output.go`: In writeValidationResultTable, add a fallback path: when both result.Inputs and result.Steps are empty, create a tableWriter and call renderStatusHeader to display the workflow name and validity before printing errors/warnings. [L1254]
- [ ] <!-- ACT-d73e13-1 --> **[correction · medium · small]** `internal/interfaces/tui/bridge.go`: Add nil guard for b.workflows in LoadWorkflows before calling ListAllWorkflows; return ErrMsg{Err: errors.New("workflows service not configured")} to fulfil the NewBridge nil-dependency contract. [L118]
- [ ] <!-- ACT-d73e13-2 --> **[correction · medium · small]** `internal/interfaces/tui/bridge.go`: Add nil guard for b.history in LoadHistory before calling List; return ErrMsg{Err: errors.New("history service not configured")}. [L148]
- [ ] <!-- ACT-d73e13-3 --> **[correction · medium · small]** `internal/interfaces/tui/bridge.go`: Add nil guard for b.workflows in ValidateWorkflow before calling ValidateWorkflow; return ValidationResultMsg{Name: name, Success: false, Error: "workflows service not configured"}. [L218]
- [ ] <!-- ACT-89ec69-1 --> **[correction · medium · small]** `internal/testutil/builders/builders.go`: Add nil guard in WithInput before dereferencing: if input == nil { return b } or accept workflow.Input by value instead of pointer. [L294]
- [ ] <!-- ACT-89ec69-2 --> **[correction · medium · small]** `internal/testutil/builders/builders.go`: Log or return an error (or panic in test context) when the type assertion on b.evaluator fails in Build(), so misconfigured evaluators are caught immediately rather than silently omitted. [L197]
- [ ] <!-- ACT-d2caf6-1 --> **[correction · medium · small]** `internal/testutil/mocks/mocks.go`: Fix the dual-mutex data race in MockPluginStateStore: instead of having MockPluginStore and MockPluginConfig each use their own sync.RWMutex to protect the shared states map, introduce a single shared mutex (e.g. a *sync.RWMutex pointer set in NewMockPluginStateStore) and pass it to both embedded structs so all accesses to the shared map are mutually exclusive. [L1773]
- [ ] <!-- ACT-9602d3-1 --> **[correction · medium · small]** `pkg/plugin/sdk/sdk.go`: Fix sprintfImpl to track the scan offset so that replacement values containing format specifiers are not consumed by subsequent arguments. The simplest fix: pass an offset into replaceFirst (or scan left-to-right and advance past each replacement position rather than rescanning from index 0 each time). [L310]
- [ ] <!-- ACT-9602d3-2 --> **[correction · medium · small]** `pkg/plugin/sdk/sdk.go`: Fix intToString to handle math.MinInt explicitly before negation. Add: `if n == math.MinInt { return "-9223372036854775808" }` (adjust constant for int32 platforms), or use strconv.Itoa instead of this hand-rolled implementation. [L335]
- [ ] <!-- ACT-3ba903-1 --> **[correction · medium · small]** `pkg/registry/version.go`: In Compare (L79–L82): replace raw string comparison with a per-identifier semver comparison — split both prerelease strings on '.', compare all-digit identifiers as integers (strconv.Atoi), alphanumeric identifiers lexicographically, and treat numeric identifiers as lower precedence than alphanumeric. [L79]
- [ ] <!-- ACT-3ba903-2 --> **[correction · medium · small]** `pkg/registry/version.go`: In Constraint.Check OpCaret branch (L125–L130): add inner guard for c.Version.Minor==0 — when both Major and Minor are 0, require v.Patch == c.Version.Patch (not >=) so ^0.0.3 rejects 0.0.4 as semver specifies. [L130]
- [ ] <!-- ACT-3ba903-3 --> **[correction · medium · small]** `pkg/registry/version.go`: In Constraint.Check OpTilde (L117) and OpCaret lower-bound returns (L130, L137): replace 'v.Patch >= c.Version.Patch' lower-bound logic with 'v.Compare(c.Version) >= 0' so prerelease versions whose patch equals the constraint patch (e.g., 1.2.0-alpha vs ~1.2.0) are correctly rejected. [L117]

## 🔧 Refactors

- [ ] <!-- ACT-1ab882-1 --> **[correction · high · large]** `internal/infrastructure/audit/file_writer.go`: In truncateInputs (L114), call delete(event.Inputs, longestKey) instead of assigning '…', OR break the loop when the key's current value is already the placeholder. This ensures len(event.Inputs) eventually reaches 0, giving the early-exit guard on L108 a reachable path. [L114]
- [ ] <!-- ACT-3a9c7e-1 --> **[correction · high · large]** `internal/interfaces/tui/command.go`: histErr early-return (line 140): either propagate the error (`return NewBridge(nil, nil), nil, nopCleanup, histErr`) so runTUI's err-check catches it, or return the (not-yet-created) inputReader alongside a non-nil error so the caller never reaches inputReader.SetSender on a nil value. [L140]
- [ ] <!-- ACT-3a9c7e-2 --> **[correction · high · large]** `internal/interfaces/tui/command.go`: buildErr early-return (line 197): either propagate the error (`return NewBridge(nil, nil), nil, nopCleanup, buildErr`) or return the already-created inputReader. Returning nil silently skips the error and delivers a nil *TUIInputReader to runTUI, which then panics at inputReader.SetSender. [L197]

## 🧹 Hygiene

- [ ] <!-- ACT-d73e13-4 --> **[correction · low · trivial]** `internal/interfaces/tui/bridge.go`: In ReadInput, after the ctx.Done() branch returns, drain responseCh with a non-blocking receive (select { case <-r.responseCh: default: }) so a late Respond call cannot be consumed as valid input by the next ReadInput invocation. [L270]
- [ ] <!-- ACT-d2caf6-2 --> **[correction · low · trivial]** `internal/testutil/mocks/mocks.go`: Fix the WithContext append-aliasing race: replace `append(m.ctxFields, ctxFields...)` with an explicit copy+append — allocate a new slice of len(m.ctxFields)+len(ctxFields), copy m.ctxFields into it, then append ctxFields. This eliminates backing-array sharing between sibling loggers and the parent's spare capacity, and should be done while holding m.mu to guard the read of m.ctxFields. [L663]
- [ ] <!-- ACT-3ba903-4 --> **[correction · low · trivial]** `pkg/registry/version.go`: In ParseVersion (L200–L203): after extracting matches[4], strip build metadata by splitting on '+' and using only the first segment as the Prerelease field, so versions differing only in build metadata compare as equal per semver §10. [L202]
