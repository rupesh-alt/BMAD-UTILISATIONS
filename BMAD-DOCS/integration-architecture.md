# Integration Architecture

## Overview
Directus is designed as a decoupled system where the **App** (Frontend) acts as a client to the **API** (Backend), typically communicating via the **SDK**.

## Communication Flow

```mermaid
graph TD
    User[User Browser]
    App[Directus App (Vue.js)]
    SDK[Directus SDK]
    API[Directus API (Node.js)]
    DB[(SQL Database)]

    User --> App
    App --> SDK
    SDK -- REST/GraphQL --> API
    API --> DB
```

### 1. App → API
- **Protocol**: HTTP/HTTPS (REST or GraphQL)
- **Authentication**: JWT (Bearer Token)
- **Format**: JSON
- **Mechanism**: The App uses the `@directus/sdk` internally to make requests to the API.

### 2. SDK → API
- The SDK (`directus/sdk`) provides a typed wrapper around the API endpoints.
- It handles authentication state, request formatting, and response parsing.

### 3. Shared Code (`packages/`)
- Both App and API consume shared logic from `directus/packages`.
- Examples: 
    - `@directus/types`: Shared TypeScript definitions.
    - `@directus/utils`: Shared helper functions.
    - `@directus/constants`: Shared capability flags and constants.

## Integration Points
- **API Root**: The App must be configured with the API's root URL.
- **Extensions**: Custom extensions (Interfaces, Displays, Modules) are loaded by the App but often have backend counterparts (Hooks, Endpoints) loaded by the API.
