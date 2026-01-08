---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Task, AskUserQuestion
description: Create a git commit
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
- Recent commits: !`git log --oneline -10`

## Your task

Based on the above changes, create a single git commit that STRICTLY follows the Conventional Commit specification defined above.

**CRITICAL VALIDATION REQUIREMENTS:**

1. **Draft** a commit message following the Conventional Commit Rules above

2. **Verify** the drafted message using the commit-verifier agent:
   - Use the Task tool to launch the commit-verifier agent
   - Pass the drafted commit message to the agent
   - The agent will validate against ALL mandatory rules and return results

3. **Handle verification results:**

   **If agent reports VALID (✅):**
   - Proceed directly to step 4

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
     - If "Use the fixed version": Use the fixed message from agent, proceed to step 4
     - If "Edit manually": Ask user for new message, go back to step 2 with new message
     - If "Abort": Stop completely, do not create commit, inform user

4. **Stage and commit** (only if validation passed or user approved fixed version):
   - Stage files using `git add`
   - Create commit using `git commit` with HEREDOC format

You have the capability to call multiple tools in a single response. After verifying the message with the agent, handle the results and either commit (if valid) or present options to the user (if invalid).
