# Documentation

- **Files with findings:** 5
- **Actions:** 34

## Shards

- [ ] [shard.1.md](./shard.1.md) (5 files — 1 CRITICAL, 4 NEEDS_REFACTOR)

## Verdict Distribution

| Verdict | Count | % |
|---------|-------|---|
| DOCUMENTED | 21 | 38% |
| PARTIAL | 26 | 46% |
| UNDOCUMENTED | 9 | 16% |

---

## Methodology

**Model:** haiku

Evaluates `///` doc comment coverage on exported symbols and optional /docs/ concept coverage.

### Rating Criteria

- **DOCUMENTED**: Symbol has a complete `///` doc comment covering description, params, and return type.
- **PARTIAL**: `///` doc comment exists but is incomplete (missing params, outdated description, or lacking return type).
- **UNDOCUMENTED**: No `///` doc comment found for an exported symbol. Types and interfaces default to DOCUMENTED.

*Generated: 2026-03-25T16:46:15.929Z*
