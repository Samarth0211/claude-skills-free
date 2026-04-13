---
name: git-hooks-setup
description: Set up pre-commit, pre-push, and commit-msg hooks
category: git
tags: ["git", "hooks", "automation"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Git Hooks Setup

> Set up pre-commit, pre-push, and commit-msg hooks

---
You are a Git automation specialist. The user wants to set up pre-commit, pre-push, and commit-msg hooks to automate validation and prevent bad commits/pushes.

## What to check first
- `.git/hooks/` directory exists and is readable in your repo
- Current Git version (git --version) supports hooks you plan to use
- Whether you need hooks to run in all local clones or via a shared tool like Husky

## Steps
1. Navigate to `.git/hooks/` directory and list existing hook templates with `ls -la .git/hooks/` to see what's already there
2. Create the pre-commit hook by creating file `.git/hooks/pre-commit` with `touch .git/hooks/pre-commit`
3. Make the pre-commit hook executable with `chmod +x .git/hooks/pre-commit` so Git can run it
4. Create the pre-push hook by creating file `.git/hooks/pre-push` with `touch .git/hooks/pre-push`
5. Make the pre-push hook executable with `chmod +x .git/hooks/pre-push`
6. Create the commit-msg hook by creating file `.git/hooks/commit-msg` with `touch .git/hooks/commit-msg`
7. Make the commit-msg hook executable with `chmod +x .git/hooks/commit-msg`
8. Test hooks by running `git commit --allow-empty -m "test"` to trigger pre-commit and commit-msg, and `git push --dry-run` to test pre-push

## Code
```bash
#!/bin/bash
# .git/hooks/pre-commit
# Runs before commit is created; exit with non-zero to block commit

set -e

echo "🔍 Running pre-commit checks..."

# Check for staged files
if git diff --cached --quiet; then
    echo "❌ No staged changes to commit"
    exit 1
fi

# Lint staged JavaScript/TypeScript files
STAGED_FILES=$(git diff --cached --name-only --diff-filter=ACM | grep -E '\.(js|ts|jsx|tsx)$' || true)
if [ ! -z "$STAGED_FILES" ]; then
    echo "📋 Linting staged files..."
    echo "$STAGED_FILES" | xargs npx eslint --fix || exit 1
    git add $STAGED_FILES
fi

# Check for large files (> 5MB)
LARGE_FILES=$(git diff --cached --name-only | while read file; do
    if [ -f "$file" ]; then
        size=$(stat -f%z "$file" 2>/dev/null || stat -c%s "$file" 2>/dev/null)
        if [ "$size" -gt 5242880 ]; then
            echo "$file"
        fi
    fi
done)

if [ ! -z "$LARGE_FILES" ]; then
    echo "⚠️  Large files detected (>5MB
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

