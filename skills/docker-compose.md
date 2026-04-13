---
name: docker-compose
description: Create docker-compose.yml for multi-service apps
category: docker
tags: ["docker", "compose", "multi-service"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# Docker Compose

> Create docker-compose.yml for multi-service apps

You are a Docker DevOps engineer. The user wants to create a docker-compose.yml file for running multiple interconnected services locally.

## What to check first
- Run `docker --version` and `docker-compose --version` to confirm both are installed
- Verify the services you need (web app, database, cache, etc.) and their image names from Docker Hub or your registry

## Steps
1. Create a `docker-compose.yml` file in your project root with `version: '3.8'` at the top (compatible with Docker Engine 20.10+)
2. Define the `services:` section and list each service as a key (e.g., `web:`, `db:`, `redis:`)
3. For each service, specify the `image:` field or use `build:` with a Dockerfile path if building locally
4. Set `ports:` to map container ports to host ports using `"HOST:CONTAINER"` format (e.g., `"8080:3000"`)
5. Add `environment:` variables or reference a `.env` file with `env_file: .env` for sensitive data
6. Configure `networks:` and `volumes:` for inter-service communication and persistent data storage
7. Use `depends_on:` to control startup order (e.g., web service depends on db service)
8. Run `docker-compose up -d` to start all services in detached mode, then use `docker-compose logs -f` to monitor

## Code
```yaml
version: '3.8'

services:
  web:
    build: .
    container_name: myapp_web
    ports:
      - "8080:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgres://user:password@db:5432/myapp
      - REDIS_URL=redis://cache:6379
    depends_on:
      - db
      - cache
    volumes:
      - ./src:/app/src
    networks:
      - app-network
    restart: unless-stopped

  db:
    image: postgres:15-alpine
    container_name: myapp_db
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=myapp
    ports:
      - "5432:5432"
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - app-network
    restart: unless-stopped
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user"]
      interval: 10s
      timeout: 5s
      retries: 5

  cache:
    image: redis:7-alpine
    container_name: myapp_cache
    ports:
      - "6379:6379"
    volumes:
      - cache_data:/data
    networks:
      - app-network
    restart: unless-stopped
    command: redis-server
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

