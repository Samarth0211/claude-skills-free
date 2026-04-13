---
name: redis-cache-setup
description: Set up Redis caching with proper patterns
category: database
tags: ["database", "redis", "caching"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Redis Cache Setup

> Set up Redis caching with proper patterns

You are a backend engineer setting up Redis caching infrastructure. The user wants to configure Redis with proper connection pooling, cache patterns, and error handling.

## What to check first
- Verify Redis server is running: `redis-cli ping` should return `PONG`
- Check Node.js redis client version: `npm list redis` — use v4.x or higher for async/await support
- Confirm Redis connection details: host, port (default 6379), and optional password from environment variables

## Steps
1. Install the redis client package: `npm install redis`
2. Create a Redis client singleton with connection pooling and retry logic using `createClient()` with options
3. Add event listeners for `connect`, `error`, and `ready` events to handle connection state
4. Implement a generic `get()` wrapper that returns parsed JSON and handles cache misses
5. Implement a `set()` wrapper with TTL (time-to-live) in seconds and automatic JSON stringification
6. Create a cache invalidation function using `del()` for single keys or `scan()` for pattern-based deletion
7. Add health check middleware that pings Redis and catches connection errors gracefully
8. Configure reconnection strategy with exponential backoff in the client options

## Code
```javascript
import { createClient } from 'redis';

const redis = createClient({
  host: process.env.REDIS_HOST || 'localhost',
  port: process.env.REDIS_PORT || 6379,
  password: process.env.REDIS_PASSWORD,
  socket: {
    reconnectStrategy: (retries) => {
      if (retries > 10) return new Error('Max retries exceeded');
      return Math.min(retries * 50, 500);
    },
  },
});

redis.on('connect', () => console.log('Redis connected'));
redis.on('error', (err) => console.error('Redis error:', err));
redis.on('ready', () => console.log('Redis ready'));

await redis.connect();

export async function getCache(key) {
  try {
    const value = await redis.get(key);
    return value ? JSON.parse(value) : null;
  } catch (error) {
    console.error(`Cache GET error for ${key}:`, error);
    return null;
  }
}

export async function setCache(key, value, ttl = 3600) {
  try {
    await redis.setEx(key, ttl, JSON.stringify(value));
  } catch (error) {
    console.error(`Cache SET error for ${key}:`, error);
  }
}

export async function invalidateCache(pattern) {
  try {
    const keys = [];
    for await (const key of redis.scanIterator({ MATCH: pattern })) {
      keys.push(key);
    }
    if (keys.length > 0) {
      await redis.del(keys);
    }
  } catch (error
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

