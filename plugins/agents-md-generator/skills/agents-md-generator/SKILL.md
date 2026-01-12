---
name: agents-md-generator
description: Generate AGENTS.md files for coding agent onboarding. Analyzes repository structure, tech stack, CI/CD, databases, and architecture. Creates AGENTS.md with CLAUDE.md symlink. Supports monorepos with nested files per module. Use when user wants to create AGENTS.md, CLAUDE.md, or onboard AI agents to their codebase.
---

# AGENTS.md Generator

Generate high-quality AGENTS.md files that onboard AI coding agents to any codebase.

## Overview

This skill creates AGENTS.md files (with CLAUDE.md symlinks) following best practices from agents.md specification and Claude Code documentation. Works for small repos and large monorepos.

**Output:** AGENTS.md + symlink CLAUDE.md → AGENTS.md

## Workflow

1. **Analyze** — Deep scan of repository structure
2. **Ask** — Clarifying questions about project purpose and preferences
3. **Propose** — Present draft for review
4. **Generate** — Create files after approval
5. **Nest** — For monorepos, propose and create nested files

## Step 1: Repository Analysis

Run comprehensive analysis to detect:

### Tech Stack Detection

```bash
# Languages & frameworks - Run analysis commands (adjust limits based on repo size)
find . -name "*.json" -o -name "*.yaml" -o -name "*.yml" -o -name "*.toml" -o -name "*.xml" | head -50
cat package.json 2>/dev/null | head -100
cat composer.json 2>/dev/null | head -100
cat requirements.txt 2>/dev/null | head -50
cat Cargo.toml 2>/dev/null | head -50
cat go.mod 2>/dev/null | head -30
cat Gemfile 2>/dev/null | head -50
cat pom.xml 2>/dev/null | head -100
cat build.gradle 2>/dev/null | head -100
```

### Build & Test Commands

```bash
# Package managers & scripts
cat package.json 2>/dev/null | jq '.scripts' 2>/dev/null
cat Makefile 2>/dev/null | grep -E "^[a-zA-Z_-]+:" | head -20
cat composer.json 2>/dev/null | jq '.scripts' 2>/dev/null
ls -la .github/workflows/ 2>/dev/null
cat .gitlab-ci.yml 2>/dev/null | head -100
```

### Project Structure

```bash
# Directory structure (2 levels, ignore common noise)
find . -maxdepth 2 -type d ! -path '*/\.*' ! -path '*/node_modules/*' ! -path '*/vendor/*' ! -path '*/__pycache__/*' ! -path '*/dist/*' ! -path '*/build/*' | head -50

# Monorepo detection
ls -d */package.json 2>/dev/null
ls -d apps/*/package.json packages/*/package.json services/*/ 2>/dev/null
cat pnpm-workspace.yaml 2>/dev/null
cat lerna.json 2>/dev/null
```

### Database & Infrastructure

```bash
# Database detection
ls -la **/migrations/ 2>/dev/null | head -20
cat docker-compose.yml 2>/dev/null | grep -E "(mysql|postgres|mongo|redis|elasticsearch)" 
cat .env.example 2>/dev/null | grep -iE "(db_|database|mysql|postgres|mongo)"
find . -name "*.sql" -type f | head -10
```

### Existing Documentation

```bash
# Check for existing docs
cat README.md 2>/dev/null | head -100
cat CONTRIBUTING.md 2>/dev/null | head -50
cat docs/README.md 2>/dev/null | head -50
ls -la docs/ 2>/dev/null
```

## Step 2: Clarifying Questions

After analysis, ask the user (adapt based on findings):

**Essential questions:**
- What is the main purpose of this project? (1-2 sentences)
- Who typically works on this codebase? (team size, experience level)

**Contextual questions** (ask only if relevant):
- [If monorepo detected] Should I create separate AGENTS.md for each module/app?
- [If no CI detected] What commands should agents run before committing?
- [If multiple databases] Which database is primary? Any special migration procedures?
- [If unclear architecture] Can you briefly describe the main architectural patterns?

**Keep questions minimal** — don't overwhelm the user. 2-4 questions max.

## Step 3: Monorepo Decision

**Create nested AGENTS.md files when:**
- Different tech stacks in subdirectories (e.g., Go backend + React frontend)
- Independent deployable units (microservices, separate apps)
- Significantly different testing/build procedures per module
- Team ownership boundaries (different teams own different parts)

**Single root AGENTS.md when:**
- Uniform tech stack across entire repo
- Shared build/test commands work everywhere
- Monorepo with shared packages but same language/framework
- Small repo (< 20 directories)

**When uncertain:** Ask user: "I detected [N] distinct modules with [differences]. Would you like separate AGENTS.md files for [list modules], or one comprehensive root file?"

## Step 4: Generate Draft

Present draft to user for approval before creating files.

### AGENTS.md Structure

Follow this template (adapt sections based on project):

```markdown
# AGENTS.md

## Project Overview
[1-2 sentences: what this project does and its main purpose]

## Tech Stack
- **Language:** [primary language(s)]
- **Framework:** [main framework(s)]
- **Database:** [if applicable]
- **Infrastructure:** [Docker, K8s, etc. if applicable]

## Getting Started

### Prerequisites
[List required tools: Node.js version, PHP version, Docker, etc.]

### Setup
```bash
[Installation commands]
```

### Development
```bash
[Start dev server command]
```

## Common Commands

| Command | Description |
|---------|-------------|
| `[cmd]` | [what it does] |

## Code Style
- [Key conventions, naming patterns]
- [Formatting rules if not handled by linter]

## Testing
```bash
[Test command]
```
[Brief testing guidelines: what to test, how to run specific tests]

## Architecture
[Brief description of main components/modules and their responsibilities]
[For monorepos: describe module relationships]

## Before Committing
1. [Pre-commit checks]
2. [Required validations]

## Troubleshooting
[Common issues and solutions — add as discovered]
```

### Content Guidelines

**DO include:**
- Commands agents will run repeatedly (build, test, lint, dev server)
- Project-specific conventions not inferable from code
- Architecture overview for navigation
- Database/migration procedures if applicable
- Environment setup steps

**DO NOT include:**
- Generic coding advice (agents know how to code)
- Obvious information (file extensions, basic git commands)
- Lengthy code style guides (use linters instead)
- Information that changes frequently (use @imports for those)

**Keep it concise:** Target < 150 lines for root file. Use progressive disclosure — reference other docs with `@path/to/doc.md` syntax for details.

## Step 5: Create Files

After user approval:

```bash
# Create AGENTS.md
cat > AGENTS.md << 'EOF'
[approved content]
EOF

# Create CLAUDE.md symlink
ln -sf AGENTS.md CLAUDE.md

# For nested files in monorepo
cat > apps/frontend/AGENTS.md << 'EOF'
[module-specific content]
EOF
ln -sf AGENTS.md apps/frontend/CLAUDE.md
```

**Always create symlink** — CLAUDE.md should point to AGENTS.md for compatibility with Claude Code while maintaining AGENTS.md as the universal standard.

## Progressive Disclosure with @imports

For large projects, use imports to keep root file lean:

```markdown
# AGENTS.md

## Quick Reference
@docs/commands.md

## Architecture
See @docs/architecture.md for detailed component overview.

## Database
Migration guide: @docs/database.md
```

This keeps AGENTS.md under 150 lines while making detailed docs available when needed.

## Examples

### Small PHP Project

```markdown
# AGENTS.md

## Project Overview
Job board platform built with Symfony 6 and MySQL.

## Tech Stack
- **Language:** PHP 8.2
- **Framework:** Symfony 6.4
- **Database:** MySQL 8.0 (Percona XtraDB Cluster)
- **Frontend:** Twig + Stimulus

## Common Commands

| Command | Description |
|---------|-------------|
| `composer install` | Install dependencies |
| `symfony serve` | Start dev server |
| `php bin/phpunit` | Run tests |
| `php bin/console doctrine:migrations:migrate` | Run migrations |

## Code Style
- PSR-12 standard (enforced by PHP-CS-Fixer)
- Entity properties: camelCase
- Database columns: snake_case

## Testing
```bash
php bin/phpunit --filter TestClassName
```
Run full suite before PR. Coverage required for new features.

## Before Committing
1. `composer run lint`
2. `php bin/phpunit`
3. Check for N+1 queries in new endpoints
```

### Monorepo Example (Root)

```markdown
# AGENTS.md

## Project Overview
E-commerce platform monorepo with React storefront, Node.js API, and Go microservices.

## Structure
- `apps/storefront` — React customer-facing app (@apps/storefront/AGENTS.md)
- `apps/admin` — React admin panel (@apps/admin/AGENTS.md)  
- `services/api` — Node.js main API (@services/api/AGENTS.md)
- `services/payments` — Go payment processor (@services/payments/AGENTS.md)
- `packages/ui` — Shared React components
- `packages/types` — Shared TypeScript types

## Global Commands

| Command | Description |
|---------|-------------|
| `pnpm install` | Install all dependencies |
| `pnpm dev` | Start all services |
| `pnpm test` | Run all tests |
| `pnpm lint` | Lint entire monorepo |

## Working on Specific Module
Navigate to module directory and check its AGENTS.md for module-specific commands.

## Before Committing
1. `pnpm lint`
2. `pnpm test --filter [changed-packages]`
3. Ensure CI passes
```

## Iteration

After initial generation, the user may request:
- Adding sections discovered during work
- Splitting into nested files as project grows
- Updating commands or conventions

Handle updates by editing existing AGENTS.md files. Symlinks don't need updates.
