<p align="center">
  <img src="https://raw.githubusercontent.com/r-via/anatoly/main/assets/imgs/logo.jpg" width="400" alt="Anatoly" />
</p>

# Anatoly Audit Report

> **41 files** reviewed in **0 min** — **$0.00** in AI analysis so you don't have to.
> Verdict: **CRITICAL** · 4 critical bugs found · 472 findings in 37 files

## Axes

| Axis | Health | Findings | Details |
|------|--------|----------|---------|
| Correction | 🟩🟩🟩🟩🟩🟩🟩🟩🟩⬜ 86% OK | 39 high · 14 med | [View →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_151445/axes/correction/index.md) |
| Utility | 🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩 99% used | 3 high · 1 med | [View →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_151445/axes/utility/index.md) |
| Duplication | 🟩🟩🟩🟩🟩🟩🟩🟩🟩⬜ 94% unique | 13 high · 9 med | [View →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_151445/axes/duplication/index.md) |
| Overengineering | 🟩🟩🟩🟩🟩🟩🟩🟩🟩⬜ 93% lean | 1 high · 1 med | [View →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_151445/axes/overengineering/index.md) |
| Tests | 🟥🟥🟥🟥⬜⬜⬜⬜⬜⬜ 39% covered | 48 high · 20 med · 162 low | [View →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_151445/axes/tests/index.md) |
| Documentation | 🟨🟨🟨🟨🟨⬜⬜⬜⬜⬜ 52% documented | 24 high · 6 med · 121 low | [View →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_151445/axes/documentation/index.md) |
| Best Practices | 🟩🟩🟩🟩🟩🟩🟩🟩⬜⬜ avg 8.1 / 10 | 10 high | [View →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_151445/axes/best-practices/index.md) |

## Top Findings

### 🐛 Correction

- 🔴 **rustguard-tun/src/xdp.rs** `XdpSocket` — Three correctness bugs found in impl methods. (1) tx_send (around L348-L352) calls copy_nonoverlapping with data.len(...
- 🟡 **rustguard-core/src/replay.rs** `ReplayWindow` — The bit-level shift inside shift_window (lines 112-116) is inverted in both direction and iteration order, causing al...
- 🔴 **rustguard-enroll/src/server.rs** `run` — Two correctness defects: (1) In the MSG_TRANSPORT handler, `decrypt_buf` is fixed at 2048 bytes, but `ct = &buf[TRANS...
- 🟡 **rustguard-kmod/src/cookie.rs** `hash` — Line 49 passes `chunks.len() as u32` as `num_chunks` to the C function, but the loop at line 44 uses `.take(4)` and p...
- 🟡 **rustguard-cli/src/main.rs** `cmd_serve` — Two bugs: (1) The .filter(|v| !v.is_empty()) guard for --pool (L82), --token (L89), and --xdp (L99) only rejects empt...

> Showing top 5 of 53 findings. [View all Correction findings →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_151445/axes/correction/index.md)

### ♻️ Utility

-  **rustguard-enroll/src/packet.rs** `parse_eth_udp` — Exported function with 0 workspace importers per pre-computed analysis. Matches rule 2: exported symbol with 0 runtim...
-  **rustguard-kmod/src/replay.rs** `ReplayWindow` — Exported pub(crate) struct with 0 workspace importers per Pre-computed Import Analysis.
-  **rustguard-enroll/src/packet.rs** `ParsedUdp` — Exported struct with 0 workspace importers per pre-computed analysis. Matches rule 2: exported symbol with 0 runtime ...

> Showing top 3 of 4 findings. [View all Utility findings →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_151445/axes/utility/index.md)

### 📋 Duplication

-  **rustguard-tun/src/linux_mq.rs** `set_name` — Identical to linux.rs version - copies string bytes to fixed buffer with length constraint
-  **rustguard-tun/src/linux_mq.rs** `make_sockaddr_in` — Identical to linux.rs version - creates sockaddr_in struct with identical field initialization
-  **rustguard-tun/src/linux_mq.rs** `close_and_error` — Identical to linux.rs version - captures OS error, closes fd, returns error in same sequence
-  **rustguard-enroll/src/client.rs** `rand_index` — Identical implementation — both generate random u32 via getrandom and le_bytes conversion. Score 0.965 with same-crat...
-  **rustguard-enroll/src/client.rs** `base64_key` — Identical implementation — both encode 32-byte key to base64 string using BASE64_STANDARD. Score 0.994 indicates near...

> Showing top 5 of 22 findings. [View all Duplication findings →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_151445/axes/duplication/index.md)

### 🏗️ Overengineering

-  **rustguard-daemon/src/tunnel.rs** `ctrlc_handler` — NIH reimplementation of what the `ctrlc` crate (~5M weekly downloads) provides in two lines. Uses raw unsafe libc (si...

> Showing top 1 of 2 findings. [View all Overengineering findings →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_151445/axes/overengineering/index.md)

### 🧪 Tests

-  **rustguard-enroll/src/control.rs** `new_window` — No test file exists for control.rs. new_window has no tests whatsoever.
-  **rustguard-tun/src/xdp.rs** `XdpSocket` — Central AF_XDP socket struct with complex unsafe lifecycle methods (create, rx_poll, rx_release, tx_send, tx_complete...
-  **rustguard-core/src/cookie.rs** `CookieChecker` — Inline tests cover create_reply, verify_mac2, and the wrong-source failure case. However, verify_mac1 is never tested...
-  **rustguard-core/src/cookie.rs** `CookieState` — new, process_reply (happy path and wrong-mac1 failure), compute_mac2 (with and without cookie) are all tested. Howeve...
-  **rustguard-enroll/src/server.rs** `ServeConfig` — No test file exists for server.rs. ServeConfig is a public struct but has no test coverage. Types/structs with no run...

> Showing top 5 of 230 findings. [View all Tests findings →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_151445/axes/tests/index.md)

### 📝 Documentation

-  **rustguard-daemon/src/config.rs** `InterfaceConfig` — Public struct with no `///` doc comment on the type or on any of its five public fields (`private_key`, `listen_port`...
-  **rustguard-daemon/src/config.rs** `PeerConfig` — Public struct with no `///` doc comment on the type or on any of its five public fields. Fields like `preshared_key` ...
-  **rustguard-enroll/src/control.rs** `new_window` — No `///` doc comment on the function itself. The `EnrollmentWindow` type alias above has docs, but the constructor fu...
-  **rustguard-tun/src/xdp.rs** `XdpSocket` — Public struct. Has a two-line type-level `///` doc comment describing purpose and zero-copy role. Only one field (`fr...
-  **rustguard-core/src/cookie.rs** `COOKIE_LEN` — Public constant has `/// Cookie size.` — present but trivially terse. Does not state the unit (bytes), no context on ...

> Showing top 5 of 151 findings. [View all Documentation findings →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_151445/axes/documentation/index.md)

### ✅ Best Practices

✨ **CLEAN** — Only low-confidence findings.

## Documentation Coverage

Measures inline doc comments (`///` in Rust, `/** */` in JS/TS, docstrings in Python) on exported symbols.
Anatoly also generates reference pages in `.anatoly/docs/` for every reviewed module.

| Metric | Coverage | Description |
|--------|----------|-------------|
| Complete doc comments | 🟥⬜⬜⬜⬜⬜⬜⬜⬜⬜ 12% (16/137) | Exported symbols with a complete inline doc comment covering description, params, and return |
| Any doc comment | 🟨🟨🟨🟨🟨🟨🟨🟨⬜⬜ 77% (106/137) | Exported symbols with at least a partial inline doc comment |
| Module guides | 🟩🟩🟩🟩🟩🟩🟩🟩🟩🟩 100% (7/7) | Modules > 200 LOC with a dedicated page in docs/ |
| Reference pages | 36 pages | Anatoly-generated module and API reference pages |

> No `docs/` directory found. Copy `.anatoly/docs/` to `docs/` to adopt the generated documentation and speed up future Anatoly runs.

**Gaps:** 1 pages to create.


## 📚 Documentation

Anatoly generated a complete documentation for this project during the audit.

**[Browse the documentation →](https://github.com/r-via/anatoly-reports/blob/main/calibrae--rustguard/2026-03-27_151445/docs/index.md)**

---

<details>
<summary><strong>Run Details</strong></summary>

Run `2026-03-27_151445` · 0.2 min · $0.00

**Phase durations:**

| Phase | Duration |
|-------|----------|
| scan | 123ms |
| estimate | 241ms |
| triage | 5ms |
| rag-index | 6.9s |
| report | 94ms |

</details>

<details>
<summary><strong>Methodology</strong></summary>

Each file is evaluated through 7 independent axis evaluators running in parallel.
Every symbol is analysed individually with a confidence score (0–100).
Findings below 30% confidence are discarded; those below 60% are excluded from verdicts.

**Verdicts:** CLEAN (no findings) · NEEDS_REFACTOR (confirmed findings) · CRITICAL (ERROR-level bugs)

**Severity:** High = ERROR or high-confidence NEEDS_FIX/DEAD/DUPLICATE · Medium = lower confidence or OVER · Low = minor

See each axis folder for detailed rating criteria.

</details>

*Generated: 2026-03-27T14:27:25.973Z*
