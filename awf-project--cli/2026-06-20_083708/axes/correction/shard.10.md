[← Back to Correction](./index.md) · [← Back to report](../../public_report.md)

# 🐛 Correction — Shard 10

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `internal/interfaces/cli/config_cmd.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacescliconfigcmdgo) |
| `internal/interfaces/cli/plugin_cmd.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinterfacescliplugincmdgo) |
| `internal/interfaces/cli/run.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacesclirungo) |
| `internal/interfaces/cli/serve.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacescliservego) |
| `internal/interfaces/cli/ui/agent_renderer.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacescliuiagentrenderergo) |
| `internal/interfaces/cli/upgrade.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinterfacescliupgradego) |
| `internal/infrastructure/tools/builtins/write.go` | 🟡 NEEDS_REFACTOR | 1 | 92% | [details](#internalinfrastructuretoolsbuiltinswritego) |
| `internal/interfaces/tui/messages.go` | 🟡 NEEDS_REFACTOR | 1 | 85% | [details](#internalinterfacestuimessagesgo) |
| `internal/application/subworkflow_executor.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalapplicationsubworkflowexecutorgo) |
| `internal/infrastructure/agents/opencode_workspace_config.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructureagentsopencodeworkspaceconfiggo) |

## 🔍 Symbol Details

### `internal/interfaces/cli/config_cmd.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `runConfigShow` | L62–L64 | 🟡 NEEDS_FIX | 95% | In Go, `fmt.Errorf` never returns nil regardless of its arguments. When `writer.WriteError` succeeds and returns nil, `fmt.Errorf("write error: %w", nil)` still produces a non-nil error with message `"write error: <nil>"`, causing the command to fail even though the JSON error was written successfully. Fix: `if werr := writer.WriteError(err, ExitUser); werr != nil { return fmt.Errorf("write error: %w", werr) }; return nil`. |

### `internal/interfaces/cli/plugin_cmd.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `runPluginRemove` | L835 | 🟡 NEEDS_FIX | 92% | findPluginDir(pluginPaths, name) uses the raw user-supplied name (e.g. 'awf-plugin-jira'). The name-resolution block above (L816–L823) sets resolvedName to the short form ('jira') that matches the actual directory created by install; passing the full name here will return '' and the function will error with 'plugin not installed' even though the plugin exists. |
| `runPluginVerify` | L950 | 🟡 NEEDS_FIX | 95% | NewJSONPluginStateStore(cfg.StoragePath) points to a different directory than every other caller (runPluginInstall, updatePlugin, initPluginSystemReadOnly all use filepath.Join(cfg.StoragePath, "plugins")). Checksums written by install/update are stored under the plugins sub-path; verify reads from the parent, so GetChecksum always returns !exists and every plugin reports MISSING. |

### `internal/interfaces/cli/run.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `runSingleStep` | L1009–L1020 | 🟡 NEEDS_FIX | 95% | Else branch on stepRes.Status != RunStateCompleted calls formatter.Error but falls through to return nil. Should return a non-nil error (e.g. via writeErrorAndExit) so callers and the shell observe a non-zero exit code. |

### `internal/interfaces/cli/serve.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `buildHTTPFacade` | L261–L263 | 🟡 NEEDS_FIX | 65% | rec, _, err := WireTranscript(runID, storagePath) — the middle return (likely a cleanup func) is thrown away. The factory signature func(runID string) (ports.Recorder, error) structurally prevents returning it, so either ports.Recorder must self-close or the transcript is never properly finalized. Cannot confirm which without seeing WireTranscript's implementation; flagged at reduced confidence. |

### `internal/interfaces/cli/ui/agent_renderer.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `formatToolMarker` | L38 | 🟡 NEEDS_FIX | 90% | `arg[:toolArgMaxLen-3]` slices by bytes, not rune boundaries. If arg contains multi-byte UTF-8 characters (e.g., Chinese, emoji, accented letters) and byte 37 falls inside a rune, the truncated prefix is invalid UTF-8 and fmt.Fprint will emit garbled/replacement bytes. Fix: convert to []rune, slice at rune index (toolArgMaxLen-1 runes), then append the ellipsis rune, or use a rune-aware truncation helper. |

### `internal/interfaces/cli/upgrade.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `extractBinary` | L179 | 🟡 NEEDS_FIX | 90% | filepath.Join(tmpDir, "awf") ignores the Windows convention that executables carry a '.exe' suffix. The platform asset downloaded for windows/amd64 (or windows/arm64) will contain 'awf.exe'; ReadFile on path ending in 'awf' returns a not-found error and the upgrade aborts. Fix: append '.exe' when runtime.GOOS == "windows". |
| `replaceBinaryAtExecPath` | L202–L207 | 🟡 NEEDS_FIX | 80% | dir = filepath.Dir(resolvedPath) checks the directory of the actual file the symlink points to, but updater.ReplaceBinary(execPath, …) operates on the symlink path itself (execPath). In common setups (e.g., /usr/local/bin/awf → /opt/homebrew/Cellar/awf/…/bin/awf), checkWritePermission probes one directory while the replacement writes to a different one, so the permission check can silently succeed yet the actual write fails, or vice versa. Fix: check filepath.Dir(execPath) if ReplaceBinary operates on execPath, or pass resolvedPath to ReplaceBinary and keep checking filepath.Dir(resolvedPath). |

### `internal/infrastructure/tools/builtins/write.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `atomicWrite` | L81–L83 | 🟡 NEEDS_FIX | 92% | If os.WriteFile succeeds in creating the file but then fails mid-write (e.g. disk full), the partial temp file is left behind. The rename-failure path (L85-87) correctly removes the tmp file, but the WriteFile-failure path has no corresponding os.Remove(tmp) before returning the error. |

### `internal/interfaces/tui/messages.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `Error` | L57 | 🟡 NEEDS_FIX | 70% | e.Err.Error() panics when Err is nil; add a nil guard and return a sentinel string (e.g. "<nil>") in that branch. |

### `internal/application/subworkflow_executor.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `executeCallWorkflowStep` | L213–L222 | 🟡 NEEDS_FIX | 75% | `result.Outputs[parentVarName]` is populated for each entry in `config.Outputs`, but `result` is a local variable never passed to `execCtx`. `state.Output` is then set to `fmt.Sprintf("sub-workflow %s completed successfully", ...)` — a plain string — instead of the structured outputs map. Any parent step that tries to interpolate `{{ steps.<name>.output.<var> }}` will find a string rather than the expected mapped value. |

### `internal/infrastructure/agents/opencode_workspace_config.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `acquireWorkspaceLock` | L49–L51 | 🟡 NEEDS_FIX | 70% | On Linux, close(2) in one thread does NOT interrupt flock(2) blocked in another thread on the same fd. After lf.Close() the OS fd number is released; if Go's runtime reuses it (via a subsequent os.OpenFile) before the goroutine's flock() returns, the goroutine acquires an advisory lock on whichever file now owns that fd number. That phantom lock persists until the new file's handle is closed, potentially blocking unrelated callers. The goroutine itself is not permanently leaked (buffered channel of capacity 1 allows eventual send), but the fd-reuse race is a real correctness defect. |
| `acquireWorkspaceLock` | L37–L39 | 🟡 NEEDS_FIX | 70% | lf.Fd() is evaluated inside the goroutine. If goroutine scheduling is delayed past workspaceLockTimeout, lf.Close() in the timeout path races with lf.Fd() here — a data race on the *os.File internals. Extremely unlikely in practice but technically unsynchronised. |

## ⚡ Quick Wins

- [ ] <!-- ACT-7b98dc-1 --> **[correction · medium · small]** `internal/application/subworkflow_executor.go`: Replace `state.Output = fmt.Sprintf("sub-workflow %s completed successfully", config.Workflow)` with `state.Output = result.Outputs` (or the appropriate structured type that `StepState.Output` accepts) so that mapped sub-workflow outputs are stored in `execCtx` and available for parent-step interpolation. [L221]
- [ ] <!-- ACT-504570-1 --> **[correction · medium · small]** `internal/infrastructure/agents/opencode_workspace_config.go`: Replace the goroutine+blocking-flock+close timeout pattern in acquireWorkspaceLock with a LOCK_NB poll loop (syscall.LOCK_EX|syscall.LOCK_NB, retry via time.NewTicker) to eliminate the fd-reuse race: the closed fd number is returned to the OS immediately and can be reused by another os.OpenFile before the goroutine's blocked flock(2) returns, causing it to acquire a lock on the wrong open file description. [L37]
- [ ] <!-- ACT-f2eaee-1 --> **[correction · medium · small]** `internal/infrastructure/tools/builtins/write.go`: After os.WriteFile returns an error, remove the partial temp file before returning: add `_ = os.Remove(tmp)` immediately before `return err` on line 83. [L83]
- [ ] <!-- ACT-f3499a-1 --> **[correction · medium · small]** `internal/interfaces/cli/config_cmd.go`: Replace `return fmt.Errorf("write error: %w", writer.WriteError(err, ExitUser))` with a conditional: assign the result of WriteError, return the wrapped error only when it is non-nil, otherwise return nil. This prevents a spurious non-nil error when JSON error output succeeds. [L63]
- [ ] <!-- ACT-93d3ac-1 --> **[correction · medium · small]** `internal/interfaces/cli/plugin_cmd.go`: In runPluginRemove, change findPluginDir(pluginPaths, name) to findPluginDir(pluginPaths, resolvedName) so the directory lookup uses the same short name that was resolved from the plugin registry and used for all other operations. [L835]
- [ ] <!-- ACT-93d3ac-2 --> **[correction · medium · small]** `internal/interfaces/cli/plugin_cmd.go`: In runPluginVerify, change NewJSONPluginStateStore(cfg.StoragePath) to NewJSONPluginStateStore(filepath.Join(cfg.StoragePath, "plugins")) to match the path used by install, update, and initPluginSystemReadOnly; without this all checksums appear MISSING. [L950]
- [ ] <!-- ACT-273972-2 --> **[correction · medium · small]** `internal/interfaces/cli/run.go`: Guard defer mirrorCancel() with a nil check: assign to a local cancel func that is always non-nil (e.g. a no-op closure) before deferring, or check 'if mirrorCancel != nil' inside the deferred call to prevent a panic when AttachMirrorSubscriber returns nil for an empty debugTranscriptMirror path. [L309]
- [ ] <!-- ACT-c74555-1 --> **[correction · medium · small]** `internal/interfaces/cli/serve.go`: Confirm whether WireTranscript's second return value is a cleanup function. If so, either (a) make the Recorder self-closing (implement io.Closer on the concrete type and call it after the run completes) so the factory signature stays compatible, or (b) extend SetRunRecorderFactory to also accept a cleanup callback. Without one of these changes, per-run transcript files risk being incomplete or leaking file descriptors. [L262]
- [ ] <!-- ACT-0c7b16-1 --> **[correction · medium · small]** `internal/interfaces/cli/ui/agent_renderer.go`: Replace byte-level truncation `arg[:toolArgMaxLen-3]` with a rune-aware equivalent (e.g., convert to []rune, take the first toolArgMaxLen-1 runes, append '…') to avoid emitting invalid UTF-8 when arg contains multi-byte characters. [L38]
- [ ] <!-- ACT-68ff4e-1 --> **[correction · medium · small]** `internal/interfaces/cli/upgrade.go`: Append '.exe' to the binary name on Windows: set binaryName := "awf"; if runtime.GOOS == "windows" { binaryName += ".exe" }; then use filepath.Join(tmpDir, binaryName). [L179]
- [ ] <!-- ACT-68ff4e-2 --> **[correction · medium · small]** `internal/interfaces/cli/upgrade.go`: Align the permission check with the actual write target: either call checkWritePermission(filepath.Dir(execPath)) so the probed directory matches where ReplaceBinary writes, or pass resolvedPath to updater.ReplaceBinary and keep probing filepath.Dir(resolvedPath). [L202]
- [ ] <!-- ACT-b3d574-1 --> **[correction · medium · small]** `internal/interfaces/tui/messages.go`: Guard against nil Err in ErrMsg.Error(): check `if e.Err == nil { return "<nil>" }` before calling e.Err.Error() to prevent a nil-pointer panic. [L57]

## 🔧 Refactors

- [ ] <!-- ACT-273972-1 --> **[correction · high · large]** `internal/interfaces/cli/run.go`: Return an error when stepRes.Status != RunStateCompleted instead of returning nil. Use writeErrorAndExit with an appropriate error and ExitExecution code so the shell and any caller observes a non-zero exit code on step failure. [L1020]
