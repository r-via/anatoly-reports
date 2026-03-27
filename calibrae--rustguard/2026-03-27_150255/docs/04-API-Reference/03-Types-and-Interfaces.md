# Types and Interfaces

> Complete reference for all public structs, enums, and constants across the RustGuard crate family.

## Overview

RustGuard is organized into seven crates. Each crate exposes a focused set of public types. This page documents every public struct and relevant constant, grouped by crate. For function signatures and behavioral details, see [Public API](01-Public-API.md). For configuration file schema, see [Configuration Schema](02-Configuration-Schema.md).

---

## `rustguard-crypto` — Cryptographic Primitives

### `StaticSecret`

A long-lived X25519 private key. Implements `ZeroizeOnDrop` — key material is securely erased when the value is dropped.

```rust
pub struct StaticSecret(/* private */);
```

| Method | Signature | Notes |
|--------|-----------|-------|
| `random()` | `() -> Self` | `std` only; uses OS RNG |
| `random_from_rng` | `(rng: &mut impl CryptoRngCore) -> Self` | `no_std` compatible |
| `from_bytes` | `(bytes: [u8; 32]) -> Self` | Deserialize from raw bytes |
| `to_bytes` | `(&self) -> [u8; 32]` | Serialize to raw bytes |
| `public_key` | `(&self) -> PublicKey` | Derive the corresponding public key |
| `diffie_hellman` | `(&self, their_public: &PublicKey) -> SharedSecret` | Perform X25519 DH |

### `EphemeralSecret`

A single-use X25519 private key. Semantically identical to `StaticSecret` but documents intent. Implements `ZeroizeOnDrop`.

```rust
pub struct EphemeralSecret(/* private */);
```

| Method | Signature |
|--------|-----------|
| `random()` | `() -> Self` — `std` only |
| `random_from_rng` | `(rng: &mut impl CryptoRngCore) -> Self` |
| `diffie_hellman` | `(&self, their_public: &PublicKey) -> SharedSecret` |
| `public_key` | `(&self) -> PublicKey` |

### `PublicKey`

An X25519 public key (32 bytes). Implements constant-time `PartialEq` via `subtle::ConstantTimeEq` to prevent timing side-channels.

```rust
pub struct PublicKey(/* private */);
```

| Method | Signature |
|--------|-----------|
| `from_bytes` | `(bytes: [u8; 32]) -> Self` |
| `as_bytes` | `(&self) -> &[u8; 32]` |

`PublicKey` also implements `AsRef<[u8]>`, `Clone`, and `Debug`.

### `SharedSecret`

The 32-byte output of an X25519 Diffie-Hellman operation. Implements `ZeroizeOnDrop`.

```rust
pub struct SharedSecret(/* private */);
```

| Method | Signature |
|--------|-----------|
| `as_bytes` | `(&self) -> &[u8; 32]` |

### `Tai64n`

A 12-byte TAI64N timestamp used in handshake initiation to prevent replay attacks. Wire format: 8 bytes TAI64 seconds (big-endian) followed by 4 bytes nanoseconds (big-endian). Implements `PartialOrd`/`Ord` for direct timestamp comparison.

```rust
pub struct Tai64n([u8; 12]);
```

| Method | Signature | Notes |
|--------|-----------|-------|
| `now()` | `() -> Self` | `std` only |
| `from_unix` | `(secs: u64, nanos: u32) -> Self` | `std` and `no_std` |
| `from_bytes` | `(bytes: [u8; 12]) -> Self` | Deserialize |
| `as_bytes` | `(&self) -> &[u8; 12]` | Serialize |
| `is_after` | `(&self, other: &Tai64n) -> bool` | Strict ordering check |

### AEAD Constants

```rust
pub const AEAD_TAG_LEN: usize = 16;
pub const MAX_PACKET_SIZE: usize = 1516; // 1500 (MTU) + 16 (tag)
```

### Protocol String Constants

```rust
pub const CONSTRUCTION: &[u8] = b"Noise_IKpsk2_25519_ChaChaPoly_BLAKE2s";
pub const IDENTIFIER:   &[u8] = b"WireGuard v1 zx2c4 Jason@zx2c4.com";
pub const LABEL_MAC1:   &[u8] = b"mac1----";
pub const LABEL_COOKIE: &[u8] = b"cookie--";
```

These constants seed the Noise handshake hash and are identical across all WireGuard implementations.

---

## `rustguard-core` — Protocol Types

### Wire Message Types

#### `Initiation`

Handshake initiation message (type 1). Fixed wire size: **148 bytes**.

```rust
pub struct Initiation {
    pub sender_index:         u32,
    pub ephemeral:            [u8; 32],
    pub encrypted_static:     [u8; 48],  // 32-byte pubkey + 16-byte AEAD tag
    pub encrypted_timestamp:  [u8; 28],  // 12-byte TAI64N + 16-byte AEAD tag
    pub mac1:                 [u8; 16],
    pub mac2:                 [u8; 16],
}
```

| Method | Returns |
|--------|---------|
| `to_bytes()` | `[u8; 148]` |
| `from_bytes(buf: &[u8; 148])` | `Self` |

#### `Response`

Handshake response message (type 2). Fixed wire size: **92 bytes**.

```rust
pub struct Response {
    pub sender_index:    u32,
    pub receiver_index:  u32,
    pub ephemeral:       [u8; 32],
    pub encrypted_empty: [u8; 16],  // AEAD over empty plaintext — 16-byte tag only
    pub mac1:            [u8; 16],
    pub mac2:            [u8; 16],
}
```

| Method | Returns |
|--------|---------|
| `to_bytes()` | `[u8; 92]` |
| `from_bytes(buf: &[u8; 92])` | `Self` |

#### `Transport`

Transport data message (type 4). Variable length.

```rust
pub struct Transport {
    pub receiver_index: u32,
    pub counter:        u64,
    pub payload:        Vec<u8>,  // ciphertext + 16-byte AEAD tag
}
```

| Method | Returns |
|--------|---------|
| `to_bytes()` | `Vec<u8>` |
| `from_bytes(buf: &[u8])` | `Option<Self>` |

#### `CookieReply`

DoS-protection cookie reply message (type 3). Fixed wire size: **64 bytes**.

```rust
pub struct CookieReply {
    pub receiver_index:   u32,
    pub nonce:            [u8; 24],  // Random XChaCha20 nonce
    pub encrypted_cookie: [u8; 32],  // 16-byte cookie + 16-byte AEAD tag
}
```

| Method | Returns |
|--------|---------|
| `to_bytes()` | `[u8; 64]` |
| `from_bytes(buf: &[u8; 64])` | `Self` |

#### Wire Format Constants

```rust
pub const MSG_INITIATION:        u32   = 1;
pub const MSG_RESPONSE:          u32   = 2;
pub const MSG_COOKIE_REPLY:      u32   = 3;
pub const MSG_TRANSPORT:         u32   = 4;
pub const INITIATION_SIZE:       usize = 148;
pub const RESPONSE_SIZE:         usize = 92;
pub const COOKIE_REPLY_SIZE:     usize = 64;
pub const TRANSPORT_HEADER_SIZE: usize = 16;
```

---

### `TransportSession`

A fully established transport session derived from a completed Noise handshake. Manages per-direction keys, the outgoing nonce counter, and the anti-replay window. Implements `ZeroizeOnDrop`.

```rust
pub struct TransportSession {
    pub our_index:   u32,
    pub their_index: u32,
    // key_send, key_recv, send_counter, recv_window — private
}
```

| Method | Signature | Notes |
|--------|-----------|-------|
| `new` | `(our_index: u32, their_index: u32, key_send: [u8; 32], key_recv: [u8; 32]) -> Self` | Called by handshake functions |
| `encrypt` | `(&mut self, plaintext: &[u8]) -> Option<(u64, Vec<u8>)>` | Returns `None` on nonce exhaustion |
| `decrypt` | `(&mut self, counter: u64, ciphertext: &[u8]) -> Option<Vec<u8>>` | Checks replay window before AEAD |
| `encrypt_to` | `(&mut self, plaintext: &[u8], out: &mut [u8]) -> Option<(u64, usize)>` | Zero-alloc variant |
| `decrypt_in_place` | `(&mut self, counter: u64, buf: &mut [u8], ct_len: usize) -> Option<usize>` | Zero-alloc variant |
| `send_counter` | `(&self) -> u64` | Current outgoing counter |

---

### `ReplayWindow`

A 2048-bit sliding window that tracks which nonce counters have been received, following the IPsec algorithm (RFC 6479). The check/update split prevents replay-window poisoning by garbage packets.

```rust
pub struct ReplayWindow {
    // top: u64, bitmap: [u64; 32] — private
}
```

| Method | Signature | Notes |
|--------|-----------|-------|
| `new` | `() -> Self` | |
| `check` | `(&self, counter: u64) -> bool` | Returns `true` if counter would be accepted |
| `update` | `(&mut self, counter: u64)` | Call only after AEAD authentication succeeds |
| `check_and_update` | `(&mut self, counter: u64) -> bool` | Combined check + update |

---

### `InitiatorHandshake`

Intermediate handshake state held by the initiator after sending the initiation message and before receiving the response. Contains the chaining key, handshake hash, ephemeral key, and PSK. Implements `ZeroizeOnDrop`.

```rust
pub struct InitiatorHandshake {
    // ck, h, ephemeral, sender_index, their_public, psk — all private
}
```

This struct is returned by `create_initiation` / `create_initiation_psk` / `create_initiation_with` and consumed by `process_response`. It is not constructed directly.

---

### `CookieChecker`

Server-side cookie state. Generates cookies and validates MAC2 under load. The rotating secret refreshes every 120 seconds.

```rust
pub struct CookieChecker {
    pub under_load: bool,
    // our_public, secret, secret_generated — private
}
```

| Method | Notes |
|--------|-------|
| `new(our_public: PublicKey) -> Self` | `std` only |
| `new_with(our_public: PublicKey, secret: [u8; 32], now_ts: Timestamp) -> Self` | `no_std` / kernel |
| `create_reply(receiver_index: u32, mac1: &[u8; 16], src: &SocketAddr) -> CookieReply` | `std` only |
| `create_reply_from_bytes(receiver_index: u32, mac1: &[u8; 16], addr_bytes: &[u8]) -> CookieReply` | `no_std` compatible |
| `verify_mac1(msg_bytes: &[u8], mac1: &[u8; 16]) -> bool` | |
| `verify_mac2(msg_with_mac1: &[u8], mac2: &[u8; 16], src: &SocketAddr) -> bool` | `std` only |

The `under_load` field must be set by the caller based on connection rate heuristics.

---

### `CookieState`

Client-side cookie state. Stores a received cookie and computes MAC2 for subsequent handshake initiations.

```rust
pub struct CookieState { /* private */ }
```

| Method | Signature |
|--------|-----------|
| `new` | `() -> Self` |
| `process_reply` | `(&mut self, reply: &CookieReply, their_public: &PublicKey, mac1: &[u8; 16]) -> bool` |
| `compute_mac2` | `(&self, msg_with_mac1: &[u8]) -> [u8; 16]` — returns `[0u8; 16]` if no fresh cookie |

---

### `SessionTimers`

Tracks the session lifecycle timestamps for a single peer. Drives rekey, keepalive, and dead-session cleanup decisions.

```rust
pub struct SessionTimers {
    pub session_established:  Option<Timestamp>,
    pub last_handshake_sent:  Option<Timestamp>,
    pub last_received:        Option<Timestamp>,
    pub last_sent:            Option<Timestamp>,
    pub rekey_requested:      bool,
}
```

| Method | Signature |
|--------|-----------|
| `new` | `() -> Self` |
| `session_started` | `(&mut self)` — `std` only |
| `packet_sent` | `(&mut self)` — `std` only |
| `packet_received` | `(&mut self)` — `std` only |
| `needs_rekey` | `(&self, send_counter: u64) -> bool` |
| `is_expired` | `(&self, send_counter: u64) -> bool` |
| `is_dead` | `(&self) -> bool` |
| `needs_keepalive` | `(&self, keepalive_interval: Option<Duration>) -> bool` |
| `should_retry_handshake` | `(&self) -> bool` |
| `handshake_timed_out` | `(&self) -> bool` |

#### Session Lifetime Constants

```rust
pub const REKEY_AFTER_TIME:      Duration = Duration::from_secs(120);
pub const REJECT_AFTER_TIME:     Duration = Duration::from_secs(180);
pub const REKEY_TIMEOUT:         Duration = Duration::from_secs(5);
pub const REKEY_ATTEMPT_TIME:    Duration = Duration::from_secs(90);
pub const DEAD_SESSION_TIMEOUT:  Duration = Duration::from_secs(240);
pub const REKEY_AFTER_MESSAGES:  u64 = (1u64 << 60) - 1;
pub const REJECT_AFTER_MESSAGES: u64 = u64::MAX - (1 << 13);
```

---

## `rustguard-tun` — TUN Device

### `Tun`

A platform-abstracted TUN network interface. On macOS this is a `utun` interface created via the kernel control socket. On Linux it opens `/dev/net/tun` with `IFF_TUN | IFF_NO_PI`. Implements `Drop` to close the file descriptor.

```rust
pub struct Tun { /* fd: i32, name: String — private */ }
```

| Method | Signature |
|--------|-----------|
| `create` | `(config: &TunConfig) -> io::Result<Self>` |
| `raw_fd` | `(&self) -> i32` — needed for io_uring and AF_XDP |
| `name` | `(&self) -> &str` — e.g. `"utun3"` or `"tun0"` |
| `read` | `(&self, buf: &mut [u8]) -> io::Result<usize>` |
| `write` | `(&self, packet: &[u8]) -> io::Result<usize>` |

### `TunConfig`

Configuration passed to `Tun::create`.

```rust
pub struct TunConfig {
    pub name:        Option<String>,    // None = OS picks the name
    pub mtu:         u16,               // Default: 1420
    pub address:     std::net::Ipv4Addr,
    pub destination: std::net::Ipv4Addr,
    pub netmask:     std::net::Ipv4Addr,
}
```

---

## `rustguard-daemon` — Daemon Configuration Types

### `Config`

A fully parsed `wg.conf`-style configuration file.

```rust
pub struct Config {
    pub interface: InterfaceConfig,
    pub peers:     Vec<PeerConfig>,
}
```

### `InterfaceConfig`

The `[Interface]` section of a WireGuard config.

```rust
pub struct InterfaceConfig {
    pub private_key:  [u8; 32],
    pub listen_port:  u16,
    pub address:      std::net::Ipv4Addr,
    pub netmask:      std::net::Ipv4Addr,
    pub address_v6:   Option<(std::net::Ipv6Addr, u8)>,  // (address, prefix_len)
}
```

### `PeerConfig`

A `[Peer]` section of a WireGuard config.

```rust
pub struct PeerConfig {
    pub public_key:           [u8; 32],
    pub preshared_key:        Option<[u8; 32]>,
    pub endpoint:             Option<std::net::SocketAddr>,
    pub allowed_ips:          Vec<CidrAddr>,
    pub persistent_keepalive: Option<u16>,  // seconds
}
```

### `CidrAddr`

An IP address with a prefix length, representing a CIDR range. Supports both IPv4 and IPv6.

```rust
pub struct CidrAddr {
    pub addr:       std::net::IpAddr,
    pub prefix_len: u8,
}
```

| Method | Signature |
|--------|-----------|
| `contains` | `(&self, ip: IpAddr) -> bool` |
| `contains_v4` | `(&self, ip: Ipv4Addr) -> bool` |
| `contains_v6` | `(&self, ip: Ipv6Addr) -> bool` |

Implements `Display` as `addr/prefix_len` (e.g. `10.0.0.0/24`).

### `Peer`

Runtime peer state maintained by the daemon. Combines the parsed config with a live transport session and timer state.

```rust
pub struct Peer {
    pub public_key:           PublicKey,
    pub psk:                  [u8; 32],
    pub endpoint:             Option<std::net::SocketAddr>,
    pub allowed_ips:          Vec<CidrAddr>,
    pub persistent_keepalive: Option<std::time::Duration>,
    pub session:              Option<TransportSession>,
    pub timers:               SessionTimers,
}
```

| Method | Signature |
|--------|-----------|
| `from_config` | `(config: &PeerConfig) -> Self` |
| `allows_ip` | `(&self, ip: IpAddr) -> bool` |
| `has_active_session` | `(&self) -> bool` |

---

## `rustguard-enroll` — Enrollment Types

### `EnrollmentOffer`

The payload embedded in an enrollment response from server to client.

```rust
pub struct EnrollmentOffer {
    pub server_pubkey: [u8; 32],
    pub assigned_ip:   std::net::Ipv4Addr,
    pub prefix_len:    u8,
}
```

Returned by `parse_response` and consumed by `build_response`.

### `PersistedPeer`

A minimal peer record written to the state file to survive server restarts. Private keys are never persisted.

```rust
pub struct PersistedPeer {
    pub public_key:  [u8; 32],
    pub assigned_ip: std::net::Ipv4Addr,
}
```

### `IpPool`

A CIDR-based IP address pool for the enrollment server. The `.1` address is always reserved for the server itself.

```rust
pub struct IpPool {
    pub prefix_len:  u8,
    pub server_addr: std::net::Ipv4Addr,
    // network, capacity, assigned — private
}
```

| Method | Signature |
|--------|-----------|
| `new` | `(network: Ipv4Addr, prefix_len: u8) -> Option<Self>` — `None` if prefix > 30 |
| `allocate` | `(&mut self) -> Option<Ipv4Addr>` — sequential allocation; `None` when exhausted |
| `allocate_specific` | `(&mut self, addr: Ipv4Addr)` — restore a persisted address |
| `release` | `(&mut self, addr: Ipv4Addr)` — no-op on `server_addr` |
| `contains` | `(&self, addr: Ipv4Addr) -> bool` |
| `assigned_count` | `(&self) -> usize` — includes server address |

---

## Examples

The following example walks through a complete type-level lifecycle: key generation, handshake, session creation, and packet encryption/decryption.

```rust
use rustguard_crypto::{StaticSecret, PublicKey};
use rustguard_core::handshake::{create_initiation, process_initiation, process_response};
use rustguard_core::timers::{SessionTimers, REKEY_AFTER_TIME};

// 1. Both peers generate long-term static key pairs.
let initiator_static = StaticSecret::random();
let responder_static  = StaticSecret::random();
let responder_public  = responder_static.public_key();

// 2. Initiator builds the handshake initiation message.
//    Returns the wire message and intermediate handshake state.
let (init_msg, init_state) =
    create_initiation(&initiator_static, &responder_public, /*sender_index=*/ 1);

// 3. Responder processes the initiation; gets back the initiator's pubkey,
//    the timestamp, a response message, and a ready transport session.
let (peer_pubkey, _timestamp, resp_msg, mut resp_session) =
    process_initiation(&responder_static, &init_msg, /*responder_index=*/ 2)
        .expect("valid initiation");

assert_eq!(peer_pubkey, initiator_static.public_key());

// 4. Initiator processes the response and gets its own transport session.
let mut init_session =
    process_response(init_state, &initiator_static, &resp_msg)
        .expect("valid response");

// 5. Both sides now have a TransportSession. Encrypt a packet.
let plaintext = b"Hello from across the tunnel";
let (counter, ciphertext) = init_session.encrypt(plaintext).unwrap();

// 6. Responder decrypts — replay window is checked automatically.
let decrypted = resp_session.decrypt(counter, &ciphertext).unwrap();
assert_eq!(&decrypted, plaintext);

// 7. Check whether rekeying is needed using SessionTimers.
let mut timers = SessionTimers::new();
timers.session_started();
assert!(!timers.needs_rekey(init_session.send_counter()));
// REKEY_AFTER_TIME = 120s
println!("Rekey after: {:?}", REKEY_AFTER_TIME);
```

---

## See Also

- [Public API](01-Public-API.md) — Function signatures and behavioral contracts
- [Configuration Schema](02-Configuration-Schema.md) — `wg.conf` field reference
- [handshake module](../05-Modules/04-handshake.md) — Noise_IKpsk2 handshake implementation
- [cookie module](../05-Modules/03-cookie.md) — DoS-protection cookie mechanism
- [rustguard-tun module](../05-Modules/07-rustguard-tun.md) — TUN device platform details
- [rustguard-core module](../05-Modules/11-rustguard-core.md) — Core protocol crate overview
- [rustguard-crypto module](../05-Modules/12-rustguard-crypto.md) — Cryptographic primitives crate overview
- [Core Concepts](../02-Architecture/02-Core-Concepts.md) — Glossary of WireGuard domain terms