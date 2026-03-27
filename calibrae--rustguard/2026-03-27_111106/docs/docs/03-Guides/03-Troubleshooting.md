# Troubleshooting

> Diagnostic guide for common errors, failure modes, and runtime issues in RustGuard.

## Overview

This page documents error messages produced by RustGuard, their likely causes, and corrective actions. RustGuard prints status messages to stdout and errors to stderr. All fatal errors exit with a non-zero status code. No log file is written by default — run the process in a terminal or capture stderr to a file for persistent logging.

---

## Config File Errors

These errors are produced by `Config::from_file` and printed as `failed to read config: <detail>` when running `rustguard up`.

| Error message | Cause | Fix |
|---|---|---|
| `failed to read config: <os error>` | Config file does not exist or is not readable | Verify the path and file permissions |
| `bad line: <line>` | A line in the config file is missing the `=` separator | Remove the malformed line or add the missing `=` |
| `key outside section: <line>` | A `key = value` pair appears before any `[Interface]` or `[Peer]` section header | Move the line under the correct section |
| `no [Interface]` | The config file has no `[Interface]` section | Add an `[Interface]` section with at least `PrivateKey` and `Address` |
| `missing PrivateKey` | The `[Interface]` section has no `PrivateKey` field | Add `PrivateKey = <base64 key>` to the `[Interface]` section |
| `missing Address` | The `[Interface]` section has no `Address` field | Add `Address = <ip>/<prefix>` to the `[Interface]` section |
| `missing PublicKey` | A `[Peer]` section has no `PublicKey` field | Add `PublicKey = <base64 key>` to the peer section |
| `bad base64: <detail>` | A key value is not valid base64 | Regenerate the key with `rustguard genkey` / `rustguard pubkey` |
| `key must be 32 bytes` | A key decoded to a length other than 32 bytes | The key is truncated or wrong; regenerate it |
| `bad port: <detail>` | `ListenPort` is not a valid `u16` integer | Use a port number between 1 and 65535 |
| `bad address: <addr>` | An address in the `Address` field is not a valid IPv4 or IPv6 address | Correct the address format |
| `bad prefix: <detail>` | A CIDR prefix length is not a valid integer | Use a numeric prefix (e.g., `24`) |
| `bad endpoint: <detail>` | `Endpoint` is not a valid `host:port` socket address | Use the format `1.2.3.4:51820` or `[::1]:51820` |
| `bad keepalive: <detail>` | `PersistentKeepalive` is not a valid `u16` integer | Use a numeric value in seconds (e.g., `25`) |
| `bad CIDR addr: <detail>` | An address in `AllowedIPs` is not a valid IP | Correct the CIDR range |
| `bad CIDR prefix: <detail>` | A prefix length in `AllowedIPs` is not a valid integer | Use a numeric prefix (e.g., `32` for a host route) |

### Example: diagnosing a config parse failure

```bash
rustguard up /etc/rustguard/wg0.conf
# failed to read config: bad base64: Invalid byte 32 at offset 0
```

The `PrivateKey` or `PublicKey` value contains a space or other invalid character. Regenerate with:

```bash
rustguard genkey > /etc/rustguard/private.key
rustguard pubkey < /etc/rustguard/private.key
```

---

## Tunnel Startup Errors

Errors printed during `rustguard up` after config parsing succeeds.

| Error message | Cause | Fix |
|---|---|---|
| `bind UDP: <detail>` | The UDP listen port is already in use or permission denied | Check `ss -ulnp \| grep 51820`; stop the conflicting process or change `ListenPort` |
| `route add <route> failed: <stderr>` | The kernel rejected the route addition | Verify the TUN interface is up; check for a conflicting route with `ip route show` |
| `route command failed: <detail>` | The `ip` (Linux) or `route` (macOS) binary was not found | Ensure `iproute2` is installed on Linux or that `/sbin/route` is present on macOS |
| `ipv6 address assignment failed: <stderr>` | The kernel rejected the IPv6 address assignment | Verify IPv6 is enabled on the host (`sysctl net.ipv6.conf.all.disable_ipv6`) |
| `failed to send handshake to <endpoint>: <detail>` | The initial UDP send to the peer's endpoint failed | Check network reachability and firewall rules for UDP on the listen port |

### TUN device creation failures

TUN device creation failures are OS-level errors returned from `Tun::create`. On Linux, `/dev/net/tun` must exist and be accessible.

```bash
# Verify the TUN kernel module is loaded
lsmod | grep tun

# Load it if missing
modprobe tun

# Check device permissions
ls -la /dev/net/tun
```

On macOS, the kernel control socket for `com.apple.net.utun_control` requires no additional setup on macOS 10.15+.

---

## Runtime Tunnel Errors

These messages are printed to stderr during operation. They do not terminate the process unless noted.

| Error message | Cause | Action |
|---|---|---|
| `TUN read error: <detail>` | A read from the TUN file descriptor failed | Usually transient; the tunnel continues. If persistent, the TUN interface may have been deleted |
| `TUN write error: <detail>` | A write to the TUN file descriptor failed | Usually transient; decrypted packets are dropped. Check interface state |
| `UDP send error: <detail>` | Failed to send an encrypted UDP datagram | Check network connectivity and whether the peer endpoint is reachable |
| `UDP recv error: <detail>` | Failed to receive a UDP datagram | Usually transient (`EAGAIN`/`EWOULDBLOCK` are silently ignored). Persistent errors indicate socket problems |
| `nonce exhausted — need rekey` | The ChaCha20-Poly1305 nonce counter reached its limit | The session is cleared; the timer thread will automatically retry the handshake. No action required |
| `failed to send response: <detail>` | Failed to send a handshake response to an initiating peer | Check network reachability back to the peer |

### Handshake not completing

If `sent handshake initiation to <endpoint>` is logged repeatedly but `handshake complete` never appears:

1. Verify UDP port 51820 is not blocked by a firewall on either end.
2. Check that the `PublicKey` in the config matches the private key on the remote peer.
3. If using a `PresharedKey`, confirm the value is identical on both sides.

The timer thread retries the handshake every 5 seconds and logs `retrying handshake with <pubkey>`. After the handshake timeout (approximately 30 seconds), the pending state is pruned; the next retry starts a fresh handshake.

### Session expired messages

```
session expired for <pubkey>
```

A transport session has exceeded its lifetime (180 seconds without traffic, or 2^60 encrypted messages). This is normal; the timer thread clears the session and the next outbound packet triggers a new handshake automatically.

---

## Enrollment Errors

Errors from `rustguard serve` and `rustguard join`.

| Error message | Cause | Fix |
|---|---|---|
| `serve error: <detail>` | `rustguard serve` failed to start | See the detail for the specific OS error (e.g., port in use) |
| `join error: <detail>` | `rustguard join` failed | See sub-errors below |
| `enrollment timeout — is the server running? (<detail>)` | `rustguard join` did not receive a response within 5 seconds | Verify the server is running and the endpoint address/port is correct |
| `invalid enrollment response — wrong token?` | The server responded but the response failed decryption | Ensure `--token` matches on both `serve` and `join` |
| `handshake failed` | The Noise_IK handshake after enrollment was rejected | Keys may be mismatched; retry with `rustguard join` |
| `invalid handshake response` | The handshake response message was too short | Network corruption or incorrect server implementation; retry |
| `bad pool address: <detail>` | The `--pool` CIDR network address is not a valid IPv4 address | Use the format `10.150.0.0/24` |
| `bad pool prefix: <detail>` | The `--pool` CIDR prefix is not a valid integer | Use a prefix between `8` and `30` |
| `bad endpoint: <detail>` | The endpoint argument to `rustguard join` is not a valid socket address | Use `host:port` format, e.g., `203.0.113.1:51820` |

### Enrollment window is closed

```bash
# Check enrollment state
rustguard status
# CLOSED, 3 peers

# Open the window on the server
rustguard open 60
# OK enrollment open for 60s
```

`rustguard open` and `rustguard close` communicate over the UNIX domain socket at `/tmp/rustguard.sock`. If `rustguard status` fails with `cannot connect to rustguard (is the server running?)`, the server process is not running or the socket at `/tmp/rustguard.sock` was not created.

---

## AF_XDP and io_uring Fallback

RustGuard gracefully degrades when hardware-accelerated I/O paths are unavailable.

| Message | Meaning |
|---|---|
| `AF_XDP failed (<detail>), falling back to standard UDP` | The AF_XDP BPF program could not be loaded or attached to the interface; the tunnel continues over standard UDP |
| `io_uring init failed: <detail>` | io_uring initialisation failed (kernel too old or `RLIMIT_MEMLOCK` too low); the tunnel falls back to the default I/O path |

To use `--xdp`, the named interface must support XDP and the process must have `CAP_NET_ADMIN` and `CAP_SYS_ADMIN` (or run as root). Check kernel support:

```bash
# Verify kernel version (5.10+ required for AF_XDP zero-copy)
uname -r

# Check driver XDP support
ethtool -i enp7s0 | grep driver
```

---

## State File Errors

The enrollment server persists peer state to `~/.rustguard/state.json`. Errors during load are non-fatal; the server starts with an empty peer list.

| Error message | Cause |
|---|---|
| `bad state line: <line>` | A line in the state file is malformed |
| `bad key: <detail>` | A key in the state file is invalid base64 |
| `key must be 32 bytes` | A key in the state file decoded to the wrong length |
| `bad ip: <detail>` | An IP address in the state file is not parseable |

To reset peer state, delete `~/.rustguard/state.json` before starting the server.

---

## Examples

### Capturing all output for diagnosis

RustGuard writes status to stdout and errors to stderr. Capture both:

```bash
rustguard up /etc/rustguard/wg0.conf > /var/log/rustguard.log 2>&1
```

### Checking whether a tunnel is running

```bash
# Standard mode — look for the process and the TUN interface
pgrep -a rustguard
ip link show type tun

# Enrollment server — query the control socket
rustguard status
# OPEN 47s remaining, 2 peers
```

### Verifying UDP reachability between peers

```bash
# On the receiver (replace 51820 with your ListenPort)
nc -ulp 51820

# On the sender
echo "test" | nc -u <receiver-ip> 51820
```

### Diagnosing a stalled handshake

```bash
# Run with stderr visible and watch for retries
rustguard up /etc/rustguard/wg0.conf 2>&1 | grep -E 'handshake|retrying|error'
```

---

## See Also

- [Common Workflows](../03-Guides/01-Common-Workflows.md)
- [Configuration Schema](../04-API-Reference/02-Configuration-Schema.md)
- [rustguard-daemon module](../05-Modules/13-rustguard-daemon.md)
- [rustguard-enroll module](../05-Modules/14-rustguard-enroll.md)
- [rustguard-tun module](../05-Modules/07-rustguard-tun.md)
- [System Overview](../02-Architecture/01-System-Overview.md)