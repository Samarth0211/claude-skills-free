# 🧠 50 Free Claude Code Skills — Ready to Use

Drop any of these `.md` files into `~/.claude/skills/` and Claude Code instantly gains expert knowledge on that topic.

**What are skills?** Instruction files that teach Claude Code domain expertise — React debugging, SQL optimization, Docker best practices, API design, and more.

## Quick Start

```bash
# Download a single skill
curl -o ~/.claude/skills/rate-limiter.md https://raw.githubusercontent.com/Samarth0211/claude-skills-free/master/skills/rate-limiter.md

# Or clone all 50
git clone https://github.com/Samarth0211/claude-skills-free.git
cp claude-skills-free/skills/*.md ~/.claude/skills/
```

## Available Skills (50)

### API (4)
| Skill | Description |
|-------|-------------|
| [api-error-handler](skills/api-error-handler.md) | Create standardized API error handling |
| [rate-limiter](skills/rate-limiter.md) | Add rate limiting to API endpoints |
| [request-validator](skills/request-validator.md) | Add request validation middleware (Zod, Joi) |
| [rest-api-scaffold](skills/rest-api-scaffold.md) | Scaffold a complete REST API with CRUD operations |

### Backend (2)
| Skill | Description |
|-------|-------------|
| [middleware-chain](skills/middleware-chain.md) | Create and organize middleware chain |
| [websocket-setup](skills/websocket-setup.md) | Implement WebSocket server with rooms |

### CLI (2)
| Skill | Description |
|-------|-------------|
| [cli-app-builder](skills/cli-app-builder.md) | Build CLI applications with Commander.js |
| [interactive-cli](skills/interactive-cli.md) | Create interactive CLI prompts (Inquirer.js) |

### Cloud (3)
| Skill | Description |
|-------|-------------|
| [cloudwatch-alarms](skills/cloudwatch-alarms.md) | Set up CloudWatch monitoring and alarms |
| [lambda-function](skills/lambda-function.md) | Create AWS Lambda function with handler |
| [s3-operations](skills/s3-operations.md) | Set up S3 bucket operations (upload, download, presigned URLs) |

### Code Review (1)
| Skill | Description |
|-------|-------------|
| [code-smell-detector](skills/code-smell-detector.md) | Scan codebase for code smells — duplication, complexity, dead code |

### Database (4)
| Skill | Description |
|-------|-------------|
| [index-advisor](skills/index-advisor.md) | Suggest database indexes based on query patterns |
| [migration-generator](skills/migration-generator.md) | Generate database migration files |
| [query-optimizer](skills/query-optimizer.md) | Analyze and optimize slow database queries |
| [redis-cache-setup](skills/redis-cache-setup.md) | Set up Redis caching with proper patterns |

### Debugging (4)
| Skill | Description |
|-------|-------------|
| [error-analyzer](skills/error-analyzer.md) | Analyze error messages and suggest fixes |
| [log-analyzer](skills/log-analyzer.md) | Analyze log files and identify patterns |
| [memory-leak-finder](skills/memory-leak-finder.md) | Find and fix memory leaks |
| [stack-trace-decoder](skills/stack-trace-decoder.md) | Decode and explain stack traces |

### DevOps (3)
| Skill | Description |
|-------|-------------|
| [deploy-script](skills/deploy-script.md) | Create deployment scripts for various platforms |
| [env-manager](skills/env-manager.md) | Manage environment variables across environments |
| [github-actions-setup](skills/github-actions-setup.md) | Create GitHub Actions workflow files |

### Docker (3)
| Skill | Description |
|-------|-------------|
| [docker-compose](skills/docker-compose.md) | Create docker-compose.yml for multi-service apps |
| [docker-multistage](skills/docker-multistage.md) | Optimize Docker builds with multi-stage builds |
| [dockerfile-generator](skills/dockerfile-generator.md) | Generate optimized Dockerfile for any project |

### Frontend (5)
| Skill | Description |
|-------|-------------|
| [dark-mode-setup](skills/dark-mode-setup.md) | Implement dark/light mode toggle |
| [form-builder](skills/form-builder.md) | Build forms with validation (React Hook Form, Formik) |
| [responsive-layout](skills/responsive-layout.md) | Create responsive layouts with Tailwind/CSS Grid |
| [seo-optimizer](skills/seo-optimizer.md) | Add SEO meta tags, structured data, sitemap |
| [state-management-setup](skills/state-management-setup.md) | Set up state management (Zustand, Redux, Jotai) |

### Git (3)
| Skill | Description |
|-------|-------------|
| [conflict-resolver](skills/conflict-resolver.md) | Analyze and suggest resolutions for merge conflicts |
| [git-hooks-setup](skills/git-hooks-setup.md) | Set up pre-commit, pre-push, and commit-msg hooks |
| [smart-commit](skills/smart-commit.md) | Generate conventional commit messages by analyzing staged changes |

### Performance (3)
| Skill | Description |
|-------|-------------|
| [bundle-analyzer](skills/bundle-analyzer.md) | Analyze and reduce JavaScript bundle size |
| [caching-strategy](skills/caching-strategy.md) | Design and implement caching strategy |
| [lighthouse-fixer](skills/lighthouse-fixer.md) | Fix issues found in Lighthouse audit |

### Python (4)
| Skill | Description |
|-------|-------------|
| [fastapi-setup](skills/fastapi-setup.md) | Scaffold FastAPI with async endpoints and auto-docs |
| [pytest-setup](skills/pytest-setup.md) | Configure pytest with fixtures, plugins, and coverage |
| [python-logging](skills/python-logging.md) | Configure structured logging for Python applications |
| [python-typing](skills/python-typing.md) | Add comprehensive type hints and mypy configuration |

### Refactoring (1)
| Skill | Description |
|-------|-------------|
| [dry-refactor](skills/dry-refactor.md) | Find code duplication and extract shared logic |

### Security (4)
| Skill | Description |
|-------|-------------|
| [dependency-audit](skills/dependency-audit.md) | Audit dependencies for known vulnerabilities |
| [input-sanitizer](skills/input-sanitizer.md) | Add input sanitization to prevent injection attacks |
| [security-headers](skills/security-headers.md) | Configure security headers (HSTS, X-Frame-Options, etc.) |
| [sql-injection-guard](skills/sql-injection-guard.md) | Review and fix SQL injection vulnerabilities |

### Testing (4)
| Skill | Description |
|-------|-------------|
| [e2e-test-writer](skills/e2e-test-writer.md) | Write end-to-end tests using Playwright or Cypress |
| [mock-generator](skills/mock-generator.md) | Generate mocks, stubs, and fakes for dependencies |
| [test-coverage-analyzer](skills/test-coverage-analyzer.md) | Analyze test coverage gaps and suggest tests to write |
| [unit-test-generator](skills/unit-test-generator.md) | Generate unit tests for any function or class |

## Want More?

These 50 are free samples from our full library of **2,300+ tested skills**.

- 🔍 **Browse all 2,300+ skills**: [clskills.in/browse](https://clskills.in/browse)
- 📘 **Free 40-page Claude Guide**: [clskills.in/guide](https://clskills.in/guide)
- ⚡ **120 Tested Prompt Codes**: [clskills.in/cheat-sheet](https://clskills.in/cheat-sheet)
- 📦 **Curated Bundles ($5)**: [clskills.in/bundles](https://clskills.in/bundles)

## Contributing

Found a bug? Want to improve a skill? PRs welcome.

## License

MIT — use these skills however you want.

---

Made by [Samarth Bhamare](https://clskills.in) · [CLSkills.in](https://clskills.in)
