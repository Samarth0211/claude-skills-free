---
name: github-actions-setup
description: Create GitHub Actions workflow files
category: devops
tags: ["devops", "github-actions", "ci-cd"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# GitHub Actions Setup

> Create GitHub Actions workflow files

You are a DevOps engineer setting up CI/CD pipelines. The user wants to create and configure GitHub Actions workflow files for automated testing, building, and deployment.

## What to check first
- Verify the `.github/workflows/` directory exists in the repository root—create it if missing
- Check the repository has write permissions and the user has admin or maintainer access to enable Actions
- Review the target branch protection rules to ensure workflows can run on pull requests

## Steps
1. Create the `.github/workflows/` directory structure at the repository root (not nested inside `src/` or other subdirectories)
2. Create a workflow file with `.yml` or `.yaml` extension—e.g., `.github/workflows/ci.yml`
3. Define the `name` field to label the workflow in the Actions UI
4. Set `on:` trigger events (e.g., `push`, `pull_request`, `schedule`) with specific branches using `branches:` key
5. Define `jobs:` with a unique job identifier and `runs-on:` specifying the runner (e.g., `ubuntu-latest`, `macos-latest`)
6. Add `steps:` with `uses:` for pre-built actions and `run:` for shell commands, including `actions/checkout@v4` as the first step
7. Use `env:` at workflow or step level to inject environment variables
8. Commit the workflow file to the default branch—GitHub automatically enables it

## Code
```yaml
name: CI Pipeline

on:
  push:
    branches:
      - main
      - develop
  pull_request:
    branches:
      - main

env:
  NODE_VERSION: '18'

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [16.x, 18.x, 20.x]

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Run tests
        run: npm test -- --coverage

      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          files: ./coverage/lcov.info
          fail_ci_if_error: false

  build:
    runs-on: ubuntu-latest
    needs: test

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci
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

