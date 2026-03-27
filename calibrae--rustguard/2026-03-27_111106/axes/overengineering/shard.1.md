[← Back to Overengineering](./index.md) · [← Back to report](../public_report.md)

# Overengineering — Shard 1

## Findings

| File | Verdict | Overengineering | Conf. | Details |
|------|---------|-----------------|-------|---------|
| `rustguard-daemon/src/tunnel.rs` | NEEDS_REFACTOR | 1 | 90% | [details](../reviews/rustguard-daemon-src-tunnel.rev.md) |
| `rustguard-tun/src/bpf_loader.rs` | NEEDS_REFACTOR | 1 | 88% | [details](../reviews/rustguard-tun-src-bpf_loader.rev.md) |

## Symbol Details

### `rustguard-daemon/src/tunnel.rs`

| Symbol | Lines | Overengineering | Conf. | Detail |
|--------|-------|-----------------|-------|--------|
| `ctrlc_handler` | L504–L525 | OVER | 80% | [USED] called at L118 to setup shutdown signal handler | [UNIQUE] Platform si... |

### `rustguard-tun/src/bpf_loader.rs`

| Symbol | Lines | Overengineering | Conf. | Detail |
|--------|-------|-----------------|-------|--------|
| `attach_xdp` | L210–L233 | OVER | 75% | [USED] Function called from XdpProgram::load_and_attach() on line 68. | [UNIQ... |

## Hygiene

- [ ] <!-- ACT-bd20cf-8 --> **[overengineering · medium · small]** `rustguard-daemon/src/tunnel.rs`: Simplify: `ctrlc_handler` is over-engineered (`ctrlc_handler`) [L504-L525]
- [ ] <!-- ACT-62f2c7-7 --> **[overengineering · medium · small]** `rustguard-tun/src/bpf_loader.rs`: Simplify: `attach_xdp` is over-engineered (`attach_xdp`) [L210-L233]
