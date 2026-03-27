[← Back to Overengineering](./index.md) · [← Back to report](../../public_report.md)

# Overengineering — Shard 1

## Findings

| File | Verdict | Overengineering | Conf. | Details |
|------|---------|-----------------|-------|---------|
| `rustguard-daemon/src/tunnel.rs` | NEEDS_REFACTOR | 1 | 90% | [details](#rustguard-daemonsrctunnelrs) |
| `rustguard-tun/src/bpf_loader.rs` | NEEDS_REFACTOR | 1 | 88% | [details](#rustguard-tunsrcbpfloaderrs) |

## Symbol Details

### `rustguard-daemon/src/tunnel.rs`

| Symbol | Lines | Overengineering | Conf. | Detail |
|--------|-------|-----------------|-------|--------|
| `ctrlc_handler` | L504–L525 | OVER | 80% | [USED] called at L118 to setup shutdown signal handler | [UNIQUE] Platform signal handler setup for SIGINT/SIGTERM via libc. No similar functions found. | [NEEDS_FIX] Signal race condition: sigprocmask(SIG_BLOCK) is called inside the newly spawned thread, not in the main thread before any threads are spawned. Between the signal() calls that install signal_noop and the moment the spawned thread's sigprocmask takes effect, SIGINT or SIGTERM can arrive and be delivered to any thread with an unblocked mask (main, outbound, inbound, or timer), where signal_noop consumes it silently. sigwait in the handler thread therefore never fires and the daemon cannot be cleanly shut down. The correct POSIX pattern is to block the signals in the main thread before spawning any threads so all children inherit the blocked mask, then call sigwait exclusively in the dedicated handler thread. | [OVER] NIH reimplementation of what the `ctrlc` crate (~5M weekly downloads) provides in two lines. Uses raw unsafe libc (signal, sigemptyset, sigaddset, sigprocmask, sigwait), wraps the callback in Mutex<Option<F>> to work around FnOnce restrictions, and spawns a dedicated thread — all complexity that ctrlc encapsulates. The companion signal_noop extern is also needed only because of this approach. | [NONE] No test file found. ctrlc_handler installs SIGINT/SIGTERM handlers via unsafe libc calls and spawns a blocking thread; signal delivery, callback invocation, and once-only semantics are all untested. | [PARTIAL] Private function with `/// Install a Ctrl-C / SIGTERM handler.` The closure parameter `f` is undescribed, and the unusual sigwait-based implementation strategy is unexplained. PARTIAL for a non-trivial private function. (deliberated: confirmed — Correction NEEDS_FIX retained: the signal race condition is valid — sigprocmask(SIG_BLOCK) runs only inside the spawned thread at L519, so between the signal() calls at L508-509 and the thread's sigprocmask, SIGINT/SIGTERM can be consumed by signal_noop in any thread (main, outbound, inbound, timer), preventing sigwait from ever firing. The correct POSIX pattern blocks signals in the main thread before spawning. Overengineering OVER retained: ~20 lines of unsafe libc with no SAFETY comments reimplements what the well-maintained ctrlc crate provides in 2-3 lines; for a security-sensitive daemon, minimizing unsafe surface area is arguably more important than minimizing dependencies. Tests NONE correct for complex signal handling with no coverage. Documentation PARTIAL correct — has one-line doc but parameter `f` and the sigwait strategy are unexplained.) |

### `rustguard-tun/src/bpf_loader.rs`

| Symbol | Lines | Overengineering | Conf. | Detail |
|--------|-------|-----------------|-------|--------|
| `attach_xdp` | L210–L233 | OVER | 75% | [USED] Function called from XdpProgram::load_and_attach() on line 68. | [UNIQUE] Tries ip command first, falls back to attach_xdp_netlink. Different semantic contract from attach_xdp_netlink: try-fallback pattern vs direct implementation. Not interchangeable despite 0.841 similarity—different code paths and responsibilities. | [OK] The `ip link ... xdpgeneric pinned /proc/self/fd/N` path may silently fail on older iproute2 (pinned expects a BPF-fs path, not a /proc fd), but the fallback to attach_xdp_netlink handles this correctly. The overall logic is sound. | [OVER] The primary path shells out to `ip link set dev <name> xdpgeneric pinned /proc/self/fd/<fd>`, which is not valid iproute2 XDP syntax (pinned expects a bpffs path, not a /proc/self/fd reference). In practice this path always fails, making it dead code that adds process-spawning overhead and fragility in a low-level system library. The raw netlink fallback (`attach_xdp_netlink`) is the only path that actually works, so the outer wrapper provides no real benefit while adding complexity. | [NONE] No test file found. The ip-command-then-netlink fallback logic is completely untested; neither the success branch nor the fallback path is exercised. | [UNDOCUMENTED] Private function with no `///` doc comment. Has extensive inline `//` comments explaining its strategy and fallback, but no external doc comment. Tolerated for private items. (deliberated: confirmed — Overengineering OVER confirmed: the `ip link set ... xdpgeneric pinned /proc/self/fd/N` path uses invalid iproute2 syntax (pinned expects a bpffs path, not a procfs fd). This primary path effectively always fails, making the wrapper dead code that adds process-spawning overhead before falling back to the netlink path. The function should just call attach_xdp_netlink directly. Tests NONE and documentation UNDOCUMENTED confirmed. Confidence raised slightly since the OVER finding is well-supported.) |

## Hygiene

- [ ] <!-- ACT-bd20cf-8 --> **[overengineering · medium · small]** `rustguard-daemon/src/tunnel.rs`: Simplify: `ctrlc_handler` is over-engineered (`ctrlc_handler`) [L504-L525]
- [ ] <!-- ACT-62f2c7-7 --> **[overengineering · medium · small]** `rustguard-tun/src/bpf_loader.rs`: Simplify: `attach_xdp` is over-engineered (`attach_xdp`) [L210-L233]
