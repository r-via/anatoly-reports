# Project — Documentation

> Project documentation reference

---

## . Monorepo

| Document | Description |
|----------|-------------|
| [Package Overview](00-Monorepo/01-Package-Overview.md) | Packages in this monorepo and their responsibilities |
| [Dependency Graph](00-Monorepo/02-Dependency-Graph.md) | Inter-package dependencies and build order |
| [Shared Conventions](00-Monorepo/03-Shared-Conventions.md) | Cross-package coding standards and shared config |

## 1. Getting Started

| Document | Description |
|----------|-------------|
| [Overview](01-Getting-Started/01-Overview.md) | What the project does and why it exists |
| [Installation](01-Getting-Started/02-Installation.md) | Prerequisites, install steps, first run |
| [Configuration](01-Getting-Started/03-Configuration.md) | Config files, environment variables, options |
| [Quick Start](01-Getting-Started/04-Quick-Start.md) | End-to-end tutorial in under 5 minutes |

## 2. Architecture

| Document | Description |
|----------|-------------|
| [System Overview](02-Architecture/01-System-Overview.md) | High-level diagram and component responsibilities |
| [Core Concepts](02-Architecture/02-Core-Concepts.md) | Glossary and key domain concepts |
| [Data Flow](02-Architecture/03-Data-Flow.md) | How data moves through the system |
| [Design Decisions](02-Architecture/04-Design-Decisions.md) | Architecture Decision Records (ADRs) |

## 3. Guides

| Document | Description |
|----------|-------------|
| [Common Workflows](03-Guides/01-Common-Workflows.md) | Step-by-step guides for frequent use cases |
| [Advanced Configuration](03-Guides/02-Advanced-Configuration.md) | Tuning, overrides, advanced options |
| [Troubleshooting](03-Guides/03-Troubleshooting.md) | Common errors, diagnostics, FAQ |

## 4. API Reference

| Document | Description |
|----------|-------------|
| [Public API](04-API-Reference/01-Public-API.md) | Exported functions, classes, and their signatures |
| [Configuration Schema](04-API-Reference/02-Configuration-Schema.md) | Complete config schema with defaults and validation |
| [Types and Interfaces](04-API-Reference/03-Types-and-Interfaces.md) | Public TypeScript types and interfaces |

## 5. Modules

| Document | Description |
|----------|-------------|
| [client](05-Modules/01-client.md) | client — 226 LOC, file-level reference |
| [config](05-Modules/02-config.md) | config — 292 LOC, file-level reference |
| [cookie](05-Modules/03-cookie.md) | cookie — 288 LOC, file-level reference |
| [handshake](05-Modules/04-handshake.md) | handshake — 363 LOC, file-level reference |
| [main](05-Modules/05-main.md) | main — 267 LOC, file-level reference |
| [rustguard-kmod](05-Modules/06-rustguard-kmod.md) | Module rustguard-kmod — 3 files, directory-level reference |
| [rustguard-tun](05-Modules/07-rustguard-tun.md) | Module rustguard-tun — 5 files, directory-level reference |
| [server](05-Modules/08-server.md) | server — 577 LOC, file-level reference |
| [tunnel](05-Modules/09-tunnel.md) | tunnel — 580 LOC, file-level reference |

## 6. Development

| Document | Description |
|----------|-------------|
| [Source Tree](06-Development/01-Source-Tree.md) | Annotated source tree with module descriptions |
| [Build and Test](06-Development/02-Build-and-Test.md) | Build, test, lint, local CI |
| [Code Conventions](06-Development/03-Code-Conventions.md) | Style guide, patterns, anti-patterns |
| [Release Process](06-Development/04-Release-Process.md) | Versioning, release, publishing |
