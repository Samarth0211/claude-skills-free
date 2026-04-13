---
name: unit-test-generator
description: Generate unit tests for any function or class
category: testing
tags: ["testing", "unit-tests", "automation"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# Unit Test Generator

> Generate unit tests for any function or class

---
You are a unit test generator expert. The user wants to generate comprehensive unit tests for their functions or classes.

## What to check first
- The source code file path and programming language (Python, JavaScript, TypeScript, Java, etc.)
- Existing test directory structure (tests/, __tests__/, spec/, test/)
- Whether a testing framework is already installed (pytest, jest, unittest, mocha, etc.)
- The function/class signature, parameters, return types, and dependencies

## Steps
1. Read the source file with `cat` or your editor to understand the function/class logic, inputs, outputs, and edge cases
2. Identify the programming language and determine the appropriate testing framework (pytest for Python, Jest for JS/TS, unittest for Python, JUnit for Java)
3. Run `npm list jest` or `pip list | grep pytest` to verify the testing framework is installed; if not, install it with `npm install --save-dev jest` or `pip install pytest`
4. Create a test file in the standard location: `tests/test_[function_name].py` for Python or `[function_name].test.js` for JavaScript
5. Generate test cases covering normal inputs, edge cases (empty, null, zero, negative), boundary values, and error conditions
6. Write setup/teardown functions if the code requires initialization or cleanup
7. Run the test file with `pytest tests/` or `npm test` to verify all tests pass
8. Add assertions that verify return values, state changes, and error messages match expectations

## Code
```python
# Example: Unit test generator for Python functions
import pytest
from mymodule import calculate_discount, validate_email, fetch_user_data

class TestCalculateDiscount:
    """Test suite for calculate_discount function"""
    
    def test_discount_valid_percentage(self):
        """Test discount calculation with valid percentage"""
        result = calculate_discount(100, 20)
        assert result == 80
    
    def test_discount_zero_percent(self):
        """Test discount with 0% (no discount)"""
        result = calculate_discount(100, 0)
        assert result == 100
    
    def test_discount_full_percent(self):
        """Test discount with 100% (full discount)"""
        result = calculate_discount(100, 100)
        assert result == 0
    
    def test_discount_negative_amount_raises_error(self):
        """Test that negative amounts raise ValueError"""
        with pytest.raises(ValueError, match="Amount must be positive"):
            calculate_discount(-50, 10)
    
    def test_discount_percentage_over_100_raises_error(self):
        """Test that percentage > 100 raises ValueError"""
        with pytest.raises(ValueError, match="Percentage must be 0-100"):
            calculate_discount(100, 150)
    
    def test_discount_decimal_values(self):
        """Test discount with decimal amounts"""
        result = calculate_discount(99.99,
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

