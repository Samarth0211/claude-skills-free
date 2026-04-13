---
name: smart-commit
description: Generate conventional commit messages by analyzing staged changes
category: git
tags: ["git", "commit", "automation"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# Smart Commit

> Generate conventional commit messages by analyzing staged changes

---
You are a Git automation expert. The user wants to generate conventional commit messages by analyzing staged changes in Git.

## What to check first
- Run `git diff --cached --name-only` to see which files are staged
- Run `git diff --cached` to inspect the actual changes in the staging area
- Verify you're in a Git repository with `git rev-parse --git-dir`
- Check if there are any staged changes with `git diff --cached --quiet`

## Steps
1. Run `git diff --cached` to get the full diff of staged changes in unified format
2. Parse the diff to extract file names, added/removed lines, and change summary
3. Categorize changes (feat, fix, docs, style, refactor, test, chore) based on file types and keywords in the diff
4. Count the number of files changed, lines added/removed using `git diff --cached --stat`
5. Generate a conventional commit subject line: `<type>(<scope>): <description>` (max 72 chars)
6. Build the body with details: list affected files, summarize changes, mention breaking changes if present
7. Output the complete commit message to stdout
8. Optionally validate the message against conventional commits spec before returning

## Code
```python
#!/usr/bin/env python3
import subprocess
import re
from typing import Tuple, List

def get_staged_diff() -> str:
    """Get the full diff of staged changes."""
    try:
        result = subprocess.run(
            ['git', 'diff', '--cached'],
            capture_output=True,
            text=True,
            check=True
        )
        return result.stdout
    except subprocess.CalledProcessError:
        raise RuntimeError("Failed to get staged diff. Ensure you're in a git repository.")

def get_staged_stats() -> str:
    """Get diff statistics for staged changes."""
    try:
        result = subprocess.run(
            ['git', 'diff', '--cached', '--stat'],
            capture_output=True,
            text=True,
            check=True
        )
        return result.stdout
    except subprocess.CalledProcessError:
        return ""

def categorize_commit_type(diff: str, files: List[str]) -> str:
    """Categorize the commit type based on changes."""
    if not diff:
        return "chore"
    
    # Check for breaking changes
    if "BREAKING CHANGE" in diff:
        return "feat"
    
    # Analyze file extensions and content
    has_test = any('test' in f or 'spec' in f for f in files)
    has_doc = any(f.endswith(('.md', '.rst', '.txt')) for f in files)
    has_style = any(f.endswith(('.css', '.scss', '.less')) for f in files)
    
    # Check diff content for patterns
    if re.search(r'^\+.*(?:bug|fix|error|issue
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

