---
allowed-tools: Bash(git remote:*), Bash(git checkout --branch:*), Bash(git add:*), Bash(git status:*), Bash(git push:*), Bash(git commit:*), Bash(gh pr create:*), Bash(glab mr create:*)
description: Commit, push, and open a PR/MR
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

## Platform Detection

- Remote URL: !`git remote get-url origin 2>/dev/null || echo "no-remote"`

Based on the remote URL:
- Contains `github.com` or `github.` → Use `gh pr create`
- Contains `gitlab.com` or `gitlab.` → Use `glab mr create`
- Other → Ask user which CLI to use

## Your task

Based on the above changes:

1. Create a new branch if on main
2. Create a single commit with a message that STRICTLY follows the Conventional Commit specification defined above
3. Push the branch to origin
4. Create a pull request or merge request:
   - GitHub: Use `gh pr create`
   - GitLab: Use `glab mr create`

**CRITICAL VALIDATION REQUIREMENTS:**

Before creating the commit, validate against the Conventional Commit Rules above:

1. **Validate** the commit message against ALL mandatory rules:
   - Check valid type, format, case, tense/mood
   - Detect task ID from branch name (pattern: `[A-Z]+-\d+`)
   - Include `Refs: <TASK-ID>` if task ID found in branch
   - Check forbidden patterns

2. **If validation fails:**
   - DO NOT proceed with commit, push, or PR creation
   - Output clear error with specific issue

3. **If validation passes:**
   - Proceed with all steps (branch, commit, push, PR)

You have the capability to call multiple tools in a single response. You MUST do all of the above in a single message. Do not use any other tools or do anything else. Do not send any other text or messages besides these tool calls.
