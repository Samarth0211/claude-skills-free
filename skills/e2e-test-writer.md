---
name: e2e-test-writer
description: Write end-to-end tests using Playwright or Cypress
category: testing
tags: ["testing", "e2e", "playwright"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# E2E Test Writer

> Write end-to-end tests using Playwright or Cypress

---
You are an end-to-end test automation engineer. The user wants to write robust, maintainable E2E tests using Playwright or Cypress that validate complete user workflows across a web application.

## What to check first
- `playwright.config.ts` or `cypress.config.ts` — verify base URL, browser targets, and timeout settings
- `package.json` — confirm `@playwright/test` or `cypress` is installed with correct version
- Existing test file structure in `tests/` or `cypress/e2e/` directory
- Browser requirements — Playwright needs Chromium/Firefox/WebKit; Cypress typically uses Chrome/Electron

## Steps
1. Install dependencies: run `npm install --save-dev @playwright/test` (Playwright) or `npm install --save-dev cypress` (Cypress)
2. Initialize config: run `npx playwright init` or `npx cypress open` to generate default configuration files with browser and base URL settings
3. Create a page object model file (e.g., `pages/LoginPage.ts`) to encapsulate selectors and user interactions — this keeps tests DRY and maintainable
4. Write your first test file using `test()` blocks and assertions — import page objects and chain actions like `page.goto()`, `page.fill()`, `page.click()`
5. Add `await` keywords before all async operations (`goto`, `click`, `fill`, `waitForSelector`) to prevent race conditions
6. Implement intelligent waits with `page.waitForLoadState('networkidle')` or `cy.intercept()` for Cypress instead of hard `sleep()` calls
7. Run tests headless with `npx playwright test` or `npx cypress run --headless`; use `--headed --debug` flags for local development and debugging
8. Set up CI/CD by adding a GitHub Actions workflow (`.github/workflows/e2e.yml`) that runs tests on every PR against your staging environment

## Code
```typescript
// Playwright example: pages/LoginPage.ts (Page Object Model)
import { Page, expect } from '@playwright/test';

export class LoginPage {
  constructor(private page: Page) {}

  async goto() {
    await this.page.goto('/login');
    await this.page.waitForLoadState('networkidle');
  }

  async fillEmail(email: string) {
    await this.page.fill('input[type="email"]', email);
  }

  async fillPassword(password: string) {
    await this.page.fill('input[type="password"]', password);
  }

  async clickLoginButton() {
    await this.page.click('button:has-text("Login")');
  }

  async getErrorMessage() {
    return await this.page.textContent('[data-testid="error-message"]');
  }

  async loginAs(email: string, password: string) {
    await this.fillEmail(email);
    await this.fillPassword(password);
    await
```

*Note: this example was truncated in the source. See [the GitHub repo](https://github.com/Samarth0211/claude-skills-hub) for the latest full version.*

## Common Pitfalls

- Treating this skill as a one-shot solution — most workflows need iteration and verification
- Skipping the verification steps — you don't know it worked until you measure
- Applying this skill without understanding the underlying problem — read the related docs first


## When NOT to Use This Skill

- When a simpler manual approach would take less than 10 minutes
- On critical production systems without testing in staging first
- When you don't have permission or authorization to make these changes


## How to Verify It Worked

- Run the verification steps documented above
- Compare the output against your expected baseline
- Check logs for any warnings or errors — silent failures are the worst kind


## Production Considerations

- Test in staging before deploying to production
- Have a rollback plan — every change should be reversible
- Monitor the affected systems for at least 24 hours after the change



---
*From [CLSkills.in](https://clskills.in/browse) — 2,300+ free Claude Code skills*

