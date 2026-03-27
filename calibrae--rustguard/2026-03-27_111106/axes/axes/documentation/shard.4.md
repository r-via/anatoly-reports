[← Back to Documentation](./index.md) · [← Back to report](../public_report.md)

# Documentation — Shard 4

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `rustguard-crypto/src/tai64n.rs` | NEEDS_REFACTOR | 1 | 87% | [details](../reviews/rustguard-crypto-src-tai64n.rev.md) |
| `rustguard-core/tests/integration.rs` | NEEDS_REFACTOR | 1 | 93% | [details](../reviews/rustguard-core-tests-integration.rev.md) |
| `rustguard-core/src/replay.rs` | NEEDS_REFACTOR | 1 | 92% | [details](../reviews/rustguard-core-src-replay.rev.md) |
| `rustguard-enroll/src/pool.rs` | NEEDS_REFACTOR | 1 | 90% | [details](../reviews/rustguard-enroll-src-pool.rev.md) |
| `rustguard-daemon/src/peer.rs` | NEEDS_REFACTOR | 1 | 86% | [details](../reviews/rustguard-daemon-src-peer.rev.md) |

## Symbol Details

### `rustguard-crypto/src/tai64n.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `Tai64n` | L9–L9 | PARTIAL | 87% | [USED] Exported struct with 1 runtime importer (rustguard-crypto/src/lib.rs) ... |

### `rustguard-core/tests/integration.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `do_handshake` | L11–L31 | PARTIAL | 85% | [USED] Helper function explicitly called by 6 test functions: full_handshake_... |

### `rustguard-core/src/replay.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `ReplayWindow` | L13–L19 | PARTIAL | 92% | [USED] Exported struct with 1 runtime importer: rustguard-core/src/session.rs... |

### `rustguard-enroll/src/pool.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `IpPool` | L9–L20 | PARTIAL | 90% | [USED] Exported struct with 1 runtime importer (rustguard-enroll/src/server.r... |

### `rustguard-daemon/src/peer.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `Peer` | L16–L26 | PARTIAL | 86% | [USED] Exported struct with 1 runtime importer (rustguard-daemon/src/tunnel.r... |

## Hygiene

- [ ] <!-- ACT-f55ce3-2 --> **[documentation · low · trivial]** `rustguard-core/src/replay.rs`: Complete `///` doc comment documentation for: `ReplayWindow` (`ReplayWindow`) [L13-L19]
- [ ] <!-- ACT-971d22-1 --> **[documentation · low · trivial]** `rustguard-core/tests/integration.rs`: Complete `///` doc comment documentation for: `do_handshake` (`do_handshake`) [L11-L31]
- [ ] <!-- ACT-310cbd-3 --> **[documentation · low · trivial]** `rustguard-crypto/src/tai64n.rs`: Complete `///` doc comment documentation for: `Tai64n` (`Tai64n`) [L9-L9]
- [ ] <!-- ACT-eacdc0-2 --> **[documentation · low · trivial]** `rustguard-daemon/src/peer.rs`: Complete `///` doc comment documentation for: `Peer` (`Peer`) [L16-L26]
- [ ] <!-- ACT-92d12d-1 --> **[documentation · low · trivial]** `rustguard-enroll/src/pool.rs`: Complete `///` doc comment documentation for: `IpPool` (`IpPool`) [L9-L20]
