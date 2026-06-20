[← Back to Correction](./index.md) · [← Back to report](../../public_report.md)

# 🐛 Correction — Shard 8

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🧹 Hygiene](#-hygiene)

## 📊 Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `internal/interfaces/cli/ui/input_collector.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacescliuiinputcollectorgo) |
| `internal/interfaces/cli/validate.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacesclivalidatego) |
| `internal/interfaces/tui/tab_monitoring.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacestuitabmonitoringgo) |
| `internal/interfaces/tui/tab_workflows.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacestuitabworkflowsgo) |
| `pkg/interpolation/template_resolver.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#pkginterpolationtemplateresolvergo) |
| `internal/infrastructure/github/client.go` | 🟡 NEEDS_REFACTOR | 0 | 92% | [details](#internalinfrastructuregithubclientgo) |
| `internal/application/role_loader.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalapplicationroleloadergo) |
| `internal/domain/errors/catalog.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internaldomainerrorscataloggo) |
| `internal/domain/operation/validate.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internaldomainoperationvalidatego) |
| `internal/domain/pluginmodel/manifest.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internaldomainpluginmodelmanifestgo) |

## 🔍 Symbol Details

### `internal/interfaces/cli/ui/input_collector.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `validate` | L265–L267 | 🟡 NEEDS_FIX | 70% | os.IsNotExist(statErr) returns false for permission-denied and other non-ErrNotExist errors, so the condition is never entered and validation returns nil (pass). A file that exists but is unreadable incorrectly passes; the subsequent use of that path will fail with a confusing error. The check should be `if statErr != nil` (or at minimum `os.IsNotExist(statErr) \|\| (statErr != nil && !os.IsPermission(statErr))`), returning an error for any stat failure. |

### `internal/interfaces/cli/validate.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `runValidateViaFacade` | L112 | 🟡 NEEDS_FIX | 95% | `validationErr` is set only when `!report.Valid && len(errMessages) > 0`. If `report.Valid == false` with zero errors, `validationErr` remains nil — diverging from the report. |
| `runValidateViaFacade` | L116 | 🟡 NEEDS_FIX | 95% | JSON/Quiet path: `Valid: validationErr == nil` reports `true` when `report.Valid` is false but `report.Errors` is empty. Should be `Valid: report.Valid`. |
| `runValidateViaFacade` | L133 | 🟡 NEEDS_FIX | 95% | Table path: same bug — `Valid: validationErr == nil` instead of `Valid: report.Valid`. |
| `runValidateViaFacade` | L153–L156 | 🟡 NEEDS_FIX | 95% | Text path: `if validationErr != nil` guard silently prints success and returns nil when the report says invalid with no error messages. |

### `internal/interfaces/tui/tab_monitoring.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `convAppendNewTurns` | L605–L608 | 🟡 NEEDS_FIX | 85% | t.stream.Reset() inside the loop body fires on the first TurnRoleAssistant encountered. If convTurnCount jumps by ≥2 and multiple assistant turns exist in the new range, subsequent assistant turns (including the actual live one) fall through to turn.Content instead of the stream content. The stream should only be consumed for i == len(conv.Turns)-1 (the last new turn) or, at minimum, only for the last assistant turn in the loop. |

### `internal/interfaces/tui/tab_workflows.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `handleValidate` | L298–L309 | 🟡 NEEDS_FIX | 65% | Sets validating=true but does not clear validationResult; stale overlay takes priority over spinner in View, so the user sees no indication that re-validation is in progress. |

### `pkg/interpolation/template_resolver.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `wrapExecutionError` | L84–L90 | 🟡 NEEDS_FIX | 92% | `start = strings.Index(errStr, "\"")` matches the opening quote of the template name in the error prefix `executing "cmd" at …`, not the opening quote of the missing key. `varName` is thus the entire middle portion of the error string rather than the key name. |

### `internal/domain/operation/validate.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `validateType` | L137–L140 | 🟡 NEEDS_FIX | 65% | For float64 values where \|v\| > math.MaxInt64 (~9.22e18), `int64(v)` overflows; Go spec says the result is implementation-defined. On amd64 the sentinel MinInt64 is produced, causing `float64(int64(v))` to equal ~-9.22e18, so the inequality `v != float64(int64(v))` still catches positive overflow accidentally, but this relies on platform-specific CVTTSD2SI behavior, not a spec guarantee. Add an explicit range guard (`v > math.MaxInt64 \|\| v < math.MinInt64`) before the conversion. |
| `validateRule` | L196–L199 | 🟡 NEEDS_FIX | 87% | mail.ParseAddress accepts inputs like `"John Doe <john@example.com>"` without error — a bare email validation should additionally assert `addr.Name == ""` on the returned *mail.Address. As written, a display-name address passes the "email" rule, violating the evident intent of validating plain email addresses. |

## ⚡ Quick Wins

- [ ] <!-- ACT-dfb9b1-1 --> **[correction · medium · small]** `internal/application/role_loader.go`: Add a nil guard for `step` at the top of BuildRoleSystemPrompt (before accessing `step.Agent`) and return `"", nil` or a descriptive error, consistent with the existing Agent nil guard. [L85]
- [ ] <!-- ACT-92adf7-1 --> **[correction · medium · small]** `internal/infrastructure/github/client.go`: Replace sync.Once with a mutex+bool guard that only marks detection complete on success, allowing retries with a fresh context after a context-cancellation or timeout failure. [L83]
- [ ] <!-- ACT-0c2be4-1 --> **[correction · medium · small]** `internal/interfaces/cli/validate.go`: Replace `Valid: validationErr == nil` with `Valid: report.Valid` in both the JSON/Quiet `ValidationResult` literal (L116) and the Table `ValidationResultTable` literal (L133), and change the text-path guard at L153 to `if !report.Valid` so all three output paths correctly reflect the facade's report when `report.Valid` is false but `report.Errors` is empty. [L112]
- [ ] <!-- ACT-bfd0b7-1 --> **[correction · medium · small]** `pkg/interpolation/template_resolver.go`: Replace `start := strings.Index(errStr, "\"")` with `end := strings.LastIndex(errStr, "\"")` followed by `start := strings.LastIndex(errStr[:end], "\"")` so that both quote boundaries isolate the final quoted token (the actual missing key name) rather than spanning from the template-name quote to the end of the string. [L84]

## 🧹 Hygiene

- [ ] <!-- ACT-3de8f7-1 --> **[correction · low · trivial]** `internal/domain/errors/catalog.go`: Apply sort.Slice or sort.Sort to the codes slice before returning it, so the output matches the documented guarantee of a 'sorted list'. For example: sort.Slice(codes, func(i, j int) bool { return codes[i] < codes[j] }) — adjust comparator to the underlying type of ErrorCode. [L132]
- [ ] <!-- ACT-92d6da-1 --> **[correction · low · trivial]** `internal/domain/operation/validate.go`: In validateType's float64 integer case, add explicit range bounds check before int64 conversion: `if v > math.MaxInt64 || v < math.MinInt64 { return error }` to avoid relying on implementation-defined int64 overflow behavior. [L137]
- [ ] <!-- ACT-92d6da-2 --> **[correction · low · trivial]** `internal/domain/operation/validate.go`: In validateRule's "email" case, after mail.ParseAddress succeeds, also check `addr.Name != ""` and return an error — to reject RFC 5322 named-address format ("Name <addr>") and accept only bare email addresses. [L196]
- [ ] <!-- ACT-436bb2-1 --> **[correction · low · trivial]** `internal/domain/pluginmodel/manifest.go`: In validateDefaultType, when configType == ConfigTypeInteger and the value is float64, verify the value has no fractional part (e.g., v == math.Trunc(v)) before accepting it; otherwise return a type-mismatch error for values like 1.5. [L126]
- [ ] <!-- ACT-8b91c6-1 --> **[correction · low · trivial]** `internal/interfaces/cli/ui/input_collector.go`: Replace `os.IsNotExist(statErr)` with `statErr != nil` so that any os.Stat failure (permission denied, I/O error, etc.) is surfaced as a validation error instead of silently passing. Current code returns nil for non-ErrNotExist errors, letting inaccessible paths through validation. [L265]
- [ ] <!-- ACT-76f7c6-1 --> **[correction · low · trivial]** `internal/interfaces/tui/tab_monitoring.go`: In convAppendNewTurns, move stream consumption outside the turn loop or restrict it to the last assistant turn in the new range (i == len(conv.Turns)-1). Currently t.stream.Reset() fires on the first TurnRoleAssistant found, so any subsequent assistant turns in the same batch use stale turn.Content instead of the live stream. [L608]
- [ ] <!-- ACT-ed9b25-1 --> **[correction · low · trivial]** `internal/interfaces/tui/tab_workflows.go`: In handleValidate, set t.validationResult = nil before setting t.validating = true so that the spinner is actually visible during re-validation instead of the stale previous result overlay. [L303]
