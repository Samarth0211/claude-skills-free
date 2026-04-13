---
name: python-logging
description: Configure structured logging for Python applications
category: python
tags: ["python", "logging", "monitoring"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# Python Logging

> Configure structured logging for Python applications

You are a Python logging specialist. The user wants to configure structured logging for Python applications with proper handlers, formatters, and log levels.

## What to check first
- Verify Python version with `python --version` (logging module available in all modern versions)
- Check if `logging` is already imported in your project with `grep -r "import logging" .`

## Steps
1. Import the `logging` module at the top of your application entry point
2. Create a logger instance using `logging.getLogger(__name__)` to get a module-specific logger
3. Set the root logger level using `logging.basicConfig()` or configure handlers explicitly
4. Create a `StreamHandler` for console output and a `FileHandler` for file persistence
5. Define a `Formatter` with structured format strings using LogRecord attributes like `%(asctime)s`, `%(name)s`, `%(levelname)s`, and `%(message)s`
6. Attach the formatter to each handler using `handler.setFormatter(formatter)`
7. Add handlers to your logger using `logger.addHandler(handler)`
8. Call logger methods (`logger.info()`, `logger.error()`, `logger.debug()`) throughout your code at appropriate severity levels

## Code
```python
import logging
import logging.handlers
from datetime import datetime

# Configure root logger
def setup_logging(log_file='app.log', level=logging.INFO):
    """
    Configure structured logging with console and file handlers.
    
    Args:
        log_file: Path to log file
        level: Logging level (DEBUG, INFO, WARNING, ERROR, CRITICAL)
    """
    
    # Create logger
    logger = logging.getLogger()
    logger.setLevel(level)
    
    # Clear existing handlers to avoid duplicates
    logger.handlers.clear()
    
    # Define formatter with structured fields
    formatter = logging.Formatter(
        '%(asctime)s | %(name)s | %(levelname)-8s | %(message)s',
        datefmt='%Y-%m-%d %H:%M:%S'
    )
    
    # Console handler (StreamHandler)
    console_handler = logging.StreamHandler()
    console_handler.setLevel(logging.DEBUG)
    console_handler.setFormatter(formatter)
    logger.addHandler(console_handler)
    
    # File handler (RotatingFileHandler for log rotation)
    file_handler = logging.handlers.RotatingFileHandler(
        log_file,
        maxBytes=10485760,  # 10 MB
        backupCount=5
    )
    file_handler.setLevel(logging.DEBUG)
    file_handler.setFormatter(formatter)
    logger.addHandler(file_handler)
    
    return logger

# Module-level logger
logger = logging.getLogger(__name__)

# Example usage in application
if __name__ == '__main__':
    # Initialize logging at application startup
    setup_logging(log_file='myapp.log
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

