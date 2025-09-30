# [System Name] System

## Overview

[2-3 sentences describing what this system does, its primary responsibility, and its role in the larger application]

<!-- TOC DECISION:
Add a Table of Contents if this document has:
- 5+ main sections
- 200+ lines
- Nested subsections needing navigation
- Reference content users will jump around in

Otherwise, skip the TOC to keep it lean.
-->

## Architecture

### System Boundaries
```
┌─────────────────────────────┐
│   [External Service/User]   │
└─────────────┬───────────────┘
              │ API/Events
              ▼
┌─────────────────────────────┐
│     THIS SYSTEM             │
│  ┌────────────────────┐     │
│  │ [Component A]      │     │
│  └──────────┬─────────┘     │
│             │               │
│  ┌──────────▼─────────┐     │
│  │ [Component B]      │     │
│  └────────────────────┘     │
└─────────────┬───────────────┘
              │ Output
              ▼
┌─────────────────────────────┐
│    [Downstream System]      │
└─────────────────────────────┘
```

### Tech Stack
- **[Core Technology]** - v[X.Y.Z] - [Docs](URL)
- **[Key Library]** - v[X.Y.Z] - [Docs](URL)
- **[Database/Service]** - [Docs](URL)

## Components

### Core Modules

#### [Component/Module Name]
**Location:** `src/[path]/`
**Purpose:** [What it does]
**Key Files:**
- `[filename].ts` - [Responsibility]
- `[filename].ts` - [Responsibility]

#### [Another Component]
**Location:** `src/[path]/`
**Purpose:** [What it does]
**Dependencies:** [What it needs]

### Data Models

```typescript
// Example key data structure
interface [ModelName] {
  id: string;
  [field]: Type;
  // ...
}
```

## API Reference

### Endpoints (for API systems)

#### `[METHOD] /path/to/endpoint`
**Purpose:** [What it does]
**Auth Required:** Yes/No
**Request:**
```json
{
  "field": "value"
}
```
**Response (200):**
```json
{
  "result": "data"
}
```

### Public Functions (for libraries/services)

#### `functionName(params)`
**Purpose:** [What it does]
**Parameters:**
- `param1: Type` - [Description]
**Returns:** `Type` - [What it returns]
**Example:**
```typescript
const result = functionName(value);
```

## Data Flow

### [Primary Flow Name]
1. **Input:** [Where data enters]
2. **Processing:** [What happens to it]
3. **Validation:** [How it's validated]
4. **Storage:** [Where it's stored]
5. **Output:** [Where it goes]

### Error Flow
1. **Detection:** [How errors are caught]
2. **Logging:** [What gets logged]
3. **Recovery:** [Recovery strategy]
4. **Response:** [What user/system sees]

## Configuration

### Required Environment Variables
```bash
# Authentication
AUTH_SECRET=        # JWT signing secret
AUTH_EXPIRES=       # Token expiration (e.g., "7d")

# Database
DATABASE_URL=       # Connection string

# External Services
API_KEY=           # Third-party API key
```

### Optional Configuration
- `CONFIG_OPTION` - [Default: value] - [Description]

## Integration Points

### Dependencies (What this system needs)
- **[System/Service Name]**
  - Used for: [Purpose]
  - Interface: [API/Direct/Event]
  - Fallback: [What happens if unavailable]

### Dependents (What needs this system)
- **[System/Component Name]**
  - Uses: [What functionality]
  - Critical: Yes/No

## State Management

### Persistent State
- **[Database/Table]:** [What's stored]
- **[Cache/Redis]:** [What's cached]

### Runtime State
- **[In-memory structure]:** [What it holds]
- **[Session data]:** [What's tracked]

## Security Considerations

### Authentication & Authorization
- [How auth is handled]
- [Permission model]

### Data Protection
- [Sensitive data handling]
- [Encryption methods]

### Rate Limiting
- [Limits applied]
- [Throttling strategy]

## Error Handling

### Common Errors
| Error | Cause | Resolution |
|-------|-------|------------|
| [ErrorCode] | [Why it happens] | [How to fix] |
| [ErrorCode] | [Why it happens] | [How to fix] |

### Monitoring & Alerts
- **Metrics:** [What's tracked]
- **Alerts:** [When triggered]
- **Logs:** [What's logged]

## Testing Strategy

### Unit Tests
**Location:** `[path]/[component].test.ts`
**Coverage Target:** [X]%
**Run:** `npm test [pattern]`

### Integration Tests
**Location:** `[path]/[feature].integration.test.ts`
**Setup:** [Required services/mocks]
**Run:** `npm run test:integration`

### E2E Tests
**Scenarios:** [What user flows are tested]
**Run:** `npm run test:e2e`

## Development

### Local Setup
```bash
# Install dependencies
npm install

# Set up environment
cp .env.example .env
# Edit .env with your values

# Run migrations
npm run migrate

# Start development
npm run dev
```

### Common Tasks
```bash
# Add new migration
npm run migrate:create [name]

# Run linter
npm run lint

# Type check
npm run typecheck
```

## Deployment

### Build Process
1. [Build step 1]
2. [Build step 2]

### Health Checks
- **Endpoint:** `/health`
- **Checks:** [What it verifies]

### Rollback Strategy
[How to rollback if deployment fails]

## Performance

### Optimization Points
- [Caching strategy]
- [Query optimization]
- [Lazy loading approach]

### Benchmarks
- [Operation]: ~[X]ms
- [Throughput]: [X] req/sec

## Migration Notes

### From Legacy System
[If replacing old system, migration strategy]

### Breaking Changes
[Version history of breaking changes]

## Troubleshooting

### Common Issues

#### [Problem Description]
**Symptoms:** [What you see]
**Cause:** [Why it happens]
**Solution:** [How to fix]

#### [Another Problem]
**Symptoms:** [What you see]
**Solution:** [How to fix]

## Future Considerations

### Planned Improvements
- [ ] [Improvement with rationale]
- [ ] [Another improvement]

### Technical Debt
- [Debt item and impact]

### Scaling Considerations
- [Current limitations]
- [Scaling strategy when needed]

---

**Status:** [Active|In Development|Deprecated]
**Owner:** [Team/Person responsible]
**Last Updated:** [Date]
**Related Docs:**
- [Link to related system]
- [Link to architecture docs]