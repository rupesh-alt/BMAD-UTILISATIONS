# Data Models - Directus

## Overview
Directus uses a dynamic schema approach where "Items" are the main content entities, defined by "Collections" and "Fields". However, the system has several core system tables/models managed by specific services.

## Core System Entities

Based on the services identified in `directus/api/src/services`, the following are the core system data models:

| Entity | Service | Description |
|--------|---------|-------------|
| **Activity** | `ActivityService` | Logs of all actions taken in the system |
| **Collections** | `CollectionsService` | Meta-definition of tables (collections) |
| **Fields** | `FieldsService` | Meta-definition of columns (fields) |
| **Files** | `FilesService` | Metadata for uploaded files (`directus_files`) |
| **Folders** | `FoldersService` | Virtual folder structure for files |
| **Permissions** | `PermissionsService` | Access rules linking Roles to Collections |
| **Presets** | `PresetsService` | User-specific view configurations |
| **Relations** | `RelationsService` | Foreign key definitions and metadata |
| **Revisions** | `RevisionsService` | History of changes to items |
| **Roles** | `RolesService` | User groups and access levels |
| **Settings** | `SettingsService` | Global project configuration (`directus_settings`) |
| **Shares** | `SharesService` | Configuration for shared links |
| **Users** | `UsersService` | System and App users (`directus_users`) |
| **Flows** | `FlowsService` | Automation logic definitions |
| **Operations** | `OperationsService` | Steps within flows |
| **Panels** | `PanelsService` | Widgets on dashboards |
| **Dashboards** | `DashboardsService` | Containers for panels |

## Dynamic Content (Items)

The `ItemsService` (`items.ts`) handles all user-defined content. It operates on dynamic schemas defined in `directus_collections` and `directus_fields`.

### Database Abstraction
The system utilizes a database abstraction layer (likely `knex` based on `database/` folder patterns) to support multiple SQL vendors (MySQL, Postgres, SQLite, etc.) without changing the core model definitions.
