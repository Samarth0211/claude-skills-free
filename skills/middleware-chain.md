---
name: middleware-chain
description: Create and organize middleware chain
category: backend
tags: ["backend", "middleware", "architecture"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Middleware Chain

> Create and organize middleware chain

You are a backend architect specializing in middleware patterns. The user wants to create and organize a middleware chain that handles sequential request processing with proper error handling and execution flow.

## What to check first
- Verify your framework supports middleware (Express, Koa, FastAPI, Django) with `npm list express` or equivalent
- Confirm the order of middleware matters—check your routing setup with `app.use()` or middleware decorator order
- Review existing middleware in your `app.js` or `main.py` to avoid conflicts with global handlers

## Steps
1. Define middleware functions that accept `(req, res, next)` signature for Express or equivalent for your framework
2. Create a middleware registry object or array to organize and name each middleware piece
3. Use `app.use()` to mount middleware in order: authentication → logging → body parsing → validation
4. Chain middleware by calling `next()` in each handler to pass control to the next function
5. Implement error-handling middleware as the last middleware with `(err, req, res, next)` signature
6. Test the chain by checking that middleware executes in expected order using `console.log()` at each step
7. Separate concerns by storing middleware in dedicated files (e.g., `middleware/auth.js`, `middleware/validation.js`)
8. Use conditional middleware with route-specific mounting: `router.use(authMiddleware)` for protected routes only

## Code
```javascript
// middleware/logger.js
const logger = (req, res, next) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.path}`);
  next();
};

// middleware/auth.js
const auth = (req, res, next) => {
  const token = req.headers.authorization;
  if (!token) return res.status(401).json({ error: 'Unauthorized' });
  req.user = { id: 1 }; // Simplified
  next();
};

// middleware/validation.js
const validateRequest = (req, res, next) => {
  if (!req.body || Object.keys(req.body).length === 0) {
    return res.status(400).json({ error: 'Empty request body' });
  }
  next();
};

// middleware/errorHandler.js
const errorHandler = (err, req, res, next) => {
  console.error('Error:', err.message);
  res.status(err.status || 500).json({ error: err.message });
};

// app.js
const express = require('express');
const app = express();

// Import middleware
const { logger } = require('./middleware/logger');
const { auth } = require('./middleware/auth');
const { validateRequest } = require('./middleware/validation');
const { errorHandler } = require('./middleware/errorHandler');

// Global middleware chain (executes for every request)
app.use(express.json());
app.use(logger);

// Protected route chain
app.post('/api/data', auth
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

