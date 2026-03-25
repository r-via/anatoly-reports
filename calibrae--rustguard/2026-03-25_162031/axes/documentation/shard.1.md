# Documentation — Shard 1

## Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `rustguard-core/src/replay.rs` | CRITICAL | 1 | 92% | [details](../reviews/rustguard-core-src-replay.rev.md) |
| `rustguard-core/src/handshake.rs` | NEEDS_REFACTOR | 15 | 82% | [details](../reviews/rustguard-core-src-handshake.rev.md) |
| `rustguard-core/src/messages.rs` | NEEDS_REFACTOR | 12 | 92% | [details](../reviews/rustguard-core-src-messages.rev.md) |
| `rustguard-core/src/timers.rs` | NEEDS_REFACTOR | 2 | 95% | [details](../reviews/rustguard-core-src-timers.rev.md) |
| `rustguard-core/src/cookie.rs` | NEEDS_REFACTOR | 4 | 92% | [details](../reviews/rustguard-core-src-cookie.rev.md) |

## Symbol Details

### `rustguard-core/src/replay.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `ReplayWindow` | L13–L19 | PARTIAL | 92% | [USED] Exported pub struct with 1 runtime importer (rustguard-core/src/sessio... |

### `rustguard-core/src/handshake.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `initial_chain_key` | L23–L25 | PARTIAL | 70% | [USED] Non-exported function called locally at lines 139 (create_initiation_w... |
| `mix_hash` | L32–L34 | PARTIAL | 70% | [USED] Non-exported function called at 8+ locations in create_initiation_with... |
| `mix_key` | L38–L41 | PARTIAL | 70% | [USED] Non-exported function called at 6+ locations in create_initiation_with... |
| `encrypt_and_hash` | L44–L48 | PARTIAL | 70% | [USED] Non-exported function called at lines 153, 156 (create_initiation_with... |
| `decrypt_and_hash` | L51–L55 | PARTIAL | 72% | [USED] Non-exported function called at lines 316, 327 (process_initiation_wit... |
| `compute_mac1` | L59–L65 | PARTIAL | 80% | [USED] Exported function with 4 runtime importers per pre-computed analysis (... |
| `InitiatorHandshake` | L72–L81 | PARTIAL | 75% | [USED] Exported struct with 4 runtime importers per pre-computed analysis. Co... |
| `create_initiation` | L85–L91 | PARTIAL | 82% | [USED] Exported function with 4 runtime importers per pre-computed analysis. ... |
| `create_initiation_psk` | L95–L104 | PARTIAL | 80% | [USED] Exported function with 4 runtime importers per pre-computed analysis. ... |
| `create_initiation_with` | L108–L170 | PARTIAL | 80% | [USED] Exported function with 4 runtime importers per pre-computed analysis. ... |
| `process_response` | L173–L221 | PARTIAL | 82% | [USED] Exported function with 4 runtime importers per pre-computed analysis. ... |
| `process_initiation` | L227–L233 | PARTIAL | 82% | [USED] Exported function with 4 runtime importers per pre-computed analysis. ... |
| `process_initiation_psk` | L237–L246 | PARTIAL | 80% | [USED] Exported function with 4 runtime importers per pre-computed analysis. ... |
| `process_initiation_with` | L249–L357 | PARTIAL | 82% | [USED] Exported function with 4 runtime importers per pre-computed analysis. ... |
| `constant_time_eq` | L360–L363 | PARTIAL | 72% | [USED] Non-exported function called at line 308 in process_initiation_with fo... |

### `rustguard-core/src/messages.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `MSG_INITIATION` | L5–L5 | UNDOCUMENTED | 85% | [USED] Exported constant imported by 3 runtime files (rustguard-core/src/cook... |
| `MSG_RESPONSE` | L6–L6 | UNDOCUMENTED | 85% | [USED] Exported constant imported by 3 runtime files (rustguard-core/src/cook... |
| `MSG_COOKIE_REPLY` | L7–L7 | UNDOCUMENTED | 85% | [USED] Exported constant imported by 3 runtime files (rustguard-core/src/cook... |
| `MSG_TRANSPORT` | L8–L8 | UNDOCUMENTED | 85% | [USED] Exported constant imported by 3 runtime files (rustguard-core/src/cook... |
| `Initiation` | L22–L29 | PARTIAL | 88% | [USED] Exported struct imported by 3 runtime files (rustguard-core/src/cookie... |
| `INITIATION_SIZE` | L31–L31 | UNDOCUMENTED | 88% | [USED] Exported constant imported by 3 runtime files (rustguard-core/src/cook... |
| `Response` | L45–L52 | PARTIAL | 88% | [USED] Exported struct imported by 3 runtime files (rustguard-core/src/cookie... |
| `RESPONSE_SIZE` | L54–L54 | UNDOCUMENTED | 88% | [USED] Exported constant imported by 3 runtime files (rustguard-core/src/cook... |
| `Transport` | L64–L68 | PARTIAL | 88% | [USED] Exported struct imported by 3 runtime files (rustguard-core/src/cookie... |
| `CookieReply` | L79–L83 | PARTIAL | 92% | [USED] Exported struct imported by 3 runtime files (rustguard-core/src/cookie... |
| `COOKIE_REPLY_SIZE` | L85–L85 | UNDOCUMENTED | 88% | [USED] Exported constant imported by 3 runtime files (rustguard-core/src/cook... |
| `TRANSPORT_HEADER_SIZE` | L87–L87 | UNDOCUMENTED | 88% | [USED] Exported constant imported by 3 runtime files (rustguard-core/src/cook... |

### `rustguard-core/src/timers.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `REKEY_TIMEOUT` | L39–L39 | PARTIAL | 55% | [USED] Exported constant with 1 runtime importer (rustguard-daemon/src/peer.r... |
| `REKEY_ATTEMPT_TIME` | L48–L48 | PARTIAL | 82% | [USED] Exported constant with 1 runtime importer (rustguard-daemon/src/peer.r... |

### `rustguard-core/src/cookie.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `COOKIE_LEN` | L24–L24 | PARTIAL | 90% | [USED] Public library crate export for cross-crate consumption; used locally ... |
| `CookieChecker` | L48–L57 | PARTIAL | 85% | [USED] Primary server-side public API in library crate; exports cookie genera... |
| `CookieState` | L60–L65 | PARTIAL | 88% | [USED] Primary client-side public API in library crate; exports cookie decryp... |
| `encode_addr` | L227–L243 | PARTIAL | 85% | [USED] Called at lines 138 and 153 to encode socket addresses for cookie MAC ... |

## Hygiene

- [ ] <!-- ACT-3c9a32-1 --> **[documentation · medium · trivial]** `rustguard-core/src/messages.rs`: Add doc comment documentation for exported symbol: `MSG_INITIATION` (`MSG_INITIATION`) [L5-L5]
- [ ] <!-- ACT-3c9a32-2 --> **[documentation · medium · trivial]** `rustguard-core/src/messages.rs`: Add doc comment documentation for exported symbol: `MSG_RESPONSE` (`MSG_RESPONSE`) [L6-L6]
- [ ] <!-- ACT-3c9a32-3 --> **[documentation · medium · trivial]** `rustguard-core/src/messages.rs`: Add doc comment documentation for exported symbol: `MSG_COOKIE_REPLY` (`MSG_COOKIE_REPLY`) [L7-L7]
- [ ] <!-- ACT-3c9a32-4 --> **[documentation · medium · trivial]** `rustguard-core/src/messages.rs`: Add doc comment documentation for exported symbol: `MSG_TRANSPORT` (`MSG_TRANSPORT`) [L8-L8]
- [ ] <!-- ACT-3c9a32-6 --> **[documentation · medium · trivial]** `rustguard-core/src/messages.rs`: Add doc comment documentation for exported symbol: `INITIATION_SIZE` (`INITIATION_SIZE`) [L31-L31]
- [ ] <!-- ACT-3c9a32-8 --> **[documentation · medium · trivial]** `rustguard-core/src/messages.rs`: Add doc comment documentation for exported symbol: `RESPONSE_SIZE` (`RESPONSE_SIZE`) [L54-L54]
- [ ] <!-- ACT-3c9a32-11 --> **[documentation · medium · trivial]** `rustguard-core/src/messages.rs`: Add doc comment documentation for exported symbol: `COOKIE_REPLY_SIZE` (`COOKIE_REPLY_SIZE`) [L85-L85]
- [ ] <!-- ACT-3c9a32-12 --> **[documentation · medium · trivial]** `rustguard-core/src/messages.rs`: Add doc comment documentation for exported symbol: `TRANSPORT_HEADER_SIZE` (`TRANSPORT_HEADER_SIZE`) [L87-L87]
- [ ] <!-- ACT-a4f709-2 --> **[documentation · low · trivial]** `rustguard-core/src/cookie.rs`: Complete doc comment documentation for: `COOKIE_LEN` (`COOKIE_LEN`) [L24-L24]
- [ ] <!-- ACT-a4f709-3 --> **[documentation · low · trivial]** `rustguard-core/src/cookie.rs`: Complete doc comment documentation for: `CookieChecker` (`CookieChecker`) [L48-L57]
- [ ] <!-- ACT-a4f709-4 --> **[documentation · low · trivial]** `rustguard-core/src/cookie.rs`: Complete doc comment documentation for: `CookieState` (`CookieState`) [L60-L65]
- [ ] <!-- ACT-a4f709-5 --> **[documentation · low · trivial]** `rustguard-core/src/cookie.rs`: Complete doc comment documentation for: `encode_addr` (`encode_addr`) [L227-L243]
- [ ] <!-- ACT-5920d4-1 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `initial_chain_key` (`initial_chain_key`) [L23-L25]
- [ ] <!-- ACT-5920d4-2 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `mix_hash` (`mix_hash`) [L32-L34]
- [ ] <!-- ACT-5920d4-3 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `mix_key` (`mix_key`) [L38-L41]
- [ ] <!-- ACT-5920d4-4 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `encrypt_and_hash` (`encrypt_and_hash`) [L44-L48]
- [ ] <!-- ACT-5920d4-5 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `decrypt_and_hash` (`decrypt_and_hash`) [L51-L55]
- [ ] <!-- ACT-5920d4-6 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `compute_mac1` (`compute_mac1`) [L59-L65]
- [ ] <!-- ACT-5920d4-7 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `InitiatorHandshake` (`InitiatorHandshake`) [L72-L81]
- [ ] <!-- ACT-5920d4-8 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `create_initiation` (`create_initiation`) [L85-L91]
- [ ] <!-- ACT-5920d4-9 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `create_initiation_psk` (`create_initiation_psk`) [L95-L104]
- [ ] <!-- ACT-5920d4-10 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `create_initiation_with` (`create_initiation_with`) [L108-L170]
- [ ] <!-- ACT-5920d4-11 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `process_response` (`process_response`) [L173-L221]
- [ ] <!-- ACT-5920d4-12 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `process_initiation` (`process_initiation`) [L227-L233]
- [ ] <!-- ACT-5920d4-13 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `process_initiation_psk` (`process_initiation_psk`) [L237-L246]
- [ ] <!-- ACT-5920d4-14 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `process_initiation_with` (`process_initiation_with`) [L249-L357]
- [ ] <!-- ACT-5920d4-15 --> **[documentation · low · trivial]** `rustguard-core/src/handshake.rs`: Complete doc comment documentation for: `constant_time_eq` (`constant_time_eq`) [L360-L363]
- [ ] <!-- ACT-3c9a32-5 --> **[documentation · low · trivial]** `rustguard-core/src/messages.rs`: Complete doc comment documentation for: `Initiation` (`Initiation`) [L22-L29]
- [ ] <!-- ACT-3c9a32-7 --> **[documentation · low · trivial]** `rustguard-core/src/messages.rs`: Complete doc comment documentation for: `Response` (`Response`) [L45-L52]
- [ ] <!-- ACT-3c9a32-9 --> **[documentation · low · trivial]** `rustguard-core/src/messages.rs`: Complete doc comment documentation for: `Transport` (`Transport`) [L64-L68]
- [ ] <!-- ACT-3c9a32-10 --> **[documentation · low · trivial]** `rustguard-core/src/messages.rs`: Complete doc comment documentation for: `CookieReply` (`CookieReply`) [L79-L83]
- [ ] <!-- ACT-f55ce3-2 --> **[documentation · low · trivial]** `rustguard-core/src/replay.rs`: Complete doc comment documentation for: `ReplayWindow` (`ReplayWindow`) [L13-L19]
- [ ] <!-- ACT-fb2f2d-5 --> **[documentation · low · trivial]** `rustguard-core/src/timers.rs`: Complete doc comment documentation for: `REKEY_TIMEOUT` (`REKEY_TIMEOUT`) [L39-L39]
- [ ] <!-- ACT-fb2f2d-6 --> **[documentation · low · trivial]** `rustguard-core/src/timers.rs`: Complete doc comment documentation for: `REKEY_ATTEMPT_TIME` (`REKEY_ATTEMPT_TIME`) [L48-L48]

## Documentation Coverage

### `rustguard-core/src/replay.rs` — 72% covered

- [ ] **WINDOW_SIZE** — MISSING → `docs/04-API-Reference/03-Types-and-Interfaces.md`: Private constant; absence from external documentation is expected and acceptable. The module-level `//!` block mentions '2048-bit bitmap', indirectly communicating the value to any reader of the source. No action required.
- [ ] **BITMAP_LEN** — MISSING → `docs/04-API-Reference/03-Types-and-Interfaces.md`: Private implementation-detail constant; not expected in user-facing documentation. Absence is appropriate for an internal bitmap sizing constant.

### `rustguard-core/src/handshake.rs` — 45% covered

- [ ] **Noise_IKpsk2 handshake protocol** — PARTIAL → `02-Architecture/02-Core-Concepts.md`: The module-level //! comment provides a concise description of the 1-RTT exchange and the role of ck/h. External docs likely cover this in Core-Concepts but inline coverage lacks a full message sequence diagram or reference to RFC 7539 / Noise specification.
- [ ] **InitiatorHandshake** — PARTIAL → `04-API-Reference/01-Public-API.md`: The struct has a brief struct-level doc. API reference likely lists it but the opaque-state usage pattern (create → process_response) is not explained in a cohesive narrative either inline or in the docs directory.
- [ ] **compute_mac1** — PARTIAL → `04-API-Reference/01-Public-API.md`: Inline doc includes the formula but the 16-byte truncation and its security implications are undocumented. No walkthrough example in docs.
- [ ] **create_initiation / process_response flow** — MISSING → `03-Guides/01-Common-Workflows.md`: No guide documents the initiator handshake lifecycle: create_initiation → send → receive response → process_response → TransportSession. This is the primary consumer workflow and has no narrative documentation.
- [ ] **Anti-replay timestamp protection** — MISSING → `04-API-Reference/01-Public-API.md`: The last_timestamp parameter in process_initiation_psk and process_initiation_with is a security-critical anti-replay mechanism. It is not documented inline beyond a single inline comment and appears nowhere in the docs directory structure.

### `rustguard-core/src/messages.rs` — 20% covered

- [ ] **Initiation** — PARTIAL → `04-API-Reference/03-Types-and-Interfaces.md`: Thorough inline wire-format doc exists. External API reference directory is present and likely covers this type, but file content is unverified. No examples or field-level prose in inline docs.
- [ ] **Response** — PARTIAL → `04-API-Reference/03-Types-and-Interfaces.md`: Same coverage pattern as Initiation. Inline struct-level doc is solid; external coverage inferred from directory structure but unconfirmed.
- [ ] **Transport** — PARTIAL → `04-API-Reference/03-Types-and-Interfaces.md`: Wire format documented inline. External types reference file is plausible but content unverified. Field-level and usage docs absent.
- [ ] **CookieReply** — PARTIAL → `04-API-Reference/03-Types-and-Interfaces.md`: Wire format documented inline including XChaCha20-Poly1305 algorithm detail. External coverage inferred; file content not readable.
- [ ] **MSG_INITIATION** — MISSING → `04-API-Reference/01-Public-API.md`: Protocol wire constants have no inline docs and no dedicated constants reference page is evident in the directory listing.
- [ ] **MSG_RESPONSE** — MISSING → `04-API-Reference/01-Public-API.md`: No inline docs. Likely absent from external reference for the same reasons as MSG_INITIATION.
- [ ] **MSG_COOKIE_REPLY** — MISSING → `04-API-Reference/01-Public-API.md`: No inline docs. No dedicated constants page visible in directory listing.
- [ ] **MSG_TRANSPORT** — MISSING → `04-API-Reference/01-Public-API.md`: No inline docs in this file. No confirmed external documentation.

### `rustguard-core/src/timers.rs` — 75% covered

- [ ] **WireGuard timer constants** — PARTIAL → `04-API-Reference/01-Public-API.md`: All public constants carry /// inline docs referencing the WireGuard whitepaper section 6 via the module-level //! block. Two constants (REKEY_TIMEOUT, REKEY_ATTEMPT_TIME) have inaccurate descriptions. External API reference doc path is plausible from directory listing but content is not accessible to confirm coverage.
- [ ] **SessionTimers** — PARTIAL → `04-API-Reference/03-Types-and-Interfaces.md`: Type is well-documented inline with struct-level and field-level /// comments. Likely candidate for the Types-and-Interfaces reference doc but content is inaccessible; coverage cannot be confirmed.
- [ ] **Session lifecycle and rekeying flow** — MISSING → `02-Architecture/02-Core-Concepts.md`: The module explains individual constants but provides no prose describing the state-machine relationships between REKEY_AFTER_TIME, REKEY_TIMEOUT, REKEY_ATTEMPT_TIME, REJECT_AFTER_TIME, and DEAD_SESSION_TIMEOUT. An architecture doc describing how these interact during a peer session lifecycle is absent from the inline docs and cannot be confirmed in the external docs.

### `rustguard-core/src/cookie.rs` — 62% covered

- [ ] **CookieChecker** — PARTIAL → `docs/04-API-Reference/01-Public-API.md`: Primary server-side public API struct. Inline /// coverage is partial; external API reference docs directory exists but content was not available to confirm explicit coverage.
- [ ] **CookieState** — PARTIAL → `docs/04-API-Reference/03-Types-and-Interfaces.md`: Primary client-side public type. Struct-level and field-level docs present inline; external reference docs directory exists but content unverified.
- [ ] **COOKIE_LEN** — PARTIAL → `docs/04-API-Reference/01-Public-API.md`: Public constant with minimal inline doc ('Cookie size.'); no WireGuard spec citation or derivation note in inline or external docs.
