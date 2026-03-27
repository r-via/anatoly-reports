[← Back to Documentation](./index.md) · [← Back to report](../public_report.md)

# Documentation — Shard 1

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `rustguard-kmod/src/lib.rs` | CRITICAL | 13 | 90% | [details](../reviews/rustguard-kmod-src-lib.rev.md) |
| `rustguard-tun/src/xdp.rs` | CRITICAL | 5 | 95% | [details](../reviews/rustguard-tun-src-xdp.rev.md) |
| `rustguard-enroll/src/server.rs` | CRITICAL | 3 | 92% | [details](../reviews/rustguard-enroll-src-server.rev.md) |
| `rustguard-kmod/src/replay.rs` | CRITICAL | 1 | 90% | [details](../reviews/rustguard-kmod-src-replay.rev.md) |
| `rustguard-kmod/src/noise.rs` | NEEDS_REFACTOR | 19 | 92% | [details](../reviews/rustguard-kmod-src-noise.rev.md) |
| `rustguard-core/src/handshake.rs` | NEEDS_REFACTOR | 14 | 88% | [details](../reviews/rustguard-core-src-handshake.rev.md) |
| `rustguard-core/src/cookie.rs` | NEEDS_REFACTOR | 5 | 92% | [details](../reviews/rustguard-core-src-cookie.rev.md) |
| `rustguard-daemon/src/tunnel.rs` | NEEDS_REFACTOR | 4 | 90% | [details](../reviews/rustguard-daemon-src-tunnel.rev.md) |
| `rustguard-core/src/messages.rs` | NEEDS_REFACTOR | 12 | 90% | [details](../reviews/rustguard-core-src-messages.rev.md) |
| `rustguard-core/src/timers.rs` | NEEDS_REFACTOR | 1 | 90% | [details](../reviews/rustguard-core-src-timers.rev.md) |

## Symbol Details

### `rustguard-kmod/src/lib.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `AEAD_TAG_SIZE` | L111–L111 | PARTIAL | 82% | [USED] Exported constant used internally for tag size calculations in encrypt... |
| `rustguard_xmit` | L382–L384 | PARTIAL | 70% | [USED] Exported C FFI callback (#[no_mangle]) for TX path; kernel calls this ... |
| `rustguard_rx` | L465–L467 | PARTIAL | 70% | [USED] Exported C FFI callback (#[no_mangle]) for RX path; kernel calls this ... |
| `handle_initiation` | L521–L620 | PARTIAL | 82% | [USED] Handshake handler called from do_rx for MSG_INITIATION (L495); process... |
| `handle_response` | L624–L671 | PARTIAL | 82% | [USED] Handshake handler called from do_rx for MSG_RESPONSE (L499); processes... |
| `handle_transport` | L676–L755 | PARTIAL | 80% | [USED] Transport handler called from do_rx for MSG_TRANSPORT (L507); decrypts... |
| `handle_cookie_reply` | L758–L781 | PARTIAL | 80% | [USED] Cookie handler called from do_rx for type 3 (L504); processes MAC2 coo... |
| `rustguard_dev_uninit` | L785–L785 | PARTIAL | 90% | [LOW_VALUE] Exported C FFI callback for device teardown; completely empty stu... |
| `rustguard_timer_tick` | L789–L791 | PARTIAL | 70% | [USED] Exported C FFI callback invoked periodically (250ms) by wg_timer_start... |
| `initiate_rekey` | L847–L877 | PARTIAL | 80% | [USED] Called from do_timer_tick for rekey trigger (L829, L835); creates hand... |
| `send_keepalive` | L880–L914 | PARTIAL | 80% | [USED] Called from do_timer_tick (L841) when keepalive needed; sends empty tr... |
| `rustguard_genl_get` | L918–L922 | PARTIAL | 88% | [LOW_VALUE] Exported C FFI callback for genetlink GET; empty stub returning 0... |
| `rustguard_genl_set` | L926–L932 | PARTIAL | 88% | [LOW_VALUE] Exported C FFI callback for genetlink SET; empty stub returning 0... |

### `rustguard-tun/src/xdp.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `XdpDesc` | L62–L66 | UNDOCUMENTED | 90% | [USED] Used locally at L279, L290 in unsafe casts for descriptor access; expo... |
| `Ring` | L89–L95 | PARTIAL | 88% | [USED] Core data structure used for rx_ring, tx_ring, fill_ring, comp_ring fi... |
| `XdpConfig` | L126–L132 | PARTIAL | 88% | [USED] Used as parameter type at L167 and fields accessed in create_inner; ex... |
| `XdpSocket` | L148–L160 | PARTIAL | 95% | [USED] Primary public struct returned from create(); used in all socket opera... |
| `if_nametoindex` | L449–L458 | UNDOCUMENTED | 90% | [USED] Called at L214 to resolve interface name to index; exported public uti... |

### `rustguard-enroll/src/server.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `ServeConfig` | L49–L62 | UNDOCUMENTED | 92% | [DEAD] Exported public struct; parameter type of public run() function at L64... |
| `run` | L64–L532 | UNDOCUMENTED | 92% | [DEAD] Exported public function (main tunnel server loop); pre-computed analy... |
| `setup_xdp` | L548–L577 | PARTIAL | 80% | [USED] Non-exported Linux-only helper (cfg gate); called at L114 to load BPF ... |

### `rustguard-kmod/src/replay.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `ReplayWindow` | L11–L14 | PARTIAL | 88% | [DEAD] Exported pub(crate) struct with 0 workspace importers per Pre-computed... |

### `rustguard-kmod/src/noise.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `zeroize` | L46–L50 | PARTIAL | 90% | [DEAD] Exported (pub(crate)) with 0 workspace importers per import analysis, ... |
| `constant_time_eq` | L53–L58 | PARTIAL | 85% | [USED] Not exported; called in process_response (L360) and process_initiation... |
| `hash` | L72–L94 | PARTIAL | 80% | [USED] Not exported; called in initial_chain_key (L180), initial_hash (L184),... |
| `mac` | L97–L107 | PARTIAL | 85% | [USED] Not exported; called in compute_mac1() at L215 for MAC1 computation. |... |
| `hkdf` | L110–L121 | PARTIAL | 85% | [USED] Not exported; called in mix_key (L193), process_response (L389, L419),... |
| `seal` | L124–L138 | PARTIAL | 85% | [USED] Not exported; called in encrypt_and_hash() at L198 for AEAD encryption... |
| `open` | L141–L155 | PARTIAL | 88% | [USED] Not exported; called in decrypt_and_hash() at L204 for AEAD decryption... |
| `dh` | L158–L162 | PARTIAL | 85% | [USED] Not exported; called in create_initiation (L304, L313), process_respon... |
| `generate_keypair` | L165–L173 | PARTIAL | 85% | [USED] Not exported; called in create_initiation (L300) and process_initiatio... |
| `encrypt_and_hash` | L196–L200 | PARTIAL | 85% | [USED] Not exported; called in create_initiation (L310, L319) and process_ini... |
| `decrypt_and_hash` | L203–L207 | PARTIAL | 85% | [USED] Not exported; called in process_response (L375, L385, L413) and proces... |
| `compute_mac1` | L210–L216 | PARTIAL | 85% | [USED] Not exported; called in create_initiation (L326), process_response (L3... |
| `tai64n_now` | L221–L235 | PARTIAL | 80% | [USED] Not exported; called in create_initiation (L318) to generate TAI64N ti... |
| `INITIATION_SIZE` | L246–L246 | UNDOCUMENTED | 90% | [DEAD] Exported (pub(crate)) with 0 importers; used locally in create_initiat... |
| `RESPONSE_SIZE` | L247–L247 | UNDOCUMENTED | 90% | [DEAD] Exported (pub(crate)) with 0 importers; used locally in process_initia... |
| `InitiatorState` | L268–L275 | PARTIAL | 90% | [DEAD] Exported (pub(crate)) with 0 importers; used locally in create_initiat... |
| `create_initiation` | L281–L344 | PARTIAL | 90% | [DEAD] Exported (pub(crate)) with 0 importers per analysis; primary handshake... |
| `process_response` | L347–L422 | PARTIAL | 90% | [DEAD] Exported (pub(crate)) with 0 importers; core handshake function likely... |
| `process_initiation` | L431–L536 | PARTIAL | 90% | [DEAD] Exported (pub(crate)) with 0 importers; core handshake function likely... |

### `rustguard-core/src/handshake.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `mix_hash` | L32–L34 | PARTIAL | 78% | [USED] Non-exported helper function called 8 times across create_initiation_w... |
| `mix_key` | L38–L41 | PARTIAL | 88% | [USED] Non-exported helper function called 8 times across create_initiation_w... |
| `encrypt_and_hash` | L44–L48 | PARTIAL | 80% | [USED] Non-exported helper function called in create_initiation_with (L135, L... |
| `decrypt_and_hash` | L51–L55 | PARTIAL | 80% | [USED] Non-exported helper function called in process_response (L203) and pro... |
| `compute_mac1` | L59–L65 | PARTIAL | 88% | [USED] Exported function with 4 runtime importers across tests, daemon, and e... |
| `InitiatorHandshake` | L72–L81 | PARTIAL | 78% | [USED] Exported struct with 4 runtime importers; core state type for initiato... |
| `create_initiation` | L85–L91 | PARTIAL | 80% | [USED] Exported convenience function with 4 runtime importers; std-only wrapp... |
| `create_initiation_psk` | L95–L104 | PARTIAL | 83% | [USED] Exported convenience function with 4 runtime importers; std-only PSK v... |
| `create_initiation_with` | L108–L170 | PARTIAL | 80% | [USED] Exported core function with 4 runtime importers; implements Noise_IKps... |
| `process_response` | L173–L221 | PARTIAL | 80% | [USED] Exported function with 4 runtime importers; processes response msg2 an... |
| `process_initiation` | L227–L233 | PARTIAL | 82% | [USED] Exported convenience function with 4 runtime importers; std-only respo... |
| `process_initiation_psk` | L237–L246 | PARTIAL | 83% | [USED] Exported convenience function with 4 runtime importers; std-only respo... |
| `process_initiation_with` | L249–L357 | PARTIAL | 80% | [USED] Exported core function with 4 runtime importers; implements responder ... |
| `constant_time_eq` | L360–L363 | PARTIAL | 78% | [USED] Non-exported helper function called in process_initiation_with (L296) ... |

### `rustguard-core/src/cookie.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `COOKIE_SECRET_LIFETIME` | L21–L21 | PARTIAL | 75% | [USED] Used locally in two places: line 96 and line 216 for timeout compariso... |
| `COOKIE_LEN` | L24–L24 | PARTIAL | 92% | [DEAD] Exported constant but has 0 external importers per import analysis | [... |
| `CookieChecker` | L58–L67 | PARTIAL | 92% | [DEAD] Exported struct but has 0 external importers per import analysis | [UN... |
| `CookieState` | L70–L75 | PARTIAL | 92% | [DEAD] Exported struct but has 0 external importers per import analysis | [UN... |
| `encode_addr` | L236–L252 | PARTIAL | 70% | [USED] Used in 2 places: create_reply (line 128) and verify_mac2 (line 150) |... |

### `rustguard-daemon/src/tunnel.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `TunnelState` | L30–L35 | PARTIAL | 80% | [USED] struct created at L116 and locked/accessed throughout threads | [UNIQU... |
| `RouteEntry` | L41–L45 | PARTIAL | 80% | [USED] struct created at L109, stored in routes_added, iterated at L480 | [UN... |
| `run` | L48–L489 | PARTIAL | 90% | [DEAD] exported pub fn but pre-computed analysis shows 0 runtime importers | ... |
| `ctrlc_handler` | L504–L525 | PARTIAL | 80% | [USED] called at L118 to setup shutdown signal handler | [UNIQUE] Platform si... |

### `rustguard-core/src/messages.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `MSG_INITIATION` | L5–L5 | UNDOCUMENTED | 82% | [USED] Exported constant with 4 runtime importers across rustguard-core and r... |
| `MSG_RESPONSE` | L6–L6 | UNDOCUMENTED | 82% | [USED] Exported constant with 4 runtime importers across rustguard-core and r... |
| `MSG_COOKIE_REPLY` | L7–L7 | UNDOCUMENTED | 90% | [USED] Exported constant with 4 runtime importers across rustguard-core and r... |
| `MSG_TRANSPORT` | L8–L8 | UNDOCUMENTED | 87% | [USED] Exported constant with 4 runtime importers across rustguard-core and r... |
| `Initiation` | L22–L29 | PARTIAL | 85% | [USED] Exported struct with 4 runtime importers; used for WireGuard handshake... |
| `INITIATION_SIZE` | L31–L31 | UNDOCUMENTED | 78% | [USED] Exported constant with 4 runtime importers; defines 148-byte size for ... |
| `Response` | L45–L52 | PARTIAL | 85% | [USED] Exported struct with 4 runtime importers; used for WireGuard handshake... |
| `RESPONSE_SIZE` | L54–L54 | UNDOCUMENTED | 78% | [USED] Exported constant with 4 runtime importers; defines 92-byte size for R... |
| `Transport` | L64–L68 | PARTIAL | 85% | [USED] Exported struct with 4 runtime importers; used for WireGuard transport... |
| `CookieReply` | L79–L83 | PARTIAL | 88% | [USED] Exported struct with 4 runtime importers; used for WireGuard cookie re... |
| `COOKIE_REPLY_SIZE` | L85–L85 | UNDOCUMENTED | 90% | [USED] Exported constant with 4 runtime importers; defines 64-byte size for C... |
| `TRANSPORT_HEADER_SIZE` | L87–L87 | UNDOCUMENTED | 87% | [USED] Exported constant with 4 runtime importers; defines 16-byte header siz... |

### `rustguard-core/src/timers.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `REKEY_TIMEOUT` | L39–L39 | PARTIAL | 88% | [USED] Exported constant with 1 runtime importer (rustguard-daemon/src/peer.r... |

## Hygiene

- [ ] <!-- ACT-3c9a32-1 --> **[documentation · medium · trivial]** `rustguard-core/src/messages.rs`: Add `///` doc comment documentation for exported symbol:  `MSG_INITIATION`, `MSG_RESPONSE`, `MSG_COOKIE_REPLY`, `MSG_TRANSPORT`, `INITIATION_SIZE`, `RESPONSE_SIZE`, `COOKIE_REPLY_SIZE`, `TRANSPORT_HEADER_SIZE` (`MSG_INITIATION, MSG_RESPONSE, MSG_COOKIE_REPLY, MSG_TRANSPORT, INITIATION_SIZE, RESPONSE_SIZE, COOKIE_REPLY_SIZE, TRANSPORT_HEADER_SIZE`) [L5-L5, L6-L6, L7-L7, L8-L8, L31-L31, L54-L54, L85-L85, L87-L87]
- [ ] <!-- ACT-bd20cf-6 --> **[documentation · medium · trivial]** `rustguard-daemon/src/tunnel.rs`: Add `///` doc comment documentation for exported symbol: `run` (`run`) [L48-L489]
- [ ] <!-- ACT-a732f2-4 --> **[documentation · medium · trivial]** `rustguard-enroll/src/server.rs`: Add `///` doc comment documentation for exported symbol: `ServeConfig` (`ServeConfig`) [L49-L62]
- [ ] <!-- ACT-a732f2-5 --> **[documentation · medium · trivial]** `rustguard-enroll/src/server.rs`: Add `///` doc comment documentation for exported symbol: `run` (`run`) [L64-L532]
- [ ] <!-- ACT-c30836-3 --> **[documentation · medium · trivial]** `rustguard-kmod/src/replay.rs`: Add `///` doc comment documentation for exported symbol: `ReplayWindow` (`ReplayWindow`) [L11-L14]
- [ ] <!-- ACT-8fc69f-5 --> **[documentation · medium · trivial]** `rustguard-tun/src/xdp.rs`: Add `///` doc comment documentation for exported symbol: `XdpDesc` (`XdpDesc`) [L62-L66]
- [ ] <!-- ACT-8fc69f-6 --> **[documentation · medium · trivial]** `rustguard-tun/src/xdp.rs`: Add `///` doc comment documentation for exported symbol: `if_nametoindex` (`if_nametoindex`) [L449-L458]
- [ ] <!-- ACT-a4f709-4 --> **[documentation · low · trivial]** `rustguard-core/src/cookie.rs`: Complete `///` doc comment documentation for: `encode_addr` (`encode_addr`) [L236-L252]
- [ ] <!-- ACT-5920d4-2 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete `///` doc comment documentation for:  `initial_chain_key`, `mix_hash`, `mix_key`, `encrypt_and_hash`, `decrypt_and_hash`, `compute_mac1`, `InitiatorHandshake`, `create_initiation`, `create_initiation_psk`, `create_initiation_with`, `process_response`, `process_initiation`, `process_initiation_psk`, `process_initiation_with`, `constant_time_eq` (`initial_chain_key, mix_hash, mix_key, encrypt_and_hash, decrypt_and_hash, compute_mac1, InitiatorHandshake, create_initiation, create_initiation_psk, create_initiation_with, process_response, process_initiation, process_initiation_psk, process_initiation_with, constant_time_eq`) [L23-L25, L32-L34, L38-L41, L44-L48, L51-L55, L59-L65, L72-L81, L85-L91, L95-L104, L108-L170, L173-L221, L227-L233, L237-L246, L249-L357, L360-L363]
- [ ] <!-- ACT-3c9a32-2 --> **[documentation · low · trivial]** `rustguard-core/src/messages.rs`: Complete `///` doc comment documentation for:  `Initiation`, `Response`, `Transport`, `CookieReply` (`Initiation, Response, Transport, CookieReply`) [L22-L29, L45-L52, L64-L68, L79-L83]
- [ ] <!-- ACT-fb2f2d-8 --> **[documentation · low · trivial]** `rustguard-core/src/timers.rs`: Complete `///` doc comment documentation for: `REKEY_TIMEOUT` (`REKEY_TIMEOUT`) [L39-L39]
- [ ] <!-- ACT-bd20cf-4 --> **[documentation · low · trivial]** `rustguard-daemon/src/tunnel.rs`: Complete `///` doc comment documentation for:  `TunnelState`, `RouteEntry`, `ctrlc_handler` (`TunnelState, RouteEntry, ctrlc_handler`) [L30-L35, L41-L45, L504-L525]
- [ ] <!-- ACT-a732f2-9 --> **[documentation · low · trivial]** `rustguard-enroll/src/server.rs`: Complete `///` doc comment documentation for: `setup_xdp` (`setup_xdp`) [L548-L577]
- [ ] <!-- ACT-bb41a6-8 --> **[documentation · low · trivial]** `rustguard-kmod/src/lib.rs`: Complete `///` doc comment documentation for:  `AEAD_TAG_SIZE`, `rustguard_xmit`, `rustguard_rx`, `handle_initiation`, `handle_response`, `handle_transport`, `handle_cookie_reply`, `rustguard_dev_uninit`, `rustguard_timer_tick`, `initiate_rekey`, `send_keepalive`, `rustguard_genl_get`, `rustguard_genl_set` (`AEAD_TAG_SIZE, rustguard_xmit, rustguard_rx, handle_initiation, handle_response, handle_transport, handle_cookie_reply, rustguard_dev_uninit, rustguard_timer_tick, initiate_rekey, send_keepalive, rustguard_genl_get, rustguard_genl_set`) [L111-L111, L382-L384, L465-L467, L521-L620, L624-L671, L676-L755, L758-L781, L785-L785, L789-L791, L847-L877, L880-L914, L918-L922, L926-L932]
- [ ] <!-- ACT-58e5ff-7 --> **[documentation · low · trivial]** `rustguard-kmod/src/noise.rs`: Complete `///` doc comment documentation for:  `constant_time_eq`, `hash`, `mac`, `hkdf`, `seal`, `open`, `dh`, `generate_keypair`, `encrypt_and_hash`, `decrypt_and_hash`, `compute_mac1`, `tai64n_now` (`constant_time_eq, hash, mac, hkdf, seal, open, dh, generate_keypair, encrypt_and_hash, decrypt_and_hash, compute_mac1, tai64n_now`) [L53-L58, L72-L94, L97-L107, L110-L121, L124-L138, L141-L155, L158-L162, L165-L173, L196-L200, L203-L207, L210-L216, L221-L235]
- [ ] <!-- ACT-8fc69f-7 --> **[documentation · low · trivial]** `rustguard-tun/src/xdp.rs`: Complete `///` doc comment documentation for:  `Ring`, `XdpConfig`, `XdpSocket` (`Ring, XdpConfig, XdpSocket`) [L89-L95, L126-L132, L148-L160]

## Documentation Coverage

### `rustguard-kmod/src/noise.rs` — 62% covered

- [ ] **create_initiation** — PARTIAL → `rustguard-kmod/src/noise.rs`: Return value and DH-zero failure documented; parameters and examples absent.
- [ ] **process_response** — PARTIAL → `rustguard-kmod/src/noise.rs`: Only a single-sentence description; MAC1 verification, index check, and key derivation outcomes are not described.
- [ ] **process_initiation** — PARTIAL → `rustguard-kmod/src/noise.rs`: Return tuple named and replay-protection caveat noted; parameters undocumented and no examples.
- [ ] **Wire format constants (MSG_*, INITIATION_SIZE, RESPONSE_SIZE)** — PARTIAL → `rustguard-kmod/src/noise.rs`: MSG_INITIATION/MSG_RESPONSE/MSG_TRANSPORT are documented with type and size; INITIATION_SIZE and RESPONSE_SIZE have no doc comments.
- [ ] **Crypto helpers (hash, mac, hkdf, seal, open, dh)** — PARTIAL → `rustguard-kmod/src/noise.rs`: All have brief one-line `///` comments; parameter semantics and failure modes are not spelled out.
- [ ] **Noise state helpers (mix_hash, mix_key, initial_hash)** — MISSING → `rustguard-kmod/src/noise.rs`: mix_hash, mix_key, and initial_hash have no `///` doc comments at all.
