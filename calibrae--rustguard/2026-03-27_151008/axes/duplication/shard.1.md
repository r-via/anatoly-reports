[← Back to Duplication](./index.md) · [← Back to report](../../public_report.md)

# 📋 Duplication — Shard 1

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [🔧 Refactors](#-refactors)

## 📊 Findings

| File | Verdict | Duplication | Conf. | Details |
|------|---------|-------------|-------|---------|
| `rustguard-enroll/src/server.rs` | 🔴 CRITICAL | 2 | 92% | [details](#rustguard-enrollsrcserverrs) |
| `rustguard-kmod/src/noise.rs` | 🟡 NEEDS_REFACTOR | 2 | 92% | [details](#rustguard-kmodsrcnoisers) |
| `rustguard-core/src/cookie.rs` | 🟡 NEEDS_REFACTOR | 2 | 92% | [details](#rustguard-coresrccookiers) |
| `rustguard-daemon/src/tunnel.rs` | 🟡 NEEDS_REFACTOR | 2 | 90% | [details](#rustguard-daemonsrctunnelrs) |
| `rustguard-tun/src/linux.rs` | 🟡 NEEDS_REFACTOR | 4 | 90% | [details](#rustguard-tunsrclinuxrs) |
| `rustguard-tun/src/macos.rs` | 🟡 NEEDS_REFACTOR | 1 | 90% | [details](#rustguard-tunsrcmacosrs) |
| `rustguard-kmod/src/cookie.rs` | 🟡 NEEDS_REFACTOR | 3 | 92% | [details](#rustguard-kmodsrccookiers) |
| `rustguard-tun/src/linux_mq.rs` | 🟡 NEEDS_REFACTOR | 4 | 90% | [details](#rustguard-tunsrclinuxmqrs) |
| `rustguard-enroll/src/client.rs` | 🟡 NEEDS_REFACTOR | 2 | 88% | [details](#rustguard-enrollsrcclientrs) |

## 🔍 Symbol Details

### `rustguard-enroll/src/server.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `rand_index` | L534–L538 |  DUPLICATE | 70% | Identical implementation found in rustguard-enroll/src/client.rs (score 0.965). Both use getrandom to generate 4 random bytes and convert to u32 via from_le_bytes. |
| `base64_key` | L541–L544 |  DUPLICATE | 70% | Identical implementation found in rustguard-enroll/src/client.rs (score 0.992) and rustguard-daemon/src/tunnel.rs (score 0.993). Both use base64::BASE64_STANDARD.encode() to encode 32-byte keys. |

### `rustguard-kmod/src/noise.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `constant_time_eq` | L53–L58 |  DUPLICATE | 85% | Identical signature and implementation to constant_time_eq in cookie.rs (score 0.962); both wrap wg_crypto_memneq with same logic |
| `mac` | L97–L107 |  DUPLICATE | 85% | Identical implementation to mac in cookie.rs (score 0.974); same FFI call to wg_blake2s_256_mac with identical parameter order and return handling |

### `rustguard-core/src/cookie.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `elapsed_since` | L38–L40 |  DUPLICATE | 70% | no_std stub returning Duration::ZERO. Identical logic and purpose as timers.rs variant. |
| `elapsed_since` | L51–L55 |  DUPLICATE | 70% | no_std stub returning Duration::ZERO. Identical logic and purpose as timers.rs variant. |

### `rustguard-daemon/src/tunnel.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `rand_index` | L492–L496 |  DUPLICATE | 70% | Generates random u32 index via getrandom. RAG score 0.960 shows nearly identical implementation in server.rs—both fill buffer and convert to u32 identically. |
| `base64_key` | L529–L532 |  DUPLICATE | 70% | Encodes 32-byte key to base64 string. RAG score 0.994 with identical logic in client.rs—both use BASE64_STANDARD.encode on [u8; 32]. |

### `rustguard-tun/src/linux.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `close_and_error` | L55–L59 |  DUPLICATE | 85% | Identical 5-line helper found in linux_mq.rs with score 0.987 |
| `make_sockaddr_in` | L61–L70 |  DUPLICATE | 85% | Identical 10-line function in linux_mq.rs with score 0.973 |
| `set_name` | L72–L76 |  DUPLICATE | 85% | Identical 5-line function in linux_mq.rs with score 0.968 |
| `configure_interface` | L124–L189 |  DUPLICATE | 85% | RAG score 0.954 with linux_mq.rs variant; both apply identical ioctl sequence (SIOCSIFADDR, SIOCSIFDSTADDR, SIOCSIFNETMASK, SIOCSIFMTU, SIOCGIFFLAGS, SIOCSIFFLAGS) |

### `rustguard-tun/src/macos.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `close_and_error` | L79–L83 |  DUPLICATE | 75% | Identical implementation found in linux.rs and linux_mq.rs with scores 0.977 and 0.979. Captures errno, closes fd, returns error—same logic and semantic contract across all three platform modules. |

### `rustguard-kmod/src/cookie.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `LABEL_MAC1` | L17–L17 |  DUPLICATE | 75% | Identical protocol constant also defined in noise.rs; same value b"mac1----" |
| `mac` | L53–L57 |  DUPLICATE | 75% | RAG score 0.974 with noise.rs mac — identical signature and implementation. Both compute BLAKE2s-256 MAC by calling wg_blake2s_256_mac with same parameter passing and return value extraction. |
| `constant_time_eq` | L202–L205 |  DUPLICATE | 80% | RAG score 0.962 with noise.rs constant_time_eq — identical implementation. Both check length equality then use wg_crypto_memneq for constant-time comparison, with same parameter passing and return logic. |

### `rustguard-tun/src/linux_mq.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `set_name` | L53–L57 |  DUPLICATE | 88% | Identical to linux.rs version - copies string bytes to fixed buffer with length constraint |
| `make_sockaddr_in` | L59–L68 |  DUPLICATE | 88% | Identical to linux.rs version - creates sockaddr_in struct with identical field initialization |
| `close_and_error` | L70–L74 |  DUPLICATE | 88% | Identical to linux.rs version - captures OS error, closes fd, returns error in same sequence |
| `configure_interface` | L206–L260 |  DUPLICATE | 85% | Very similar to linux.rs version (RAG score 0.954) - identical configuration sequence for address, destination, netmask, MTU, and flags via ioctl calls |

### `rustguard-enroll/src/client.rs`

| Symbol | Lines | Duplication | Conf. | Detail |
|--------|-------|-------------|-------|--------|
| `rand_index` | L194–L198 |  DUPLICATE | 85% | Identical implementation — both generate random u32 via getrandom and le_bytes conversion. Score 0.965 with same-crate target indicates textbook trivial helper duplication. |
| `base64_key` | L200–L203 |  DUPLICATE | 85% | Identical implementation — both encode 32-byte key to base64 string using BASE64_STANDARD. Score 0.994 indicates nearly perfect structural match. Trivial 3-line helper appears identically across multiple files. |

## 🔧 Refactors

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
