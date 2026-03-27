[← Back to report](../public_report.md)

# Duplication

- **Files with findings:** 9
- **Actions:** 21

## Shards

- [ ] [shard.1.md](./shard.1.md) (9 files — 1 CRITICAL, 8 NEEDS_REFACTOR)

## Verdict Distribution

| Verdict | Count | % |
|---------|-------|---|
| UNIQUE | 129 | 85% |
| DUPLICATE | 22 | 15% |

---

## Methodology

**Model:** haiku

Identifies code clones via RAG semantic vector search against the codebase index.

### Rating Criteria

- **UNIQUE**: No semantically similar function found, or similarity score < 0.75.
- **DUPLICATE**: Similarity score >= 0.85 with matching logic/behavior. The duplicate target file and symbol are reported.

*Generated: 2026-03-27T12:36:36.651Z*
