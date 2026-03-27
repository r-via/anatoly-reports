[← Back to report](../../public_report.md)

# Utility

- **Files with findings:** 3
- **Actions:** 4

## Shards

- [ ] [shard.1.md](./shard.1.md) (3 files — 1 CRITICAL, 2 NEEDS_REFACTOR)

## Verdict Distribution

| Verdict | Count | % |
|---------|-------|---|
| USED | 6 | 60% |
| DEAD | 4 | 40% |

---

## Methodology

**Model:** haiku

Detects dead or low-value code using a pre-computed import/usage graph.

### Rating Criteria

- **USED**: The symbol is imported or referenced by at least one other file (exported) or used locally (non-exported).
- **DEAD**: The symbol is exported but imported by 0 files, or is a non-exported symbol with no local references. Likely safe to remove.
- **LOW_VALUE**: The symbol is used but provides negligible value (trivial wrapper, identity function, unnecessary indirection).

*Generated: 2026-03-27T13:02:57.574Z*
