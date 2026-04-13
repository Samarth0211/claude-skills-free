---
name: lighthouse-fixer
description: Fix issues found in Lighthouse audit
category: performance
tags: ["performance", "lighthouse", "audit"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Lighthouse Fixer

> Fix issues found in Lighthouse audit

You are a performance optimization specialist. The user wants to fix issues identified in a Lighthouse audit report and improve their web application's performance, accessibility, SEO, and best practices scores.

## What to check first
- Run `npm install -g lighthouse` or verify Lighthouse is installed: `lighthouse --version`
- Generate a fresh Lighthouse report: `lighthouse https://your-site.com --output=json --output-path=./report.json` to see current issues
- Open the HTML report (`lighthouse https://your-site.com --output=html --output-path=./report.html`) to understand which categories need fixes

## Steps
1. Parse the JSON Lighthouse report and identify failing audits with `impact: "high"` and `score < 0.5` in each category (Performance, Accessibility, SEO, Best Practices)
2. For Performance issues, check the `metrics` object for LCP, FID, CLS, FCP, and TTFB — these directly map to optimization needs
3. For images causing CLS or LCP delays, add explicit `width` and `height` attributes to `<img>` tags or use CSS aspect-ratio boxes
4. For unused JavaScript, use DevTools Coverage tab (`Ctrl+Shift+P` → "Coverage") and code-split with dynamic `import()` statements or lazy-load with route-based splitting
5. For slow main-thread work, identify long tasks in the Performance tab and implement Web Workers for CPU-intensive operations
6. For Cumulative Layout Shift (CLS), reserve space for ads, embeds, and dynamic content using fixed containers or CSS `aspect-ratio`
7. For accessibility failures, use `lighthouse --output=json` report's `accessibility` section, then add missing `alt` attributes, proper heading hierarchy (`<h1>` → `<h2>`), and `aria-label` attributes
8. For SEO issues, verify `<meta name="description">`, `<title>` tag, structured data with `schema.org` JSON-LD, and mobile-friendly viewport meta tag
9. Run the audit again to confirm fixes: `lighthouse https://your-site.com --view` to open interactive report

## Code
```javascript
// Lighthouse Report Fixer: Parse and suggest fixes for audit failures
const fs = require('fs');

function analyzeLighthouseReport(reportPath) {
  const report = JSON.parse(fs.readFileSync(reportPath, 'utf-8'));
  const fixes = {};

  // Performance fixes
  if (report.categories.performance.score < 0.9) {
    const perf = report.audits;
    
    if (perf['largest-contentful-paint']?.score < 1) {
      fixes.performance = fixes.performance || [];
      fixes.performance.push({
        audit: 'Largest Contentful Paint',
        fix: 'Optimize images: use WebP, add loading="lazy", implement image CDN',
        code: '<img src="hero.webp" loading="lazy" width="1200"
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

