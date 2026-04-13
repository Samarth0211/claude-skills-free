---
name: docker-multistage
description: Optimize Docker builds with multi-stage builds
category: docker
tags: ["docker", "multistage", "optimization"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Docker Multistage

> Optimize Docker builds with multi-stage builds

You are a Docker optimization expert. The user wants to implement multi-stage builds to reduce final image size and optimize Docker build performance.

## What to check first
- Run `docker --version` to confirm Docker is installed (v17.05+)
- Check your current Dockerfile with `cat Dockerfile` to identify build artifacts that could be removed
- Run `docker images` to see current image sizes before optimization

## Steps
1. Create a new `Dockerfile` with multiple `FROM` statements—each `FROM` starts a new build stage
2. In the first stage (builder), install build tools and compile/transpile your application
3. Use `COPY --from=<stage>` to copy only compiled artifacts from the builder stage to the final stage
4. In the final stage, use a minimal base image (alpine, distroless, or scratch) with only runtime dependencies
5. Add labels and metadata in the final stage with `LABEL` instructions
6. Build with `docker build -t myapp:latest .` and inspect the resulting image size
7. Compare file sizes using `docker history myapp:latest` to verify build artifacts were stripped
8. Push to registry or run with `docker run myapp:latest`

## Code
```dockerfile
# Stage 1: Builder
FROM node:18-alpine AS builder
WORKDIR /build

# Install build dependencies
RUN apk add --no-cache python3 make g++

# Copy source and package files
COPY package*.json ./
RUN npm ci --only=production && npm run build

# Stage 2: Runtime
FROM node:18-alpine
WORKDIR /app

# Add security labels
LABEL maintainer="your-email@example.com"
LABEL description="Optimized Node.js app with multi-stage build"

# Copy runtime dependencies and built artifacts from builder
COPY --from=builder /build/node_modules ./node_modules
COPY --from=builder /build/dist ./dist
COPY --from=builder /build/package.json .

# Non-root user for security
RUN addgroup -g 1001 -S nodejs && adduser -S nodejs -u 1001
USER nodejs

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD node -e "require('http').get('http://localhost:3000/health', (r) => {if (r.statusCode !== 200) throw new Error(r.statusCode)})"

EXPOSE 3000
CMD ["node", "dist/index.js"]
```

## Pitfalls
- **Forgetting to exclude node_modules from builder stage**: Use `.dockerignore` with `node_modules` and `dist` to prevent copying unnecessary files into the builder layer
- **Using outdated base images**: Alpine is lightweight but may have glibc compatibility issues; use `node:18-alpine` for Node or distroless images for production
- **Not specifying exact stage name in COPY --from**: If you reference a stage, use the exact name after

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

