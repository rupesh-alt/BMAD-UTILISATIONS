# Component Inventory - Directus App

## Overview
The Directus App (`directus/app`) is a Vue.js 3 application built with Vite. It features a modular architecture where core features are organized into "Modules" and UI elements are reusable "Components".

## Core Modules

Located in `directus/app/src/modules`, these represent the main functional areas of the application:

| Module | Description |
|--------|-------------|
| **Activity** | System activity and history view |
| **Content** | The main content management interface (Item browsing/editing) |
| **Files** | File asset library and management |
| **Insights** | Dashboard and data visualization support (generic/base) |
| **Settings** | Global project configuration (Fields, Roles, Flows, etc.) |
| **Users** | User management interface |
| **Visual** | (Likely related to specific visual editors or tools) |

## Component Categories

The `directus/app/src` directory contains several key component categories:

- **Components (`components/`)**: Generic reusable UI elements (Buttons, Inputs, Modals, etc.) - *Found 246 items*
- **Interfaces (`interfaces/`)**: Input components for specific field types (Text, WYSIWYG, Toggle, etc.) - *Found 257 items*
- **Displays (`displays/`)**: Read-only presentation components for field values - *Found 44 items*
- **Layouts (`layouts/`)**: Page structure templates (Table, Cards, Kanban) - *Found 32 items*
- **Panels (`panels/`)**: Widgets for Dashboards - *Found 38 items*
- **Views (`views/`)**: Top-level route views - *Found 88 items*

## State Management
State is managed using Pinia (as seen in `package.json`). Stores are located in `stores/` (22 stores found), handling data permissions, user session, and local app state.
