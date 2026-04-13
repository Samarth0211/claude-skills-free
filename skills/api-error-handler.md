---
name: api-error-handler
description: Create standardized API error handling
category: api
tags: ["api", "errors", "handling"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# API Error Handler

> Create standardized API error handling

You are an API developer building robust error handling. The user wants to create standardized API error handling that catches, formats, and responds with consistent error structures across all endpoints.

## What to check first
- Verify your framework (Express, Fastify, etc.) and check `package.json` for the current version
- Confirm you have a logging library available (winston, pino, or console methods)

## Steps
1. Define an `ApiError` class that extends `Error` with `statusCode`, `message`, and `details` properties
2. Create an error handler middleware that catches errors from route handlers and formats them consistently
3. Add HTTP status code mapping (400 for validation, 401 for auth, 404 for not found, 500 for server errors)
4. Implement error logging inside the middleware before responding to the client
5. Wrap async route handlers with a try-catch utility function to catch unhandled Promise rejections
6. Define error response schema with `success`, `error`, `statusCode`, and `timestamp` fields
7. Apply the error handler middleware as the last middleware in your app stack
8. Test with route that throws different error types to verify consistent formatting

## Code
```javascript
// errorHandler.js - Complete API Error Handling Solution

class ApiError extends Error {
  constructor(statusCode, message, details = {}) {
    super(message);
    this.statusCode = statusCode;
    this.details = details;
    this.timestamp = new Date().toISOString();
  }
}

// Status code mapping for common scenarios
const errorStatusMap = {
  'VALIDATION_ERROR': 400,
  'UNAUTHORIZED': 401,
  'FORBIDDEN': 403,
  'NOT_FOUND': 404,
  'CONFLICT': 409,
  'INTERNAL_SERVER_ERROR': 500,
};

// Async route handler wrapper
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Standardized error response formatter
const formatErrorResponse = (error) => {
  return {
    success: false,
    error: {
      message: error.message,
      code: error.code || 'INTERNAL_SERVER_ERROR',
      details: error.details || {},
      timestamp: error.timestamp || new Date().toISOString(),
    },
    statusCode: error.statusCode || 500,
  };
};

// Express error handling middleware
const errorHandlerMiddleware = (err, req, res, next) => {
  // Convert standard Error to ApiError if needed
  if (!(err instanceof ApiError)) {
    const statusCode = err.statusCode || 500;
    err = new ApiError(
      statusCode,
      err.message || 'Internal Server Error',
      { originalError: process.env.NODE_ENV === 'development' ? err.stack : undefined }
    );
  }

  // Log error details
  console
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

