---
name: dependency-audit
description: Audit dependencies for known vulnerabilities
category: security
tags: ["security", "audit", "dependencies"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# Dependency Audit

> Audit dependencies for known vulnerabilities

You are a security engineer. The user wants to audit their project dependencies for known vulnerabilities using industry-standard tools.

## What to check first
- Run `npm list --depth=0` to see what package manager and dependencies you're working with
- Check if `package-lock.json` or `yarn.lock` exists to confirm dependency lock status
- Verify Node.js version with `node --version` (npm audit requires Node 6.4.0+)

## Steps
1. Run `npm audit` to scan `package-lock.json` against the npm vulnerability database and get a report with severity levels (critical, high, moderate, low)
2. Review the output table showing package name, vulnerability type, severity, and affected versions
3. Run `npm audit fix` to automatically patch vulnerabilities where safe patches exist (updates to compatible versions)
4. For vulnerabilities `npm audit fix` cannot resolve, run `npm audit fix --force` to update major versions (use cautiously and test thoroughly)
5. If using Yarn instead of npm, run `yarn audit` for the same scanning, then `yarn upgrade` to patch
6. Add `npm audit` to your CI/CD pipeline by including it in your build script to catch new vulnerabilities before deployment
7. For detailed JSON output suitable for automated processing, run `npm audit --json` and parse the results
8. Periodically re-run audits and keep dependencies updated with `npm update` to stay ahead of newly disclosed vulnerabilities

## Code
```javascript
// audit-dependencies.js - Automated audit script
const { execSync } = require('child_process');
const fs = require('fs');

function auditDependencies() {
  console.log('🔍 Starting dependency audit...\n');

  try {
    // Run npm audit with JSON output for parsing
    const auditOutput = execSync('npm audit --json', {
      encoding: 'utf-8',
      stdio: ['pipe', 'pipe', 'pipe']
    });

    const auditData = JSON.parse(auditOutput);
    const metadata = auditData.metadata;

    console.log(`📊 Audit Results:`);
    console.log(`   Total dependencies: ${metadata.totalDependencies}`);
    console.log(`   Vulnerabilities found: ${metadata.vulnerabilities.total}`);
    console.log(`   Critical: ${metadata.vulnerabilities.critical || 0}`);
    console.log(`   High: ${metadata.vulnerabilities.high || 0}`);
    console.log(`   Moderate: ${metadata.vulnerabilities.moderate || 0}`);
    console.log(`   Low: ${metadata.vulnerabilities.low || 0}\n`);

    // Save detailed report
    fs.writeFileSync(
      'audit-report.json',
      JSON.stringify(auditData, null, 2)
    );
    console.log('✅ Detailed report saved to audit-report.json');

    // Exit with error code if critical/high vulnerabilities exist
    if (
      (
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

