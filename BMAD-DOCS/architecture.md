---
stepsCompleted: [1, 2, 3, 4, 5, 6, 7]
inputDocuments: [
  'c:/Users/DELL/Desktop/bmad/docs/project-overview.md',
  'c:/Users/DELL/Desktop/bmad/docs/api-contracts.md',
  'c:/Users/DELL/Desktop/bmad/docs/data-models.md',
  'c:/Users/DELL/Desktop/bmad/docs/integration-architecture.md',
  'c:/Users/DELL/Desktop/bmad/docs/source-tree-analysis.md'
]
workflowType: 'architecture'
project_name: 'bmad'
user_name: 'Rupesh'
date: '2026-02-04'
---

# Architecture Decision Document

_This document builds collaboratively through step-by-step discovery. Sections are appended as we work through each architectural decision together._

## Project Context Analysis

### Requirements Overview

**Functional Requirements:**
- **Dynamic Content Engine**: `ItemsService` providing CRUD on arbitrary SQL schemas.
- **Extensible System**: Modular architecture supporting 3rd party Interfaces, Modules, and Hooks.
- **Decoupled Admin App**: Vue.js SPA consuming the API via SDK (Headless architecture).
- **Real-time Collaboration**: WebSocket integration for live updates.

**Non-Functional Requirements:**
- **Ecosystem Accessibility**: Tech stack must lower the barrier for extension developers (Node/Vue/TS).
- **Database Agnosticism**: Abstraction layer (`knex`) to support multi-vendor SQL.
- **Type Safety**: Shared `packages/types` ensuring contract alignment between API/App.
- **Module Isolation**: `packages/` must follow layered architecture to prevent circular dependencies.

**Scale & Complexity:**
- **Primary domain**: Full-Stack Monorepo
- **Complexity level**: Enterprise / High
- **Architectural Drivers**:
    - **Extensibility > Raw Performance**: We optimize for plugin authors over micro-optimizations.
    - **Developer Experience > Separation**: We couple code in a Monorepo to improve DevX (Integrated Types).

### Technical Constraints & Dependencies
- **Strict Separation of Concerns**: API code cannot leak into App (browser bundle bloat).
- **Shared Kernel**: `packages/` is the critical dependency layer.
- **Frontend Stack**: Vue.js 3 + Vite (Hard constraint).
- **Backend Stack**: Node.js + Express (Hard constraint).

### Cross-Cutting Concerns Identified
- **Permissions (RBAC)**: Deeply integrated into the query builder layer.
- **Extensions Loading**: Dynamic loading of JS modules at runtime.
- **Internationalization (i18n)**: Required across App and system messages.

## Foundational Architecture (Starter Evaluation)

### Primary Technology Domain
**Full-Stack Monorepo** (Node.js API + Vue.js App)

### Selected Foundation: Directus Monorepo

**Rationale for Selection:**
*   **Brownfield Dominance**: Chosen for its "Database Mirroring" architecture, allowing control over existing SQL schemas (unlike Strapi/Payload).
*   **Accessible Extensibility**: Vue.js was selected over React to lower the barrier for creating custom Displays/Interfaces.
*   **Stability**: The Monorepo structure prevents "Plugin Version Mismatch" common in polyrepo CMS architectures.

**Architectural Decisions Provided by Foundation:**

**Language & Runtime:**
- **TypeScript**: Used universally across API, App, and Packages.
- **Node.js**: Runtime for API and CLI.

**Styling Solution:**
- **Vue Scoped CSS**: Primary styling mechanism.
- **Internal Design System**: Custom tokens and components in `app` modules.

**Build Tooling:**
- **Vite**: Used for the Frontend App (Fast HMR, Rollup bundling).
- **tsc / tsdown**: Used for compiling API and Packages.

**Testing Framework:**
- **Vitest**: Primary test runner found in `package.json` scripts.

**Code Organization:**
- **Monorepo**: Workspace-based structure.
- **Packages**: Shared logic extracted to `packages/`.

## Core Architectural Decisions

### Decision Priority Analysis

**Critical Decisions (Block Implementation):**
- **Data Abstraction**: Must use `knex` for all DB operations to maintain vendor neutrality.
- **Auth Strategy**: JWT Bearer tokens for stateless API interaction.
- **Module Boundary**: Strict separation of `api`, `app`, and `packages`.

### Data Architecture

*   **Database Choice**: **Agnostic (SQL)**. Supports Postgres, MySQL, SQLite, MSSQL via Knex.js.
*   **Data Modeling**: **Dynamic Schema**. Tables are runtime-defined in `directus_collections`.
*   **Validation**: **Joi / Zod**. Schema validation occurs in the Service layer before DB insertion.
*   **Migration**: **Knex Migrations**. System migrations are versioned; User schema changes represent distinct migrations.

### Authentication & Security

*   **Authentication**: **JWT (JSON Web Tokens)**. Short-lived access tokens + long-lived refresh tokens.
*   **Authorization**: **Granular RBAC**. Permissions are defined at the Field level in `directus_permissions`.
*   **Security Middleware**: **Helmet & CORS**. Standard Express security headers.
*   **Sanitization**: Input sanitization handled by the Abstraction Layer to prevent SQL injection.

### API & Communication Patterns

*   **Design Pattern**: **Hybrid REST & GraphQL**. The system auto-generates both APIs from the same Service layer logic.
*   **Error Handling**: **Standardized Exceptions**. Errors are mapped to HTTP codes (403 Forbidden, 400 Bad Request) via a global error handler.
*   **Rate Limiting**: **Redis/Memory**. Built-in rate limiter middleware.

### Frontend Architecture

*   **State Management**: **Pinia**. Modular stores for efficient state handling (User, Collections, Presets).
*   **Component Pattern**: **Atomic Design**. Reusable `components/` (atoms/molecules) vs `modules/` (organisms/pages).
*   **Routing**: **Vue Router**. Dynamic routing based on installed Modules.
*   **Build Optimization**: **Vite**. Leverages ES modules for rapid development and optimized Rollup builds.

### Infrastructure & Deployment

*   **Hosting**: **Docker / Node.js**. Stateless containers suitable for Kubernetes/ECS.
*   **CI/CD**: **GitHub Actions** (Inferred from standard practices).
*   **Logging**: **Pino / Winston**. Structured JSON logging for observability.

## Implementation Patterns & Consistency Rules

### Pattern Categories Defined

**Critical Conflict Points Identified:**
- **Naming Divergence**: Database vs API vs Code casing (snake_case vs camelCase).
- **Logic Placement**: Controller vs Service layer logic.
- **Frontend State**: Local ref vs Pinia store usage.

### Naming Patterns

**Database Naming Conventions:**
- **Tables**: `snake_case` (e.g., `directus_users`, `customer_orders`).
- **Columns**: `snake_case` (e.g., `first_name`, `is_active`).
- **Foreign Keys**: `singular_table_id` (e.g., `user_id`, `role_id`).

**API Naming Conventions:**
- **Endpoints**: Kebab-case plurals (e.g., `/items/my-custom-items`).
- **JSON Fields**: `snake_case` (Matches DB columns directly).
- **Status Codes**: 200 (OK), 204 (No Content), 400 (Bad Request), 403 (Forbidden).

**Code Naming Conventions:**
- **Variables/Functions**: `camelCase` (e.g., `getUserData`).
- **Components**: `PascalCase` (e.g., `UserCard.vue`).
- **Classes**: `PascalCase` (e.g., `ItemsService`).
- **Constants**: `SCREAMING_SNAKE_CASE` (e.g., `MAX_LOGIN_ATTEMPTS`).

### Structure Patterns

**Project Organization:**
- **Business Logic**: Must reside in `services/`. Controllers should only handle request parsing and response formatting.
- **Shared Logic**: If used by API and App, move to `packages/` workspace.
- **Tests**: Co-located `*.test.ts` files next to the source file.

**File Structure Patterns:**
- **Modules**: `index.ts` should export the public API of a folder.
- **Vue Components**: Single File Components (SFC) `<script setup lang="ts">`.

### Communication Patterns

**Event System Patterns:**
- **Hooks (API)**: `action` (async, non-blocking) vs `filter` (blocking, distinct payload/response).
- **Naming**: `scope.event` (e.g., `items.create`, `users.update`).

**State Management Patterns (App):**
- **Global State**: Use Pinia stores for data shared across pages.
- **Local State**: Use `ref()` / `reactive()` for component-local UI state.

### Process Patterns

**Error Handling:**
- **API**: Throw `ServiceException` (or subclass) in Services. Global error handler converts to JSON response.
- **App**: Use `useSystem()` for toast notifications on error.

**Loading States:**
- **UI**: Use `v-loading` directive or skeleton loaders for data fetching.

### Enforcement Guidelines

**All AI Agents MUST:**
1.  **Respect Layering**: Never query the DB directly from a Controller; use a Service.
2.  **Follow Casing**: Ensure JSON output matches `snake_case` database columns.
3.  **Type Everything**: No `any`. Use interfaces from `@directus/types`.

**Anti-Patterns:**
- **Direct DB Access**: Writing raw SQL in controllers.
- **Global Event Bus**: Using global emitting for tight coupling instead of Stores/Props.
- **Logic in Templates**: Complex logic in Vue templates instead of computed properties.

## Project Structure & Boundaries

### Complete Project Directory Structure

```bash
directus/
├── api/                        # Backend API (Node.js/Express)
│   ├── src/
│   │   ├── controllers/        # Request Handlers (Layer 1)
│   │   ├── services/           # Business Logic (Layer 2)
│   │   ├── database/           # DB Migrations & Seeds
│   │   ├── extensions/         # API Hooks & Endpoints
│   │   ├── utils/              # API-specific utilities
│   │   └── app.ts              # Entry Point
│   ├── package.json
│   └── tsconfig.json
├── app/                        # Admin App (Vue.js/Vite)
│   ├── src/
│   │   ├── components/         # Atomic UI Components
│   │   ├── modules/            # Feature Modules (Pages/Routes)
│   │   ├── stores/             # Pinia State Stores
│   │   ├── styles/             # Global CSS & Tokens
│   │   ├── interfaces/         # Custom Input Interfaces
│   │   └── displays/           # Custom Data Displays
│   ├── index.html
│   └── vite.config.ts
├── packages/                   # Shared Workspace Packages
│   ├── types/                  # Shared TypeScript Interfaces
│   ├── utils/                  # Universal Utilities (Lodash-like)
│   ├── constants/              # Shared Config Constraints
│   └── sdk/                    # JavaScript SDK
└── package.json                # Monorepo Config
```

### Architectural Boundaries

**API Boundaries:**
- **External**: REST (`/items/:collection`) & GraphQL (`/graphql`) endpoints.
- **Internal**: `Services` are the public API for the system logic; Controllers consume Services.
- **Extensions**: Custom Extensions are loaded at runtime and isolated in their own sandbox.

**Component Boundaries:**
- **Atomic**: Components in `src/components` must not contain business logic or API calls.
- **Modules**: Feature logic resides in `src/modules` and uses Stores for state.

**Data Boundaries:**
- **Access**: All DB access goes through `knex` (via `database/index.ts`). No raw SQL in controllers.
- **Schema**: System tables (`directus_*`) vs User tables (Dynamic).

### Integration Points

**Internal Communication:**
- **App -> API**: Communicates exclusively via the `@directus/sdk`.
- **API -> DB**: Communicates via `knex` Query Builder.
- **Service -> Service**: Direct class instantiation (e.g., `ItemsService` calling `PermissionsService`).

**External Integrations:**
- **Webhooks**: Event-driven HTTP POST on data changes.
- **Extensions**: npm packages installed to `node_modules` or `extensions/` folder.

**Data Flow:**
1.  **Request**: Client (App/SDK) sends JSON payload ->
2.  **Controller**: Parses Input & Validates (Joi) ->
3.  **Service**: Applies Business Logic & Permissions ->
4.  **Database**: Executes SQL via Knex.

## Architecture Validation Results

### Coherence Validation ✅

**Decision Compatibility:**
- **Monorepo & Types**: The decision to use a Monorepo perfectly supports the Requirement for Type Safety via shared `packages/types`.
- **Knex & Agnosticism**: The choice of Knex.js ensures the "Database Mirroring" capability works across all supported vendors without code changes.

**Structure Alignment:**
- The strict separation of `app` and `api` directories enforces the "Decoupled Admin App" constraint, preventing frontend code from leaking into the backend.

### Requirements Coverage Validation ✅

**Functional Requirements Coverage:**
- **Dynamic Content**: Fully supported by the `ItemsService` -> `Knex` pipeline.
- **Extensibility**: Supported by the `extensions/` directory and runtime loader.
- **Real-time**: Supported by the WebSocket event hooks in the API layer.

**Non-Functional Requirements Coverage:**
- **Accessibility**: Node/Vue stack lowers the barrier to entry for plugin devs (as requested).
- **Security**: Granular RBAC at the Layer 2 (Service) level ensures no data access bypass.

### Implementation Readiness Validation ✅

**Decision Completeness:**
- All critical decisions (Auth, DB, Stack) are defined.
- Versions are verified (Node, Vue 3, Vite).

**Structure Completeness:**
- The File Tree is explicit and maps to specific features.
- Integration boundaries are clear.

### Gap Analysis Results

**Gap**: **Testing Strategy Detail**. While we defined "Vitest", the specific Integration Test patterns for Database Transactions could be more detailed.
**Priority**: Medium. Can be addressed in the first Implementation Story.

### Architecture Completeness Checklist

**✅ Requirements Analysis**
- [x] Project context thoroughly analyzed
- [x] Scale and complexity assessed
- [x] Technical constraints identified

**✅ Architectural Decisions**
- [x] Critical decisions documented
- [x] Technology stack specified
- [x] Integration patterns defined

**✅ Implementation Patterns**
- [x] Naming conventions established
- [x] Structure patterns defined
- [x] Enforcement guidelines set

**✅ Project Structure**
- [x] Complete directory structure defined
- [x] Component boundaries established

### Architecture Readiness Assessment

**Overall Status:** READY FOR IMPLEMENTATION

**Confidence Level:** High

**First Implementation Priority:**
Initialize the Monorepo structure and validation of the Database Connection (`knex`) connectivity.
