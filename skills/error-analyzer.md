---
name: error-analyzer
description: Analyze error messages and suggest fixes
category: debugging
tags: ["debugging", "errors", "analysis"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# Error Analyzer

> Analyze error messages and suggest fixes

You are a debugging expert. The user wants to analyze error messages and receive specific, actionable fix suggestions.

## What to check first
- Capture the complete error message including stack trace, line numbers, and context
- Identify the error type (SyntaxError, TypeError, ReferenceError, NetworkError, etc.)
- Note the programming language and framework/runtime where the error occurred

## Steps
1. Extract the error name and message from the first line of the error output
2. Scan the stack trace to identify the file path and exact line number where the error originated
3. Look for the root cause indicator — check for undefined variables, missing imports, type mismatches, or syntax violations
4. Cross-reference the error code or message with common patterns for that language/framework
5. Identify the immediate trigger — what line of code caused the error to throw
6. Determine the fix category: missing dependency, incorrect API usage, data type issue, or logic error
7. Provide the specific code change needed, with exact syntax and imports if applicable
8. Suggest preventative measures like type checking, linting, or unit tests

## Code
```javascript
// Error Analyzer - analyzes error messages and suggests fixes
const analyzeError = (errorMessage) => {
  const errorPatterns = {
    'undefined is not a function': {
      category: 'TypeError',
      cause: 'Calling a method on undefined or null',
      fixes: [
        'Check if the object exists before calling the method',
        'Verify the method name is spelled correctly',
        'Ensure the object is properly initialized'
      ],
      example: 'const user = { name: "John" };\nconst email = user.getEmail?.(); // Use optional chaining'
    },
    'Cannot read property': {
      category: 'TypeError',
      cause: 'Accessing property on null or undefined',
      fixes: [
        'Add null/undefined checks before accessing properties',
        'Use optional chaining operator (?.) for safe access',
        'Validate object structure before use'
      ],
      example: 'const value = obj?.prop?.nested ?? "default"; // Safe property access'
    },
    'is not defined': {
      category: 'ReferenceError',
      cause: 'Variable used before declaration or out of scope',
      fixes: [
        'Check for typos in variable name',
        'Ensure variable is declared with const/let/var',
        'Verify variable is in correct scope'
      ],
      example: 'const myVar = 10; // Declare before use\nconsole.log(myVar);'
    },
    'Unexpected token': {
      category: 'SyntaxError',
      cause: 'Malformed code structure or invalid syntax',
      fixes: [
        'Check for missing parentheses, brackets, or braces',
        'Verify proper quote matching (single vs double)',
        'Look for missing semicolons or commas'
      ],
      example:
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

