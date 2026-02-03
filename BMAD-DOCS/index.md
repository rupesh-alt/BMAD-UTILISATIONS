# Directus Project Documentation

## Project Overview

- **Type:** Monorepo with 4 parts
- **Primary Language:** TypeScript
- **Architecture:** Headless CMS / BaaS
- **Generated:** 2026-02-04

## Quick Reference

- **Backend (API)**: Node.js / Express (`directus/api`) - [Contracts](./api-contracts.md)
- **Frontend (App)**: Vue.js 3 / Vite (`directus/app`) - [Components](./component-inventory.md)
- **SDK**: TypeScript Library (`directus/sdk`)

## Generated Documentation

- [Project Overview](./project-overview.md)
- [Source Tree Analysis](./source-tree-analysis.md)
- [API Contracts](./api-contracts.md) (35+ Controllers identified)
- [Data Models](./data-models.md) (Core System + Dynamic Items)
- [Component Inventory](./component-inventory.md) (App Modules & Component stats)
- [Integration Architecture](./integration-architecture.md)

## Existing Documentation
Found 60+ existing files, including:
- [AGENTS.md](../directus/AGENTS.md)
- [CONTRIBUTING.md](../directus/contributing.md)
- [API README](../directus/api/README.md)
- [Packages READMEs](../directus/packages/)

## Getting Started

1.  **Install Dependencies**: `pnpm install` (Root)
2.  **Setup Environment**: Configure `.env` files in `api` and `app`.
3.  **Run Development**: `pnpm dev` (Starts API + App)

For detailed contribution guidelines, see [CONTRIBUTING.md](../directus/contributing.md).
