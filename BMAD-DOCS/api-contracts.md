# API Contracts - Directus API

## Overview
This document outlines the API endpoints identified during the project scan of the `directus/api` backend. The API is built using Node.js/Express and serves as the core data engine.

## Controllers / Endpoints

The following controllers were identified in `directus/api/src/controllers`, corresponding to the main API resources:

| Resource | Controller File | Description |
|----------|----------------|-------------|
| **Access** | `access.ts` | Access control and dashboard access |
| **Activity** | `activity.ts` | Activity logs and history |
| **Assets** | `assets.ts` | File assets and internal resources |
| **Auth** | `auth.ts` | Authentication (Login, Refresh, SSO) |
| **Collections** | `collections.ts` | Data model collection management |
| **Comments** | `comments.ts` | Item comments and discussions |
| **Dashboards** | `dashboards.ts` | Insights dashboards |
| **Extensions** | `extensions.ts` | Marketplace and custom extensions |
| **Fields** | `fields.ts` | Field configuration for collections |
| **Files** | `files.ts` | File management and media |
| **Flows** | `flows.ts` | Automation flows |
| **Folders** | `folders.ts` | File folder organization |
| **GraphQL** | `graphql.ts` | GraphQL Endpoint |
| **Items** | `items.ts` | Dynamic CRUD for all content |
| **Metrics** | `metrics.ts` | System metrics |
| **Notifications** | `notifications.ts` | User notifications |
| **Operations** | `operations.ts` | Flow operations |
| **Panels** | `panels.ts` | Dashboard panels |
| **Permissions** | `permissions.ts` | Role-based access control rules |
| **Policies** | `policies.ts` | Security policies |
| **Presets** | `presets.ts` | User preferences and bookmarks |
| **Relations** | `relations.ts` | Schema relationships |
| **Revisions** | `revisions.ts` | Content versioning |
| **Roles** | `roles.ts` | User roles |
| **Schema** | `schema.ts` | System schema snapshot/diff |
| **Server** | `server.ts` | Server info and health (info/ping) |
| **Settings** | `settings.ts` | Global project settings |
| **Shares** | `shares.ts` | Public sharing of content |
| **Translations** | `translations.ts` | UI translations |
| **Users** | `users.ts` | User management |
| **Versions** | `versions.ts` | Content versions (save points) |
| **Webhooks** | `webhooks.ts` | (Inferred from generic patterns) |

## API Structure

The API follows a standard RESTful pattern:
- base URL: `/` (or configured root)
- Resource URLs: `/<resource>`
- Item URLs: `/<resource>/<id>`

Authentication is handled via the `Auth` controller, likely returning JWTs for Bearer authentication.
