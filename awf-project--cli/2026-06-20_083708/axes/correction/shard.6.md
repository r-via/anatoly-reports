[← Back to Correction](./index.md) · [← Back to report](../../public_report.md)

# 🐛 Correction — Shard 6

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🧹 Hygiene](#-hygiene)

## 📊 Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `pkg/plugin/sdk/host_client.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#pkgpluginsdkhostclientgo) |
| `internal/application/parallel_executor.go` | 🟡 NEEDS_REFACTOR | 2 | 95% | [details](#internalapplicationparallelexecutorgo) |
| `internal/application/tools/proxy_service.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalapplicationtoolsproxyservicego) |
| `internal/domain/workflow/mcp_proxy.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internaldomainworkflowmcpproxygo) |
| `internal/infrastructure/agents/base_cli_provider.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructureagentsbasecliprovidergo) |
| `internal/infrastructure/logger/masker.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructureloggermaskergo) |
| `internal/infrastructure/tools/plugin_adapter.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinfrastructuretoolspluginadaptergo) |
| `internal/interfaces/cli/plugins.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internalinterfacesclipluginsgo) |
| `internal/testutil/fixtures/fixtures.go` | 🟡 NEEDS_REFACTOR | 0 | 95% | [details](#internaltestutilfixturesfixturesgo) |
| `pkg/expression/evaluator.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#pkgexpressionevaluatorgo) |

## 🔍 Symbol Details

### `internal/application/parallel_executor.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `executeAllSucceed` | L107–L115 | 🟡 NEEDS_FIX | 88% | if err != nil block creates a new BranchResult{Name, Error, timestamps} but never checks whether branchResult != nil. RunBranchWithSemaphore in the same file shows the correct pattern: if branchResult != nil { result.AddResult(branchResult) } rather than constructing a synthetic record. The discarded branchResult may carry non-zero ExitCode or partial output needed for diagnostics and downstream logic. |
| `executeBestEffort` | L329–L337 | 🟡 NEEDS_FIX | 87% | if err != nil branch constructs a new BranchResult without checking branchResult != nil first. Consistent with RunBranchWithSemaphore's pattern, the fix is: if branchResult != nil { result.AddResult(branchResult) } else { result.AddResult(&workflow.BranchResult{…}) }. The synthetic record silently drops exit code and any partial output the step executor populated. |

### `pkg/expression/evaluator.go`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `preprocessExpression` | L77–L78 | 🟡 NEEDS_FIX | 78% | `!contains(a, b)` → `!a contains b`; unary `!` has higher precedence than binary `contains` in expr-lang, yielding `(!a) contains b` instead of `!(a contains b)`. |

## ⚡ Quick Wins

- [ ] <!-- ACT-206272-1 --> **[correction · medium · small]** `internal/application/parallel_executor.go`: In executeAllSucceed, when ExecuteStep returns a non-nil branchResult alongside an error, add the real branchResult instead of a synthetic one: check `if branchResult != nil { result.AddResult(branchResult) } else { result.AddResult(&workflow.BranchResult{…}) }`. [L107]
- [ ] <!-- ACT-206272-2 --> **[correction · medium · small]** `internal/application/parallel_executor.go`: In executeBestEffort, apply the same fix: check `if branchResult != nil { result.AddResult(branchResult) } else { result.AddResult(&workflow.BranchResult{…}) }` rather than unconditionally constructing a synthetic record. [L329]
- [ ] <!-- ACT-2a2ca9-1 --> **[correction · medium · small]** `internal/application/tools/proxy_service.go`: After r.Register fails, iterate over the remaining (unregistered) providers and call their Close/cleanup method before returning noopCleanup, ensuring every provider returned by providerFactory is eventually closed regardless of partial-registration failure. [L143]
- [ ] <!-- ACT-c93571-1 --> **[correction · medium · small]** `internal/domain/workflow/mcp_proxy.go`: Add a nil receiver guard at the top of Validate: `if m == nil { return nil }` [L34]
- [ ] <!-- ACT-f43b67-1 --> **[correction · medium · small]** `internal/infrastructure/agents/base_cli_provider.go`: In execute(): move `defer func() { mcpCleanup() }()` to before the `if b.hooks.mcpInjector != nil` block, and add `mcpCleanup = cleanup` before the `if injErr != nil` early return so that the deferred cleanup fires even when the injector returns an error. [L224]
- [ ] <!-- ACT-f43b67-2 --> **[correction · medium · small]** `internal/infrastructure/agents/base_cli_provider.go`: In executeConversation(): apply the identical fix — register the defer before the mcpInjector block and assign mcpCleanup = cleanup before the injErr early return. [L333]
- [ ] <!-- ACT-3b7780-1 --> **[correction · medium · small]** `internal/infrastructure/logger/masker.go`: In NewSecretMasker, uppercase every pattern before storing it (patterns = append(patterns, strings.ToUpper(p)) for each additional pattern, and likewise for DefaultSecretPatterns if it may ever hold mixed-case entries). Alternatively, uppercase the pattern inside the HasPrefix call in IsSecretKey: strings.HasPrefix(upperKey, strings.ToUpper(pattern)). [L31]
- [ ] <!-- ACT-8bcf0f-1 --> **[correction · medium · small]** `internal/infrastructure/tools/plugin_adapter.go`: When json.Marshal(result.Outputs) fails in CallTool, propagate the error (return nil, marshalErr) or set IsError=true with a diagnostic content entry rather than silently dropping the outputs and returning a success-shaped ToolResult with empty content. [L100]
- [ ] <!-- ACT-c3f659-1 --> **[correction · medium · small]** `internal/testutil/fixtures/fixtures.go`: In ParallelWorkflow, remove `OnSuccess` from branch steps or point it back to the parallel step, so that only the parallel step's own `OnSuccess` drives the post-parallel transition after the strategy condition is satisfied. Leaving branch steps with `OnSuccess: "done"` risks triggering the terminal step once per branch independently. [L194]
- [ ] <!-- ACT-0fdca5-1 --> **[correction · medium · small]** `pkg/expression/evaluator.go`: Wrap the replacement in parentheses: change the replacement string from `$1 contains $2` to `($1 contains $2)` so that `!contains(a, b)` produces `!(a contains b)` rather than `(!a) contains b`. [L78]
- [ ] <!-- ACT-fcbea1-1 --> **[correction · medium · small]** `pkg/plugin/sdk/host_client.go`: Prepend `if h == nil { return errors.New("host client not initialized") }` as the first statement in Emit, before the h.client access. [L53]

## 🧹 Hygiene

- [ ] <!-- ACT-c93571-2 --> **[correction · low · trivial]** `internal/domain/workflow/mcp_proxy.go`: Rework the duplicate-plugin check to emit exactly one error per duplicated plugin name, e.g. by recording which names have already been flagged in a separate `reported` set, or by collecting all duplicates and reporting once after the loop. [L55]
- [ ] <!-- ACT-16be32-1 --> **[correction · low · trivial]** `internal/interfaces/cli/plugins.go`: Pass effectiveCLIVersion() to registerBuiltins instead of the raw Version constant so that built-ins are registered with the same version the system was initialized with. Either capture the result (v := effectiveCLIVersion()) and reuse it, or change the call on line 40 to registerBuiltins(sysResult.Service, effectiveCLIVersion()). [L40]
