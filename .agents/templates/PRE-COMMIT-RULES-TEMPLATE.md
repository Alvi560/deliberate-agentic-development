# Pre-Commit Rules Template

Commands and checks that must pass before committing code.
The agent runs checks based on which files were modified.

## Frontend Checks
<!-- Only run if frontend files modified -->

### format:frontend
```bash
# Frontend formatting check
# Example: prettier --check "src/**/*.{js,jsx,ts,tsx}"
# [YOUR_COMMAND_HERE]
```

### lint:frontend
```bash
# Frontend linting
# Example: eslint . --ext .js,.jsx,.ts,.tsx
# [YOUR_COMMAND_HERE]
```

### typecheck:frontend
```bash
# Frontend type checking
# Example: tsc --noEmit
# [YOUR_COMMAND_HERE]
```

### test:frontend
```bash
# Frontend tests (SLOW mode only)
# Example: npm test -- --watchAll=false
# [YOUR_COMMAND_HERE]
```

### build:frontend
```bash
# Frontend build validation
# Example: npm run build
# [YOUR_COMMAND_HERE]
```

## Backend Checks
<!-- Only run if backend files modified -->

### format:backend
```bash
# Backend formatting check
# Example: black --check .
# Example: ruff format --check .
# [YOUR_COMMAND_HERE]
```

### lint:backend
```bash
# Backend linting
# Example: pylint src/
# Example: ruff check .
# [YOUR_COMMAND_HERE]
```

### typecheck:backend
```bash
# Backend type checking
# Example: mypy .
# [YOUR_COMMAND_HERE]
```

### test:backend
```bash
# Backend tests (SLOW mode only)
# Example: pytest
# [YOUR_COMMAND_HERE]
```

## Database Checks
<!-- Only run if database files modified -->

### validate:database
```bash
# Database migration validation
# Example: alembic check
# Example: npm run db:validate
# [YOUR_COMMAND_HERE]
```

## Integration Checks
<!-- Run for full-stack changes -->

### test:integration
```bash
# Integration tests (SLOW mode only)
# Example: npm run test:integration
# [YOUR_COMMAND_HERE]
```

### test:e2e
```bash
# E2E tests (SLOW mode, E2E testing issues only)
# Example: npm run test:e2e
# Example: playwright test
# [YOUR_COMMAND_HERE]
```

## Usage Notes
- Commands with [YOUR_COMMAND_HERE] are placeholders — replace with actual commands.
- Comments (#) are ignored by the agent.
- Agent determines which sections to run based on modified files.
- All applicable commands must pass (exit code 0) before commit.
- SLOW mode runs test commands, FAST mode skips them.
- E2E tests are typically configured in PRE-PUSH-RULES.md to keep commits fast; include them here only if you want E2E on commit.
