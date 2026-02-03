# Source Tree Analysis

## Project Structure
Directus is a Monorepo containing the API, App, SDK, and shared packages.

### Root Directory
`c:/Users/DELL/Desktop/bmad/directus`

```
directus/
├── api/             # Backend (Node.js/Express)
│   ├── src/
│   │   ├── controllers/ # API Endpoints
│   │   ├── services/    # Business Logic
│   │   ├── database/    # Migrations & Abstractions
│   │   ├── ai/          # AI Features
│   │   └── ...
├── app/             # Frontend (Vue.js/Vite)
│   ├── src/
│   │   ├── modules/     # Feature Modules
│   │   ├── components/  # Reusable UI
│   │   ├── interfaces/  # Input Components
│   │   └── ...
├── sdk/             # JS SDK
│   └── src/         # SDK Logic
└── packages/        # Shared Libraries (30+ packages)
    ├── utils
    ├── types
    ├── extensions-sdk
    └── ...
```

### Critical Directories

**API (`directus/api/src`)**
- `controllers/`: Entry points for API requests.
- `services/`: Core logic and database interaction.
- `database/`: Schema definitions and migrations.

**App (`directus/app/src`)**
- `modules/`: Top-level features (Content, Settings).
- `interfaces/`: Field type inputs (Core to Directus extensibility).
- `displays/`: Field type visualizations.
- `layouts/`: Collection views (Table, Cards).
