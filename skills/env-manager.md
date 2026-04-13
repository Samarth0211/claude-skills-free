---
name: env-manager
description: Manage environment variables across environments
category: devops
tags: ["devops", "environment", "variables"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# Env Manager

> Manage environment variables across environments

You are a DevOps engineer managing environment variables across development, staging, and production environments. The user wants to create a robust system for loading, validating, and switching between environment configurations without exposing secrets.

## What to check first
- Check if a `.env` file exists in your project root: `ls -la | grep .env`
- Verify Node.js version supports your chosen dotenv library: `node --version` (need v14+)
- Confirm `.env` files are in `.gitignore` to prevent accidental secret commits: `cat .gitignore | grep .env`

## Steps
1. Install `dotenv` package: `npm install dotenv` (loads `.env` files into `process.env`)
2. Create environment-specific files: `.env.development`, `.env.staging`, `.env.production`, and `.env.example`
3. In `.env.example`, list all required variables without values for documentation: `DATABASE_URL=` `API_KEY=` `NODE_ENV=`
4. At the very top of your application entry point (before other imports), add `require('dotenv').config({ path: \`.env.\${process.env.NODE_ENV || 'development'}\` })`
5. Create an `env-validator.js` file that checks all required variables are set using `process.env[key]`
6. Run the validator on startup: call it in your main server file before listening
7. Add npm scripts for each environment: `"dev": "NODE_ENV=development node server.js"`, `"start:staging": "NODE_ENV=staging node server.js"`
8. Use `process.env.VARIABLE_NAME` throughout your code to access loaded variables

## Code
```javascript
// env-validator.js
const requiredVars = [
  'DATABASE_URL',
  'API_KEY',
  'JWT_SECRET',
  'NODE_ENV',
  'PORT'
];

function validateEnvironment() {
  const missing = requiredVars.filter(
    variable => !process.env[variable]
  );

  if (missing.length > 0) {
    console.error(
      `❌ Missing required environment variables: ${missing.join(', ')}`
    );
    process.exit(1);
  }

  console.log(`✅ Environment validated for ${process.env.NODE_ENV}`);
}

module.exports = validateEnvironment;

// server.js (entry point)
require('dotenv').config({
  path: `.env.${process.env.NODE_ENV || 'development'}`
});

const validateEnvironment = require('./env-validator');
validateEnvironment();

const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/health', (req, res) => {
  res.json({ 
    status: 'ok',
    environment: process.env.NODE_ENV,
    dbConnected: !!process.env.
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

