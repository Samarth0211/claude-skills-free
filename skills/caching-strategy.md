---
name: caching-strategy
description: Design and implement caching strategy
category: performance
tags: ["performance", "caching", "strategy"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Caching Strategy

> Design and implement caching strategy

You are a performance engineer implementing caching strategies. The user wants to design and implement a multi-layer caching strategy that balances memory usage, hit rates, and retrieval speed.

## What to check first
- Run `npm list` to verify you have a caching library installed (redis, node-cache, lru-cache)
- Check your application's data access patterns to identify hot data and query frequency
- Verify memory constraints and TTL requirements for your use case

## Steps
1. Install `redis` or `lru-cache` — for Redis: `npm install redis`, for in-memory: `npm install lru-cache`
2. Define cache layers: L1 (in-memory, fast, small), L2 (Redis, medium speed, larger), L3 (database, slow, authoritative)
3. Set TTL (time-to-live) values per layer — e.g., L1: 5 minutes, L2: 30 minutes, L3: persistent
4. Implement cache-aside pattern: check cache first, if miss load from source and populate cache
5. Add cache invalidation logic — use event emitters or webhooks to clear stale entries on data mutations
6. Monitor cache hit/miss ratios using middleware that logs `cache.hits / (cache.hits + cache.misses)`
7. Implement cache warming for predictable hot data on application startup
8. Add circuit breaker logic to fall back to source if cache service fails (e.g., Redis down)

## Code
```javascript
const LRU = require('lru-cache');
const redis = require('redis');

class CacheStrategy {
  constructor(options = {}) {
    // L1: In-memory LRU cache
    this.l1Cache = new LRU({
      max: options.l1Max || 500,
      ttl: 1000 * 60 * 5, // 5 minutes
      updateAgeOnGet: true
    });

    // L2: Redis cache
    this.l2Client = redis.createClient({
      host: options.redisHost || 'localhost',
      port: options.redisPort || 6379
    });
    this.l2TTL = options.l2TTL || 1800; // 30 minutes

    // Stats
    this.stats = { hits: 0, misses: 0, l1Hits: 0, l2Hits: 0 };
  }

  async get(key) {
    // L1 check
    if (this.l1Cache.has(key)) {
      this.stats.hits++;
      this.stats.l1Hits++;
      return this.l1Cache.get(key);
    }

    // L2 check
    try {
      const l2Value = await this.l2Client.get(key);
      if (l2Value) {
        this.stats.hits++;
        this.stats.l2Hits++;
        const parsed = JSON.
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

