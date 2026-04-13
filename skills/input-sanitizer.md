---
name: input-sanitizer
description: Add input sanitization to prevent injection attacks
category: security
tags: ["security", "sanitization", "injection"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Input Sanitizer

> Add input sanitization to prevent injection attacks

You are a security engineer. The user wants to add input sanitization to prevent SQL injection, XSS, command injection, and other common injection attacks.

## What to check first
- Identify all user input entry points in your application (form fields, URL parameters, API request bodies, file uploads)
- Run `npm list` to see if you already have sanitization libraries like `sanitize-html`, `xss`, or `sql.js` installed
- Check your framework's built-in escaping/parameterization features (Express middleware, Django ORM, etc.)

## Steps
1. Install a sanitization library appropriate for your context: `npm install sanitize-html xss validator` for Node.js applications
2. Identify the type of injection risk at each input point (database queries need parameterized statements; HTML output needs HTML escaping; shell commands need argument arrays)
3. For database inputs, use parameterized queries or prepared statements instead of string concatenation—never build SQL strings with user input
4. For HTML content that users submit, use `sanitize-html` to strip dangerous tags while preserving safe formatting
5. For URL parameters and form fields, validate against whitelist patterns using `validator.js` before processing
6. For command execution, pass arguments as array elements to functions like `child_process.execFile()` instead of shell string interpolation
7. Implement a centralized sanitization middleware in your application framework that runs on all incoming requests
8. Test your sanitization by attempting common payloads: `<script>alert('xss')</script>`, `'; DROP TABLE users; --`, `$(rm -rf /)` and verify they are neutralized

## Code
```javascript
const sanitizeHtml = require('sanitize-html');
const validator = require('validator');
const { execFile } = require('child_process');

// Database: Use parameterized queries (example with sqlite3)
const sqlite3 = require('sqlite3');
const db = new sqlite3.Database(':memory:');

// ✓ SAFE: Parameterized query
function getUserById(userId) {
  return new Promise((resolve, reject) => {
    db.get(
      'SELECT * FROM users WHERE id = ?',
      [userId], // Separated from SQL string
      (err, row) => {
        if (err) reject(err);
        else resolve(row);
      }
    );
  });
}

// HTML sanitization: Strip dangerous tags
function sanitizeUserContent(htmlInput) {
  return sanitizeHtml(htmlInput, {
    allowedTags: ['b', 'i', 'em', 'strong', 'a', 'p', 'br'],
    allowedAttributes: {
      'a': ['href']
    },
    allowedSchemes: ['http', 'https']
  });
}

// URL/form validation: Whitelist patterns
function validateEmail(email) {
  if (!validator.isEmail(email)) {
    throw new Error('Invalid email format');
  }
  return email
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

