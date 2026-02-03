# Project Overview: Directus

## Executive Summary
Directus is a real-time API and App dashboard for managing SQL database content. It acts as a headless CMS and/or a Backend-as-a-Service (BaaS). It is an open-source monorepo project built with Node.js/Vue.js.

## Technology Stack

| Part | Type | Tech Stack |
|------|------|------------|
| **API** | Backend | Node.js, Express, TypeScript, SQL (Agnostic) |
| **App** | Web | Vue.js 3, Vite, Pinia, TypeScript |
| **SDK** | Library | TypeScript (Universal) |
| **Packages**| Shared | TypeScript |

## Architecture Type
**Monolithic Monorepo**: While code is split into packages, the core `api` and `app` are tightly coupled in versioning and often deployed together (though they can be decoupled). The architecture is **Headless**, separating data storage/management (API) from presentation/consumption (App/SDK).

## Key Features
- **Dynamic Schema**: Mirrors SQL database structure instantly.
- **Extensible**: Highly modular extension system (Interfaces, Displays, Layouts, Modules).
- **Agnostic**: Works with most major SQL databases (Postgres, MySQL, SQLite, etc.).
- **Auth & Access**: Granular role-based access control (RBAC).

## Documentation Index
- [Source Tree Analysis](./source-tree-analysis.md)
- [API Contracts](./api-contracts.md)
- [Data Models](./data-models.md)
- [Component Inventory](./component-inventory.md)
- [Integration Architecture](./integration-architecture.md)
