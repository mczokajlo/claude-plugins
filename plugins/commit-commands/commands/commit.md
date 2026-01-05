---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*)
description: Create a git commit
---

## Conventional Commit Rules

**Valid Types:** feat, fix, refactor, docs, build, test, perf, style, chore, ci, revert

**Format:** `<type>: <description>` or `<type>(scope): <description>`

**Mandatory Rules:**
- Type: lowercase, from valid types list
- Description: lowercase start, imperative mood (add/fix, not added/fixing), no trailing period, min 3 meaningful words
- Scope: lowercase if present
- Task reference: `Refs: <TASK-ID>` required IF branch name contains pattern `[A-Z]+-\d+` (e.g., PA-1234, PROJ-567)

**Forbidden Patterns:** `fix: ci`, `ci: fix`, `refactor: changes after CR`, vague descriptions, wrong case/tense

## Context

- Current git status: !`git status`
- Current git diff (staged and unstaged changes): !`git diff HEAD`
- Current branch: !`git branch --show-current`
- Recent commits: !`git log --oneline -10`

## Your task

Based on the above changes, create a single git commit that STRICTLY follows the Conventional Commit specification defined above.

**CRITICAL VALIDATION REQUIREMENTS:**

1. **Draft** a commit message following the Conventional Commit Rules above

2. **Validate** the message against ALL mandatory rules:
   - Check valid type, format, case, tense/mood
   - Detect task ID from branch name (pattern: `[A-Z]+-\d+`)
   - Include `Refs: <TASK-ID>` if task ID found in branch
   - Check forbidden patterns

3. **If validation fails:**
   - DO NOT create the commit
   - Output clear error with specific rule violated and fix
   - Format: `❌ Commit validation failed: [Issue] → Fix: [corrected version]`

4. **If validation passes:**
   - Stage files and create commit using HEREDOC format

You have the capability to call multiple tools in a single response. Stage and create the commit using a single message. Do not use any other tools or do anything else. Do not send any other text or messages besides these tool calls.
