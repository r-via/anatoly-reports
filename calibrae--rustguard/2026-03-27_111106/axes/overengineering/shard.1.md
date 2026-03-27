[← Back to Overengineering](./index.md) · [← Back to report](../../public_report.md)

# 🏗️ Overengineering — Shard 1

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [🧹 Hygiene](#-hygiene)

## 📊 Findings

| File | Verdict | Overengineering | Conf. | Details |
|------|---------|-----------------|-------|---------|
| `rustguard-daemon/src/tunnel.rs` | 🟡 NEEDS_REFACTOR | 1 | 90% | [details](#rustguard-daemonsrctunnelrs) |
| `rustguard-tun/src/bpf_loader.rs` | 🟡 NEEDS_REFACTOR | 1 | 88% | [details](#rustguard-tunsrcbpfloaderrs) |

## 🔍 Symbol Details

### `rustguard-daemon/src/tunnel.rs`

| Symbol | Lines | Overengineering | Conf. | Detail |
|--------|-------|-----------------|-------|--------|
| `ctrlc_handler` | L504–L525 |  OVER | 80% | NIH reimplementation of what the `ctrlc` crate (~5M weekly downloads) provides in two lines. Uses raw unsafe libc (signal, sigemptyset, sigaddset, sigprocmask, sigwait), wraps the callback in Mutex<Option<F>> to work around FnOnce restrictions, and spawns a dedicated thread — all complexity that ctrlc encapsulates. The companion signal_noop extern is also needed only because of this approach. |

### `rustguard-tun/src/bpf_loader.rs`

| Symbol | Lines | Overengineering | Conf. | Detail |
|--------|-------|-----------------|-------|--------|
| `attach_xdp` | L210–L233 |  OVER | 75% | The primary path shells out to `ip link set dev <name> xdpgeneric pinned /proc/self/fd/<fd>`, which is not valid iproute2 XDP syntax (pinned expects a bpffs path, not a /proc/self/fd reference). In practice this path always fails, making it dead code that adds process-spawning overhead and fragility in a low-level system library. The raw netlink fallback (`attach_xdp_netlink`) is the only path that actually works, so the outer wrapper provides no real benefit while adding complexity. |

## 🧹 Hygiene

- [ ] <!-- ACT-bd20cf-8 --> **[overengineering · medium · small]** `rustguard-daemon/src/tunnel.rs`: Simplify: `ctrlc_handler` is over-engineered (`ctrlc_handler`) [L504-L525]
- [ ] <!-- ACT-62f2c7-7 --> **[overengineering · medium · small]** `rustguard-tun/src/bpf_loader.rs`: Simplify: `attach_xdp` is over-engineered (`attach_xdp`) [L210-L233]
