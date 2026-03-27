[← Back to Correction](./index.md) · [← Back to report](../public_report.md)

# Correction — Shard 2

## Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `rustguard-cli/src/main.rs` | NEEDS_REFACTOR | 2 | 90% | [details](../reviews/rustguard-cli-src-main.rev.md) |
| `rustguard-daemon/src/config.rs` | NEEDS_REFACTOR | 3 | 95% | [details](../reviews/rustguard-daemon-src-config.rev.md) |
| `rustguard-tun/src/linux.rs` | NEEDS_REFACTOR | 2 | 90% | [details](../reviews/rustguard-tun-src-linux.rev.md) |
| `rustguard-tun/src/bpf_loader.rs` | NEEDS_REFACTOR | 3 | 88% | [details](../reviews/rustguard-tun-src-bpf_loader.rev.md) |
| `rustguard-tun/src/macos.rs` | NEEDS_REFACTOR | 3 | 90% | [details](../reviews/rustguard-tun-src-macos.rev.md) |
| `rustguard-kmod/src/cookie.rs` | NEEDS_REFACTOR | 1 | 92% | [details](../reviews/rustguard-kmod-src-cookie.rev.md) |
| `rustguard-tun/src/linux_mq.rs` | NEEDS_REFACTOR | 2 | 90% | [details](../reviews/rustguard-tun-src-linux_mq.rev.md) |
| `rustguard-enroll/src/client.rs` | NEEDS_REFACTOR | 1 | 88% | [details](../reviews/rustguard-enroll-src-client.rev.md) |
| `rustguard-enroll/src/packet.rs` | NEEDS_REFACTOR | 1 | 88% | [details](../reviews/rustguard-enroll-src-packet.rev.md) |
| `rustguard-tun/examples/tun_echo.rs` | NEEDS_REFACTOR | 1 | 85% | [details](../reviews/rustguard-tun-examples-tun_echo.rev.md) |

## Symbol Details

### `rustguard-cli/src/main.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `cmd_serve` | L68–L161 | NEEDS_FIX | 90% | [USED] Called in main() at line 11 via match arm for 'serve' command. Handles... |
| `cmd_join` | L163–L211 | NEEDS_FIX | 90% | [USED] Called in main() at line 12 via match arm for 'join' command. Handles ... |

### `rustguard-daemon/src/config.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `Config` | L13–L16 | NEEDS_FIX | 72% | [USED] Main exported struct holding parsed WireGuard configuration. Runtime-i... |
| `parse_interface` | L176–L226 | NEEDS_FIX | 88% | [USED] Non-exported function called in Config::parse (line 163). Parses [Inte... |
| `parse_cidr` | L271–L282 | NEEDS_FIX | 75% | [USED] Non-exported function called in parse_peer (line 259). Parses CIDR not... |

### `rustguard-tun/src/linux.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `IfreqAddr` | L39–L42 | NEEDS_FIX | 88% | [USED] Struct for kernel ifreq with sockaddr_in; used in configure_interface(... |
| `configure_interface` | L124–L189 | NEEDS_FIX | 85% | [USED] Internal helper function called once from create() at line 113 for int... |

### `rustguard-tun/src/bpf_loader.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `XdpProgram` | L37–L41 | NEEDS_FIX | 88% | [DEAD] Exported public struct with 0 runtime importers and 0 type-only import... |
| `attach_xdp_netlink` | L235–L342 | NEEDS_FIX | 80% | [USED] Called from attach_xdp() on line 231 and from detach_xdp() on line 346... |
| `parse_and_patch_elf` | L361–L470 | NEEDS_FIX | 75% | [USED] Function called from XdpProgram::load_and_attach() on line 57. | [UNIQ... |

### `rustguard-tun/src/macos.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `IfreqMtu` | L65–L68 | NEEDS_FIX | 72% | [USED] Instantiated in set_mtu() with MTU value passed to ioctl() command (L2... |
| `configure_address` | L194–L218 | NEEDS_FIX | 88% | [USED] Called from create() to configure interface IP address via ioctl (L180... |
| `set_mtu` | L220–L242 | NEEDS_FIX | 88% | [USED] Called from create() to set interface MTU via ioctl (L186) | [UNIQUE] ... |

### `rustguard-kmod/src/cookie.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `hash` | L41–L51 | NEEDS_FIX | 92% | [USED] Internal helper function called in verify_mac1 (line 101), create_repl... |

### `rustguard-tun/src/linux_mq.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `IfreqAddr` | L41–L44 | NEEDS_FIX | 85% | [USED] Instantiated at L230 and reused at L239, L244 for setting address, des... |
| `MultiQueueTun` | L77–L81 | NEEDS_FIX | 90% | [DEAD] Exported public struct with 0 workspace importers per analysis, sugges... |

### `rustguard-enroll/src/client.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `add_route` | L205–L226 | NEEDS_FIX | 88% | [USED] Internal function called locally at line 83 to configure OS routing fo... |

### `rustguard-enroll/src/packet.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `parse_ipv4_udp` | L30–L52 | NEEDS_FIX | 85% | [USED] Non-exported function called from parse_eth_udp (line 24) within match... |

### `rustguard-tun/examples/tun_echo.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `main` | L12–L79 | NEEDS_FIX | 78% | [USED] Binary entry point that orchestrates TUN interface creation, packet re... |

## Quick Wins

- [ ] <!-- ACT-8bab2a-1 --> **[correction · medium · small]** `rustguard-cli/src/main.rs`: In cmd_serve, --token/--pool/--xdp parsers silently accept the next flag name as an option value because only empty strings are rejected. Add a check that the consumed value does not start with '--', or replace the filter with an explicit missing-value guard, to prevent `--token --open` from setting token="--open" and dropping the --open flag. [L89]
- [ ] <!-- ACT-8bab2a-2 --> **[correction · medium · small]** `rustguard-cli/src/main.rs`: In cmd_join, the --token parser has the same flag-as-value bug: `--token --someFlag` silently sets token to "--someFlag" and discards the subsequent flag, leading to a runtime auth failure instead of a parse error. [L172]
- [ ] <!-- ACT-dbf131-1 --> **[correction · medium · small]** `rustguard-daemon/src/config.rs`: In parse_interface Address parsing (L195), replace the hardcoded fallback '24' with an address-family-aware default: determine the family by attempting Ipv4Addr parse first, then choose '32' (or the intended IPv4 subnet default) for v4 and '128' for v6 — matching the approach already used in parse_cidr at L277. [L195]
- [ ] <!-- ACT-ca1d92-1 --> **[correction · medium · small]** `rustguard-enroll/src/client.rs`: Add a `#[cfg(not(any(target_os = "macos", target_os = "linux")))]` branch that either returns early with an `eprintln!` or assigns a stub value so that `result` is always defined before the unconditional `match result` block, preventing a compile error on unsupported platforms. [L216]
- [ ] <!-- ACT-4cf3f3-1 --> **[correction · medium · small]** `rustguard-enroll/src/packet.rs`: Add a minimum IHL validation check (`if ihl < 20 { return None; }`) immediately after computing `ihl` on line 35. Without it, malformed packets with an IHL nibble of 0–4 pass both length guards but cause `src_port` to be read from inside the IP header rather than from the UDP source-port field, and the returned payload will reference the wrong byte range. [L35]
- [ ] <!-- ACT-9105c7-1 --> **[correction · medium · small]** `rustguard-kmod/src/cookie.rs`: In `hash`, replace `chunks.len() as u32` with `chunks.len().min(4) as u32` (equivalently `ptrs.len().min(chunks.len()) as u32`). Passing the unclamped length instructs the C function to access indices beyond the 4-element `ptrs`/`lens` stack arrays, causing an out-of-bounds read and undefined behavior for any caller that passes more than 4 chunks. [L49]
- [ ] <!-- ACT-fe7cbd-1 --> **[correction · medium · small]** `rustguard-tun/examples/tun_echo.rs`: Consult the IHL field to determine the IP header length before accessing ICMP data: compute ihl = (buf[0] & 0x0f) as usize * 4; guard with n >= ihl + 8; access ICMP type as buf[ihl], and compute ICMP checksum over &reply[ihl..n]. Also guard the src/dst IP println with 'if n >= 20' (or use ihl) to avoid printing stale buffer bytes. [L48]
- [ ] <!-- ACT-62f2c7-1 --> **[correction · medium · small]** `rustguard-tun/src/bpf_loader.rs`: In load_and_attach, close xsks_map_fd (and prog_fd if already open) before propagating errors from parse_and_patch_elf, bpf_prog_load, or attach_xdp to prevent fd leaks when partial initialization fails. [L54]
- [ ] <!-- ACT-22946f-1 --> **[correction · medium · small]** `rustguard-tun/src/linux_mq.rs`: Fix fd resource leak in MultiQueueTun::create: replace `let fd = open_tun_queue(None)?;` inside the additional-queues loop with explicit error handling that closes all fds already accumulated in `fds` before returning, matching the cleanup already performed on the ioctl-failure path immediately below. [L116]
- [ ] <!-- ACT-c62ee4-1 --> **[correction · medium · small]** `rustguard-tun/src/linux.rs`: Add '_pad: [u8; 8]' to IfreqAddr so its total size is 40 bytes, matching the 64-bit Linux ifreq struct. Without this, all three SIOCSIFADDR / SIOCSIFDSTADDR / SIOCSIFNETMASK ioctl calls in configure_interface cause the kernel to read 8 bytes past the end of the allocated struct (copy_from_user reads sizeof(ifreq)=40 bytes). Parallel structs IfreqFlags and IfreqMtu already carry the correct padding. [L42]
- [ ] <!-- ACT-b59607-1 --> **[correction · medium · small]** `rustguard-tun/src/macos.rs`: In configure_address, capture the ioctl errno before calling libc::close(sock). Replace the current pattern with: let err = last_os_error(); libc::close(sock); return Err(err); — identical to the close_and_error helper already present in the file. [L212]
- [ ] <!-- ACT-b59607-2 --> **[correction · medium · small]** `rustguard-tun/src/macos.rs`: In set_mtu, apply the same fix: capture errno before close(), as done by close_and_error. The current pattern calls close() before last_os_error(), risking the wrong error being surfaced on ioctl failure. [L234]
- [ ] <!-- ACT-8bab2a-3 --> **[correction · low · small]** `rustguard-cli/src/main.rs`: In cmd_serve, --port and --queues silently fall back to defaults (51820 and 1 respectively) when the supplied value is missing or non-numeric. This is inconsistent with --pool/--token which exit on missing values. Add explicit error reporting for malformed or absent values. [L106]
- [ ] <!-- ACT-dbf131-2 --> **[correction · low · small]** `rustguard-daemon/src/config.rs`: In Config::parse, add a flush of current_peer_kvs (and a subsequent clear) inside the [Interface] branch (around L121), so that a [Peer] section appearing before [Interface] is not silently dropped or merged into the next peer. [L121]
- [ ] <!-- ACT-dbf131-3 --> **[correction · low · small]** `rustguard-daemon/src/config.rs`: In parse_cidr, after computing prefix_len, validate it is within range: return an InvalidData error when prefix_len > 32 for IPv4 addresses or prefix_len > 128 for IPv6 addresses, preventing silent misclassification in contains_v4/contains_v6. [L278]
- [ ] <!-- ACT-9105c7-2 --> **[correction · low · small]** `rustguard-kmod/src/cookie.rs`: In `create_reply`, the i32 return value of `wg_xchacha20poly1305_encrypt` is silently discarded. If encryption fails for any reason, `encrypted_cookie` remains all-zeros and the function returns a structurally valid but cryptographically invalid cookie reply with no indication of failure. The return value should be checked and the error propagated to the caller. [L131]
- [ ] <!-- ACT-62f2c7-2 --> **[correction · low · small]** `rustguard-tun/src/bpf_loader.rs`: Change the NLMSG_ERROR response length guard in attach_xdp_netlink from `n >= 16` to `n >= 20` before reading resp[16..20], to avoid treating zero-initialized bytes as a successful ACK when a short malformed response is received. [L327]
- [ ] <!-- ACT-62f2c7-3 --> **[correction · low · small]** `rustguard-tun/src/bpf_loader.rs`: In attach_xdp_netlink, capture the recv error via io::Error::last_os_error() before calling libc::close(sock) so that a close() failure cannot overwrite errno and misreport the root cause error. [L318]
- [ ] <!-- ACT-62f2c7-4 --> **[correction · low · small]** `rustguard-tun/src/bpf_loader.rs`: Remove the dead first `let sh_info = u32::from_le_bytes(sh[28..32]...)` declaration in parse_and_patch_elf that reads bytes from inside sh_offset at the wrong offset and is immediately shadowed by the correct binding at sh[44..48]. [L401]
- [ ] <!-- ACT-22946f-2 --> **[correction · low · small]** `rustguard-tun/src/linux_mq.rs`: Add `_pad: [u8; 8]` to IfreqAddr (initialised to [0;8] at every construction site) to bring its size to 40 bytes, matching sizeof(struct ifreq) and preventing copy_from_user from reading 8 bytes past the struct boundary. [L41]
- [ ] <!-- ACT-b59607-3 --> **[correction · low · small]** `rustguard-tun/src/macos.rs`: IfreqMtu is 20 bytes but SIOCSIFMTU encodes a 32-byte copyin. Pad the struct to 32 bytes by adding a `_padding: [u8; 12]` field initialized to zero, or use a full ifreq-compatible layout, so the ioctl contract is fully satisfied. [L65]
