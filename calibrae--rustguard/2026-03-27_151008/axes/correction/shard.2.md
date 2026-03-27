[← Back to Correction](./index.md) · [← Back to report](../../public_report.md)

# 🐛 Correction — Shard 2

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [⚡ Quick Wins](#-quick-wins)

## 📊 Findings

| File | Verdict | Correction | Conf. | Details |
|------|---------|------------|-------|---------|
| `rustguard-cli/src/main.rs` | 🟡 NEEDS_REFACTOR | 2 | 90% | [details](#rustguard-clisrcmainrs) |
| `rustguard-daemon/src/config.rs` | 🟡 NEEDS_REFACTOR | 3 | 95% | [details](#rustguard-daemonsrcconfigrs) |
| `rustguard-tun/src/linux.rs` | 🟡 NEEDS_REFACTOR | 2 | 90% | [details](#rustguard-tunsrclinuxrs) |
| `rustguard-tun/src/bpf_loader.rs` | 🟡 NEEDS_REFACTOR | 3 | 88% | [details](#rustguard-tunsrcbpfloaderrs) |
| `rustguard-tun/src/macos.rs` | 🟡 NEEDS_REFACTOR | 3 | 90% | [details](#rustguard-tunsrcmacosrs) |
| `rustguard-kmod/src/cookie.rs` | 🟡 NEEDS_REFACTOR | 1 | 92% | [details](#rustguard-kmodsrccookiers) |
| `rustguard-tun/src/linux_mq.rs` | 🟡 NEEDS_REFACTOR | 2 | 90% | [details](#rustguard-tunsrclinuxmqrs) |
| `rustguard-enroll/src/client.rs` | 🟡 NEEDS_REFACTOR | 1 | 88% | [details](#rustguard-enrollsrcclientrs) |
| `rustguard-enroll/src/packet.rs` | 🟡 NEEDS_REFACTOR | 1 | 88% | [details](#rustguard-enrollsrcpacketrs) |
| `rustguard-tun/examples/tun_echo.rs` | 🟡 NEEDS_REFACTOR | 1 | 85% | [details](#rustguard-tunexamplestunechors) |

## 🔍 Symbol Details

### `rustguard-cli/src/main.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `cmd_serve` | L68–L161 | 🟡 NEEDS_FIX | 90% | Two bugs: (1) The .filter(|v| !v.is_empty()) guard for --pool (L82), --token (L89), and --xdp (L99) only rejects empty strings. If a flag is immediately followed by another flag — e.g., `--token --open` — the next flag name is silently consumed as the option value (token becomes "--open"), and that flag is never processed, so open_immediately stays false. This produces a wrong token passed to the server at runtime with no parsing-level error. (2) --queues (L106) and --port (L113-L116) use .unwrap_or(default) on parse failure, silently ignoring missing or malformed values and falling back to defaults — inconsistent with --pool/--token which exit with an error on missing values. |
| `cmd_join` | L163–L211 | 🟡 NEEDS_FIX | 90% | --token parsing at L172 uses the same .filter(|v| !v.is_empty()) guard. If --token is immediately followed by another flag (e.g., `--token --endpoint foo`), that flag's name becomes the token value and is never processed as a flag. The join will fail at runtime with an authentication error rather than a clear argument-parsing error. |

### `rustguard-daemon/src/config.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `Config` | L13–L16 | 🟡 NEEDS_FIX | 72% | Config::parse (L120-L122) sets current_section to 'interface' but never flushes current_peer_kvs when a [Interface] header arrives while parsing a [Peer] section. If any [Peer] block precedes [Interface], its accumulated KVs are not cleared; the next [Peer] header's flush guard (current_section == Some('peer')) is then false, so those stale KVs merge with the next peer's KVs. The end-of-file flush emits a single peer that silently drops the first peer's data (last-writer-wins for duplicated keys like PublicKey). |
| `parse_interface` | L176–L226 | 🟡 NEEDS_FIX | 88% | L195: when no '/' is present in an Address part, the fallback prefix is hardcoded as the string '24'. For an IPv6 address without an explicit prefix (e.g. 'Address = fd00::1'), this silently assigns prefix_len=24 instead of a valid IPv6 prefix, corrupting the address_v6 tuple stored in InterfaceConfig. parse_cidr (L277) solves the identical problem by branching on addr.is_ipv4(); parse_interface should do the same here. |
| `parse_cidr` | L271–L282 | 🟡 NEEDS_FIX | 75% | L278: prefix_len is parsed as u8 (0-255) with no upper-bound validation against the address family. A CIDR like '10.0.0.1/64' is silently accepted with prefix_len=64. contains_v4 then saturates this to /32 behaviour via the 'prefix_len >= 32' guard (L51), producing semantically incorrect match results without any error. Should reject prefix_len > 32 for IPv4 and > 128 for IPv6. |

### `rustguard-tun/src/linux.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `IfreqAddr` | L39–L42 | 🟡 NEEDS_FIX | 88% | Struct is only 32 bytes (16 ifr_name + 16 sockaddr_in), but on 64-bit Linux the kernel's ifreq is 40 bytes because struct ifmap (unsigned long × 2 = 16 bytes + 8 bytes of shorter fields) sizes the union to 24 bytes, not 16. Unlike IfreqFlags and IfreqMtu, which both carry explicit _pad fields to reach 40 bytes, IfreqAddr is missing _pad: [u8; 8]. The kernel's copy_from_user reads sizeof(struct ifreq) = 40 bytes from the userspace pointer, so 8 bytes beyond the allocation are consumed on every SIOCSIFADDR / SIOCSIFDSTADDR / SIOCSIFNETMASK call. For these SIOCS set operations the kernel does not write those bytes back, so the interface is configured correctly in practice, but the read past the allocation is undefined behaviour and will trip AddressSanitizer. |
| `configure_interface` | L124–L189 | 🟡 NEEDS_FIX | 85% | All three address-setting ioctls (SIOCSIFADDR, SIOCSIFDSTADDR, SIOCSIFNETMASK) use IfreqAddr, which is 8 bytes short of the kernel's 40-byte ifreq on 64-bit Linux. The kernel reads 40 bytes from the pointer on each call, touching 8 bytes of adjacent stack. Functional in practice for these SIOCS operations but is UB and fails under ASAN. Fix: add _pad: [u8; 8] to IfreqAddr. |

### `rustguard-tun/src/bpf_loader.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `XdpProgram` | L37–L41 | 🟡 NEEDS_FIX | 88% | load_and_attach leaks file descriptors on partial failure. xsks_map_fd is created at L51 but if parse_and_patch_elf (L54), bpf_prog_load (L58), or attach_xdp (L63) subsequently return an error, the function exits via ? before the XdpProgram value is constructed, so its Drop impl never runs and the open kernel fds are never closed. Similarly, if bpf_prog_load succeeds but attach_xdp fails, both xsks_map_fd and prog_fd are leaked. |
| `attach_xdp_netlink` | L235–L342 | 🟡 NEEDS_FIX | 80% | Two correctness issues: (1) The NLMSG_ERROR payload guard checks `n >= 16` but then reads resp[16..20] (the 4-byte error code) — NLMSG_ERROR requires at least 20 bytes. When 16 <= n < 20 (malformed kernel message) the code reads zero-initialized stack bytes and incorrectly returns Ok(()), silently swallowing the error. The guard must be `n >= 20`. (2) `libc::close(sock)` is called unconditionally before `io::Error::last_os_error()` on the recv-failure path; if close() itself fails (e.g., EINTR), it overwrites errno and the returned error describes the close failure rather than the recv failure. |
| `parse_and_patch_elf` | L361–L470 | 🟡 NEEDS_FIX | 75% | Inside the section-parsing loop, a first binding `let sh_info = u32::from_le_bytes(sh[28..32]...)` reads bytes 28–31, which are the upper half of the sh_offset field (bytes 24–31) in Elf64_Shdr — the wrong offset. This dead binding is immediately shadowed by the correct `let sh_info = u32::from_le_bytes(sh[44..48]...)`. Because only the second binding is used, runtime behavior for a well-formed ELF is unaffected, but the erroneous first declaration (acknowledged by the self-correction comment) is live dead code that will produce a spurious 'unused variable' compiler warning and could mislead a maintainer who removes the second binding. |

### `rustguard-tun/src/macos.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `IfreqMtu` | L65–L68 | 🟡 NEEDS_FIX | 72% | IfreqMtu is 20 bytes (16 + 4) but SIOCSIFMTU encodes size 0x20 = 32 bytes in the ioctl number. The kernel's copyin() reads 32 bytes from the user-space pointer, consuming 12 bytes of uninitialized stack memory beyond the struct. The full Darwin struct ifreq has a 16-byte sockaddr union at the same offset. While the kernel only uses the name and mtu fields and this is IOC_IN (read-only from kernel's perspective), the struct is under-sized relative to the ioctl contract. |
| `configure_address` | L194–L218 | 🟡 NEEDS_FIX | 88% | close() is called before checking whether the ioctl returned an error. If close() modifies errno (e.g. EINTR on macOS), the subsequent last_os_error() call returns the wrong error. The code already contains close_and_error() to solve exactly this problem, but it was not used here. The fix is to capture the ioctl errno before calling close(), mirroring the pattern in close_and_error. |
| `set_mtu` | L220–L242 | 🟡 NEEDS_FIX | 88% | Same errno-clobbering defect as configure_address: libc::close(sock) is called before the `if ret < 0` check, risking that close() overwrites the ioctl errno. The correct pattern is to save the error before closing (as done in close_and_error). |

### `rustguard-kmod/src/cookie.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `hash` | L41–L51 | 🟡 NEEDS_FIX | 92% | Line 49 passes `chunks.len() as u32` as `num_chunks` to the C function, but the loop at line 44 uses `.take(4)` and populates at most 4 entries in `ptrs` and `lens`. If `chunks.len() > 4`, the C function is told there are more chunks than have been written, causing it to read beyond the bounds of the 4-element Rust arrays — a stack over-read and undefined behavior. The argument must be clamped: `chunks.len().min(4) as u32`. |

### `rustguard-tun/src/linux_mq.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `IfreqAddr` | L41–L44 | 🟡 NEEDS_FIX | 85% | IfreqAddr is 32 bytes ([u8;16] + sockaddr_in[16]) but sizeof(struct ifreq) on Linux x86-64 is 40 bytes (the union is sized to accommodate struct ifmap, which is 24 bytes). When &IfreqAddr is passed to SIOCSIFADDR/SIOCSIFDSTADDR/SIOCSIFNETMASK the kernel's copy_from_user reads 40 bytes, pulling 8 bytes past the end of the struct from adjacent stack memory. For these write-only ioctls the trailing 8 bytes are not consumed by the kernel so there is no functional mis-set, but the out-of-bounds read is a structural correctness violation. Adding a _pad: [u8; 8] field would correctly size the struct to 40 bytes. |
| `MultiQueueTun` | L77–L81 | 🟡 NEEDS_FIX | 90% | In MultiQueueTun::create, the additional-queues loop calls open_tun_queue(None)? which can propagate an error via ?. At that point fds (Vec<i32>) is dropped by Rust's stack unwinding, but Vec<i32>::drop only frees heap memory — it does not call libc::close on the contained file descriptors. Any fds pushed into the vector before the failing iteration are silently leaked. The ioctl-failure path in the same loop correctly closes all prior fds before returning, but the open_tun_queue failure path (handled by ?) does not. |

### `rustguard-enroll/src/client.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `add_route` | L205–L226 | 🟡 NEEDS_FIX | 88% | The binding `let result = …` is guarded by `#[cfg(target_os = "macos")]` and `#[cfg(target_os = "linux")]`. On any other platform (e.g. FreeBSD, OpenBSD, Windows) neither branch compiles, leaving `result` undefined when the unconditional `match result { … }` is reached, which is a compile-time error. A catch-all `#[cfg(not(any(target_os = "macos", target_os = "linux")))]` branch (or restructuring with a single `match` on the OS) is required to compile on other targets. |

### `rustguard-enroll/src/packet.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `parse_ipv4_udp` | L30–L52 | 🟡 NEEDS_FIX | 85% | The computed IHL is never validated against its minimum legal value of 20 bytes (IHL nibble >= 5). After the initial `len < 20` guard, `ihl` can legally be 0, 4, 8, 12, or 16 for a nibble value of 0–4. In those cases `ihl + 8` is still less than the slice length (>= 20), so the second guard also passes, and `ip_data[ihl]` / `ip_data[ihl+1]` index well inside the IP header instead of the UDP source-port field. The resulting `src_port` is garbage and the wrong region is returned as the payload. A check `if ihl < 20 { return None; }` immediately after computing `ihl` is required. |

### `rustguard-tun/examples/tun_echo.rs`

| Symbol | Lines | Correction | Conf. | Detail |
|--------|-------|------------|-------|--------|
| `main` | L12–L79 | 🟡 NEEDS_FIX | 78% | Two correctness issues: (1) The println! at L40-44 accesses buf[12..=19] unconditionally for every Ok(n), but the guard 'version == 4 && n >= 20' only gates the proto check. Because buf is a fixed 1500-byte array this won't panic, but when n < 20 stale or zero bytes are printed as src/dst IPs — minor but incorrect log output. (2) More importantly, the ICMP processing block at L48-72 hard-codes the IP header length as exactly 20 bytes: it uses buf[9] for protocol (correct for IHL=5) and then assumes ICMP starts at byte 20 (buf[20] == 8, reply[20] = 0, &reply[20..n]). The actual header length is ((buf[0] & 0x0f) * 4) bytes. If a packet arrives with IP options (IHL > 5), buf[20] is part of the options region, not the ICMP type, so the guard condition 'buf[20] == 8' is evaluated against the wrong byte and — if it accidentally matches — the code overwrites and checksums data at the wrong offsets, producing a malformed reply. |

## ⚡ Quick Wins

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
