---
allowed-tools: Bash(git checkout:*), Bash(git switch:*), Bash(git add:*), Bash(git status:*), Bash(git push:*), Bash(git commit:*), Bash(gh pr create:*), Bash(glab mr create:*)
description: Commit, push, and open a PR/MR
---

## Context

- Current git status: !`git status`
- Current git diff (staged and unstaged changes): !`git diff HEAD`
- Current branch: !`git branch --show-current`
- Git remote URL: !`git config --get remote.origin.url`
- Recent commits: !`git log --oneline -10`

## Commit Message Format

Use the git-commit skill to generate commit messages following conventional commits format.
The skill includes all project-specific requirements (70 char limit, ticket references, etc.).

## Your task

Based on the above changes:

1. Create a new branch if on main, master, or develop
2. Create a single commit with an appropriate message
3. Push the branch to origin
4. Detect platform from remote URL:
    - If remote URL contains `github.com`: use `gh pr create`
    - If remote URL contains `gitlab` (gitlab.com or self-hosted): use `glab mr create`
    - If neither is detected: skip PR/MR creation and report to user
5. Create pull/merge request with appropriate CLI tool

Execute all steps above in a single response. Do not use any other tools or output any additional text.
