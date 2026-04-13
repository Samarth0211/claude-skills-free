---
name: interactive-cli
description: Create interactive CLI prompts (Inquirer.js)
category: cli
tags: ["cli", "interactive", "inquirer"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Interactive CLI

> Create interactive CLI prompts (Inquirer.js)

You are a Node.js CLI developer building interactive command-line interfaces. The user wants to create interactive prompts using Inquirer.js to gather user input with validation, choices, and conditional flows.

## What to check first
- Run `npm list inquirer` to confirm Inquirer.js is installed (version 8+ recommended)
- Verify Node.js version is 12+ with `node --version`

## Steps
1. Install Inquirer.js with `npm install inquirer` (or `npm install inquirer@latest` for v9+)
2. Import the Inquirer prompt function at the top of your CLI file using CommonJS or ES modules
3. Define a prompt array with question objects, each containing `type`, `name`, `message`, and optionally `choices` or `default`
4. Use `inquirer.prompt()` and chain with `.then()` or `await` to handle user responses
5. Add `validate` callbacks to enforce input rules (non-empty strings, number ranges, email format)
6. Use `when` conditionals to show/hide questions based on previous answers
7. Chain multiple `inquirer.prompt()` calls or nest them in `.then()` blocks for sequential flows
8. Handle errors with `.catch()` or try/catch blocks around async/await

## Code
```javascript
const inquirer = require('inquirer');

async function runInteractiveCLI() {
  try {
    // First prompt: basic questions
    const answers = await inquirer.prompt([
      {
        type: 'input',
        name: 'projectName',
        message: 'Project name:',
        default: 'my-app',
        validate: (input) => input.length > 0 || 'Project name cannot be empty'
      },
      {
        type: 'list',
        name: 'framework',
        message: 'Choose a framework:',
        choices: ['React', 'Vue', 'Angular', 'Svelte']
      },
      {
        type: 'confirm',
        name: 'typescript',
        message: 'Use TypeScript?',
        default: false
      },
      {
        type: 'checkbox',
        name: 'features',
        message: 'Select features:',
        choices: ['ESLint', 'Prettier', 'Testing', 'CI/CD'],
        validate: (choices) => choices.length > 0 || 'Select at least one feature'
      },
      {
        type: 'password',
        name: 'apiKey',
        message: 'Enter API key:',
        mask: '*'
      },
      {
        type: 'number',
        name: 'port',
        message: 'Server port:',
        default: 3000,
        validate: (num) => (num > 1024 && num < 65535) || 'Port must be 1024-65535'
      },
      {
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

