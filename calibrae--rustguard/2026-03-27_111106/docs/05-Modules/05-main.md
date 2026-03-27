# main

> Entry point and command dispatcher for the `rustguard` binary — `rustguard-cli/src/main.rs`, 267 LOC.

## Overview

`rustguard-cli/src/main.rs` is the sole binary entry point for the `rustguard` executable. It implements a hand-rolled command dispatcher that reads `argv[1]` and delegates to one of eight private handler functions. No third-party argument-parsing framework is used.

The file imports directly from three workspace crates:

| Crate | Usage |
|---|---|
| `rustguard_daemon` | `Config::from_file`, `tunnel::run` |
| `rustguard_enroll` | `server::run`, `client::run`, `control::send_command`, `state::default_state_path` |
| `rustguard_crypto` | `StaticSecret::random`, `StaticSecret::from_bytes`, `StaticSecret::public_key` |

The `base64` crate (`base64::prelude::BASE64_STANDARD`) handles encoding and decoding for key output and input.

For full user-facing documentation of every command, flag, and exit code see [rustguard-cli](05-Modules/10-rustguard-cli.md).

## Functions

### `fn main()`

```rust
fn main()
```

Collects `std::env::args()` into a `Vec<String>`, then matches on `args.get(1).map(|s| s.as_str())` to dispatch to the appropriate handler. Unknown commands print to `stderr` and exit with code `1`. If no subcommand is given the `usage()` text is printed and the process exits `1`.

### `fn usage()`

```rust
fn usage()
```

Prints a brief command summary to `stderr`. Called on missing or unrecognised subcommands.

### `fn cmd_up(args: &[String])`

```rust
fn cmd_up(args: &[String])
```

Reads `args[0]` as a `PathBuf` config path. Calls `Config::from_file(&config_path)` and on success calls `rustguard_daemon::tunnel::run(config)`, which blocks until the tunnel exits. Exits `1` on a missing argument, config parse error, or tunnel error.

### `fn cmd_serve(args: &[String])`

```rust
fn cmd_serve(args: &[String])
```

Parses the following flags via an index-based `while` loop:

| Flag | Type | Required | Default |
|---|---|---|---|
| `--pool <cidr>` | `String` | Yes | — |
| `--token <token>` | `String` | Yes | — |
| `--port <n>` | `u16` | No | `51820` |
| `--open` | flag | No | `false` |
| `--xdp <ifname>` | `String` | No | `None` |
| `--queues <n>` | `usize` | No | `1` |
| `--uring` | flag | No | `false` |

The CIDR pool is split on `/` into a `Ipv4Addr` network and `u8` prefix. A `rustguard_enroll::server::ServeConfig` is constructed and passed to `rustguard_enroll::server::run`. The `state_path` field is always set to `rustguard_enroll::state::default_state_path()`.

### `fn cmd_join(args: &[String])`

```rust
fn cmd_join(args: &[String])
```

Parses `--token <token>` and a bare positional `<endpoint>` argument. The endpoint is parsed as a `std::net::SocketAddr`. Constructs a `rustguard_enroll::client::JoinConfig` and calls `rustguard_enroll::client::run`, which blocks for the tunnel lifetime.

### `fn cmd_open(args: &[String])`

```rust
fn cmd_open(args: &[String])
```

Reads the optional first element of `args` as a `u64` (default `60`). Sends `"OPEN <secs>"` via `rustguard_enroll::control::send_command`. Prints the server response to `stdout` or exits `1` on error.

### `fn cmd_close()`

```rust
fn cmd_close()
```

Sends `"CLOSE"` via `rustguard_enroll::control::send_command`. Prints the server response.

### `fn cmd_status()`

```rust
fn cmd_status()
```

Sends `"STATUS"` via `rustguard_enroll::control::send_command`. Prints the server response.

### `fn cmd_genkey()`

```rust
fn cmd_genkey()
```

Calls `rustguard_crypto::StaticSecret::random()` to obtain a CSPRNG-backed X25519 private key, encodes it with `BASE64_STANDARD`, and prints to `stdout`.

### `fn cmd_pubkey()`

```rust
fn cmd_pubkey()
```

Reads one line from `stdin`, decodes it from `BASE64_STANDARD`, converts the 32-byte slice to `[u8; 32]`, constructs a `StaticSecret` via `StaticSecret::from_bytes`, derives the public key with `secret.public_key()`, and prints it base64-encoded to `stdout`. Panics with a descriptive message if the input is not valid base64 or not exactly 32 bytes — this is intentional for a key-utility path.

## Argument Parsing Strategy

The file does not use `clap`, `argh`, or any other CLI framework. Argument parsing is performed with two patterns:

1. **Subcommand dispatch** — `args.get(1).map(|s| s.as_str())` matched in a `match` expression.
2. **Flag parsing** — `while i < args.len()` with `match args[i].as_str()` and `i += 1` to consume the following value. Required flags call `process::exit(1)` via `unwrap_or_else` closures when absent.

Unknown flags cause an `eprintln!` to `stderr` and `process::exit(1)`.

## Exit Codes

| Code | Condition |
|---|---|
| `0` | Success (tunnel exited cleanly, control command completed) |
| `1` | Missing argument, parse failure, tunnel error, or control socket unreachable |

## Examples

### Keypair generation pipeline

```bash
# Generate a private key, then derive its public key
rustguard genkey | rustguard pubkey
```

### Standard tunnel mode

```bash
rustguard up /etc/wireguard/wg0.conf
```

### Zero-config enrollment

```bash
# Server — start with enrollment open immediately, AF_XDP on enp7s0
rustguard serve \
  --pool 10.150.0.0/24 \
  --token s3cr3t \
  --open \
  --xdp enp7s0 \
  --queues 4

# Client — join the running server
rustguard join 192.168.99.1:51820 --token s3cr3t
```

### Runtime enrollment control

```bash
# Open enrollment window for 90 seconds
rustguard open 90

# Check current state
rustguard status
# OPEN 87s remaining, 2 peers

# Revoke immediately
rustguard close
# OK enrollment closed
```

### Inline key derivation (Rust)

The `genkey` / `pubkey` commands are thin wrappers around the `rustguard-crypto` public API:

```rust
use base64::prelude::*;
use rustguard_crypto::StaticSecret;

// Equivalent to `rustguard genkey`
let secret = StaticSecret::random();
let private_b64 = BASE64_STANDARD.encode(secret.to_bytes());
println!("{}", private_b64);

// Equivalent to `rustguard pubkey`
let bytes: [u8; 32] = BASE64_STANDARD
    .decode(private_b64.trim())
    .expect("invalid base64")
    .try_into()
    .expect("key must be 32 bytes");
let secret2 = StaticSecret::from_bytes(bytes);
let public_b64 = BASE64_STANDARD.encode(secret2.public_key().as_bytes());
println!("{}", public_b64);
```

## See Also

- [rustguard-cli](05-Modules/10-rustguard-cli.md) — user-facing CLI reference with full flag tables and workflow examples
- [rustguard-daemon](05-Modules/13-rustguard-daemon.md) — `Config::from_file` and `tunnel::run` invoked by `cmd_up`
- [rustguard-enroll](05-Modules/14-rustguard-enroll.md) — `server::run`, `client::run`, and `control::send_command` invoked by enrollment commands
- [rustguard-crypto](05-Modules/12-rustguard-crypto.md) — `StaticSecret` used by `cmd_genkey` and `cmd_pubkey`
- [client](05-Modules/01-client.md) — `JoinConfig` and `client::run` internals
- [server](05-Modules/08-server.md) — `ServeConfig` and `server::run` internals
- [Common Workflows](03-Guides/01-Common-Workflows.md) — end-to-end usage guides