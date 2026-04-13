---
name: rest-api-scaffold
description: Scaffold a complete REST API with CRUD operations
category: api
tags: ["api", "rest", "scaffold"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# REST API Scaffold

> Scaffold a complete REST API with CRUD operations

You are a backend API scaffolding expert. The user wants to generate a complete REST API structure with CRUD operations for a resource.

## What to check first
- Verify Node.js version with `node --version` (v14+ required)
- Check if Express.js is installed with `npm list express`
- Confirm you have a package.json file in your project root

## Steps
1. Install Express.js and required middleware: `npm install express body-parser cors uuid`
2. Create a `server.js` file in your project root to define the Express app and port configuration
3. Define a data model by creating a `models` directory and adding a resource schema (e.g., `User.js`)
4. Create a `routes` directory and build a resource router file (e.g., `userRoutes.js`) that handles all HTTP methods
5. Implement GET (all), GET (by id), POST (create), PUT (update), and DELETE (remove) handlers in the router
6. Use a simple in-memory array or Map for storage to avoid database setup
7. Mount the router in `server.js` using `app.use('/api/users', userRoutes)`
8. Start the server with `node server.js` and test endpoints using curl or Postman

## Code
```javascript
// server.js
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const { v4: uuidv4 } = require('uuid');

const app = express();
const PORT = 3000;

// Middleware
app.use(cors());
app.use(bodyParser.json());

// In-memory storage
let users = [
  { id: uuidv4(), name: 'Alice', email: 'alice@example.com' },
  { id: uuidv4(), name: 'Bob', email: 'bob@example.com' }
];

// CRUD Routes

// GET all users
app.get('/api/users', (req, res) => {
  res.json(users);
});

// GET user by ID
app.get('/api/users/:id', (req, res) => {
  const user = users.find(u => u.id === req.params.id);
  if (!user) return res.status(404).json({ error: 'User not found' });
  res.json(user);
});

// POST create new user
app.post('/api/users', (req, res) => {
  const { name, email } = req.body;
  if (!name || !email) {
    return res.status(400).json({ error: 'Name and email required' });
  }
  const newUser = { id: uuidv4(), name, email };
  users.push(newUser);
  res.status(201).json(newUser);
});

// PUT update user
app.put('/api/users/:id', (req, res) => {
  const user =
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

