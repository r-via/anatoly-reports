[← Back to Utility](./index.md) · [← Back to report](../../public_report.md)

# ♻️ Utility — Shard 30

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `internal/interfaces/cli/run_help.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#internalinterfacesclirunhelpgo) |
| `pkg/plugin/sdk/serve.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#pkgpluginsdkservego) |
| `pkg/validation/name.go` | 🟡 NEEDS_REFACTOR | 1 | 95% | [details](#pkgvalidationnamego) |
| `internal/infrastructure/audit/factory.go` | 🟡 NEEDS_REFACTOR | 1 | 92% | [details](#internalinfrastructureauditfactorygo) |
| `internal/infrastructure/skills/frontmatter.go` | 🟡 NEEDS_REFACTOR | 1 | 92% | [details](#internalinfrastructureskillsfrontmattergo) |
| `internal/infrastructure/tools/schema_mapper.go` | 🟡 NEEDS_REFACTOR | 1 | 92% | [details](#internalinfrastructuretoolsschemamappergo) |
| `internal/interfaces/cli/migration.go` | 🟡 NEEDS_REFACTOR | 1 | 92% | [details](#internalinterfacesclimigrationgo) |
| `internal/testutil/env.go` | 🟡 NEEDS_REFACTOR | 1 | 92% | [details](#internaltestutilenvgo) |
| `pkg/interpolation/serializer.go` | 🟡 NEEDS_REFACTOR | 1 | 92% | [details](#pkginterpolationserializergo) |
| `internal/application/script_file.go` | 🟡 NEEDS_REFACTOR | 1 | 85% | [details](#internalapplicationscriptfilego) |

## 🔍 Symbol Details

### `internal/interfaces/cli/run_help.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `RenderWorkflowHelp` | L21–L36 | 🔴 DEAD | 92% | Exported but imported by 0 files per pre-computed analysis. No runtime or type-only consumers found. |

### `pkg/plugin/sdk/serve.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `Serve` | L23–L32 | 🔴 DEAD | 70% | 0 importers in the analyzed codebase. Confidence reduced from 95 because this is clearly an SDK entry point intended for external plugin binaries calling it from their main(); absence of in-repo callers is expected for a library package. |

### `pkg/validation/name.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ValidateName` | L35–L40 | 🔴 DEAD | 95% | Exported but imported by 0 files |

### `internal/infrastructure/audit/factory.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `NewWriterFromEnv` | L15–L37 | 🔴 DEAD | 92% | Exported but imported by 0 files |

### `internal/infrastructure/skills/frontmatter.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `StripFrontmatter` | L5–L24 | 🔴 DEAD | 92% | Exported but imported by 0 files |

### `internal/infrastructure/tools/schema_mapper.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `MapOperationSchema` | L26–L74 | 🔴 DEAD | 92% | Exported but imported by 0 files |

### `internal/interfaces/cli/migration.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `CheckMigration` | L17–L42 | 🔴 DEAD | 92% | Exported but imported by 0 files |

### `internal/testutil/env.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ChdirIsolated` | L30–L44 | 🔴 DEAD | 92% | Exported but imported by 0 files |

### `pkg/interpolation/serializer.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `SerializeLoopItem` | L13–L48 | 🔴 DEAD | 92% | Exported but imported by 0 files |

### `internal/application/script_file.go`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `loadScriptFile` | L10–L17 | 🔴 DEAD | 85% | Not called anywhere in this file. Also a trivial pass-through wrapper — identical signature and body delegating directly to loadExternalFile with no added logic, making it LOW_VALUE even if referenced elsewhere in the package. |

## ⚡ Quick Wins

- [ ] <!-- ACT-e5afb4-1 --> **[utility · high · trivial]** `internal/application/script_file.go`: Remove dead code: `loadScriptFile` is exported but unused (`loadScriptFile`) [L10-L17]
- [ ] <!-- ACT-805eeb-1 --> **[utility · high · trivial]** `internal/infrastructure/audit/factory.go`: Remove dead code: `NewWriterFromEnv` is exported but unused (`NewWriterFromEnv`) [L15-L37]
- [ ] <!-- ACT-c8cf82-1 --> **[utility · high · trivial]** `internal/infrastructure/skills/frontmatter.go`: Remove dead code: `StripFrontmatter` is exported but unused (`StripFrontmatter`) [L5-L24]
- [ ] <!-- ACT-6280e1-1 --> **[utility · high · trivial]** `internal/infrastructure/tools/schema_mapper.go`: Remove dead code: `MapOperationSchema` is exported but unused (`MapOperationSchema`) [L26-L74]
- [ ] <!-- ACT-e11ad8-1 --> **[utility · high · trivial]** `internal/interfaces/cli/migration.go`: Remove dead code: `CheckMigration` is exported but unused (`CheckMigration`) [L17-L42]
- [ ] <!-- ACT-ee91fa-1 --> **[utility · high · trivial]** `internal/interfaces/cli/run_help.go`: Remove dead code: `RenderWorkflowHelp` is exported but unused (`RenderWorkflowHelp`) [L21-L36]
- [ ] <!-- ACT-ea1a4b-1 --> **[utility · high · trivial]** `internal/testutil/env.go`: Remove dead code: `ChdirIsolated` is exported but unused (`ChdirIsolated`) [L30-L44]
- [ ] <!-- ACT-1f9057-1 --> **[utility · high · trivial]** `pkg/interpolation/serializer.go`: Remove dead code: `SerializeLoopItem` is exported but unused (`SerializeLoopItem`) [L13-L48]
- [ ] <!-- ACT-341c84-1 --> **[utility · high · trivial]** `pkg/validation/name.go`: Remove dead code: `ValidateName` is exported but unused (`ValidateName`) [L35-L40]

## 🔧 Refactors

- [ ] <!-- ACT-b8b88c-1 --> **[utility · medium · trivial]** `pkg/plugin/sdk/serve.go`: Remove dead code: `Serve` is exported but unused (`Serve`) [L23-L32]
