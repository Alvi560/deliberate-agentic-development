# Pre-Commit Rules

Commands and checks that must pass before committing code.
The agent runs checks based on which files were modified.

## Frontend Checks
<!-- Only run if frontend files modified -->

### format:frontend
```bash
# Frontend formatting check
# Example: prettier --check "src/**/*.{js,jsx,ts,tsx}"
[YOUR_COMMAND_HERE]
```

### lint:frontend
```bash
# Frontend linting
# Example: eslint . --ext .js,.jsx,.ts,.tsx
[YOUR_COMMAND_HERE]
```

### typecheck:frontend
```bash
# Frontend type checking
# Example: tsc --noEmit
[YOUR_COMMAND_HERE]
```

### test:frontend
```bash
# Frontend tests (SLOW mode)
# Example: npm test -- --watchAll=false
[YOUR_COMMAND_HERE]
```

### build:frontend
```bash
# Frontend build validation
# Example: npm run build
[YOUR_COMMAND_HERE]
```

## Backend Checks
<!-- Only run if backend files modified -->

### format:backend
```bash
# Backend formatting check
# Example: black --check .
[YOUR_COMMAND_HERE]
```

### lint:backend
```bash
# Backend linting
# Example: ruff check
[YOUR_COMMAND_HERE]
```

### typecheck:backend
```bash
# Backend type checking
# Example: mypy .
[YOUR_COMMAND_HERE]
```

### test:backend
```bash
# Backend tests (SLOW mode)
# Example: pytest
[YOUR_COMMAND_HERE]
```

### build:backend
```bash
# Backend build validation
# Example: python -m py_compile **/*.py
[YOUR_COMMAND_HERE]
```

## Database Checks
<!-- Only run if database/migration files modified -->

### migrations
```bash
# Migration validation
# Example: npm run db:migrate:check
[YOUR_COMMAND_HERE]
```

## Usage Notes

- The agent determines which sections apply based on modified files:
  - Frontend files → Frontend Checks
  - Backend files → Backend Checks
  - Database files → Database Checks
  - Multiple types → Run all applicable sections

- Replace `[YOUR_COMMAND_HERE]` with actual commands
- Remove or add sections as needed
- Commands must exit with code 0 for success
- Test commands are required in SLOW mode
- In FAST mode, focus on format/lint for quick iteration