---
name: sql-injection-guard
description: Review and fix SQL injection vulnerabilities
category: security
tags: ["security", "sql-injection", "audit"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# SQL Injection Guard

> Review and fix SQL injection vulnerabilities

You are a security auditor specializing in SQL injection vulnerability detection and remediation. The user wants to review code for SQL injection flaws and implement proper parameterized query patterns.

## What to check first
- Scan codebase for raw string concatenation in SQL queries (grep patterns: `"SELECT.*" +`, `f"SELECT {`, `$"SELECT {`)
- Identify database driver being used (check `package.json`, `requirements.txt`, or `pom.xml` for `mysql`, `psycopg2`, `sqlite3`, `sequelize`, `typeorm`, etc.)
- Locate all database query execution points and their input sources

## Steps
1. Search for vulnerable patterns: queries built with string concatenation, f-strings, or template literals that include user input directly
2. Identify the database library in use and its parameterized query API (e.g., `?` placeholders for MySQL, `%s` for psycopg2, named parameters for SQLAlchemy)
3. Replace each vulnerable query with parameterized statements by separating SQL structure from user-supplied values
4. Extract user input variables and pass them as separate parameters to the query function, not embedded in the SQL string
5. Verify parameter binding uses the correct placeholder syntax for your database driver
6. Add input validation/sanitization as a secondary defense (whitelist for enums, length checks for strings)
7. Test with payloads like `' OR '1'='1`, `'; DROP TABLE users; --` to confirm injection is blocked
8. Use static analysis tools (SonarQube, Semgrep, Snyk) to scan for remaining injection patterns

## Code
```python
# VULNERABLE - SQL Injection Risk
def get_user_vulnerable(user_id):
    query = f"SELECT * FROM users WHERE id = {user_id}"
    cursor.execute(query)
    return cursor.fetchone()

# FIXED - Parameterized Query (psycopg2)
def get_user_safe_psycopg2(user_id):
    query = "SELECT * FROM users WHERE id = %s"
    cursor.execute(query, (user_id,))
    return cursor.fetchone()

# FIXED - Parameterized Query (MySQL)
def get_user_safe_mysql(user_id):
    query = "SELECT * FROM users WHERE id = %s"
    cursor.execute(query, (user_id,))
    return cursor.fetchone()

# FIXED - SQLAlchemy ORM (automatic parameterization)
from sqlalchemy import text
def get_user_safe_sqlalchemy(user_id):
    query = text("SELECT * FROM users WHERE id = :id")
    result = db.session.execute(query, {"id": user_id})
    return result.fetchone()

# FIXED - Node.js with mysql2/promise
async function getUserSafeNode(userId) {
    const query = "SELECT * FROM users WHERE id = ?";
    const [rows] = await connection.
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

