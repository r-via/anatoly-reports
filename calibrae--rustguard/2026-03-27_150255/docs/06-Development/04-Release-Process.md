# Release Process

> Versioning scheme, release build configuration, and the steps to cut and publish a new version of RustGuard.

## Overview

RustGuard is a Cargo workspace of six crates sharing a single synchronized version. All crates declare `version.workspace = true`, so every release advances every crate together. The current published version is `0.1.0`. The project is dual-licensed under `MIT OR Apache-2.0`.

Binary artifacts are produced from the `rustguard-cli` crate, which compiles to a single `rustguard` executable. The workspace release profile is tuned for minimal binary size and maximum runtime performance.

## Versioning

RustGuard follows [Semantic Versioning](https://semver.org/):

| Component | Meaning |
|-----------|---------|
| **MAJOR** | Breaking protocol-level or API-level change |
| **MINOR** | New backward-compatible feature (e.g., new CLI subcommand) |
| **PATCH** | Bug fix or security fix with no public-API change |

Because all crates share `version.workspace = true`, bumping the version in the root `Cargo.toml` propagates to every crate simultaneously. Never edit individual crate `[package].version` fields.

```toml
# Cargo.toml (workspace root) — single source of truth for version
[workspace.package]
version = "0.2.0"   # bump here only
edition = "2024"
license = "MIT OR Apache-2.0"
repository = "https://github.com/cali/rustguard"
```

## Release Profile

The workspace defines an aggressive release profile that must be used for all distributed binaries:

```toml
[profile.release]
strip        = true          # strip debug symbols → smallest binary
lto          = true          # link-time optimisation across all crates
opt-level    = "z"           # optimise for size over speed
codegen-units = 1            # single codegen unit → best inter-procedural opts
panic        = "abort"       # no unwinding machinery in release builds
```

All performance benchmarks in the README were produced with this profile. Do not ship binaries built with `opt-level = 3` and claim the same numbers.

## Pre-Release Checklist

Before tagging, every item below must pass cleanly:

```bash
# 1. Format — no style diffs allowed
cargo fmt --check

# 2. Clippy — zero warnings, deny all lints
cargo clippy --workspace -- -D warnings

# 3. Full test suite — all 80 tests must pass
cargo test --workspace

# 4. Crypto and protocol tests explicitly (most critical)
cargo test -p rustguard-crypto
cargo test -p rustguard-core
```

Do not skip or suppress any failing test. The crypto and state machine tests are non-negotiable.

## Building Release Binaries

### Native (Linux x86-64)

```bash
cargo build --release
# Binary: target/release/rustguard
```

### Cross-compiled (Linux musl static binary, from macOS or non-musl Linux)

```bash
cargo build --release --target x86_64-unknown-linux-musl
# Binary: target/x86_64-unknown-linux-musl/release/rustguard
```

The musl target produces a fully static binary with no dynamic library dependencies, suitable for distribution on any Linux system.

### Verifying the binary

```bash
# Confirm the binary is stripped and statically linked (musl build)
file target/x86_64-unknown-linux-musl/release/rustguard
# → ELF 64-bit LSB executable, statically linked, stripped

# Confirm the correct version is embedded
./target/release/rustguard status 2>&1 || true
```

## Tagging a Release

```bash
# Ensure working tree is clean
git status

# Bump version in workspace Cargo.toml, then commit
git add Cargo.toml
git commit -m "chore: bump version to 0.2.0"

# Tag with annotated tag
git tag -a v0.2.0 -m "Release v0.2.0"

# Push commit and tag
git push origin main
git push origin v0.2.0
```

Use annotated tags (`-a`), not lightweight tags. Annotated tags carry the tagger identity and timestamp and show up properly in `git describe`.

## License

All contributions and releases are governed by the dual license declared in the workspace:

```
MIT OR Apache-2.0
```

Contributors implicitly license their contributions under these terms on PR submission. See `LICENSE-MIT` and `LICENSE-APACHE` at the repository root.

## Examples

### Complete release walkthrough for v0.2.0

```bash
# Step 1: ensure everything is clean and tests pass
cargo fmt --check && cargo clippy --workspace -- -D warnings && cargo test --workspace

# Step 2: bump version
sed -i 's/^version = "0.1.0"/version = "0.2.0"/' Cargo.toml

# Step 3: verify Cargo.lock updated
cargo build --release

# Step 4: commit, tag, push
git add Cargo.toml Cargo.lock
git commit -m "chore: bump version to 0.2.0"
git tag -a v0.2.0 -m "Release v0.2.0"
git push origin main v0.2.0

# Step 5: build distribution artifacts
cargo build --release --target x86_64-unknown-linux-musl
ls -lh target/x86_64-unknown-linux-musl/release/rustguard
```

## See Also

- [Build and Test](06-Development/02-Build-and-Test.md) — Full build, test, and lint reference
- [Source Tree](06-Development/01-Source-Tree.md) — Crate layout and entry points
- [Package Overview](00-Monorepo/01-Package-Overview.md) — Workspace crate responsibilities
- [Dependency Graph](00-Monorepo/02-Dependency-Graph.md) — Inter-crate build order