---
name: deploy-script
description: Create deployment scripts for various platforms
category: devops
tags: ["devops", "deployment", "automation"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# Deploy Script

> Create deployment scripts for various platforms

You are a DevOps automation engineer. The user wants to create reusable deployment scripts for multiple platforms (AWS, Docker, Kubernetes, traditional servers).

## What to check first
- Verify target platform credentials are configured (`aws configure`, `kubectl config view`, Docker daemon running)
- Check if deployment target environment variables exist (`.env` file or system exports for API keys, registry URLs, image names)
- Confirm deployment artifact exists (built Docker image, compiled binary, or packaged application)

## Steps
1. Define deployment configuration using environment variables and a `.env.example` file for reference
2. Create platform-specific deployment functions (AWS, Docker, Kubernetes, SSH-based)
3. Implement pre-deployment health checks (connectivity, permissions, service availability)
4. Add rollback logic that captures previous state before applying changes
5. Implement logging and error handling with timestamps and exit codes
6. Create idempotent operations so re-running the script is safe
7. Add deployment verification steps (health checks, endpoint tests, log inspection)
8. Package the script with a `Makefile` or wrapper for easy invocation

## Code
```bash
#!/bin/bash
set -euo pipefail

# Configuration
DEPLOYMENT_ENV="${DEPLOYMENT_ENV:-staging}"
DEPLOYMENT_LOG="/var/log/deployment-$(date +%Y%m%d-%H%M%S).log"
BACKUP_DIR="/opt/backups"
ROLLBACK_SNAPSHOT=""

# Colors for output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m'

log() {
  echo -e "${GREEN}[$(date +'%Y-%m-%d %H:%M:%S')]${NC} $*" | tee -a "$DEPLOYMENT_LOG"
}

error() {
  echo -e "${RED}[ERROR]${NC} $*" | tee -a "$DEPLOYMENT_LOG"
  exit 1
}

warn() {
  echo -e "${YELLOW}[WARN]${NC} $*" | tee -a "$DEPLOYMENT_LOG"
}

# Pre-deployment checks
pre_deployment_checks() {
  log "Running pre-deployment checks..."
  
  if [[ -z "${DOCKER_IMAGE:-}" ]]; then
    error "DOCKER_IMAGE environment variable not set"
  fi
  
  if ! command -v docker &> /dev/null; then
    error "Docker is not installed or not in PATH"
  fi
  
  if ! docker ps &> /dev/null; then
    error "Cannot connect to Docker daemon"
  fi
  
  log "Pre-deployment checks passed"
}

# Deploy to Kubernetes
deploy_kubernetes() {
  local app_name="$1"
  local image="$2"
  local replicas="${3:-3}"
  
  log "Deploying $app_name to Kubernetes cluster..."
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

