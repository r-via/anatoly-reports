[← Back to Duplication](./index.md) · [← Back to report](../public_report.md)

# Duplication — Shard 1

## Findings

| File | Verdict | Duplication | Conf. | Details |
|------|---------|-------------|-------|---------|
| `rustguard-enroll/src/server.rs` | CRITICAL | 2 | 92% | [details](../reviews/rustguard-enroll-src-server.rev.md) |
| `rustguard-kmod/src/noise.rs` | NEEDS_REFACTOR | 2 | 92% | [details](../reviews/rustguard-kmod-src-noise.rev.md) |
| `rustguard-core/src/cookie.rs` | NEEDS_REFACTOR | 2 | 92% | [details](../reviews/rustguard-core-src-cookie.rev.md) |
| `rustguard-daemon/src/tunnel.rs` | NEEDS_REFACTOR | 2 | 90% | [details](../reviews/rustguard-daemon-src-tunnel.rev.md) |
| `rustguard-tun/src/linux.rs` | NEEDS_REFACTOR | 4 | 90% | [details](../reviews/rustguard-tun-src-linux.rev.md) |
| `rustguard-tun/src/macos.rs` | NEEDS_REFACTOR | 1 | 90% | [details](../reviews/rustguard-tun-src-macos.rev.md) |
| `rustguard-kmod/src/cookie.rs` | NEEDS_REFACTOR | 3 | 92% | [details](../reviews/rustguard-kmod-src-cookie.rev.md) |
| `rustguard-tun/src/linux_mq.rs` | NEEDS_REFACTOR | 4 | 90% | [details](../reviews/rustguard-tun-src-linux_mq.rev.md) |
| `rustguard-enroll/src/client.rs` | NEEDS_REFACTOR | 2 | 88% | [details](../reviews/rustguard-enroll-src-client.rev.md) |

## Symbol Details

### `rustguard-enroll/src/server.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `rand_index` | L534–L538 | DUPLICATE | 70% | [USED] Non-exported helper; called once at L347 during WireGuard initiation h... |
| `base64_key` | L541–L544 | DUPLICATE | 70% | [USED] Non-exported helper wrapper around base64::encode; called at L267 (enr... |

### `rustguard-kmod/src/noise.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `constant_time_eq` | L53–L58 | DUPLICATE | 85% | [USED] Not exported; called in process_response (L360) and process_initiation... |
| `mac` | L97–L107 | DUPLICATE | 85% | [USED] Not exported; called in compute_mac1() at L215 for MAC1 computation. |... |

### `rustguard-core/src/cookie.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `elapsed_since` | L38–L40 | DUPLICATE | 70% | [USED] no_std stub version called in 2 places: maybe_rotate_secret and is_fre... |
| `elapsed_since` | L51–L55 | DUPLICATE | 70% | [USED] no_std stub version called in 2 places: maybe_rotate_secret and is_fre... |

### `rustguard-daemon/src/tunnel.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `rand_index` | L492–L496 | DUPLICATE | 70% | [USED] called at L154 and L424 to generate random sender indices | [DUPLICATE... |
| `base64_key` | L529–L532 | DUPLICATE | 70% | [USED] called at L96, L309, L356, L430 to format public keys | [DUPLICATE] En... |

### `rustguard-tun/src/linux.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `close_and_error` | L55–L59 | DUPLICATE | 85% | [USED] Helper function to close fd and return error; used throughout create()... |
| `make_sockaddr_in` | L61–L70 | DUPLICATE | 85% | [USED] Helper function called 3 times in configure_interface() to construct s... |
| `set_name` | L72–L76 | DUPLICATE | 85% | [USED] Helper function used in create() and configure_interface() to copy int... |
| `configure_interface` | L124–L189 | DUPLICATE | 85% | [USED] Internal helper function called once from create() at line 113 for int... |

### `rustguard-tun/src/macos.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `close_and_error` | L79–L83 | DUPLICATE | 75% | [USED] Called 4 times to safely preserve OS error before closing file descrip... |

### `rustguard-kmod/src/cookie.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `LABEL_MAC1` | L17–L17 | DUPLICATE | 75% | [USED] Label constant used in hash computation for MAC1 key generation in ver... |
| `mac` | L53–L57 | DUPLICATE | 75% | [USED] Internal helper for BLAKE2s MAC computation, called in verify_mac1 (10... |
| `constant_time_eq` | L202–L205 | DUPLICATE | 80% | [USED] Internal constant-time comparison wrapper using kernel crypto_memneq, ... |

### `rustguard-tun/src/linux_mq.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `set_name` | L53–L57 | DUPLICATE | 88% | [USED] Called at L100, L122, L233, L242, L257 to copy interface name into ifr... |
| `make_sockaddr_in` | L59–L68 | DUPLICATE | 88% | [USED] Called at L232, L239, L244 to construct sockaddr_in for address, desti... |
| `close_and_error` | L70–L74 | DUPLICATE | 88% | [USED] Called at L106, L128, L237, L242, L247, L261, L266 to close file descr... |
| `configure_interface` | L206–L260 | DUPLICATE | 85% | [USED] Called at L136 within create() to set IP address, netmask, MTU, and br... |

### `rustguard-enroll/src/client.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `rand_index` | L194–L198 | DUPLICATE | 85% | [USED] Internal function called locally at line 110 within run() to generate ... |
| `base64_key` | L200–L203 | DUPLICATE | 85% | [USED] Internal function called locally at line 69 to display the server publ... |

## Refactors

- [ ] <!-- ACT-a4f709-8 --> **[duplication · medium · small]** `rustguard-core/src/cookie.rs`: Deduplicate: `elapsed_since` duplicates `elapsed_since` in `rustguard-core/src/timers.rs` (`elapsed_since`) [L51-L55]
- [ ] <!-- ACT-bd20cf-7 --> **[duplication · medium · small]** `rustguard-daemon/src/tunnel.rs`: Deduplicate: `rand_index` duplicates `rand_index` in `rustguard-enroll/src/server.rs` (`rand_index`) [L492-L496]
- [ ] <!-- ACT-bd20cf-9 --> **[duplication · medium · small]** `rustguard-daemon/src/tunnel.rs`: Deduplicate: `base64_key` duplicates `base64_key` in `rustguard-enroll/src/client.rs` (`base64_key`) [L529-L532]
- [ ] <!-- ACT-ca1d92-4 --> **[duplication · medium · small]** `rustguard-enroll/src/client.rs`: Deduplicate: `rand_index` duplicates `rand_index` in `rustguard-enroll/src/server.rs` (`rand_index`) [L194-L198]
- [ ] <!-- ACT-ca1d92-5 --> **[duplication · medium · small]** `rustguard-enroll/src/client.rs`: Deduplicate: `base64_key` duplicates `base64_key` in `rustguard-daemon/src/tunnel.rs` (`base64_key`) [L200-L203]
- [ ] <!-- ACT-a732f2-7 --> **[duplication · medium · small]** `rustguard-enroll/src/server.rs`: Deduplicate: `rand_index` duplicates `rand_index` in `rustguard-enroll/src/client.rs` (`rand_index`) [L534-L538]
- [ ] <!-- ACT-a732f2-8 --> **[duplication · medium · small]** `rustguard-enroll/src/server.rs`: Deduplicate: `base64_key` duplicates `base64_key` in `rustguard-enroll/src/client.rs` (`base64_key`) [L541-L544]
- [ ] <!-- ACT-9105c7-3 --> **[duplication · medium · small]** `rustguard-kmod/src/cookie.rs`: Deduplicate: `LABEL_MAC1` duplicates `LABEL_MAC1` in `rustguard-kmod/src/noise.rs` (`LABEL_MAC1`) [L17-L17]
- [ ] <!-- ACT-9105c7-4 --> **[duplication · medium · small]** `rustguard-kmod/src/cookie.rs`: Deduplicate: `mac` duplicates `mac` in `rustguard-kmod/src/noise.rs` (`mac`) [L53-L57]
- [ ] <!-- ACT-9105c7-6 --> **[duplication · medium · small]** `rustguard-kmod/src/cookie.rs`: Deduplicate: `constant_time_eq` duplicates `constant_time_eq` in `rustguard-kmod/src/noise.rs` (`constant_time_eq`) [L202-L205]
- [ ] <!-- ACT-58e5ff-6 --> **[duplication · medium · small]** `rustguard-kmod/src/noise.rs`: Deduplicate: `constant_time_eq` duplicates `constant_time_eq` in `rustguard-kmod/src/cookie.rs` (`constant_time_eq`) [L53-L58]
- [ ] <!-- ACT-58e5ff-8 --> **[duplication · medium · small]** `rustguard-kmod/src/noise.rs`: Deduplicate: `mac` duplicates `mac` in `rustguard-kmod/src/cookie.rs` (`mac`) [L97-L107]
- [ ] <!-- ACT-22946f-3 --> **[duplication · medium · small]** `rustguard-tun/src/linux_mq.rs`: Deduplicate: `set_name` duplicates `set_name` in `rustguard-tun/src/linux.rs` (`set_name`) [L53-L57]
- [ ] <!-- ACT-22946f-4 --> **[duplication · medium · small]** `rustguard-tun/src/linux_mq.rs`: Deduplicate: `make_sockaddr_in` duplicates `make_sockaddr_in` in `rustguard-tun/src/linux.rs` (`make_sockaddr_in`) [L59-L68]
- [ ] <!-- ACT-22946f-5 --> **[duplication · medium · small]** `rustguard-tun/src/linux_mq.rs`: Deduplicate: `close_and_error` duplicates `close_and_error` in `rustguard-tun/src/linux.rs` (`close_and_error`) [L70-L74]
- [ ] <!-- ACT-22946f-8 --> **[duplication · medium · small]** `rustguard-tun/src/linux_mq.rs`: Deduplicate: `configure_interface` duplicates `configure_interface` in `rustguard-tun/src/linux.rs` (`configure_interface`) [L206-L260]
- [ ] <!-- ACT-c62ee4-3 --> **[duplication · medium · small]** `rustguard-tun/src/linux.rs`: Deduplicate: `close_and_error` duplicates `close_and_error` in `rustguard-tun/src/linux_mq.rs` (`close_and_error`) [L55-L59]
- [ ] <!-- ACT-c62ee4-4 --> **[duplication · medium · small]** `rustguard-tun/src/linux.rs`: Deduplicate: `make_sockaddr_in` duplicates `make_sockaddr_in` in `rustguard-tun/src/linux_mq.rs` (`make_sockaddr_in`) [L61-L70]
- [ ] <!-- ACT-c62ee4-5 --> **[duplication · medium · small]** `rustguard-tun/src/linux.rs`: Deduplicate: `set_name` duplicates `set_name` in `rustguard-tun/src/linux_mq.rs` (`set_name`) [L72-L76]
- [ ] <!-- ACT-c62ee4-8 --> **[duplication · medium · small]** `rustguard-tun/src/linux.rs`: Deduplicate: `configure_interface` duplicates `configure_interface` in `rustguard-tun/src/linux_mq.rs` (`configure_interface`) [L124-L189]
- [ ] <!-- ACT-b59607-4 --> **[duplication · medium · small]** `rustguard-tun/src/macos.rs`: Deduplicate: `close_and_error` duplicates `close_and_error` in `rustguard-tun/src/linux.rs` (`close_and_error`) [L79-L83]
