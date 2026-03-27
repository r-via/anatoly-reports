# rustguard-crypto

> Cryptographic primitives crate for RustGuard — X25519 key exchange, ChaCha20-Poly1305 AEAD, BLAKE2s hashing and HMAC-based HKDF, and TAI64N timestamps. Supports both `std` and `no_std` targets.

## Overview

`rustguard-crypto` is the foundational cryptography crate in the RustGuard workspace. It provides every primitive required by the WireGuard Noise_IKpsk2 handshake and transport layer, implemented without `libwg`. The crate is `no_std`-compatible (using `alloc`) for use in the kernel module crate (`rustguard-kmod`), and enables OS-RNG convenience methods when the `std` feature is active.

All types that hold secret key material implement `ZeroizeOnDrop`. Public-key comparisons use constant-time equality via the `subtle` crate.

### Exported symbols

| Symbol | Source | Description |
|---|---|---|
| `StaticSecret` | `x25519.rs` | Long-lived X25519 secret key |
| `EphemeralSecret` | `x25519.rs` | Single-use X25519 secret key |
| `PublicKey` | `x25519.rs` | X25519 public key (32 bytes) |
| `SharedSecret` | `x25519.rs` | DH output (32 bytes, zeroized on drop) |
| `seal` / `open` | `aead.rs` | Allocating ChaCha20-Poly1305 encrypt / decrypt |
| `seal_to` / `open_to` | `aead.rs` | Zero-allocation in-place encrypt / decrypt |
| `xseal` / `xopen` | `aead.rs` | XChaCha20-Poly1305 encrypt / decrypt (cookie layer) |
| `AEAD_TAG_LEN` | `aead.rs` | `16` — Poly1305 authentication tag length |
| `MAX_PACKET_SIZE` | `aead.rs` | `1516` — maximum Ethernet payload + tag |
| `hash` | `blake2s.rs` | BLAKE2s-256 hash over one or more byte slices |
| `mac` | `blake2s.rs` | BLAKE2s-256 keyed MAC (used for MAC1 / MAC2) |
| `hkdf` | `blake2s.rs` | HMAC-BLAKE2s HKDF returning three 32-byte outputs |
| `Tai64n` | `tai64n.rs` | TAI64N timestamp for handshake replay prevention |

### Protocol constants

```rust
pub const CONSTRUCTION: &[u8] = b"Noise_IKpsk2_25519_ChaChaPoly_BLAKE2s";
pub const IDENTIFIER:   &[u8] = b"WireGuard v1 zx2c4 Jason@zx2c4.com";
pub const LABEL_MAC1:   &[u8] = b"mac1----";
pub const LABEL_COOKIE: &[u8] = b"cookie--";
```

---

## X25519 Key Exchange (`x25519.rs`)

### `StaticSecret`

A long-lived X25519 private key. Implements `Zeroize` and `ZeroizeOnDrop`.

```rust
impl StaticSecret {
    pub fn random_from_rng(rng: &mut impl CryptoRngCore) -> Self;
    #[cfg(feature = "std")]
    pub fn random() -> Self;
    pub fn from_bytes(bytes: [u8; 32]) -> Self;
    pub fn diffie_hellman(&self, their_public: &PublicKey) -> SharedSecret;
    pub fn public_key(&self) -> PublicKey;
    pub fn to_bytes(&self) -> [u8; 32];
}
```

### `EphemeralSecret`

A single-use X25519 private key. Semantically single-use, but the `diffie_hellman` method does not consume `self` because WireGuard's handshake requires the ephemeral key to participate in multiple DH operations during the same handshake.

```rust
impl EphemeralSecret {
    pub fn random_from_rng(rng: &mut impl CryptoRngCore) -> Self;
    #[cfg(feature = "std")]
    pub fn random() -> Self;
    pub fn diffie_hellman(&self, their_public: &PublicKey) -> SharedSecret;
    pub fn public_key(&self) -> PublicKey;
}
```

### `PublicKey`

A 32-byte X25519 public key. `PartialEq` / `Eq` use `subtle::ConstantTimeEq` to prevent timing side-channels.

```rust
impl PublicKey {
    pub fn from_bytes(bytes: [u8; 32]) -> Self;
    pub fn as_bytes(&self) -> &[u8; 32];
}
```

### `SharedSecret`

The 32-byte output of a DH operation. Implements `ZeroizeOnDrop`.

```rust
impl SharedSecret {
    pub fn as_bytes(&self) -> &[u8; 32];
}
```

---

## AEAD (`aead.rs`)

WireGuard transport packets use ChaCha20-Poly1305 with a 96-bit nonce constructed as `[0x00; 4] || counter.to_le_bytes()`. Cookie messages use XChaCha20-Poly1305 with a caller-supplied 24-byte nonce.

### Allocating functions

#### `seal`

```rust
pub fn seal(key: &[u8; 32], counter: u64, aad: &[u8], plaintext: &[u8]) -> Vec<u8>
```

Encrypts `plaintext` and returns `ciphertext || tag`. Panics if the underlying cipher reports failure (which cannot happen for valid inputs).

#### `open`

```rust
pub fn open(key: &[u8; 32], counter: u64, aad: &[u8], ciphertext: &[u8]) -> Option<Vec<u8>>
```

Decrypts and authenticates `ciphertext`. Returns `None` on authentication failure — wrong key, tampered data, mismatched AAD, or wrong counter.

#### `xseal` / `xopen`

```rust
pub fn xseal(key: &[u8; 32], nonce: &[u8; 24], aad: &[u8], plaintext: &[u8]) -> Vec<u8>
pub fn xopen(key: &[u8; 32], nonce: &[u8; 24], aad: &[u8], ciphertext: &[u8]) -> Option<Vec<u8>>
```

XChaCha20-Poly1305 variants used by the cookie layer. The 24-byte nonce is caller-supplied and must be unique per (key, message) pair.

### Zero-allocation in-place functions

#### `seal_to`

```rust
pub fn seal_to(key: &[u8; 32], counter: u64, plaintext: &[u8], buf: &mut [u8]) -> usize
```

Encrypts `plaintext` directly into `buf` and appends the 16-byte Poly1305 tag. `buf` must be at least `plaintext.len() + AEAD_TAG_LEN` bytes. Returns the total number of bytes written (`plaintext.len() + 16`). Panics if `buf` is too small.

#### `open_to`

```rust
pub fn open_to(key: &[u8; 32], counter: u64, buf: &mut [u8], ct_len: usize) -> Option<usize>
```

Decrypts `buf[..ct_len]` in place. `ct_len` must include the 16-byte tag. Returns `Some(plaintext_len)` on success, `None` on authentication failure or if `ct_len < AEAD_TAG_LEN`.

### Constants

| Constant | Value | Description |
|---|---|---|
| `AEAD_TAG_LEN` | `16` | Poly1305 tag length in bytes |
| `MAX_PACKET_SIZE` | `1516` | Maximum Ethernet MTU (1500) + tag |

---

## BLAKE2s and HKDF (`blake2s.rs`)

### `hash`

```rust
pub fn hash(data: &[&[u8]]) -> [u8; 32]
```

Computes BLAKE2s-256 over the concatenation of all slices in `data`. Equivalent to hashing a single contiguous buffer — `hash(&[a, b])` produces the same result as `hash(&[ab])`.

### `mac`

```rust
pub fn mac(key: &[u8], data: &[&[u8]]) -> [u8; 32]
```

BLAKE2s-256 in keyed MAC mode. Used by the WireGuard handshake for MAC1 and MAC2 computation. **Not HMAC** — this uses BLAKE2s's native key parameter. For MAC1/MAC2, callers must truncate the 32-byte output to 16 bytes: `&mac_result[..16]`. Key must be ≤ 32 bytes.

### `hkdf`

```rust
pub fn hkdf(key: &[u8; 32], input: &[u8]) -> ([u8; 32], [u8; 32], [u8; 32])
```

HMAC-BLAKE2s HKDF as specified by WireGuard. Uses the full RFC 2104 HMAC construction (ipad/opad double-hash) with BLAKE2s-256 as the hash function — **not** keyed BLAKE2s. Returns three independent 32-byte outputs `(T1, T2, T3)`; callers use as many as the handshake step requires.

> **Important:** WireGuard's KDF requires HMAC-BLAKE2s (RFC 2104), while MAC1/MAC2 use keyed BLAKE2s. These two constructions produce different outputs and are not interchangeable.

---

## TAI64N Timestamps (`tai64n.rs`)

### `Tai64n`

A 12-byte timestamp: 8 bytes of TAI64 seconds (big-endian) followed by 4 bytes of nanoseconds (big-endian). Used in handshake initiation messages; each new handshake must carry a timestamp strictly greater than the last accepted one to prevent replay.

`Tai64n` derives `PartialOrd` and `Ord`, so ordering can be compared with standard operators. `is_after` provides an explicit named check.

```rust
impl Tai64n {
    #[cfg(feature = "std")]
    pub fn now() -> Self;
    pub fn from_unix(secs: u64, nanos: u32) -> Self;
    pub fn as_bytes(&self) -> &[u8; 12];
    pub fn from_bytes(bytes: [u8; 12]) -> Self;
    pub fn is_after(&self, other: &Tai64n) -> bool;
}
```

`from_unix` clamps `nanos` to `999_999_999` to preserve the big-endian byte-wise ordering that WireGuard's timestamp comparison relies on. In `no_std` contexts the kernel module calls `from_unix` directly with a kernel-sourced clock value; `now()` is only available under `std`.

---

## Examples

### Key generation and Diffie-Hellman

```rust
use rustguard_crypto::{StaticSecret, EphemeralSecret, PublicKey};

// Long-lived peer identity keys
let alice_static = StaticSecret::random();
let alice_public = alice_static.public_key();

let bob_static = StaticSecret::random();
let bob_public = bob_static.public_key();

// Initiator side: ephemeral key + static DH
let alice_eph = EphemeralSecret::random();
let eph_dh  = alice_eph.diffie_hellman(&bob_public);
let static_dh = alice_static.diffie_hellman(&bob_public);

assert_eq!(
    bob_static.diffie_hellman(&alice_static.public_key()).as_bytes(),
    static_dh.as_bytes(),
);
```

### Transport packet encryption (zero-allocation path)

```rust
use rustguard_crypto::{seal_to, open_to, AEAD_TAG_LEN, MAX_PACKET_SIZE};

let send_key = [0x11u8; 32];
let recv_key = send_key;
let counter: u64 = 42;
let plaintext = b"Hello, WireGuard!";

// Encrypt in place
let mut buf = [0u8; MAX_PACKET_SIZE];
let enc_len = seal_to(&send_key, counter, plaintext, &mut buf);

// Decrypt in place
let pt_len = open_to(&recv_key, counter, &mut buf, enc_len)
    .expect("authentication failed");

assert_eq!(&buf[..pt_len], plaintext);
```

### BLAKE2s MAC and HKDF

```rust
use rustguard_crypto::{mac, hkdf, LABEL_MAC1};

// Compute MAC1 for a handshake message (truncated to 16 bytes per WireGuard spec)
let responder_pubkey = [0xAAu8; 32];
let mac1_key = mac(LABEL_MAC1, &[&responder_pubkey]);
let message_body = [0u8; 148]; // handshake initiation body
let mac1_full = mac(&mac1_key, &[&message_body]);
let mac1: [u8; 16] = mac1_full[..16].try_into().unwrap();

// Derive session keys via HKDF
let chaining_key = [0x55u8; 32];
let dh_output = [0x77u8; 32];
let (new_ck, send_key, recv_key) = hkdf(&chaining_key, &dh_output);
```

### TAI64N timestamp for handshake replay prevention

```rust
use rustguard_crypto::Tai64n;

let last_timestamp = Tai64n::now();

// … time passes, new handshake arrives …
let new_timestamp = Tai64n::now();

if !new_timestamp.is_after(&last_timestamp) {
    // Reject: replayed or out-of-order handshake initiation
    return Err("timestamp replay detected");
}
```

### XChaCha20-Poly1305 cookie encryption

```rust
use rustguard_crypto::{xseal, xopen};

let cookie_key = [0xCCu8; 32];
let nonce = [0x01u8; 24]; // must be unique per (key, message) pair
let cookie_value = [0xDDu8; 16];
let aad = b"initiator-mac1";

let encrypted = xseal(&cookie_key, &nonce, aad, &cookie_value);
let decrypted = xopen(&cookie_key, &nonce, aad, &encrypted)
    .expect("cookie decryption failed");

assert_eq!(decrypted, cookie_value);
```

## See Also

- [rustguard-core](05-Modules/11-rustguard-core.md) — Noise_IK handshake and transport sessions built on top of this crate
- [cookie](05-Modules/03-cookie.md) — Cookie mechanism using `xseal` / `xopen` and `mac`
- [handshake](05-Modules/04-handshake.md) — Handshake implementation consuming `hkdf`, `StaticSecret`, and `Tai64n`
- [rustguard-kmod](05-Modules/06-rustguard-kmod.md) — Kernel module using the `no_std` build of this crate
- [Types and Interfaces](04-API-Reference/03-Types-and-Interfaces.md) — Cross-crate type reference