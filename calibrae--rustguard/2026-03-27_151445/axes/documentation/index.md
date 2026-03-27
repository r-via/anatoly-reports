[← Back to report](../../public_report.md)

# 📝 Documentation

> Inline doc comment coverage on exported symbols — description, params, and return values.

- 📁 **Files with findings:** 35
- 🎯 **Actions:** 48

## 📦 Shards

Findings are split into shards of up to 10 files each to keep pages readable and avoid GitHub rendering limits.

- [ ] [shard.1.md](./shard.1.md) (10 files — 4 CRITICAL, 6 NEEDS_REFACTOR)
- [ ] [shard.2.md](./shard.2.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.3.md](./shard.3.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.4.md](./shard.4.md) (5 files — 5 NEEDS_REFACTOR)

## 📈 Verdict Distribution

| Verdict | Count | % |
|---------|-------|---|
| DOCUMENTED | 190 | 52% |
| PARTIAL | 124 | 34% |
| UNDOCUMENTED | 53 | 14% |

---

## 📖 Methodology

**Model:** haiku

Evaluates `///` doc comment coverage on exported symbols and optional /docs/ concept coverage.

### Rating Criteria

- **DOCUMENTED**: Symbol has a complete `///` doc comment covering description, params, and return type.
- **PARTIAL**: `///` doc comment exists but is incomplete (missing params, outdated description, or lacking return type).
- **UNDOCUMENTED**: No `///` doc comment found for an exported symbol. Types and interfaces default to DOCUMENTED.

*Generated: 2026-03-27T14:27:25.984Z*
