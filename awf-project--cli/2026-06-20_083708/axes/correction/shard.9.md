[← Back to Correction](./index.md) · [← Back to report](../../public_report.md)

# 🐛 Correction — Shard 9

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)
- [🧹 Hygiene](#-hygiene)

## 📊 Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `internal/infrastructure/notify/desktop.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructurenotifydesktopgo) |
| `internal/infrastructure/repository/yaml_types.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructurerepositoryyamltypesgo) |
| `internal/interfaces/cli/pack_resolver.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacesclipackresolvergo) |
| `internal/interfaces/tui/tab_logs.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinterfacestuitablogsgo) |
| `pkg/interpolation/escaping.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#pkginterpolationescapinggo) |
| `internal/interfaces/cli/resume.go` | 🟡 NEEDS_REFACTOR | 2 | 92% | [details](#internalinterfacescliresumego) |
| `pkg/output/format.go` | 🟡 NEEDS_REFACTOR | 0 | 90% | [details](#pkgoutputformatgo) |
| `internal/application/external_file.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalapplicationexternalfilego) |
| `internal/application/single_step.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalapplicationsinglestepgo) |
| `internal/infrastructure/otel/factory.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructureotelfactorygo) |

## 🔍 Symbol Details

### `internal/infrastructure/notify/desktop.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `escapeAppleScript` | L202 | 🟡 NEEDS_FIX | 70% | Replacing `"` with `\"` does not produce a valid AppleScript escaped quote. AppleScript has no `\"` escape; a `"` in the input causes the parser to close the string literal early, producing a syntax error. The correct approach is to split on `"` and rejoin with `" & quote & "` concatenation. |
| `escapeAppleScript` | L200 | 🟡 NEEDS_FIX | 70% | Replacing `\` with `\\` is also wrong: AppleScript treats `\` as two literal backslash characters, not one, so a single backslash in the input becomes two backslashes in the notification text. |

### `internal/infrastructure/repository/yaml_types.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `yamlStep` | L58 | 🟡 NEEDS_FIX | 75% | ExitCode int has no yaml struct tag. yaml.v3 maps it to the lowercase key `exitcode`. A YAML step block containing `exitcode: N` will silently overwrite a value that should only be synthesized at runtime (FR-004), potentially corrupting exit-code handling for inline error terminals. |

### `internal/interfaces/cli/pack_resolver.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `resolvePackDir` | L95 | 🟡 NEEDS_FIX | 92% | os.Stat error on local path (e.g. EACCES, EIO) is silently ignored; execution falls through to try the global path, potentially loading a different pack or misreporting the root cause. |
| `resolvePackDir` | L106 | 🟡 NEEDS_FIX | 92% | os.Stat error on global path (e.g. EACCES, EIO) is silently ignored; the function then returns ErrorCodeUserInputMissingFile instead of propagating the real OS error. |

### `internal/interfaces/tui/tab_logs.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `Update` | L119–L122 | 🟡 NEEDS_FIX | 72% | After t.rotationNotice is set, resizeViewport() is not called. View() then renders the notice as an extra line before the viewport, so total height = 4 (parent chrome) + 2 (path header) + 1 (notice) + viewport(height-6) = height+1, overflowing the terminal by 1 line until the next WindowSizeMsg resizes the window. |
| `resizeViewport` | L228–L230 | 🟡 NEEDS_FIX | 65% | headerLines += 2 for rotationNotice, but View() emits exactly one rendered line (logsEmptyStyle.Render(t.rotationNotice) + "\n"). If StyleEmptyState carries no bottom padding, this makes the viewport 1 line shorter than optimal, leaving a blank line at the bottom of the terminal whenever the rotation notice is visible. |

### `pkg/interpolation/escaping.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `needsEscaping` | L21–L22 | 🟡 NEEDS_FIX | 72% | `~` is absent from the switch-case list. A value like `~/foo` passes through unquoted, and the shell expands `~` to `$HOME` at the call site. |

### `internal/interfaces/cli/resume.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `runResumeListViaFacade` | L108 | 🟡 NEEDS_FIX | 91% | rec.CompletedAt is the zero value for all non-completed runs (they have not completed, so CompletedAt has never been written). Format(time.RFC3339) on a zero time.Time emits "0001-01-01T00:00:00Z". Should use StartedAt or an UpdatedAt field that tracks the last modification time of an in-progress run. |
| `buildResumeFacade` | L235–L237 | 🟡 NEEDS_FIX | 80% | uuid.New().String() generates a random runID that is used as the transcript session key via WireTranscript. buildResumeFacade receives no workflowID parameter, so the transcript written during resume is stored under an unrelated UUID. The run command counterpart uses the actual run's ID; resumed-run transcripts will be permanently detached from the run they describe. The function signature must accept workflowID and pass it to WireTranscript. |

### `internal/application/external_file.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `replaceAWFPathsInString` | L221–L224 | 🟡 NEEDS_FIX | 82% | suffix == "" means the character right after globalPrefix is a shell delimiter (space, quote, pipe, etc.), not end-of-string. Writing str[offset:] and breaking skips any later globalPrefix occurrences. The correct fix is to write str[offset:absIdx+len(globalPrefix)] (keeping this unresolvable occurrence verbatim) and set offset = absIdx+len(globalPrefix), then continue — not break. The end-of-string case (remainingStr == "") is equally handled by this approach: the next iteration finds no match, writes the (empty) remainder, and breaks normally. |

## ⚡ Quick Wins

- [ ] <!-- ACT-57ae4f-1 --> **[correction · medium · small]** `internal/application/external_file.go`: In replaceAWFPathsInString, replace the suffix=="" early-break with: write str[offset:absIdx+len(globalPrefix)] (keep unresolvable occurrence verbatim), set offset = absIdx+len(globalPrefix), and continue the loop. This ensures subsequent occurrences of globalPrefix are still scanned and resolved when a delimiter immediately follows an unresolvable one. [L221]
- [ ] <!-- ACT-141f2c-2 --> **[correction · medium · small]** `internal/application/single_step.go`: Change the post-hooks call at L134 from ExecuteHooks(stepCtx, ...) to ExecuteHooks(ctx, ...) so that a step timeout does not also cancel post-hook execution. Pre-hooks intentionally run within the timeout budget; post-hooks must always be able to run regardless of whether the timeout fired. [L134]
- [ ] <!-- ACT-9d9e68-1 --> **[correction · medium · small]** `internal/infrastructure/notify/desktop.go`: Rewrite escapeAppleScript to split the input on double quotes and reassemble using AppleScript quote concatenation (e.g. `part1" & quote & "part2`), and drop the backslash-doubling since AppleScript treats backslash as a plain character. [L198]
- [ ] <!-- ACT-3418e4-1 --> **[correction · medium · small]** `internal/infrastructure/otel/factory.go`: Validate or document accepted endpoint formats. Reject (or strip) unrecognised URL schemes instead of silently forwarding them to the gRPC dialer, which will fail at dial time with an opaque error. [L28]
- [ ] <!-- ACT-37a14b-1 --> **[correction · medium · small]** `internal/interfaces/cli/pack_resolver.go`: In resolvePackDir, replace bare `err == nil` guards with explicit os.IsNotExist checks: propagate non-NotExist errors (e.g. permission denied, I/O error) rather than falling through to the next search path. [L95]
- [ ] <!-- ACT-37a14b-2 --> **[correction · medium · small]** `internal/interfaces/cli/pack_resolver.go`: Apply the same os.IsNotExist guard to the global pack path os.Stat call to prevent misclassifying I/O errors as 'pack not found'. [L106]
- [ ] <!-- ACT-234ba5-1 --> **[correction · medium · small]** `internal/interfaces/cli/resume.go`: Replace rec.CompletedAt with the appropriate last-activity field (e.g. StartedAt or an UpdatedAt from the history record) when building ResumableInfo for non-completed runs; CompletedAt is always zero for runs that have not finished. [L108]
- [ ] <!-- ACT-234ba5-2 --> **[correction · medium · small]** `internal/interfaces/cli/resume.go`: Add a workflowID string parameter to buildResumeFacade and pass it (instead of uuid.New().String()) to WireTranscript so the resume transcript is stored under the same ID as the run it resumes. [L235]
- [ ] <!-- ACT-14e37d-1 --> **[correction · medium · small]** `internal/interfaces/tui/tab_logs.go`: In the logRotationMsg case (line 122), call t.resizeViewport() before returning so the viewport height is reduced to account for the new rotation notice line, preventing 1-line layout overflow. [L122]
- [ ] <!-- ACT-649203-1 --> **[correction · medium · small]** `pkg/interpolation/escaping.go`: Add `'~'` to the character switch in `needsEscaping` so strings beginning with `~` are detected and subsequently wrapped in single quotes by `ShellEscape`, preventing unintended tilde expansion in shell commands. [L22]

## 🔧 Refactors

- [ ] <!-- ACT-141f2c-1 --> **[correction · high · large]** `internal/application/single_step.go`: After the workflow load+fallback chain (around L56), add an explicit nil-guard: if wf == nil { return nil, fmt.Errorf("load workflow: returned nil without error") }. This prevents a nil-dereference panic at wf.Steps[stepName]. [L57]

## 🧹 Hygiene

- [ ] <!-- ACT-3c4111-1 --> **[correction · low · trivial]** `internal/infrastructure/repository/yaml_types.go`: Add `yaml:"-"` tag to yamlStep.ExitCode so yaml.v3 never populates it from user-supplied YAML; its value must only be set programmatically during inline-error-terminal synthesis (FR-004). [L58]
- [ ] <!-- ACT-14e37d-2 --> **[correction · low · trivial]** `internal/interfaces/tui/tab_logs.go`: In resizeViewport(), change headerLines += 2 to headerLines += 1 for the rotationNotice branch (line 229) to match the single rendered line emitted by View() — verify StyleEmptyState has no bottom padding before applying. [L229]
- [ ] <!-- ACT-8eafe3-1 --> **[correction · low · trivial]** `pkg/output/format.go`: Preserve the original `err` from `ValidateAndParseJSON(processed)` when the fallback parse also fails. Introduce a separate variable (e.g. `fallbackParsed, fallbackErr`) for the embedded-fence attempt and only replace `parsed`/`err` when `fallbackErr == nil`. [L71]
