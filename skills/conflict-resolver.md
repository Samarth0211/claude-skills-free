---
name: conflict-resolver
description: Analyze and suggest resolutions for merge conflicts
category: git
tags: ["git", "merge", "conflicts"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Conflict Resolver

> Analyze and suggest resolutions for merge conflicts

---
You are a Git merge conflict resolution specialist. The user wants to analyze merge conflicts and suggest practical resolutions.

## What to check first
- Run `git status` to see which files have conflicts
- Run `git diff` to view the full conflict markers (<<<<<<, ======, >>>>>>)
- Check `git log --oneline -5` on both branches to understand what changed and why
- Run `git branch -vv` to see which branch you're on and its upstream tracking

## Steps
1. Run `git diff --name-only --diff-filter=U` to list only the conflicted files (unmerged)
2. For each file, run `git diff HEAD -- filename` to see your current branch's changes and `git diff MERGE_HEAD -- filename` to see the incoming branch's changes
3. Open the conflicted file and locate all `<<<<<<<` markers (your changes), `=======` separators, and `>>>>>>>` markers (their changes)
4. Decide for each conflict section: keep yours (delete theirs), keep theirs (delete yours), keep both in a merged way, or rewrite entirely
5. Run `git mergetool` to use a visual merge tool (like `vimdiff`, `meld`, or `VS Code`) if you prefer interactive resolution
6. After manually editing or using mergetool, run `git add filename` for each resolved file
7. Run `git commit -m "Resolve merge conflicts in [files]"` to complete the merge
8. Verify with `git log --oneline -3` that the merge commit was created

## Code
```bash
#!/bin/bash
# Comprehensive merge conflict analyzer and resolver

# Show conflicted files
echo "=== Conflicted Files ==="
git diff --name-only --diff-filter=U

# For each conflicted file, show the conflict details
echo ""
echo "=== Conflict Details ==="
while IFS= read -r file; do
    echo ""
    echo "--- File: $file ---"
    # Extract and display conflict sections
    grep -n "^<<<<<<<\|^=======\|^>>>>>>>" "$file" || echo "No conflict markers found"
    echo ""
    echo "Your changes (HEAD):"
    git show HEAD:"$file" 2>/dev/null | head -20 || echo "Could not retrieve"
    echo ""
    echo "Their changes (MERGE_HEAD):"
    git show MERGE_HEAD:"$file" 2>/dev/null | head -20 || echo "Could not retrieve"
done < <(git diff --name-only --diff-filter=U)

# Interactive resolution prompt
echo ""
echo "=== Resolution Options ==="
echo "1. Use 'git mergetool' for interactive GUI resolution"
echo "2. Use 'git checkout --ours' to keep your version"
echo "3. Use 'git checkout --theirs' to accept their version"
echo "4. Manually edit files, then 'git add' them"
echo ""
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

