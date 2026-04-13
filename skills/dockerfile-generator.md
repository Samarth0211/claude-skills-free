---
name: dockerfile-generator
description: Generate optimized Dockerfile for any project
category: docker
tags: ["docker", "dockerfile", "containers"]
difficulty: beginner
version: 1.0.0
author: Claude Skills Hub
---

# Dockerfile Generator

> Generate optimized Dockerfile for any project

You are a Docker expert. The user wants to generate an optimized Dockerfile tailored to their project's tech stack and requirements.

## What to check first
- Run `ls -la` to identify project root files (package.json, requirements.txt, go.mod, pom.xml, etc.)
- Check if a `.dockerignore` file exists; if not, one should be created alongside the Dockerfile
- Identify the primary language/framework by examining the project structure and dependency files

## Steps
1. Detect the project type by examining package.json (Node.js), requirements.txt (Python), go.mod (Go), pom.xml (Java), Gemfile (Ruby), or dockerfile-equivalent indicators
2. Choose an appropriate base image version (e.g., `node:18-alpine`, `python:3.11-slim`, `golang:1.21-alpine`)
3. Set the working directory with `WORKDIR /app` to establish consistent file paths
4. Copy dependency files first (package.json, requirements.txt, go.mod) before copying source code to leverage Docker layer caching
5. Install dependencies using the language-specific package manager (npm ci, pip install, go mod download, etc.)
6. Copy the entire application source code using `COPY . .`
7. Expose the application port (check package.json scripts or main.py for port number, common defaults: 3000, 5000, 8080)
8. Define the startup command with `CMD` or `ENTRYPOINT`, using the exact command from package.json or language runtime
9. Add `.dockerignore` to exclude node_modules, .git, .env, and build artifacts to reduce image size

## Code
```dockerfile
# Detect project type and generate optimized Dockerfile
# Save this as: Dockerfile

# Node.js / Express example (multi-stage for production)
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

FROM node:18-alpine
WORKDIR /app
RUN addgroup -g 1001 -S nodejs && adduser -S nodejs -u 1001
COPY --from=builder --chown=nodejs:nodejs /app/node_modules ./node_modules
COPY --chown=nodejs:nodejs . .
EXPOSE 3000
USER nodejs
CMD ["node", "server.js"]

---

# Python / Flask example (single-stage)
FROM python:3.11-slim
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc \
    && rm -rf /var/lib/apt/lists/*
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
ENV FLASK_APP=app.py
CMD ["flask", "run", "--host=0.0.0.0"]

---

# Go example (multi-stage build)
FROM golang:1.21-alpine
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

