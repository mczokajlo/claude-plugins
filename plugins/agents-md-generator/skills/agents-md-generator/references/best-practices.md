# Best Practices for AGENTS.md

Reference guide for writing effective agent onboarding files.

## Core Principles

### 1. Less is More

LLMs can reliably follow ~150-200 instructions. Claude Code's system prompt already uses ~50. Every instruction in AGENTS.md competes for attention.

**Rules:**
- Keep root AGENTS.md under 150 lines
- Include only universally applicable instructions
- Move task-specific content to separate files with @imports

### 2. AGENTS.md Onboards the Agent

Cover three areas:

| Area | What to Include |
|------|-----------------|
| **WHAT** | Tech stack, project structure, module map |
| **WHY** | Project purpose, what each part does |
| **HOW** | Build commands, test commands, dev workflow |

### 3. Progressive Disclosure

Don't dump everything in one file:

```
agent_docs/
├── building.md
├── testing.md
├── database.md
└── architecture.md
```

Reference from AGENTS.md:
```markdown
## Testing
See @agent_docs/testing.md for detailed testing guidelines.
```

### 4. Prefer Pointers to Copies

Don't copy code into AGENTS.md — it becomes stale. Reference files:
```markdown
See `src/auth/middleware.ts:15-40` for authentication pattern.
```

### 5. Don't Replace Linters

Never include code style rules that a linter should enforce. LLMs are expensive and slow compared to formatters.

**Bad:**
```markdown
## Code Style
- Use 2 spaces for indentation
- Always use semicolons
- Prefer const over let
```

**Good:**
```markdown
## Code Style
Run `pnpm lint` — Biome handles all formatting.
```

## What to Include

### Always Include
- [ ] Project purpose (1-2 sentences)
- [ ] Setup commands (install, dev server)
- [ ] Test commands
- [ ] Pre-commit checklist

### Include if Relevant
- [ ] Database/migration commands
- [ ] Architecture overview for larger projects
- [ ] Environment variables setup
- [ ] Common troubleshooting

### Never Include
- [ ] Generic coding advice
- [ ] Obvious information (what .js files are)
- [ ] Detailed style guides (use linters)
- [ ] Frequently changing info (use @imports)

## Monorepo Guidelines

### When to Split

Create separate AGENTS.md files when modules have:
- Different languages/frameworks
- Different test procedures
- Different deployment targets
- Different team ownership

### Root File for Monorepos

Keep root file as navigation:
```markdown
# AGENTS.md

## Structure
- `apps/web` — Next.js frontend (@apps/web/AGENTS.md)
- `services/api` — Go API (@services/api/AGENTS.md)

## Global Commands
| Command | Description |
|---------|-------------|
| `make all` | Build everything |
| `make test` | Test everything |
```

### Nested File Template

Each nested AGENTS.md should be self-contained:
```markdown
# apps/web AGENTS.md

## Overview
Customer-facing Next.js application.

## Commands
| Command | Description |
|---------|-------------|
| `pnpm dev` | Start dev server on :3000 |
| `pnpm test` | Run Vitest |

## Testing
```bash
pnpm test --filter web
```

## Module-Specific Notes
[Anything unique to this module]
```

## @import Syntax

CLAUDE.md supports importing files:

```markdown
See @README.md for project overview.
Follow @docs/git-workflow.md for commits.
```

**Rules:**
- Relative and absolute paths work
- Won't evaluate inside code blocks
- Max depth: 5 hops
- Use for team member individual preferences: `@~/.claude/my-project-prefs.md`

## Anti-Patterns

### 1. Kitchen Sink AGENTS.md
```markdown
# DON'T: Everything in one file
## All 50 API endpoints...
## Complete database schema...
## Every coding convention...
```

### 2. Hotfix Accumulation
```markdown
# DON'T: Random fixes appended over time
## IMPORTANT: Never use var
## NOTE: Always check null
## REMEMBER: Use async/await
```

### 3. Duplicate Information
```markdown
# DON'T: Copy from README
## About
[Same text as README.md]
```

### 4. Unstable Information
```markdown
# DON'T: Things that change often
## Current Sprint Goals
- Finish feature X by Friday
```
