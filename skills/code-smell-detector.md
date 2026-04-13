---
name: code-smell-detector
description: Scan the codebase for code smells — duplication, complexity, dead code, and poor naming
category: code-review
tags: ["review", "code-smells", "quality", "refactoring"]
difficulty: intermediate
version: 2.0.0
author: Claude Skills Hub
---

# Code Smell Detector

You are a code quality expert. Scan the codebase for code smells and produce a prioritized list of improvements.

## Step 1: Find Long Functions

```bash
# Find files with functions longer than 50 lines (likely need splitting)
# List the largest source files first
find . -name "*.ts" -o -name "*.tsx" -o -name "*.js" -o -name "*.jsx" -o -name "*.py" | grep -v node_modules | grep -v .next | xargs wc -l 2>/dev/null | sort -rn | head -20
```

Read the top 10 largest files. For each, identify:
- Functions longer than 50 lines — these should be split into focused, single-purpose functions
- Functions with more than 4 parameters — use a parameter object instead
- Functions with more than 3 levels of nesting — flatten with early returns or extract helper functions

## Step 2: Find Code Duplication

```bash
# Look for similar function signatures that might be duplicated
grep -rn --include="*.ts" --include="*.tsx" --include="*.js" "export function\|export const.*=.*=>" . --exclude-dir=node_modules --exclude-dir=.next | sort -t: -k3 | head -40

# Find similar import patterns (indicates shared utilities needed)
grep -rn --include="*.ts" --include="*.tsx" "^import" . --exclude-dir=node_modules --exclude-dir=.next | awk -F'from' '{print $2}' | sort | uniq -c | sort -rn | head -20
```

Read files that look like they might contain duplicate logic. Common duplication spots:
- Validation logic repeated across multiple API routes
- Error handling patterns copy-pasted between files
- Data transformation functions that do the same thing with slight variations
- Utility functions re-implemented in multiple places

For each duplication, suggest extracting to a shared utility.

## Step 3: Find Dead Code

```bash
# Find exported functions — check if they're imported anywhere
grep -rn --include="*.ts" --include="*.tsx" "export function\|export const\|export class\|export interface\|export type" . --exclude-dir=node_modules --exclude-dir=.next | head -40

# Find unused variables/imports (TypeScript)
npx tsc --noEmit --noUnusedLocals --noUnusedParameters 2>&1 | grep "declared but" | head -20

# Find files that aren't imported by anything
find . -name "*.ts" -o -name "*.tsx" | grep -v node_modules | grep -v .next | grep -v "page\.\|layout\.\|route\." | while read f; do
  base=$(basename "$f" | sed 's/\.[^.]*$//')
  count=$(grep -rn --include="*.ts" --include="*.tsx" "$base" . --exclude-dir=node_modules --exclude-dir=.next | grep -v "$f" | wc -l)
  if [ "$count" -eq 0 ]; then echo "POSSIBLY UNUSED: $f"; fi
done 2>/dev/null | head -15
```

For each potentially dead code finding, verify by reading the file and checking if it's used via dynamic imports, config references, or framework conventions (like Next.js page files).

## Step 4: Find Complexity Issues

Read source files and look for:

**Deeply Nested Conditionals**
- More than 3 levels of if/else nesting
- Fix: use early returns, guard clauses, or extract helper functions

**God Objects/Files**
- Single files that handle too many responsibilities
- Fix: split into focused modules with single responsibility

**Boolean Parameters**
- Functions that take boolean flags to control behavior: `processOrder(order, true, false, true)`
- Fix: use named options object: `processOrder(order, { validate: true, notify: false, async: true })`

**Magic Numbers & Strings**
- Hardcoded values like `if (retries > 3)` or `role === "admin"`
- Fix: extract to named constants: `const MAX_RETRIES = 3`

**Inappropriate Intimacy**
- Modules reaching deep into other modules' internal structures
- Fix: expose proper interfaces/APIs

## Step 5: Check Naming Quality

Read through the main source files and flag:

- Single-letter variables outside of loop iterators: `const d = getData()` should be `const dashboardData = getData()`
- Misleading names: function called `getUser` that also modifies the database
- Inconsistent naming: mixing `camelCase` and `snake_case` in the same codebase
- Vague names: `data`, `result`, `temp`, `info`, `handler`, `manager`, `utils`
- Boolean variables not starting with `is`, `has`, `should`, `can`

## Step 6: Output the Report

```
## Code Smell Report

**Files Scanned**: [count]
**Total Smells Found**: [count]

---

### High Priority (fix these first)

1. **[file:line]** — [Smell type]: [Title]
   **Why it matters**: [Impact on maintainability, readability, or bug risk]
   **Fix**:
   ```suggestion
   // Show the improved code
   ```

### Medium Priority

1. **[file:line]** — [Smell type]: [Title]
   **Suggestion**: [How to improve]

### Low Priority

1. **[file:line]** — [Smell type]: [Title]
   **Note**: [Brief suggestion]

### Dead Code to Remove

- `[file]` — [What's unused and can be safely deleted]

### Summary

| Smell Type       | Count |
|-----------------|-------|
| Long Functions   | ...   |
| Duplication      | ...   |
| Dead Code        | ...   |
| Complex Nesting  | ...   |
| Poor Naming      | ...   |
| Magic Numbers    | ...   |

**Top 3 refactoring priorities:**
1. [Most impactful]
2. [Second]
3. [Third]
```

## Rules

- Focus on smells that impact maintainability and bug risk, not personal style preferences.
- For every smell, explain WHY it's a problem, not just WHAT it is.
- Show the fix for high-priority items.
- Verify dead code findings — framework convention files (pages, routes, layouts) are NOT dead code.
- Group related smells together if they should be fixed as a single refactor.

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

