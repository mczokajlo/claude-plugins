---
allowed-tools: Bash(git remote:*), Bash(git checkout --branch:*), Bash(git add:*), Bash(git status:*), Bash(git push:*), Bash(git commit:*), Bash(gh pr create:*), Bash(glab mr create:*), Task, AskUserQuestion
description: Commit, push, and open a PR/MR
---

## Conventional Commit Rules

**Valid Types:** feat, fix, refactor, docs, build, test, perf, style, chore, ci, revert

**Format:** `<type>: <description>` or `<type>(scope): <description>`

**Mandatory Rules:**
- Type: lowercase, from valid types list
- Description: lowercase start, imperative mood (add/fix, not added/fixing), no trailing period, min 3 meaningful words
- Scope: lowercase if present
- Line length: max 70 characters per line (title, body, task reference)
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

Before creating the commit, validate the message using the commit-verifier agent:

1. **Draft** a commit message following the Conventional Commit Rules above

2. **Verify** the drafted message using the commit-verifier agent:
   - Use the Task tool to launch the commit-verifier agent
   - Pass the drafted commit message to the agent
   - The agent will validate against ALL mandatory rules and return results

3. **Handle verification results:**

   **If agent reports VALID (✅):**
   - Proceed with all steps (branch creation if needed, commit, push, PR)

   **If agent reports INVALID (❌):**
   - Display the agent's output showing:
     - All issues found with explanations
     - The fixed commit message
     - Options for the user
   - Use AskUserQuestion tool to ask: "The commit message has validation issues. What would you like to do?"
     - Option 1: "Use the fixed version" (Recommended)
     - Option 2: "Edit manually"
     - Option 3: "Abort"
   - Handle user response:
     - If "Use the fixed version": Use the fixed message, proceed with commit, push, and PR
     - If "Edit manually": Ask user for new message, go back to step 2 with new message
     - If "Abort": Stop completely, do not commit/push/create PR, inform user

4. **Execute workflow** (only if validation passed or user approved):
   - Create new branch if on main
   - Stage files and create commit
   - Push to origin
   - Create pull request or merge request

You have the capability to call multiple tools in a single response. After verifying the message with the agent, handle the results and either proceed with the workflow (if valid) or present options to the user (if invalid).
