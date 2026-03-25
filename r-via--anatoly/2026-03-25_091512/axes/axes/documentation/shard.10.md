# Documentation — Shard 10

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `src/core/sdk-semaphore.ts` | NEEDS_REFACTOR | 1 | 85% | [details](../reviews/src-core-sdk-semaphore.rev.md) |

## Symbol Details

### `src/core/sdk-semaphore.ts`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `Semaphore` | L13–L60 | PARTIAL | 85% | [PARTIAL] The module-level JSDoc block (lines 6–11) covers the class purpose,... |

## Hygiene

- [ ] <!-- ACT-c94602-1 --> **[documentation · low · trivial]** `src/core/sdk-semaphore.ts`: Complete JSDoc documentation for: `Semaphore` (`Semaphore`) [L13-L60]

## Documentation Coverage

### `src/core/sdk-semaphore.ts` — 0% covered

- [ ] **Global SDK Concurrency Semaphore** — MISSING: None of the listed documentation pages covers the Semaphore or SDK-level concurrency bounding. The nearest candidate, 04-Core-Modules/06-Worker-Pool.md, addresses file-level parallelism rather than the cross-cutting cap on concurrent Claude SDK calls that this class enforces. The concept is absent from architecture, design-decision, and core-module pages alike.
