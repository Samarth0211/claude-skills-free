---
name: pytest-setup
description: Configure pytest with fixtures, plugins, and coverage
category: python
tags: ["python", "pytest", "testing"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# Pytest Setup

> Configure pytest with fixtures, plugins, and coverage

You are a Python testing expert. The user wants to configure pytest with fixtures, plugins, and coverage reporting.

## What to check first
- Run `python --version` to confirm Python 3.6+ is installed
- Run `pip list | grep pytest` to see what pytest packages are already installed
- Check if a `conftest.py` file exists in your project root

## Steps
1. Install pytest and coverage plugins: `pip install pytest pytest-cov pytest-mock pytest-xdist`
2. Create a `conftest.py` file in your project root to define shared fixtures
3. Define session-scoped fixtures for expensive setup (database, API mocks) using `@pytest.fixture(scope="session")`
4. Define function-scoped fixtures for test isolation using `@pytest.fixture(scope="function")`
5. Add fixture dependencies by including fixture names as function parameters
6. Create a `pytest.ini` file to configure test discovery patterns and coverage options
7. Run pytest with coverage: `pytest --cov=src --cov-report=html` to generate an HTML coverage report
8. Use `pytest-mock` by injecting the `mocker` fixture parameter to mock external dependencies

## Code
```python
# conftest.py - shared fixtures and configuration
import pytest
from unittest.mock import MagicMock
import tempfile
import os

# Session-scoped fixture: expensive setup, runs once per test session
@pytest.fixture(scope="session")
def test_db():
    """Mock database connection for entire test session"""
    db = MagicMock()
    db.connect = MagicMock(return_value=True)
    db.query = MagicMock(return_value={"id": 1, "name": "test"})
    yield db
    db.close()

# Function-scoped fixture: runs before each test, fresh instance
@pytest.fixture(scope="function")
def sample_user():
    """Create a test user object for each test"""
    return {
        "id": 1,
        "username": "testuser",
        "email": "test@example.com"
    }

# Fixture with dependency on another fixture
@pytest.fixture
def api_client(test_db):
    """Create an API client that uses the test database"""
    client = MagicMock()
    client.db = test_db
    client.get_user = MagicMock(return_value={"id": 1})
    return client

# Fixture with temporary file handling
@pytest.fixture
def temp_config_file():
    """Create a temporary config file for testing"""
    with tempfile.NamedTemporaryFile(mode='w', delete=False, suffix='.json') as f:
        f.write('{"key": "value"}')
        temp_path = f.name
    yield temp_path
    os.unlink(temp_path)

# Example test using fixtures
def test_user_creation(sample_user):
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

