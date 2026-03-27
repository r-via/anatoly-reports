[← Back to Utility](./index.md) · [← Back to report](../public_report.md)

# Utility — Shard 1

## Findings

| File | Verdict | Utility | Conf. | Details |
|------|---------|---------|-------|---------|
| `rustguard-kmod/src/replay.rs` | CRITICAL | 1 | 90% | [details](../reviews/rustguard-kmod-src-replay.rev.md) |
| `rustguard-enroll/src/packet.rs` | NEEDS_REFACTOR | 2 | 88% | [details](../reviews/rustguard-enroll-src-packet.rev.md) |
| `rustguard-kmod/src/allowedips.rs` | NEEDS_REFACTOR | 1 | 85% | [details](../reviews/rustguard-kmod-src-allowedips.rev.md) |

## Symbol Details

### `rustguard-kmod/src/replay.rs`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ReplayWindow` | L11–L14 | DEAD | 88% | [DEAD] Exported pub(crate) struct with 0 workspace importers per Pre-computed... |

### `rustguard-enroll/src/packet.rs`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `ParsedUdp` | L9–L12 | DEAD | 87% | [DEAD] Exported struct with 0 workspace importers per pre-computed analysis. ... |
| `parse_eth_udp` | L16–L28 | DEAD | 88% | [DEAD] Exported function with 0 workspace importers per pre-computed analysis... |

### `rustguard-kmod/src/allowedips.rs`

| Symbol | Lines | Utility | Conf. | Detail |
|--------|-------|---------|-------|--------|
| `MAX_PEERS` | L13–L13 | DEAD | 78% | [DEAD] Exported constant with 0 workspace importers and no usage within the m... |

## Quick Wins

- [ ] <!-- ACT-4cf3f3-2 --> **[utility · high · trivial]** `rustguard-enroll/src/packet.rs`: Remove dead code: `ParsedUdp` is exported but unused (`ParsedUdp`) [L9-L12]
- [ ] <!-- ACT-4cf3f3-3 --> **[utility · high · trivial]** `rustguard-enroll/src/packet.rs`: Remove dead code: `parse_eth_udp` is exported but unused (`parse_eth_udp`) [L16-L28]
- [ ] <!-- ACT-a01ecd-4 --> **[utility · high · trivial]** `rustguard-kmod/src/allowedips.rs`: Remove dead code: `MAX_PEERS` is exported but unused (`MAX_PEERS`) [L13-L13]

## Refactors

- [ ] <!-- ACT-c30836-2 --> **[utility · medium · trivial]** `rustguard-kmod/src/replay.rs`: Remove dead code: `ReplayWindow` is exported but unused (`ReplayWindow`) [L11-L14]
