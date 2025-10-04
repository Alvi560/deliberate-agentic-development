# Pre-Push Checks Template

Commands and checks that run before pushing code. Copy relevant blocks into `.agents/rules/PRE-PUSH-RULES.md` and adjust to your project.

## E2E Tests (Playwright)
```bash
# End-to-end browser tests
# One-time setup: npx playwright install --with-deps
npx playwright test --config=playwright.config.ts
```

## Optional Build Check (Frontend)
```bash
# Production build (optional locally, recommended in CI)
vite build
```

## Optional: Fail on Console Errors
```ts
// Example Playwright setup snippet
import { test } from '@playwright/test'

test.beforeEach(async ({ page }) => {
  const errors: string[] = []
  page.on('console', (msg) => {
    if (msg.type() === 'error') errors.push(msg.text())
  })
  test.info().attach('console-errors', { body: () => errors.join('\n') })
})
```

## Usage Notes
- Keep commits fast: format/lint/type/unit/integration belong in PRE-COMMIT-RULES.md.
- Commands must exit with code 0 for success.
- Run these checks on pre-push or in CI.
- Use this template as a starting point and tailor checks to your stack.

## Mode Behavior (FAST vs SLOW)
- SLOW mode (required):
  - Run the full Playwright E2E suite.
  - Run a production build (`vite build`).
  - Treat browser console errors as failures.
- FAST mode (skipped):
  - Skip pre-push checks. Optionally run E2E/build in CI.
