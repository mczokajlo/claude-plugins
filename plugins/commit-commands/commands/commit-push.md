---
allowed-tools: Bash(git remote:*), Bash(git checkout --branch:*), Bash(git add:*), Bash(git status:*), Bash(git push:*), Bash(git commit:*), Bash(gh pr create:*), Bash(glab mr create:*)
description: Commit, push, and open a PR/MR
---

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
2. Create a single commit with an appropriate message
3. Push the branch to origin
4. Create a pull request or merge request:
   - GitHub: Use `gh pr create`
   - GitLab: Use `glab mr create`
5. You have the capability to call multiple tools in a single response. You MUST do all of the above in a single message. Do not use any other tools or do anything else. Do not send any other text or messages besides these tool calls.
