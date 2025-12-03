# Project Structure

## Directory Layout

```
spooky-job-hunter/
├── manifest.json                 # Manifest V3 configuration
├── src/
│   ├── popup/                    # React UI
│   │   ├── App.tsx
│   │   ├── components/           # UI components
│   │   ├── theme/                # Halloween theme configuration
│   │   └── index.tsx
│   ├── background/               # Service worker
│   │   ├── service-worker.ts
│   │   ├── orchestrator.ts
│   │   ├── platform-registry.ts
│   │   ├── profile-manager.ts
│   │   ├── cv-parser.ts
│   │   ├── relevance-filter.ts
│   │   ├── cover-letter-generator.ts
│   │   ├── application-manager.ts
│   │   ├── question-database.ts
│   │   └── storage-manager.ts
│   ├── content/                  # Content scripts
│   │   ├── platform-adapters/    # Job platform implementations
│   │   ├── form-filler.ts
│   │   └── utils/                # Automation utilities
│   ├── shared/                   # Shared code
│   │   ├── types.ts
│   │   ├── messages.ts
│   │   └── constants.ts
│   └── assets/                   # Static assets
└── public/
    └── index.html
```

## Key Components

### Popup UI (`src/popup/`)
React components for user interface including job search, results display, profile management, CV upload, application preview, history tracking, and settings.

### Background (`src/background/`)
Service worker containing core business logic:
- Search orchestration across platforms
- Platform registry management
- Profile and CV management
- AI services (CV parsing, relevance filtering, cover letter generation)
- Application tracking and question database

### Content Scripts (`src/content/`)
Scripts injected into job platform pages:
- Platform adapters (LinkedIn, Wellfound) implementing common interface
- Form auto-filler for application automation
- Automation utilities for DOM interaction

### Shared (`src/shared/`)
Common TypeScript types, message definitions, and constants used across all layers.

## Architectural Principles

- **Modularity**: Each job platform is a separate adapter class
- **Extensibility**: New platforms can be added without modifying core logic
- **Separation of Concerns**: Clear boundaries between UI, business logic, and data layers
- **Type Safety**: TypeScript interfaces define contracts between components
