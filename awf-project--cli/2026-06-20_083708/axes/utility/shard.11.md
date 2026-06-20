[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 11

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/infrastructure/agents/copilot_provider.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinfrastructureagentscopilotprovidergo) |
| `internal/infrastructure/agents/gemini_provider.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinfrastructureagentsgeminiprovidergo) |
| `internal/infrastructure/agents/opencode_provider.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinfrastructureagentsopencodeprovidergo) |
| `internal/infrastructure/agents/registry.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinfrastructureagentsregistrygo) |
| `internal/infrastructure/agents/stream_filter.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinfrastructureagentsstreamfiltergo) |
| `internal/infrastructure/github/batch.go` | 🟡 NEEDS_REFACTOR | 5 | 95% | [details](#internalinfrastructuregithubbatchgo) |
| `internal/infrastructure/logger/console_logger.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinfrastructureloggerconsoleloggergo) |
| `internal/infrastructure/notify/provider.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinfrastructurenotifyprovidergo) |
| `internal/infrastructure/repository/errors.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinfrastructurerepositoryerrorsgo) |
| `internal/infrastructure/store/sqlite_history_store.go` | 🟡 NEEDS_REFACTOR | 7 | 95% | [details](#internalinfrastructurestoresqlitehistorystorego) |

## 🔍 Symbol Details

### `internal/infrastructure/agents/copilot_provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `CopilotProvider` | L22–L27 | 🔴 DEAD | 95% | 0 cross-package importers per exhaustive import analysis. No external file instantiates or references this type. |
| `NewCopilotProvider` | L29–L36 | 🔴 DEAD | 95% | 0 cross-package importers. Constructor is never called from outside the package. |
| `NewCopilotProviderWithOptions` | L38–L48 | 🔴 DEAD | 95% | 0 cross-package importers. Options-based constructor is never called from outside the package. |
| `Execute` | L67–L80 | 🔴 DEAD | 95% | 0 cross-package importers. Interface method unreachable externally because no external code holds a CopilotProvider instance. |
| `ExecuteConversation` | L82–L88 | 🔴 DEAD | 95% | 0 cross-package importers. Same reasoning as Execute — no external caller can reach this method. |
| `Name` | L90–L92 | 🔴 DEAD | 95% | 0 cross-package importers. Returns hardcoded string 'github_copilot'; unreachable without an external holder of CopilotProvider. |
| `Validate` | L94–L100 | 🔴 DEAD | 95% | 0 cross-package importers. LookPath check is never triggered externally. |

### `internal/infrastructure/agents/gemini_provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `GeminiProvider` | L22–L29 | 🔴 DEAD | 95% | 0 external importers. No other package constructs or references this type. |
| `NewGeminiProvider` | L31–L38 | 🔴 DEAD | 95% | 0 importers. Constructor never called outside this file. |
| `NewGeminiProviderWithOptions` | L40–L50 | 🔴 DEAD | 95% | 0 importers. Options-based constructor never called outside this file. |
| `Execute` | L86–L111 | 🔴 DEAD | 85% | 0 external importers. Likely satisfies an interface but no external caller or type assertion found in provided context. |
| `ExecuteConversation` | L113–L119 | 🔴 DEAD | 85% | 0 external importers. Interface implementation with no observed caller outside this file. |
| `Name` | L121–L123 | 🔴 DEAD | 85% | 0 external importers. Interface method with no observed caller outside this file. |
| `Validate` | L125–L131 | 🔴 DEAD | 85% | 0 external importers. Interface method with no observed caller outside this file. |

### `internal/infrastructure/agents/opencode_provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `OpenCodeProvider` | L21–L26 | 🔴 DEAD | 95% | Exported struct with 0 cross-package importers. No external code instantiates or references this type. |
| `NewOpenCodeProvider` | L28–L35 | 🔴 DEAD | 95% | Exported constructor with 0 importers. No caller creates an OpenCodeProvider via this function. |
| `NewOpenCodeProviderWithOptions` | L37–L47 | 🔴 DEAD | 95% | Exported constructor with 0 importers. Options-based variant of NewOpenCodeProvider, also unreachable externally. |
| `Execute` | L66–L91 | 🔴 DEAD | 92% | Exported method with 0 importers. Interface method never invoked by any external caller. |
| `ExecuteConversation` | L94–L100 | 🔴 DEAD | 95% | Exported method with 0 importers. Multi-turn variant of Execute, equally unreachable. |
| `Name` | L179–L181 | 🔴 DEAD | 95% | Exported method with 0 importers. Returns hardcoded string "opencode" but is never called externally. |
| `Validate` | L184–L190 | 🔴 DEAD | 95% | Exported method with 0 importers. PATH check for opencode binary is never invoked externally. |

### `internal/infrastructure/agents/registry.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `AgentRegistry` | L15–L18 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewAgentRegistry` | L20–L24 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Register` | L26–L37 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Get` | L39–L49 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `List` | L51–L61 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Has` | L63–L68 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `RegisterDefaults` | L78–L101 | 🔴 DEAD | 90% | Exported but imported by 0 files |

### `internal/infrastructure/agents/stream_filter.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `DisplayEventRenderer` | L18–L18 | 🔴 DEAD | 95% | Exported type with 0 external importers. Used only as field/param type within this same dead package surface. |
| `StreamFilterWriter` | L22–L29 | 🔴 DEAD | 95% | Exported struct with 0 external importers. All constructors are also dead; no external code can obtain an instance. |
| `NewStreamFilterWriterWithParser` | L44–L49 | 🔴 DEAD | 95% | Exported constructor with 0 external importers. |
| `NewStreamFilterWriter` | L52–L56 | 🔴 DEAD | 95% | Exported constructor with 0 external importers. |
| `Write` | L58–L91 | 🔴 DEAD | 92% | Exported io.Writer method with 0 external importers; parent type StreamFilterWriter is also dead so no caller can invoke it. |
| `Flush` | L142–L174 | 🔴 DEAD | 90% | Exported method with 0 external importers; no caller outside the package. |
| `extractDisplayTextFromEvents` | L186–L209 | 🔴 DEAD | 90% | Unexported function with no callers anywhere in this file. |

### `internal/infrastructure/github/batch.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `BatchExecutor` | L19–L22 | 🔴 DEAD | 95% | Exported struct with 0 runtime and 0 type-only importers across the codebase. |
| `NewBatchExecutor` | L32–L37 | 🔴 DEAD | 95% | Exported constructor with 0 importers; no external caller instantiates BatchExecutor. |
| `BatchConfig` | L40–L45 | 🔴 DEAD | 95% | Exported type with 0 importers. Used only as parameter type in unexported methods of a dead receiver. |
| `BatchResult` | L48–L53 | 🔴 DEAD | 95% | Exported type with 0 importers. Only referenced internally within the dead BatchExecutor method set. |
| `Execute` | L70–L126 | 🔴 DEAD | 92% | Exported method with 0 importers; BatchExecutor is never instantiated externally. |

### `internal/infrastructure/logger/console_logger.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ConsoleLogger` | L14–L20 | 🔴 DEAD | 95% | 0 runtime and 0 type-only importers per exhaustive import analysis. |
| `NewConsoleLogger` | L23–L30 | 🔴 DEAD | 95% | 0 runtime and 0 type-only importers; constructor never called outside this file. |
| `Debug` | L32–L37 | 🔴 DEAD | 95% | 0 importers. Only reachable via ConsoleLogger which is itself dead. |
| `Info` | L39–L44 | 🔴 DEAD | 95% | 0 importers. Only reachable via ConsoleLogger which is itself dead. |
| `Warn` | L46–L51 | 🔴 DEAD | 95% | 0 importers. Only reachable via ConsoleLogger which is itself dead. |
| `Error` | L53–L58 | 🔴 DEAD | 95% | 0 importers. Only reachable via ConsoleLogger which is itself dead. |
| `WithContext` | L60–L72 | 🔴 DEAD | 90% | 0 importers. Only reachable via ConsoleLogger which is itself dead. |

### `internal/infrastructure/notify/provider.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `NotifyOperationProvider` | L20–L27 | 🔴 DEAD | 95% | Exported struct with 0 runtime and 0 type-only importers across the codebase. |
| `NewNotifyOperationProvider` | L40–L53 | 🔴 DEAD | 95% | Exported constructor with 0 importers; no external caller instantiates this provider. |
| `RegisterBackend` | L67–L86 | 🔴 DEAD | 95% | Exported method with 0 importers; no external code registers backends. |
| `SetDefaultBackend` | L96–L98 | 🔴 DEAD | 90% | Exported method with 0 importers; default backend is never configured externally. |
| `GetOperation` | L102–L105 | 🔴 DEAD | 95% | Exported method with 0 importers; ports.OperationProvider interface implementation never called. |
| `ListOperations` | L109–L115 | 🔴 DEAD | 95% | Exported method with 0 importers; operation enumeration never invoked externally. |
| `Execute` | L121–L251 | 🔴 DEAD | 90% | Exported method with 0 importers; dispatch logic is unreachable from outside the package. |

### `internal/infrastructure/repository/errors.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ParseError` | L13–L20 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `Error` | L23–L31 | 🔴 DEAD | 75% | Exported but imported by 0 files |
| `Unwrap` | L34–L36 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `NewParseError` | L39–L47 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `WrapParseError` | L56–L82 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ParseErrorWithLine` | L85–L92 | 🔴 DEAD | 95% | Exported but imported by 0 files |
| `ToStructuredError` | L100–L124 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/store/sqlite_history_store.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `SQLiteHistoryStore` | L24–L29 | 🔴 DEAD | 95% | Exported struct with 0 runtime and 0 type-only importers. Concrete HistoryStore implementation is never wired up externally. |
| `NewSQLiteHistoryStore` | L34–L80 | 🔴 DEAD | 95% | Exported constructor with 0 importers. No external caller instantiates the store. |
| `Record` | L110–L148 | 🔴 DEAD | 95% | Exported method with 0 importers. Never called externally. |
| `List` | L151–L230 | 🔴 DEAD | 95% | Exported method with 0 importers. Never called externally. |
| `GetStats` | L233–L291 | 🔴 DEAD | 95% | Exported method with 0 importers. Never called externally. |
| `Cleanup` | L295–L326 | 🔴 DEAD | 90% | Exported method with 0 importers. Never called externally. |
| `Close` | L330–L342 | 🔴 DEAD | 95% | Exported method with 0 importers. Never called externally. |

## ⚡ Quick Wins

- [ ] <!-- ACT-a9c6ac-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/copilot_provider.go`: Remove dead code: `CopilotProvider` is exported but unused `CopilotProvider`, `NewCopilotProvider`, `NewCopilotProviderWithOptions`, `Execute`, `ExecuteConversation`, `Name`, `Validate` (`CopilotProvider, NewCopilotProvider, NewCopilotProviderWithOptions, Execute, ExecuteConversation, Name, Validate`) [L22-L27, L29-L36, L38-L48, L67-L80, L82-L88, L90-L92, L94-L100]
- [ ] <!-- ACT-00c419-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/gemini_provider.go`: Remove dead code: `GeminiProvider` is exported but unused `GeminiProvider`, `NewGeminiProvider`, `NewGeminiProviderWithOptions`, `Execute`, `ExecuteConversation`, `Name`, `Validate` (`GeminiProvider, NewGeminiProvider, NewGeminiProviderWithOptions, Execute, ExecuteConversation, Name, Validate`) [L22-L29, L31-L38, L40-L50, L86-L111, L113-L119, L121-L123, L125-L131]
- [ ] <!-- ACT-a346ed-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/opencode_provider.go`: Remove dead code: `OpenCodeProvider` is exported but unused `OpenCodeProvider`, `NewOpenCodeProvider`, `NewOpenCodeProviderWithOptions`, `Execute`, `ExecuteConversation`, `Name`, `Validate` (`OpenCodeProvider, NewOpenCodeProvider, NewOpenCodeProviderWithOptions, Execute, ExecuteConversation, Name, Validate`) [L21-L26, L28-L35, L37-L47, L66-L91, L94-L100, L179-L181, L184-L190]
- [ ] <!-- ACT-ff98f6-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/registry.go`: Remove dead code: `AgentRegistry` is exported but unused `AgentRegistry`, `NewAgentRegistry`, `Register`, `Get`, `List`, `Has`, `RegisterDefaults` (`AgentRegistry, NewAgentRegistry, Register, Get, List, Has, RegisterDefaults`) [L15-L18, L20-L24, L26-L37, L39-L49, L51-L61, L63-L68, L78-L101]
- [ ] <!-- ACT-d1b521-1 --> **[utility · high · trivial]** `internal/infrastructure/agents/stream_filter.go`: Remove dead code: `DisplayEventRenderer` is exported but unused `DisplayEventRenderer`, `StreamFilterWriter`, `NewStreamFilterWriterWithParser`, `NewStreamFilterWriter`, `Write`, `Flush`, `extractDisplayTextFromEvents` (`DisplayEventRenderer, StreamFilterWriter, NewStreamFilterWriterWithParser, NewStreamFilterWriter, Write, Flush, extractDisplayTextFromEvents`) [L18-L18, L22-L29, L44-L49, L52-L56, L58-L91, L142-L174, L186-L209]
- [ ] <!-- ACT-cc92cb-3 --> **[utility · high · trivial]** `internal/infrastructure/github/batch.go`: Remove dead code: `BatchExecutor` is exported but unused `BatchExecutor`, `NewBatchExecutor`, `BatchConfig`, `BatchResult`, `Execute` (`BatchExecutor, NewBatchExecutor, BatchConfig, BatchResult, Execute`) [L19-L22, L32-L37, L40-L45, L48-L53, L70-L126]
- [ ] <!-- ACT-d5c797-2 --> **[utility · high · trivial]** `internal/infrastructure/logger/console_logger.go`: Remove dead code: `ConsoleLogger` is exported but unused `ConsoleLogger`, `NewConsoleLogger`, `Debug`, `Info`, `Warn`, `Error`, `WithContext` (`ConsoleLogger, NewConsoleLogger, Debug, Info, Warn, Error, WithContext`) [L14-L20, L23-L30, L32-L37, L39-L44, L46-L51, L53-L58, L60-L72]
- [ ] <!-- ACT-18585f-1 --> **[utility · high · trivial]** `internal/infrastructure/notify/provider.go`: Remove dead code: `NotifyOperationProvider` is exported but unused `NotifyOperationProvider`, `NewNotifyOperationProvider`, `RegisterBackend`, `SetDefaultBackend`, `GetOperation`, `ListOperations`, `Execute` (`NotifyOperationProvider, NewNotifyOperationProvider, RegisterBackend, SetDefaultBackend, GetOperation, ListOperations, Execute`) [L20-L27, L40-L53, L67-L86, L96-L98, L102-L105, L109-L115, L121-L251]
- [ ] <!-- ACT-a92744-2 --> **[utility · high · trivial]** `internal/infrastructure/repository/errors.go`: Remove dead code: `ParseError` is exported but unused `ParseError`, `Unwrap`, `NewParseError`, `WrapParseError`, `ParseErrorWithLine`, `ToStructuredError` (`ParseError, Unwrap, NewParseError, WrapParseError, ParseErrorWithLine, ToStructuredError`) [L13-L20, L34-L36, L39-L47, L56-L82, L85-L92, L100-L124]
- [ ] <!-- ACT-a6cdf2-1 --> **[utility · high · trivial]** `internal/infrastructure/store/sqlite_history_store.go`: Remove dead code: `SQLiteHistoryStore` is exported but unused `SQLiteHistoryStore`, `NewSQLiteHistoryStore`, `Record`, `List`, `GetStats`, `Cleanup`, `Close` (`SQLiteHistoryStore, NewSQLiteHistoryStore, Record, List, GetStats, Cleanup, Close`) [L24-L29, L34-L80, L110-L148, L151-L230, L233-L291, L295-L326, L330-L342]

## 🔧 Refactors

- [ ] <!-- ACT-a92744-3 --> **[utility · medium · trivial]** `internal/infrastructure/repository/errors.go`: Remove dead code: `Error` is exported but unused (`Error`) [L23-L31]
