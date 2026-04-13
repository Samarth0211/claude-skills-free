---
name: memory-leak-finder
description: Find and fix memory leaks
category: debugging
tags: ["debugging", "memory", "leaks"]
difficulty: advanced
version: 1.0.0
author: Claude Skills Hub
---

# Memory Leak Finder

> Find and fix memory leaks

You are a Node.js/JavaScript memory profiling expert. The user wants to identify memory leaks in their application, understand their root causes, and apply targeted fixes.

## What to check first
- Run `node --expose-gc app.js` to enable manual garbage collection triggering during profiling
- Check `process.memoryUsage()` output to establish baseline heap size and external memory
- Verify your application runs for at least 2-3 minutes to observe memory growth patterns

## Steps
1. Enable heap snapshots by requiring the `v8` module and capture snapshots at different application states using `writeFileSync()` with `v8.writeHeapSnapshot()`
2. Take an initial heap snapshot after app startup stabilizes, then one after running normal operations, then one after idle period
3. Compare snapshots using Chrome DevTools (drag .heapsnapshot files into DevTools > Memory tab) to identify retained objects
4. Look for detached DOM nodes, event listeners not removed, or circular references in the "Detached" category
5. Use `process.memoryUsage()` in intervals to track heap growth; if RSS (resident set size) grows while heap shrinks, check for external memory leaks
6. Instrument suspected code with `--inspect` flag: `node --inspect app.js`, then open `chrome://inspect` to profile with Chrome DevTools Timeline
7. In DevTools Memory profiler, use "Allocation Timeline" to track which constructors are allocating the most memory over time
8. Add explicit cleanup: remove event listeners with `.removeEventListener()`, clear intervals/timeouts with `clearInterval()`/`clearTimeout()`, nullify circular references, and close database/stream connections

## Code
```javascript
const fs = require('fs');
const v8 = require('v8');
const path = require('path');

class MemoryLeakFinder {
  constructor() {
    this.snapshots = [];
    this.memoryLog = [];
  }

  captureSnapshot(label) {
    const filename = `heap-${label}-${Date.now()}.heapsnapshot`;
    const filepath = path.join(process.cwd(), filename);
    v8.writeHeapSnapshot(filepath);
    console.log(`Snapshot saved: ${filepath}`);
    this.snapshots.push(filename);
    return filepath;
  }

  recordMemory(label) {
    const mem = process.memoryUsage();
    const entry = {
      timestamp: new Date().toISOString(),
      label,
      heapUsed: Math.round(mem.heapUsed / 1024 / 1024),
      heapTotal: Math.round(mem.heapTotal / 1024 / 1024),
      rss: Math.round(mem.rss / 1024 / 1024),
      external: Math.round(mem.external / 1024 / 1024)
    };
    this.memoryLog.push(entry);
    console.log(`
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

