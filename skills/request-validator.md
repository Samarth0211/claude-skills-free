---
name: request-validator
description: Add request validation middleware (Zod, Joi)
category: api
tags: ["api", "validation", "middleware"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Request Validator

> Add request validation middleware (Zod, Joi)

You are an API developer implementing request validation middleware. The user wants to add request validation to an Express.js API using Zod or Joi to validate incoming request bodies, query parameters, and route parameters.

## What to check first
- Confirm Express.js is installed: `npm list express`
- Choose your validation library: `npm list zod` or `npm list joi`
- Check if you have a middleware directory structure in your project

## Steps
1. Install Zod (or Joi as alternative): `npm install zod` — Zod is more TypeScript-friendly and smaller
2. Import Zod at the top of your middleware file: `import { z } from 'zod'`
3. Define a schema object using `z.object()` with typed fields (e.g., `z.string()`, `z.number()`)
4. Create a middleware factory function that accepts a schema and returns `(req, res, next) => {}`
5. Call `schema.parse(req.body)` inside the middleware wrapped in try-catch
6. On validation success, call `next()` to pass control to the route handler
7. On validation error (ZodError), catch it and send a 400 response with error details
8. Attach the middleware to specific routes: `app.post('/users', validateRequest(userSchema), handler)`

## Code
```javascript
import express from 'express';
import { z } from 'zod';

const app = express();
app.use(express.json());

// Define validation schemas
const createUserSchema = z.object({
  body: z.object({
    name: z.string().min(1, 'Name is required').max(100),
    email: z.string().email('Invalid email format'),
    age: z.number().int().positive().optional(),
  }),
  query: z.object({
    notify: z.enum(['true', 'false']).optional(),
  }).optional(),
  params: z.object({}).optional(),
});

const updateUserSchema = z.object({
  body: z.object({
    name: z.string().min(1).max(100).optional(),
    email: z.string().email().optional(),
  }),
  params: z.object({
    id: z.string().uuid('Invalid user ID'),
  }),
});

// Validation middleware factory
const validateRequest = (schema) => {
  return (req, res, next) => {
    try {
      const validated = schema.parse({
        body: req.body,
        query: req.query,
        params: req.params,
      });
      
      // Attach validated data to request for use in handler
      req.validated = validated;
      next();
    } catch (error) {
      if (error instanceof z.ZodError) {
        const formattedErrors = error.errors.map((err) => ({
          path: err.path.join('.'),
          message: err.message,
          code
```

*Note: this example was truncated in the source. See [the GitHub repo](https://github.com/Samarth0211/claude-skills-hub) for the latest full version.*

## Common Pitfalls

- Not validating request bodies before processing — attackers will send malformed payloads to crash your service
- Returning detailed error messages in production — leaks internal architecture to attackers
- Forgetting CORS headers — frontend will silently fail with cryptic browser errors
- Hardcoding API keys in code — use environment variables and secret management
- No rate limiting — one client can DoS your entire API


## When NOT to Use This Skill

- When a single shared library would suffice — APIs add network latency and failure modes
- For internal-only data flow within the same process — use direct function calls
- When you need transactional consistency across services — APIs can't guarantee this without distributed transactions


## How to Verify It Worked

- Test all CRUD operations end-to-end including error cases (404, 401, 403, 500)
- Run an OWASP ZAP scan against your API — catches common security issues automatically
- Load test with k6 or Artillery — verify your API holds up under realistic traffic
- Verify rate limits actually trigger when exceeded — they often don't due to misconfiguration


## Production Considerations

- Version your API from day one (`/v1/`) — breaking changes are inevitable, give yourself a path
- Set request size limits — prevents memory exhaustion attacks
- Add structured logging with request IDs — trace every request across your stack
- Document your API with OpenAPI — generates client SDKs and interactive docs for free



---
*From [CLSkills.in](https://clskills.in/browse) — 2,300+ free Claude Code skills*

