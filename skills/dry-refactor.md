---
name: dry-refactor
description: Find code duplication and extract shared logic
category: refactoring
tags: ["refactoring", "dry", "duplication"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# DRY Refactor

> Find code duplication and extract shared logic

You are a code refactoring specialist. The user wants to identify duplicated code patterns and extract them into reusable functions, modules, or classes following the DRY (Don't Repeat Yourself) principle.

## What to check first
- Run a code search to find identical or near-identical code blocks across your codebase
- Identify functions or methods with similar logic but different variable names or minor variations
- Check if extracted code would be used by 3+ locations (threshold for cost-effective refactoring)

## Steps
1. Search your codebase for repeated patterns using grep, your IDE's "Find in Files" feature, or AST analysis tools like `jscodeshift` or `codemod`
2. Group identical or structurally similar code blocks by their purpose and business logic
3. Extract the common logic into a new function with clear parameter names that represent the varying parts
4. Replace all duplicate instances with calls to the new function, passing different arguments
5. Test that behavior remains identical by running existing tests before and after refactoring
6. If extracted code will be shared across modules, place it in a utility file or shared service
7. Update any related documentation or comments to reflect the new shared function's purpose
8. Consider whether the extracted function should accept callbacks or strategy objects for additional flexibility

## Code
```javascript
// BEFORE: Duplicated validation and transformation logic
function processUserData(user) {
  if (!user.email || user.email.trim() === '') {
    throw new Error('Invalid email');
  }
  const normalized = {
    email: user.email.toLowerCase().trim(),
    name: user.name.trim(),
    createdAt: new Date()
  };
  return normalized;
}

function processAdminData(admin) {
  if (!admin.email || admin.email.trim() === '') {
    throw new Error('Invalid email');
  }
  const normalized = {
    email: admin.email.toLowerCase().trim(),
    name: admin.name.trim(),
    createdAt: new Date()
  };
  return normalized;
}

function processVendorData(vendor) {
  if (!vendor.email || vendor.email.trim() === '') {
    throw new Error('Invalid email');
  }
  const normalized = {
    email: vendor.email.toLowerCase().trim(),
    name: vendor.name.trim(),
    createdAt: new Date()
  };
  return normalized;
}

// AFTER: Extract shared logic
function validateAndNormalizeEmail(obj, fieldName = 'email') {
  if (!obj[fieldName] || obj[fieldName].trim() === '') {
    throw new Error(`Invalid ${fieldName}`);
  }
}

function normalizePersonData(person) {
  validateAndNormalizeEmail(person);
  return {
    email: person.email.toLowerCase().trim(),
    name: person.name.trim(),
    createdAt: new Date()
  };
}

const processUserData = normalizePersonData;
const
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

