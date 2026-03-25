# Best Practices — Shard 1

## Findings

| File | Verdict | BP Score | Details |
|------|---------|----------|---------|
| `rustguard-core/src/messages.rs` | NEEDS_REFACTOR | 6/10 | [details](../reviews/rustguard-core-src-messages.rev.md) |

## Details

### `rustguard-core/src/messages.rs` — 6/10

**Failed rules:**

- Rule 1: No unwrap in production code (CRITICAL)

- Replace try_into().unwrap() with copy_from_slice into a mut local array to eliminate unwrap from production from_bytes methods.
  - Before: `sender_index: u32::from_le_bytes(buf[4..8].try_into().unwrap()),
ephemeral: buf[8..40].try_into().unwrap(),`
  - After: `let sender_index = u32::from_le_bytes([buf[4], buf[5], buf[6], buf[7]]);
let mut ephemeral = [0u8; 32];
ephemeral.copy_from_slice(&buf[8..40]);`
- Derive Debug and PartialEq on all public message structs to support diagnostics and testing.
  - Before: `#[derive(Clone)]
pub struct Initiation {`
  - After: `#[derive(Clone, Debug, PartialEq)]
pub struct Initiation {`
- Add doc comments to public constants and impl methods.
  - Before: `pub const INITIATION_SIZE: usize = 148;`
  - After: `/// Total wire size of a Handshake Initiation message in bytes.
pub const INITIATION_SIZE: usize = 148;`
- Add doc comments to to_bytes / from_bytes noting panic conditions (or their absence).
  - Before: `pub fn from_bytes(buf: &[u8; INITIATION_SIZE]) -> Self {`
  - After: `/// Deserialises an `Initiation` from a fixed-size 148-byte wire buffer.
/// This function is infallible because the input length is statically guaranteed.
pub fn from_bytes(buf: &[u8; INITIATION_SIZE]) -> Self {`
