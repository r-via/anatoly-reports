[← Back to Correction](./index.md) · [← Back to report](../../public_report.md)

# 🐛 Correction — Shard 11

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)
- [🧹 Hygiene](#-hygiene)

## 📊 Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `internal/interfaces/cli/acp_serve.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacescliacpservego) |
| `internal/interfaces/cli/history.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacesclihistorygo) |
| `internal/interfaces/cli/list.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacesclilistgo) |
| `internal/interfaces/cli/status.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacesclistatusgo) |
| `pkg/plugin/sdk/grpc_plugin.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#pkgpluginsdkgrpcplugingo) |
| `internal/infrastructure/tools/builtins/read.go` | 🟢 CLEAN | 0 | 95% | [details](#internalinfrastructuretoolsbuiltinsreadgo) |

## 🔍 Symbol Details

### `internal/interfaces/cli/acp_serve.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `runProtocolInterceptor` | L562–L565 | 🟡 NEEDS_FIX | 82% | The guard `if err := scanner.Err(); err != nil` fires for ANY non-nil scanner error, including OS file-closed errors injected by the C-1 defer or signal goroutine closing os.Stdin. The comment only documents the ErrTooLong case. Fix: check `ctx.Err() == nil` before calling writeJSONRPCParseError, or narrow the guard to `errors.Is(err, bufio.ErrTooLong)`. |

### `internal/interfaces/cli/history.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `writeHistoryRunRecordsStats` | L151–L153 | 🟡 NEEDS_FIX | 85% | totalDurationMs += r.DurationMs is unconditional, so in-progress records (with DurationMs == 0 or a partial elapsed value) are included in the sum. |
| `writeHistoryRunRecordsStats` | L155–L157 | 🟡 NEEDS_FIX | 85% | avgDurationMs divides by total (all records), but total counts pending/running entries that did not contribute meaningful duration, producing a lower-than-actual average for terminal executions. |

### `internal/interfaces/cli/list.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `buildPromptInfo` | L198–L200 | 🟡 NEEDS_FIX | 85% | `len(relName) >= 2 && relName[0] == '.' && relName[1] == '.'` matches any name beginning with two dots, not just actual parent-directory escapes. A file legitimately named `..notes.md` inside basePath produces a `relName` of `..notes.md`, satisfies all three conditions, and is silently dropped. The correct guard is `relName == ".." \|\| strings.HasPrefix(relName, ".."+string(filepath.Separator))`. |

### `internal/interfaces/cli/status.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `displayRunStatus` | L175–L178 | 🟡 NEEDS_FIX | 90% | stepDuration is only set when both StartedAt and CompletedAt are non-zero. For a running step (StartedAt set, CompletedAt zero) it stays at the zero value (0s), producing a misleading '(0s)' next to a running step. The overall-duration block at L162-164 correctly handles the same case with time.Since(s.StartedAt), but that pattern is not applied here. Fix: add an else branch — if !st.StartedAt.IsZero() && st.CompletedAt.IsZero() { stepDuration = time.Since(st.StartedAt).Round(time.Millisecond) }. |

### `pkg/plugin/sdk/grpc_plugin.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `Execute` | L205–L210 | 🟡 NEEDS_FIX | 90% | Silent drop on marshal failure: `if encErr == nil { data[key] = encoded }` skips unencodable keys without setting Success=false or propagating an error. The response returns Success:true with missing data, causing silent, undetectable data loss. |

## ⚡ Quick Wins

- [ ] <!-- ACT-f74657-1 --> **[correction · medium · small]** `internal/interfaces/cli/history.go`: Accumulate totalDurationMs and a separate terminal-count variable only inside the completed/failed/cancelled cases, then divide by terminal count instead of total. This ensures avgDurationMs reflects actual finished-execution duration and is not diluted by pending/running records with zero or partial DurationMs. [L151]
- [ ] <!-- ACT-746ca8-1 --> **[correction · medium · small]** `pkg/plugin/sdk/grpc_plugin.go`: In Execute, when json.Marshal fails for a result.Data key, return an error response (Success:false, Error set) rather than silently skipping the key, so callers can detect incomplete results. [L207]

## 🔧 Refactors

- [ ] <!-- ACT-46265d-1 --> **[correction · high · large]** `internal/infrastructure/tools/builtins/read.go`: Guard against negative offset: after `offset = n`, add `if offset < 0 { offset = 0 }` before the upper-bound clamp and the slice. [L62]
- [ ] <!-- ACT-46265d-2 --> **[correction · high · large]** `internal/infrastructure/tools/builtins/read.go`: Guard against negative limit: add `if n < 0 { n = 0 }` (or skip applying the limit entirely) inside the limit block before `lines = lines[:n]`. [L66]

## 🧹 Hygiene

- [ ] <!-- ACT-c7213a-1 --> **[correction · low · trivial]** `internal/interfaces/cli/acp_serve.go`: In runProtocolInterceptor, guard the writeJSONRPCParseError call with a ctx.Err()==nil check (or narrow to bufio.ErrTooLong) so that a file-closed error injected by shutdown does not send a spurious parse error to the client. [L562]
- [ ] <!-- ACT-45363f-1 --> **[correction · low · trivial]** `internal/interfaces/cli/list.go`: Replace raw two-byte `..` check in buildPromptInfo with `relName == ".." || strings.HasPrefix(relName, ".."+string(filepath.Separator))` to avoid silently dropping valid files whose names start with two dots. [L198]
- [ ] <!-- ACT-68bf7e-1 --> **[correction · low · trivial]** `internal/interfaces/cli/status.go`: In the verbose step loop in displayRunStatus, add a time.Since(st.StartedAt) fallback for steps that have started but not yet completed, mirroring the logic used for the overall run duration on L162-164. [L175]
