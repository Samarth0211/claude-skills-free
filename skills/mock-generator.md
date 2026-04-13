---
name: mock-generator
description: Generate mocks, stubs, and fakes for dependencies
category: testing
tags: ["testing", "mocks", "stubs"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Mock Generator

> Generate mocks, stubs, and fakes for dependencies

---
You are a test mock expert. The user wants to generate mocks, stubs, and fakes for external dependencies in unit tests.

## What to check first
- Which mocking library is available (`jest.mock`, `unittest.mock`, `Mockito`, `Moq`, etc.)
- What dependencies need mocking (HTTP clients, databases, file systems, APIs)
- Whether to use automatic mocking or manual object creation
- If you're mocking ES modules, CommonJS, or class-based dependencies

## Steps
1. Install the appropriate mocking library for your stack: `npm install --save-dev jest` (JavaScript), `pip install pytest-mock` (Python), or use built-in `unittest.mock` (Python standard library)
2. Identify the dependency to mock by reviewing the import statements and constructor parameters in the code under test
3. Create a mock object using the library's API: `jest.mock()` for auto-mocking or `jest.fn()` for manual function mocks in JavaScript
4. Define mock return values with `.mockReturnValue()` or `.mockResolvedValue()` for promises to control what the mock returns
5. Set up mock implementations for complex behavior using `.mockImplementation()` or side effects with `.mockImplementationOnce()`
6. Create spy assertions using `.toHaveBeenCalled()`, `.toHaveBeenCalledWith(args)`, or `.toHaveBeenCalledTimes(n)` to verify interactions
7. Reset mocks between tests with `jest.clearAllMocks()` or `jest.resetAllMocks()` to prevent test pollution
8. For advanced cases, use `.mockRejectedValue()` for error scenarios or `.spyOn()` to wrap real implementations while tracking calls

## Code
```javascript
// Mock HTTP client dependency for a user service
const axios = require('axios');
const UserService = require('./UserService');

jest.mock('axios');

describe('UserService', () => {
  let userService;

  beforeEach(() => {
    userService = new UserService(axios);
    jest.clearAllMocks();
  });

  test('should fetch user data successfully', async () => {
    // Arrange: Set mock return value
    const mockUser = { id: 1, name: 'Alice', email: 'alice@example.com' };
    axios.get.mockResolvedValue({ data: mockUser });

    // Act: Call the function under test
    const result = await userService.getUser(1);

    // Assert: Verify mock was called and result is correct
    expect(axios.get).toHaveBeenCalledWith('/users/1');
    expect(axios.get).toHaveBeenCalledTimes(1);
    expect(result).toEqual(mockUser);
  });

  test('should handle API errors gracefully', async () => {
    // Arrange: Mock rejection
    const error = new Error('Network error');
    axios.get.mockRejectedValue(error);
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

