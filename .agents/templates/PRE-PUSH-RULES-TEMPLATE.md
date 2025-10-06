# Pre-Push Rules Template

Commands and checks that run before pushing code. Copy relevant blocks into `.agents/rules/PRE-PUSH-RULES.md` and adjust to your project.

## E2E Tests
```bash
# End-to-end tests (browser, API, or integration)
# [YOUR_COMMAND_HERE]
# Examples:
#   npx playwright test
#   npm run test:e2e
#   pytest tests/e2e
```

## Production Build
```bash
# Production build validation
# [YOUR_COMMAND_HERE]
# Examples:
#   npm run build
#   vite build
#   python -m build
```

## Usage Notes
- Keep commits fast: format/lint/type/unit/integration belong in PRE-COMMIT-RULES.md.
- Commands must exit with code 0 for success.
- Run these checks on pre-push or in CI.
- Use this template as a starting point and tailor checks to your stack.

## Mode Behavior (FAST vs SLOW)
- **SLOW mode (required):**
  - Run full E2E test suite
  - Run production build
- **FAST mode (skipped):**
  - Skip pre-push checks locally. Optionally run E2E/build in CI.
