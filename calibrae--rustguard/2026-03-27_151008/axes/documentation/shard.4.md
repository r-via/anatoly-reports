[← Back to Documentation](./index.md) · [← Back to report](../../public_report.md)

# 📝 Documentation — Shard 4

- [📊 Findings](#-findings)
- [🔍 Symbol Details](#-symbol-details)
- [🧹 Hygiene](#-hygiene)

## 📊 Findings

| File | Verdict | Documentation | Conf. | Details |
|------|---------|---------------|-------|---------|
| `rustguard-crypto/src/tai64n.rs` | 🟡 NEEDS_REFACTOR | 1 | 87% | [details](#rustguard-cryptosrctai64nrs) |
| `rustguard-core/tests/integration.rs` | 🟡 NEEDS_REFACTOR | 1 | 93% | [details](#rustguard-coretestsintegrationrs) |
| `rustguard-core/src/replay.rs` | 🟡 NEEDS_REFACTOR | 1 | 92% | [details](#rustguard-coresrcreplayrs) |
| `rustguard-enroll/src/pool.rs` | 🟡 NEEDS_REFACTOR | 1 | 90% | [details](#rustguard-enrollsrcpoolrs) |
| `rustguard-daemon/src/peer.rs` | 🟡 NEEDS_REFACTOR | 1 | 86% | [details](#rustguard-daemonsrcpeerrs) |

## 🔍 Symbol Details

### `rustguard-crypto/src/tai64n.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `Tai64n` | L9–L9 |  PARTIAL | 87% | The struct carries four lines of `///` commentary (lines 4–8) that explain its purpose (replay prevention), binary layout (8 B seconds + 4 B nanoseconds), and WireGuard semantics. However, no `# Examples` section is present on this public type, which is expected by Rust doc conventions for public API structs. Individual methods have their own docs, but the type-level entry point lacks a usage example showing how to construct and compare timestamps. (deliberated: confirmed — Correction NEEDS_FIX confirmed for both issues. (1) Overflow in `secs + TAI64_EPOCH_OFFSET` at L34 is low-severity but valid — in a crypto crate with a public API, defensive arithmetic is warranted; saturating_add is a trivial fix. (2) `from_bytes` lacking nanos validation is the more important issue: the crate explicitly documents the nanos ≤ 999_999_999 invariant as critical for WireGuard byte-wise ordering, yet `from_bytes` (which deserializes untrusted wire data) does not enforce it, creating a real attack surface for replay-protection bypass. Tests WEAK confirmed: the three tests cover happy-path monotonicity, roundtrip, and from_unix roundtrip, but miss nanos-clamping verification (e.g. from_unix with nanos > 999_999_999), the false case of is_after, and known-good byte comparison against WireGuard reference vectors — all meaningful gaps for a crypto primitive. Documentation PARTIAL confirmed: struct-level docs are good but `as_bytes` and `from_bytes` lack any `///` comments, and no `# Examples` section exists on the public type.) |

### `rustguard-core/tests/integration.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `do_handshake` | L11–L31 |  PARTIAL | 85% | Has one `///` line ('Helper: run a full handshake between two peers, return their sessions.') which describes purpose, but gives no detail on the returned tuple structure, the index semantics of the two sessions, or why two distinct static keys are generated. Private helper in a test file, so expectations are reduced, but the single-line comment leaves the tuple ordering and session roles implicit. (deliberated: confirmed — Documentation PARTIAL is a fair assessment. The single-line `///` comment describes purpose but omits tuple ordering (initiator, responder). However, the return type signature explicitly shows two TransportSessions, variable naming in callers consistently destructures as (init, resp), and this is a private helper in a test file where documentation expectations are reduced. PARTIAL is accurate but the action severity (low/trivial) correctly reflects this is minor hygiene, not a real deficiency.) |

### `rustguard-core/src/replay.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `ReplayWindow` | L13–L19 |  PARTIAL | 92% | Public struct with no `///` doc comment on the type declaration itself — the module-level `//!` block explains the algorithm but does not substitute for a struct-level doc comment. Both private fields carry `///` comments describing their roles, which is good but does not satisfy the requirement for a type-level description. No `# Examples` section is present on the struct or its `new()` constructor. (deliberated: confirmed — Correction NEEDS_FIX is CONFIRMED with higher confidence after independent verification. The bit-level shift in shift_window (L112-116) is inverted. The bitmap layout (line 16-17) encodes age as flat_index = word*64+bit, with age 0 at bitmap[0] bit 0. When the window advances by `shift`, existing bits must move to higher flat indices (left-shift within words, carry from word j to j+1, iterating low→high). The code does the opposite: right-shifts within words and iterates high→low (.rev()), causing carry to flow from high words to low words — moving bits toward lower ages rather than higher. Concrete trace: top=5, bitmap[0]=0b111 (counters 3,4,5), counter 7 arrives (shift=2). Expected: bitmap[0]=0b11100. Actual: bitmap[0]=(0b111>>2)|0=0b1, destroying counters 3 and 4. All 9 tests pass only because none attempt to verify a previously-accepted counter survives a window advance — sequential tests never replay, out-of-order tests don't trigger shifts, and boundary tests check fresh counters. This silently defeats the anti-replay security guarantee in any real deployment where packets arrive out of order after window advances. Raising confidence from 55 to 92 based on complete manual trace. Documentation PARTIAL is also correct — struct lacks its own `///` doc comment despite being the sole public type.) |

### `rustguard-enroll/src/pool.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `IpPool` | L9–L20 |  PARTIAL | 90% | The struct itself has no `///` doc comment directly on the `pub struct IpPool` declaration. All five fields carry individual `///` comments with examples (e.g. '10.150.0.0'), which is good. The module-level `//!` block describes the pool's purpose and the .1 reservation, but module-level inner docs do not substitute for struct-level `///` docs. Additionally, there is no `# Examples` section on the type, which is expected for a public API struct in a library crate. (deliberated: confirmed — Tests: WEAK is accurate. Verified against the inline test module: `new()` with prefix_len > 30 (None path) is never tested, `allocate_specific()` has zero coverage, `contains()` with /0 is untested, and `assigned_count()` is never asserted. The five existing tests cover the core happy paths well but leave meaningful gaps in the public API surface. Documentation: PARTIAL is accurate. The `pub struct IpPool` declaration at line 9 has no `///` doc comment; only the module-level `//!` block (lines 1-4) describes the pool. All fields and public methods do have `///` comments, which is good, but rustdoc consumers browsing the struct type directly will see no summary. Both findings are low-severity hygiene issues that do not affect correctness.) |

### `rustguard-daemon/src/peer.rs`

| Symbol | Lines | Documentation | Conf. | Detail |
|--------|-------|---------------|-------|--------|
| `Peer` | L16–L26 |  PARTIAL | 86% | The struct carries a brief type-level doc comment ('Runtime state for a WireGuard peer.') and two of its seven public fields (session, timers) have individual /// comments. However, five public fields — public_key, psk, endpoint, allowed_ips, and persistent_keepalive — have no doc comments at all. Notably, psk ([u8; 32]) is a sensitive security field whose purpose and zeroing semantics deserve explicit documentation. Per the evaluation rule, DOCUMENTED requires /// on the type AND on each public field; the majority of fields being undocumented makes this PARTIAL. No # Examples section is present on the type or its impl block. (deliberated: confirmed — Tests NONE is correct: no test file was provided or referenced that covers Peer, from_config(), allows_ip(), or has_active_session(). The methods contain real logic (CIDR containment, session expiry) that would benefit from testing, but for a simple daemon struct this is not critical. Documentation PARTIAL is accurate: the struct itself has a `///` doc comment on L15, and 2 of 7 fields (session, timers) are documented, but the remaining 5 public fields — including the security-sensitive `psk: [u8; 32]` — lack doc comments. Note that best_practices rule 9 incorrectly states the struct has no `///` doc comment (it does, on L15), but the PARTIAL classification from the documentation axis is still correct because most fields are undocumented. Neither finding rises to the level of NEEDS_REFACTOR for the overall file verdict given the struct is a straightforward domain model with self-descriptive field names.) |

## 🧹 Hygiene

- [ ] <!-- ACT-f55ce3-2 --> **[documentation · low · trivial]** `rustguard-core/src/replay.rs`: Complete `///` doc comment documentation for: `ReplayWindow` (`ReplayWindow`) [L13-L19]
- [ ] <!-- ACT-971d22-1 --> **[documentation · low · trivial]** `rustguard-core/tests/integration.rs`: Complete `///` doc comment documentation for: `do_handshake` (`do_handshake`) [L11-L31]
- [ ] <!-- ACT-310cbd-3 --> **[documentation · low · trivial]** `rustguard-crypto/src/tai64n.rs`: Complete `///` doc comment documentation for: `Tai64n` (`Tai64n`) [L9-L9]
- [ ] <!-- ACT-eacdc0-2 --> **[documentation · low · trivial]** `rustguard-daemon/src/peer.rs`: Complete `///` doc comment documentation for: `Peer` (`Peer`) [L16-L26]
- [ ] <!-- ACT-92d12d-1 --> **[documentation · low · trivial]** `rustguard-enroll/src/pool.rs`: Complete `///` doc comment documentation for: `IpPool` (`IpPool`) [L9-L20]
