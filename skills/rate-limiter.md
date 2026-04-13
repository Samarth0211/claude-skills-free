---
name: rate-limiter
description: Add rate limiting to API endpoints
category: api
tags: ["api", "rate-limiting", "security"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Rate Limiter

> Add rate limiting to API endpoints

You are a backend API security engineer. The user wants to add rate limiting to API endpoints to prevent abuse and ensure fair resource usage.

## What to check first
- Verify your framework (Express, FastAPI, Django, etc.) and confirm which rate-limiting library is compatible
- Check if you're using centralized storage (Redis) or in-memory storage for tracking request counts
- Identify which endpoints need rate limiting and what limits make sense (requests per minute/hour)

## Steps
1. Install the appropriate rate-limiting package for your framework (e.g., `npm install express-rate-limit` for Express, or `pip install slowapi` for FastAPI)
2. Choose a store backend: Redis for distributed systems, or memory store for single-server applications
3. Define rate limit rules with specific windows (e.g., 100 requests per 15 minutes) and identify key identifiers (IP address, user ID, or API key)
4. Apply the middleware/decorator to specific routes or globally to all endpoints
5. Configure response behavior: set HTTP status code (429 Too Many Requests), custom error messages, and retry headers
6. Test rate limiting by making requests exceeding the limit and verify the 429 response and `Retry-After` header
7. Monitor rate limit hits using logging to identify patterns and adjust thresholds if needed
8. Implement skip logic to exclude health checks, webhooks, or authenticated admin requests from rate limits

## Code
```javascript
// Express.js example with express-rate-limit
const express = require('express');
const rateLimit = require('express-rate-limit');
const RedisStore = require('rate-limit-redis');
const redis = require('redis');
const app = express();

// Create Redis client
const redisClient = redis.createClient({
  host: 'localhost',
  port: 6379,
});

// General API limiter: 100 requests per 15 minutes
const generalLimiter = rateLimit({
  store: new RedisStore({
    client: redisClient,
    prefix: 'rl:general:',
  }),
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100,
  standardHeaders: true, // Return rate limit info in `RateLimit-*` headers
  legacyHeaders: false, // Disable `X-RateLimit-*` headers
  message: 'Too many requests, please try again later.',
  statusCode: 429,
  skip: (req) => req.path === '/health', // Skip health checks
  keyGenerator: (req) => req.ip, // Use IP as key
});

// Strict limiter for login endpoint: 5 attempts per 15 minutes
const loginLimiter = rateLimit({
  store: new RedisStore({
    client: redisClient,
    prefix: 'rl:login:',
  }),
  windowMs: 15 * 60 * 1000,
  max: 5,
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

