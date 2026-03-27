[← Back to Documentation](./index.md) · [← Back to report](../../public_report.md)

# Documentation — Shard 3

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `rustguard-enroll/src/client.rs` | NEEDS_REFACTOR | 2 | 88% | [details](../reviews/rustguard-enroll-src-client.rev.md) |
| `rustguard-crypto/src/blake2s.rs` | NEEDS_REFACTOR | 3 | 90% | [details](../reviews/rustguard-crypto-src-blake2s.rev.md) |
| `rustguard-crypto/src/x25519.rs` | NEEDS_REFACTOR | 4 | 88% | [details](../reviews/rustguard-crypto-src-x25519.rev.md) |
| `rustguard-enroll/src/packet.rs` | NEEDS_REFACTOR | 2 | 88% | [details](../reviews/rustguard-enroll-src-packet.rev.md) |
| `rustguard-enroll/src/state.rs` | NEEDS_REFACTOR | 4 | 88% | [details](../reviews/rustguard-enroll-src-state.rev.md) |
| `rustguard-tun/examples/tun_echo.rs` | NEEDS_REFACTOR | 1 | 85% | [details](../reviews/rustguard-tun-examples-tun_echo.rev.md) |
| `rustguard-kmod/src/timers.rs` | NEEDS_REFACTOR | 1 | 95% | [details](../reviews/rustguard-kmod-src-timers.rev.md) |
| `rustguard-kmod/src/allowedips.rs` | NEEDS_REFACTOR | 2 | 85% | [details](../reviews/rustguard-kmod-src-allowedips.rev.md) |
| `rustguard-tun/src/uring.rs` | NEEDS_REFACTOR | 1 | 90% | [details](../reviews/rustguard-tun-src-uring.rev.md) |
| `rustguard-tun/src/lib.rs` | NEEDS_REFACTOR | 2 | 88% | [details](../reviews/rustguard-tun-src-lib.rev.md) |

## Symbol Details

### `rustguard-enroll/src/client.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `JoinConfig` | L23–L26 | UNDOCUMENTED | 85% | [USED] Public struct in library crate with 0 workspace importers. Pattern fro... |
| `run` | L28–L192 | UNDOCUMENTED | 85% | [USED] Public function in library crate with 0 workspace importers. Pattern f... |

### `rustguard-crypto/src/blake2s.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `hash` | L9–L15 | PARTIAL | 80% | [USED] Exported function runtime-imported by rustguard-crypto/src/lib.rs for ... |
| `mac` | L24–L32 | PARTIAL | 85% | [USED] Exported function runtime-imported by rustguard-crypto/src/lib.rs for ... |
| `hkdf` | L40–L60 | PARTIAL | 80% | [USED] Exported function runtime-imported by rustguard-crypto/src/lib.rs for ... |

### `rustguard-crypto/src/x25519.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `StaticSecret` | L7–L7 | PARTIAL | 88% | [USED] Exported struct runtime-imported by rustguard-crypto/src/lib.rs. Core ... |
| `EphemeralSecret` | L11–L11 | PARTIAL | 88% | [USED] Exported struct runtime-imported by rustguard-crypto/src/lib.rs. Ephem... |
| `PublicKey` | L16–L16 | PARTIAL | 87% | [USED] Exported struct runtime-imported by rustguard-crypto/src/lib.rs. X2551... |
| `SharedSecret` | L28–L28 | PARTIAL | 87% | [USED] Exported struct runtime-imported by rustguard-crypto/src/lib.rs. DH sh... |

### `rustguard-enroll/src/packet.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `ParsedUdp` | L9–L12 | PARTIAL | 87% | [DEAD] Exported struct with 0 workspace importers per pre-computed analysis. ... |
| `parse_eth_udp` | L16–L28 | PARTIAL | 88% | [DEAD] Exported function with 0 workspace importers per pre-computed analysis... |

### `rustguard-enroll/src/state.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `PersistedPeer` | L13–L16 | PARTIAL | 85% | [USED] Exported struct with 1 runtime importer (rustguard-enroll/src/server.r... |
| `default_state_path` | L19–L21 | PARTIAL | 85% | [USED] Exported function with 1 runtime importer (rustguard-enroll/src/server... |
| `save` | L25–L45 | PARTIAL | 88% | [USED] Exported function with 1 runtime importer (rustguard-enroll/src/server... |
| `load` | L48–L84 | PARTIAL | 85% | [USED] Exported function with 1 runtime importer (rustguard-enroll/src/server... |

### `rustguard-tun/examples/tun_echo.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `main` | L12–L79 | PARTIAL | 78% | [USED] Binary entry point that orchestrates TUN interface creation, packet re... |

### `rustguard-kmod/src/timers.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `SessionTimers` | L36–L49 | PARTIAL | 88% | [USED] pub(crate) struct serving as crate-level session timer API with 6+ pub... |

### `rustguard-kmod/src/allowedips.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `MAX_PEERS` | L13–L13 | PARTIAL | 78% | [DEAD] Exported constant with 0 workspace importers and no usage within the m... |
| `AllowedIps` | L28–L31 | PARTIAL | 85% | [DEAD] Exported routing table struct with 0 workspace importers; defined with... |

### `rustguard-tun/src/uring.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `UringTun` | L96–L102 | PARTIAL | 88% | [DEAD] Exported struct with 0 workspace importers. Provides public constructo... |

### `rustguard-tun/src/lib.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `Tun` | L25–L30 | PARTIAL | 78% | [USED] Public struct in library crate with documented public API methods (cre... |
| `TunConfig` | L33–L44 | PARTIAL | 88% | [USED] Public configuration struct required as parameter for Tun::create() pu... |

## Hygiene

- [ ] <!-- ACT-ca1d92-2 --> **[documentation · medium · trivial]** `rustguard-enroll/src/client.rs`: Add `///` doc comment documentation for exported symbol: `JoinConfig` (`JoinConfig`) [L23-L26]
- [ ] <!-- ACT-ca1d92-3 --> **[documentation · medium · trivial]** `rustguard-enroll/src/client.rs`: Add `///` doc comment documentation for exported symbol: `run` (`run`) [L28-L192]
- [ ] <!-- ACT-4cf3f3-4 --> **[documentation · medium · trivial]** `rustguard-enroll/src/packet.rs`: Add `///` doc comment documentation for exported symbol: `ParsedUdp` (`ParsedUdp`) [L9-L12]
- [ ] <!-- ACT-4cf3f3-5 --> **[documentation · medium · trivial]** `rustguard-enroll/src/packet.rs`: Add `///` doc comment documentation for exported symbol: `parse_eth_udp` (`parse_eth_udp`) [L16-L28]
- [ ] <!-- ACT-bc7d6e-1 --> **[documentation · low · trivial]** `rustguard-crypto/src/blake2s.rs`: Complete `///` doc comment documentation for:  `hash`, `mac`, `hkdf` (`hash, mac, hkdf`) [L9-L15, L24-L32, L40-L60]
- [ ] <!-- ACT-7726d0-3 --> **[documentation · low · trivial]** `rustguard-crypto/src/x25519.rs`: Complete `///` doc comment documentation for:  `StaticSecret`, `EphemeralSecret`, `PublicKey`, `SharedSecret` (`StaticSecret, EphemeralSecret, PublicKey, SharedSecret`) [L7-L7, L11-L11, L16-L16, L28-L28]
- [ ] <!-- ACT-ee407e-1 --> **[documentation · low · trivial]** `rustguard-enroll/src/state.rs`: Complete `///` doc comment documentation for:  `PersistedPeer`, `default_state_path`, `save`, `load` (`PersistedPeer, default_state_path, save, load`) [L13-L16, L19-L21, L25-L45, L48-L84]
- [ ] <!-- ACT-050448-4 --> **[documentation · low · trivial]** `rustguard-kmod/src/timers.rs`: Complete `///` doc comment documentation for: `SessionTimers` (`SessionTimers`) [L36-L49]
- [ ] <!-- ACT-4d0b70-3 --> **[documentation · low · trivial]** `rustguard-tun/src/lib.rs`: Complete `///` doc comment documentation for: `Tun` (`Tun`) [L25-L30]
- [ ] <!-- ACT-4d0b70-4 --> **[documentation · low · trivial]** `rustguard-tun/src/lib.rs`: Complete `///` doc comment documentation for: `TunConfig` (`TunConfig`) [L33-L44]
