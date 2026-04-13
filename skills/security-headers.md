---
name: security-headers
description: Configure security headers (HSTS, X-Frame-Options, etc.)
category: security
tags: ["security", "headers", "configuration"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# Security Headers

> Configure security headers (HSTS, X-Frame-Options, etc.)

You are a security engineer implementing HTTP security headers. The user wants to configure HSTS, X-Frame-Options, Content-Security-Policy, and other protective headers in their web application.

## What to check first
- Run `npm list express` (or your framework) to verify the server framework is installed
- Check if you're using a middleware system (Express, Fastify, etc.) or configuring at reverse-proxy level (Nginx, Apache)
- Review your current `package.json` to see if `helmet` is already listed as a dependency

## Steps
1. Install the `helmet` package: `npm install helmet` (it's the standard Node.js security headers middleware)
2. Import helmet at the top of your main server file: `const helmet = require('helmet');`
3. Register helmet as middleware before your route handlers: `app.use(helmet());` for Express
4. Customize individual headers by passing options to helmet — for example, `app.use(helmet.hsts({ maxAge: 31536000, includeSubDomains: true }));`
5. Set Content-Security-Policy with `helmet.contentSecurityPolicy({ directives: { defaultSrc: ["'self'"], scriptSrc: ["'self'", "trusted-cdn.com"] } })`
6. Configure X-Frame-Options with `helmet.frameguard({ action: 'deny' })` to block clickjacking
7. Test headers are present using `curl -i https://your-domain.com` and look for `Strict-Transport-Security`, `X-Frame-Options`, `Content-Security-Policy` in response
8. Adjust policies based on your app's third-party dependencies (fonts, scripts, images) — avoid overly restrictive CSP that breaks functionality

## Code
```javascript
const express = require('express');
const helmet = require('helmet');

const app = express();

// Apply helmet with default secure headers
app.use(helmet());

// Override or customize specific headers
app.use(helmet.hsts({
  maxAge: 31536000, // 1 year in seconds
  includeSubDomains: true,
  preload: true
}));

app.use(helmet.contentSecurityPolicy({
  directives: {
    defaultSrc: ["'self'"],
    scriptSrc: ["'self'", "'unsafe-inline'", "trusted-script-cdn.com"],
    styleSrc: ["'self'", "'unsafe-inline'", "fonts.googleapis.com"],
    imgSrc: ["'self'", "data:", "https:"],
    connectSrc: ["'self'", "api.example.com"],
    fontSrc: ["'self'", "fonts.gstatic.com"],
    objectSrc: ["'none'"],
    upgradeInsecureRequests: []
  }
}));

app.use(helmet.frameguard({
  action: 'deny' // or 'sameorigin' if you need iframe on same domain
}));

app
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

