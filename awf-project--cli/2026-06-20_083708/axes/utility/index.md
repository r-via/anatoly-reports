[← Back to report](../../public_report.md)

# ♻️ Utility

> Dead and low-value code detected via import/usage graph analysis.

- 📁 **Files with findings:** 307
- 🎯 **Actions:** 337

## 📦 Shards

Findings are split into shards of up to 10 files each to keep pages readable and avoid GitHub rendering limits.

- [ ] [shard.1.md](./shard.1.md) (10 files — 2 CRITICAL, 8 NEEDS_REFACTOR)
- [ ] [shard.2.md](./shard.2.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.3.md](./shard.3.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.4.md](./shard.4.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.5.md](./shard.5.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.6.md](./shard.6.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.7.md](./shard.7.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.8.md](./shard.8.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.9.md](./shard.9.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.10.md](./shard.10.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.11.md](./shard.11.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.12.md](./shard.12.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.13.md](./shard.13.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.14.md](./shard.14.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.15.md](./shard.15.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.16.md](./shard.16.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.17.md](./shard.17.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.18.md](./shard.18.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.19.md](./shard.19.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.20.md](./shard.20.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.21.md](./shard.21.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.22.md](./shard.22.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.23.md](./shard.23.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.24.md](./shard.24.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.25.md](./shard.25.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.26.md](./shard.26.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.27.md](./shard.27.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.28.md](./shard.28.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.29.md](./shard.29.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.30.md](./shard.30.md) (10 files — 10 NEEDS_REFACTOR)
- [ ] [shard.31.md](./shard.31.md) (7 files — 5 NEEDS_REFACTOR)

## 📈 Verdict Distribution

| Verdict | Count | % |
|---------|-------|---|
| USED | 1136 | 34% |
| DEAD | 2214 | 66% |
| LOW_VALUE | 2 | 0% |

---

## 📖 Methodology

**Model:** sonnet

Detects dead or low-value code using a pre-computed import/usage graph.

### Rating Criteria

- **USED**: The symbol is imported or referenced by at least one other file (exported) or used locally (non-exported).
- **DEAD**: The symbol is exported but imported by 0 files, or is a non-exported symbol with no local references. Likely safe to remove.
- **LOW_VALUE**: The symbol is used but provides negligible value (trivial wrapper, identity function, unnecessary indirection).

*Generated: 2026-06-20T08:54:06.761Z*
