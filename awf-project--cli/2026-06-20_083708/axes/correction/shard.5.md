[← Back to Correction](./index.md) · [← Back to report](../../public_report.md)

# 🐛 Correction — Shard 5

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🧹 Hygiene](#-hygiene)

## 📊 Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `internal/infrastructure/github/batch.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalinfrastructuregithubbatchgo) |
| `internal/infrastructure/logger/console_logger.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructureloggerconsoleloggergo) |
| `internal/infrastructure/repository/errors.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructurerepositoryerrorsgo) |
| `pkg/retry/backoff.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#pkgretrybackoffgo) |
| `internal/application/output_limiter.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalapplicationoutputlimitergo) |
| `internal/domain/pluginmodel/operation.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internaldomainpluginmodeloperationgo) |
| `internal/infrastructure/diagram/dot_generator.go` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#internalinfrastructurediagramdotgeneratorgo) |
| `internal/infrastructure/http/provider.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructurehttpprovidergo) |
| `internal/infrastructure/workflowpkg/types.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinfrastructureworkflowpkgtypesgo) |
| `internal/testutil/fixtures/cli_fixtures.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internaltestutilfixturesclifixturesgo) |

## 🔍 Symbol Details

### `internal/infrastructure/github/batch.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `executeAllSucceed` | L170–L172 | 🟡 NEEDS_FIX | 93% | The `case <-gCtx.Done(): return gCtx.Err()` path returns to errgroup with no mutex lock, no result.Results append, and no result.Failed increment. executeAnySucceed and executeBestEffort both construct and append a failed OperationResult for the same cancelled-semaphore-wait scenario. The inconsistency leaves result.Total inconsistent with the aggregate counts and len(result.Results), which callers relying on those invariants will misinterpret. |
| `executeAnySucceed` | L226–L238 | 🟡 NEEDS_FIX | 87% | When cancel() fires after firstSuccess is set, goroutines still waiting on the semaphore hit `case <-ctx.Done()` and execute `result.Failed++`. Goroutines already inside executeOperation also return a context-cancellation error and likewise hit `result.Failed++`. Neither group actually failed; they were preempted by a successful sibling. A 3-operation batch where all could succeed but only the first completes will report Succeeded=1 and Failed=2 instead of Succeeded=1 and Cancelled=2, giving callers an inaccurate picture of how many operations genuinely errored. |

### `internal/infrastructure/diagram/dot_generator.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `generateNodes` | L102–L115 | 🟡 NEEDS_FIX | 92% | When the loop reaches a parallel step it calls `generateParallelSubgraph`, which places each branch step inside a `subgraph cluster_*` block, then `continue`s. Those same branch step names are also keys in `wf.Steps` and are processed again in a later iteration as regular top-level nodes. In Graphviz, a node defined both inside a cluster and outside loses its cluster membership; the visual grouping becomes empty. Fix: before the loop, collect all branch step names from parallel steps into a set and skip them during the main iteration so they are only defined inside the cluster. |
| `generateParallelSubgraph` | L292 | 🟡 NEEDS_FIX | 93% | `fmt.Fprintf(&sb, "    subgraph cluster_%s {\n", escapeDOTID(step.Name))` — for step names with hyphens, spaces, or other special characters (e.g., `deploy-service`), `escapeDOTID` returns `"deploy-service"` and the generated line is `subgraph cluster_"deploy-service" {`. A DOT subgraph name must be a single valid ID; concatenating `cluster_` with a quoted string is not. Fix: quote the entire composed name, e.g., `fmt.Fprintf(&sb, "    subgraph %q {\n", "cluster_"+step.Name)`, or sanitize non-identifier characters to underscores before prefixing. |
| `escapeDOTID` | L394–L396 | 🟡 NEEDS_FIX | 90% | `strings.ReplaceAll(s, "\\", "\\\\")` doubles each `\`; `strings.ReplaceAll(escaped, "\"", "\\\"")` prefixes each `"` with `\`; then `fmt.Sprintf("%q", escaped)` escapes those characters a second time. Example: input `a\b` (one backslash) → after pre-escape `a\\b` (two backslashes) → after `%q` `"a\\\\b"` (four backslashes in output) → DOT renders `a\b` (two backslashes) instead of `a\b` (one). `%q` alone correctly produces valid DOT string literals; remove L394–L395 and change L396 to `return fmt.Sprintf("%q", s)`. |

### `internal/infrastructure/http/provider.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `extractTimeout` | L222–L223 | 🟡 NEEDS_FIX | 75% | time.Duration(v) converts float64 to int64 (nanoseconds) via truncation before multiplying by time.Second. For v=1.5: time.Duration(1.5)=1ns, then 1ns*time.Second=1s instead of 1.5s. The correct expression is time.Duration(v * float64(time.Second)). |

### `internal/infrastructure/workflowpkg/types.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `parseTimeValue` | L103 | 🟡 NEEDS_FIX | 95% | time.Parse(time.RFC3339, v) fails on any string with fractional seconds (e.g. "2023-01-01T12:00:00.123456789Z") because RFC3339 layout has no fractional-second component. The error is discarded and time.Time{} (zero) is returned. Fix: use time.RFC3339Nano, whose .999999999 component is optional in Go's parser, so it correctly handles both whole-second and sub-second RFC3339 strings. |

## ⚡ Quick Wins

- [ ] <!-- ACT-0be5b8-1 --> **[correction · medium · small]** `internal/domain/pluginmodel/operation.go`: In validateDefaultType, after matching float64 for InputTypeInteger, add a fractional-part check: cast to float64 and return an error if math.Trunc(v) != v. This prevents values like 3.14 from being accepted as valid integer defaults. [L141]
- [ ] <!-- ACT-f8ba04-1 --> **[correction · medium · small]** `internal/infrastructure/diagram/dot_generator.go`: Before the main step loop in generateNodes, collect all branch step names referenced by parallel steps into a set, then skip those names during iteration so each branch node is declared only inside its cluster subgraph. [L102]
- [ ] <!-- ACT-f8ba04-2 --> **[correction · medium · small]** `internal/infrastructure/diagram/dot_generator.go`: Fix the invalid DOT subgraph name in generateParallelSubgraph by quoting the entire composed identifier, e.g., `fmt.Fprintf(&sb, "    subgraph %q {\n", "cluster_"+step.Name)`, instead of concatenating `cluster_` with the output of `escapeDOTID` which may return a quoted string. [L292]
- [ ] <!-- ACT-cc92cb-1 --> **[correction · medium · small]** `internal/infrastructure/github/batch.go`: In executeAllSucceed, the `case <-gCtx.Done()` branch must acquire mu, append a failed OperationResult to result.Results, and increment result.Failed before returning, matching the accounting done in executeAnySucceed and executeBestEffort for cancelled goroutines. [L170]
- [ ] <!-- ACT-cc92cb-2 --> **[correction · medium · small]** `internal/infrastructure/github/batch.go`: In executeAnySucceed, track whether ctx.Done() fired because of firstSuccess (vs. parent cancellation) and skip incrementing result.Failed for operations cancelled solely because a sibling succeeded — or introduce a separate Cancelled counter — so callers can accurately distinguish failed-from-error versus cancelled-due-to-strategy. [L226]
- [ ] <!-- ACT-d5c797-1 --> **[correction · medium · small]** `internal/infrastructure/logger/console_logger.go`: In WithContext, replace `append(l.ctxFields, ctxFields...)` with an explicit copy into a pre-allocated slice (`make([]any, 0, len(l.ctxFields)+len(ctxFields))` + two appends) to prevent multiple children from sharing and corrupting the parent's backing array. [L69]
- [ ] <!-- ACT-82ad65-1 --> **[correction · medium · small]** `internal/infrastructure/workflowpkg/types.go`: Replace time.RFC3339 with time.RFC3339Nano in the time.Parse call inside parseTimeValue (L103). RFC3339Nano's fractional-second component is optional in Go's time.Parse, so it parses both "…T12:00:00Z" and "…T12:00:00.123456789Z" correctly, preventing silent zero-time returns for any timestamp with sub-second precision. [L103]
- [ ] <!-- ACT-4e977b-1 --> **[correction · medium · small]** `pkg/retry/backoff.go`: Add a nil guard for rng before calling rng.Float64(): if rng == nil { return delay } or document that nil is explicitly forbidden and enforce it at the call site. [L65]

## 🧹 Hygiene

- [ ] <!-- ACT-460231-1 --> **[correction · low · trivial]** `internal/application/output_limiter.go`: When maxContentLen <= 0, return marker[:min(int(l.config.MaxSize), markerLen)] instead of the full marker, so the result never exceeds MaxSize. [L114]
- [ ] <!-- ACT-f8ba04-3 --> **[correction · low · trivial]** `internal/infrastructure/diagram/dot_generator.go`: Remove the manual pre-escaping lines (L394–L395) in escapeDOTID and use `fmt.Sprintf("%q", s)` directly; the `%q` verb already produces correctly escaped DOT string literals and the prior manual escaping causes double-encoding for inputs containing backslashes or double quotes. [L394]
- [ ] <!-- ACT-6ab135-1 --> **[correction · low · trivial]** `internal/infrastructure/http/provider.go`: Replace time.Duration(v) * time.Second with time.Duration(v * float64(time.Second)) so fractional-second timeout inputs (e.g. 1.5) are preserved rather than truncated to the nearest second. [L222]
- [ ] <!-- ACT-a92744-1 --> **[correction · low · trivial]** `internal/infrastructure/repository/errors.go`: Change `e.Line > 0` to `e.Line >= 0` in Error() to align with the documented sentinel (-1 = unknown) and with ToStructuredError's guard, so a ParseError with line=0 is formatted with line/column instead of being silently dropped. [L24]
- [ ] <!-- ACT-2dd90b-1 --> **[correction · low · trivial]** `internal/testutil/fixtures/cli_fixtures.go`: In CreateTestWorkflow, replace both '/' and filepath.Separator when sanitising the name: first replace '/', then replace string(filepath.Separator) (or combine into a single strings.NewReplacer call). This prevents forward-slash-separated paths supplied by test callers from creating nested directories on Windows. [L153]
