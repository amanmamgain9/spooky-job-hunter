# Technology Stack

## Core Technologies

- **Chrome Extension**: Manifest V3 architecture
- **Frontend**: React with TypeScript
- **UI Framework**: React-based popup interface
- **AI Integration**: OpenAI API (or compatible services)
- **Storage**: Chrome Storage API
- **Messaging**: Chrome Message Passing API

## Architecture Layers

1. **Presentation Layer**: React components with Halloween theming
2. **Business Logic Layer**: Platform adapters, search orchestration, AI services
3. **Data Layer**: Application tracking, profile storage, question database
4. **Infrastructure Layer**: Chrome APIs, content scripts, service workers

## Key Patterns

- **Platform Adapters**: Each job platform (LinkedIn, Wellfound) implemented as separate adapter class with common interface
- **Content Scripts**: Injected into job platform pages for DOM interaction and data extraction
- **Service Worker**: Background script for orchestration and business logic
- **Message Passing**: Communication between popup, background, and content scripts

## File Organization

```
src/
├── popup/          # React UI components
├── background/     # Service worker and business logic
├── content/        # Content scripts and platform adapters
├── shared/         # Shared types, messages, constants
└── assets/         # Icons and images
```

## Development Notes

- Use TypeScript for type safety across all components
- Content scripts use CSS selectors for DOM manipulation
- MutationObserver or polling for detecting page load completion
- Chrome tabs API for navigation and page management
- Automation utilities library for common tasks (waiting, clicking, form filling)
