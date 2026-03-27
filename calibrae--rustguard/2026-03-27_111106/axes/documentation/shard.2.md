[← Back to Documentation](./index.md) · [← Back to report](../../public_report.md)

# Documentation — Shard 2

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `rustguard-enroll/src/control.rs` | NEEDS_REFACTOR | 9 | 95% | [details](../reviews/rustguard-enroll-src-control.rev.md) |
| `rustguard-daemon/src/config.rs` | NEEDS_REFACTOR | 5 | 95% | [details](../reviews/rustguard-daemon-src-config.rev.md) |
| `rustguard-enroll/src/protocol.rs` | NEEDS_REFACTOR | 8 | 92% | [details](../reviews/rustguard-enroll-src-protocol.rev.md) |
| `rustguard-tun/src/linux.rs` | NEEDS_REFACTOR | 3 | 90% | [details](../reviews/rustguard-tun-src-linux.rev.md) |
| `rustguard-tun/src/bpf_loader.rs` | NEEDS_REFACTOR | 1 | 88% | [details](../reviews/rustguard-tun-src-bpf_loader.rev.md) |
| `rustguard-crypto/src/aead.rs` | NEEDS_REFACTOR | 7 | 92% | [details](../reviews/rustguard-crypto-src-aead.rev.md) |
| `rustguard-tun/src/macos.rs` | NEEDS_REFACTOR | 4 | 90% | [details](../reviews/rustguard-tun-src-macos.rev.md) |
| `rustguard-kmod/src/cookie.rs` | NEEDS_REFACTOR | 3 | 92% | [details](../reviews/rustguard-kmod-src-cookie.rev.md) |
| `rustguard-tun/src/linux_mq.rs` | NEEDS_REFACTOR | 1 | 90% | [details](../reviews/rustguard-tun-src-linux_mq.rev.md) |
| `rustguard-enroll/src/fast_udp.rs` | NEEDS_REFACTOR | 6 | 85% | [details](../reviews/rustguard-enroll-src-fast_udp.rev.md) |

## Symbol Details

### `rustguard-enroll/src/control.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `socket_path` | L18–L20 | PARTIAL | 88% | [USED] Exported, runtime-imported by rustguard-enroll/src/server.rs. Also cal... |
| `new_window` | L26–L28 | UNDOCUMENTED | 95% | [USED] Exported, runtime-imported by rustguard-enroll/src/server.rs. Public A... |
| `is_open` | L31–L41 | PARTIAL | 87% | [USED] Exported, runtime-imported by rustguard-enroll/src/server.rs. Public A... |
| `open_window` | L44–L51 | PARTIAL | 87% | [USED] Exported, runtime-imported by rustguard-enroll/src/server.rs. Also cal... |
| `close_window` | L54–L56 | PARTIAL | 85% | [USED] Exported, runtime-imported by rustguard-enroll/src/server.rs. Also cal... |
| `remaining` | L59–L73 | PARTIAL | 87% | [USED] Exported, runtime-imported by rustguard-enroll/src/server.rs. Also cal... |
| `start_listener` | L77–L104 | PARTIAL | 78% | [USED] Exported, runtime-imported by rustguard-enroll/src/server.rs. Substant... |
| `send_command` | L147–L166 | PARTIAL | 88% | [USED] Exported, runtime-imported by rustguard-enroll/src/server.rs. Public A... |
| `cleanup` | L169–L171 | PARTIAL | 85% | [USED] Exported, runtime-imported by rustguard-enroll/src/server.rs. Utility ... |

### `rustguard-daemon/src/config.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `Config` | L13–L16 | PARTIAL | 72% | [USED] Main exported struct holding parsed WireGuard configuration. Runtime-i... |
| `InterfaceConfig` | L19–L25 | UNDOCUMENTED | 95% | [USED] Exported struct for WireGuard interface configuration. Runtime-importe... |
| `PeerConfig` | L28–L34 | UNDOCUMENTED | 95% | [USED] Exported struct for WireGuard peer configuration. Runtime-imported by ... |
| `CidrAddr` | L38–L41 | PARTIAL | 85% | [USED] Exported struct with three utility methods (contains_v4, contains_v6, ... |
| `prefix_to_netmask` | L284–L292 | UNDOCUMENTED | 88% | [USED] Exported function converting prefix length to IPv4 netmask. Runtime-im... |

### `rustguard-enroll/src/protocol.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `ENROLL_REQUEST_SIZE` | L17–L17 | UNDOCUMENTED | 92% | [USED] Exported constant imported by 2 runtime files (client.rs, server.rs). ... |
| `ENROLL_RESPONSE_SIZE` | L18–L18 | UNDOCUMENTED | 92% | [USED] Exported constant imported by 2 runtime files (client.rs, server.rs). ... |
| `derive_token_key` | L22–L24 | PARTIAL | 88% | [USED] Exported function imported by 2 runtime files (client.rs, server.rs). ... |
| `build_request` | L27–L36 | PARTIAL | 85% | [USED] Exported function imported by 2 runtime files (client.rs, server.rs). ... |
| `parse_request` | L40–L51 | PARTIAL | 85% | [USED] Exported function imported by 2 runtime files (client.rs, server.rs). ... |
| `EnrollmentOffer` | L54–L58 | PARTIAL | 88% | [USED] Exported struct imported by 2 runtime files (client.rs, server.rs). Co... |
| `build_response` | L61–L75 | PARTIAL | 85% | [USED] Exported function imported by 2 runtime files (client.rs, server.rs). ... |
| `parse_response` | L78–L102 | PARTIAL | 86% | [USED] Exported function imported by 2 runtime files (client.rs, server.rs). ... |

### `rustguard-tun/src/linux.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `create` | L78–L122 | UNDOCUMENTED | 90% | [DEAD] Exported pub fn with 0 runtime importers per analysis; public API for ... |
| `read` | L193–L199 | PARTIAL | 90% | [DEAD] Exported pub fn with 0 runtime importers per analysis; public API for ... |
| `write` | L203–L212 | PARTIAL | 90% | [DEAD] Exported pub fn with 0 runtime importers per analysis; public API for ... |

### `rustguard-tun/src/bpf_loader.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `XdpProgram` | L37–L41 | PARTIAL | 88% | [DEAD] Exported public struct with 0 runtime importers and 0 type-only import... |

### `rustguard-crypto/src/aead.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `AEAD_TAG_LEN` | L7–L7 | UNDOCUMENTED | 92% | [USED] Exported constant with 1 runtime importer in rustguard-crypto/src/lib.... |
| `seal` | L16–L28 | PARTIAL | 85% | [USED] Exported function with 1 runtime importer in rustguard-crypto/src/lib.... |
| `open` | L33–L45 | PARTIAL | 85% | [USED] Exported function with 1 runtime importer in rustguard-crypto/src/lib.... |
| `xseal` | L48–L54 | PARTIAL | 85% | [USED] Exported function with 1 runtime importer in rustguard-crypto/src/lib.... |
| `xopen` | L57–L63 | PARTIAL | 85% | [USED] Exported function with 1 runtime importer in rustguard-crypto/src/lib.... |
| `seal_to` | L68–L84 | PARTIAL | 88% | [USED] Exported function with 1 runtime importer in rustguard-crypto/src/lib.... |
| `open_to` | L89–L102 | PARTIAL | 88% | [USED] Exported function with 1 runtime importer in rustguard-crypto/src/lib.... |

### `rustguard-tun/src/macos.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `close_and_error` | L79–L83 | PARTIAL | 75% | [USED] Called 4 times to safely preserve OS error before closing file descrip... |
| `create` | L95–L192 | UNDOCUMENTED | 90% | [DEAD] Exported pub fn with 0 workspace imports and no local usage. May be pa... |
| `read` | L245–L263 | PARTIAL | 90% | [DEAD] Exported pub fn with 0 workspace imports and no local usage. Platform-... |
| `write` | L266–L296 | PARTIAL | 90% | [DEAD] Exported pub fn with 0 workspace imports and no local usage. Platform-... |

### `rustguard-kmod/src/cookie.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `CookieChecker` | L66–L71 | PARTIAL | 85% | [USED] Primary server-side cookie validation API in library crate; zero works... |
| `CookieState` | L74–L77 | PARTIAL | 85% | [USED] Primary client-side cookie storage API in library crate; zero workspac... |
| `constant_time_eq` | L202–L205 | PARTIAL | 80% | [USED] Internal constant-time comparison wrapper using kernel crypto_memneq, ... |

### `rustguard-tun/src/linux_mq.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `MultiQueueTun` | L77–L81 | PARTIAL | 90% | [DEAD] Exported public struct with 0 workspace importers per analysis, sugges... |

### `rustguard-enroll/src/fast_udp.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `PKT_BUF_SIZE` | L16–L16 | PARTIAL | 80% | [USED] Public const in library crate API; used locally and by downstream cons... |
| `RecvBatch` | L19–L24 | PARTIAL | 85% | [USED] Public struct; primary type returned by recv_batch in library API | [U... |
| `recv_batch` | L39–L83 | PARTIAL | 80% | [USED] macOS fallback implementation; public library API for batch receives |... |
| `send_packet` | L106–L108 | UNDOCUMENTED | 85% | [USED] macOS fallback implementation; public library API for packet transmiss... |
| `recv_batch` | L112–L126 | PARTIAL | 80% | [USED] macOS fallback implementation; public library API for batch receives |... |
| `send_packet` | L129–L131 | UNDOCUMENTED | 85% | [USED] macOS fallback implementation; public library API for packet transmiss... |

## Hygiene

- [ ] <!-- ACT-efa3ec-1 --> **[documentation · medium · trivial]** `rustguard-crypto/src/aead.rs`: Add `///` doc comment documentation for exported symbol: `AEAD_TAG_LEN` (`AEAD_TAG_LEN`) [L7-L7]
- [ ] <!-- ACT-dbf131-6 --> **[documentation · medium · trivial]** `rustguard-daemon/src/config.rs`: Add `///` doc comment documentation for exported symbol:  `InterfaceConfig`, `PeerConfig`, `prefix_to_netmask` (`InterfaceConfig, PeerConfig, prefix_to_netmask`) [L19-L25, L28-L34, L284-L292]
- [ ] <!-- ACT-18d76d-3 --> **[documentation · medium · trivial]** `rustguard-enroll/src/control.rs`: Add `///` doc comment documentation for exported symbol: `new_window` (`new_window`) [L26-L28]
- [ ] <!-- ACT-c2ad01-3 --> **[documentation · medium · trivial]** `rustguard-enroll/src/fast_udp.rs`: Add `///` doc comment documentation for exported symbol: `send_packet` (`send_packet`) [L129-L131]
- [ ] <!-- ACT-d84ed3-1 --> **[documentation · medium · trivial]** `rustguard-enroll/src/protocol.rs`: Add `///` doc comment documentation for exported symbol: `ENROLL_REQUEST_SIZE` (`ENROLL_REQUEST_SIZE`) [L17-L17]
- [ ] <!-- ACT-d84ed3-2 --> **[documentation · medium · trivial]** `rustguard-enroll/src/protocol.rs`: Add `///` doc comment documentation for exported symbol: `ENROLL_RESPONSE_SIZE` (`ENROLL_RESPONSE_SIZE`) [L18-L18]
- [ ] <!-- ACT-62f2c7-6 --> **[documentation · medium · trivial]** `rustguard-tun/src/bpf_loader.rs`: Add `///` doc comment documentation for exported symbol: `XdpProgram` (`XdpProgram`) [L37-L41]
- [ ] <!-- ACT-22946f-7 --> **[documentation · medium · trivial]** `rustguard-tun/src/linux_mq.rs`: Add `///` doc comment documentation for exported symbol: `MultiQueueTun` (`MultiQueueTun`) [L77-L81]
- [ ] <!-- ACT-c62ee4-7 --> **[documentation · medium · trivial]** `rustguard-tun/src/linux.rs`: Add `///` doc comment documentation for exported symbol:  `create`, `read`, `write` (`create, read, write`) [L78-L122, L193-L199, L203-L212]
- [ ] <!-- ACT-b59607-7 --> **[documentation · medium · trivial]** `rustguard-tun/src/macos.rs`: Add `///` doc comment documentation for exported symbol:  `create`, `read`, `write` (`create, read, write`) [L95-L192, L245-L263, L266-L296]
- [ ] <!-- ACT-efa3ec-2 --> **[documentation · low · trivial]** `rustguard-crypto/src/aead.rs`: Complete `///` doc comment documentation for:  `MAX_PACKET_SIZE`, `seal`, `open`, `xseal`, `xopen`, `seal_to`, `open_to` (`MAX_PACKET_SIZE, seal, open, xseal, xopen, seal_to, open_to`) [L10-L10, L16-L28, L33-L45, L48-L54, L57-L63, L68-L84, L89-L102]
- [ ] <!-- ACT-dbf131-4 --> **[documentation · low · trivial]** `rustguard-daemon/src/config.rs`: Complete `///` doc comment documentation for: `Config` (`Config`) [L13-L16]
- [ ] <!-- ACT-dbf131-5 --> **[documentation · low · trivial]** `rustguard-daemon/src/config.rs`: Complete `///` doc comment documentation for: `CidrAddr` (`CidrAddr`) [L38-L41]
- [ ] <!-- ACT-18d76d-2 --> **[documentation · low · trivial]** `rustguard-enroll/src/control.rs`: Complete `///` doc comment documentation for:  `socket_path`, `is_open`, `open_window`, `close_window`, `remaining`, `start_listener`, `send_command`, `cleanup` (`socket_path, is_open, open_window, close_window, remaining, start_listener, send_command, cleanup`) [L18-L20, L31-L41, L44-L51, L54-L56, L59-L73, L77-L104, L147-L166, L169-L171]
- [ ] <!-- ACT-c2ad01-1 --> **[documentation · low · trivial]** `rustguard-enroll/src/fast_udp.rs`: Complete `///` doc comment documentation for:  `PKT_BUF_SIZE`, `RecvBatch`, `recv_batch`, `recv_batch` (`PKT_BUF_SIZE, RecvBatch, recv_batch, recv_batch`) [L16-L16, L19-L24, L39-L83, L112-L126]
- [ ] <!-- ACT-d84ed3-3 --> **[documentation · low · trivial]** `rustguard-enroll/src/protocol.rs`: Complete `///` doc comment documentation for:  `derive_token_key`, `build_request`, `parse_request`, `EnrollmentOffer`, `build_response`, `parse_response` (`derive_token_key, build_request, parse_request, EnrollmentOffer, build_response, parse_response`) [L22-L24, L27-L36, L40-L51, L54-L58, L61-L75, L78-L102]
- [ ] <!-- ACT-9105c7-5 --> **[documentation · low · trivial]** `rustguard-kmod/src/cookie.rs`: Complete `///` doc comment documentation for:  `CookieChecker`, `CookieState`, `constant_time_eq` (`CookieChecker, CookieState, constant_time_eq`) [L66-L71, L74-L77, L202-L205]
