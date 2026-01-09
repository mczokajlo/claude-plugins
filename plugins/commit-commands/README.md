# Commit Commands Plugin

Streamline your git workflow with simple commands for committing, pushing, and creating pull requests.

## Overview

The Commit Commands Plugin automates common git operations, reducing context switching and manual command execution. Instead of running multiple git commands, use a single slash command to handle your entire workflow.

## Commands

### `/commit`

Creates a git commit with an automatically generated commit message based on staged and unstaged changes.

**What it does:**
1. Analyzes current git status
2. Reviews both staged and unstaged changes
3. Generates commit message using git-commit skill
4. Stages relevant files
5. Creates the commit

**Usage:**
```bash
/commit
```

**Example workflow:**
```bash
# Make some changes to your code
# Then simply run:
/commit

# Claude will:
# - Review your changes
# - Stage the files
# - Create a commit with an appropriate message
# - Show you the commit status
```

**Features:**
- Uses git-commit skill for message generation
- Follows conventional commit format (70-char limit, ticket references)
- Avoids committing files with secrets (.env, credentials.json)
- Includes Claude Code attribution in commit message

### `/commit-push`

Complete workflow command that commits, pushes, and creates a pull/merge request in one step.

**What it does:**
1. Creates a new branch (if currently on main, master, or develop)
2. Generates commit message using git-commit skill
3. Stages and commits changes
4. Pushes the branch to origin
5. Detects platform (GitHub/GitLab) and creates PR/MR using appropriate CLI tool

**Usage:**
```bash
/commit-push
```

**Example workflow:**
```bash
# Make your changes
# Then run:
/commit-push

# Claude will:
# - Create a feature branch (if needed)
# - Commit your changes using git-commit skill
# - Push to remote
# - Create PR/MR with appropriate CLI tool
# - Give you the PR/MR URL to review
```

**Features:**
- Uses git-commit skill for message generation
- Handles branch creation automatically
- Platform detection (GitHub/GitLab)
- Creates PR using `gh pr create` (GitHub) or `glab mr create` (GitLab)
- Follows conventional commit format (70-char limit, ticket references)

**Requirements:**
- GitHub CLI (`gh`) must be installed and authenticated (for GitHub repos)
- GitLab CLI (`glab`) must be installed and authenticated (for GitLab repos)
- Repository must have a remote named `origin`

### `/clean_gone`

Cleans up local branches that have been deleted from the remote repository.

**What it does:**
1. Lists all local branches to identify [gone] status
2. Identifies and removes worktrees associated with [gone] branches
3. Deletes all branches marked as [gone]
4. Provides feedback on removed branches

**Usage:**
```bash
/clean_gone
```

**Example workflow:**
```bash
# After PRs are merged and remote branches are deleted
/clean_gone

# Claude will:
# - Find all branches marked as [gone]
# - Remove any associated worktrees
# - Delete the stale local branches
# - Report what was cleaned up
```

**Features:**
- Handles both regular branches and worktree branches
- Safely removes worktrees before deleting branches
- Shows clear feedback about what was removed
- Reports if no cleanup was needed

**When to use:**
- After merging and deleting remote branches
- When your local branch list is cluttered with stale branches
- During regular repository maintenance

## Skills

### Git Commit Skill

The git-commit skill generates conventional commit messages following the Conventional Commits specification with project-specific requirements.

**What it provides:**
1. Standard conventional commit types (feat, fix, chore, docs, refactor, test, build, perf, style, ci, revert)
2. Structured format with optional scope, body, and footer
3. Breaking change indicators
4. Project-specific validations (70-char line limit, ticket references)

**Features:**
- Enforces 70-character maximum line length (stricter than standard)
- Requires ticket reference footer (Refs: TICKET-ID)
- Validates conventional commit format
- Provides commit type guidelines and examples

**Used by:**
- `/commit` command for single commit creation
- `/commit-push` command for commit and PR/MR workflows

**Details:**
See `plugins/commit-commands/skills/git-commit/SKILL.md` for complete format specification and examples.

## Installation

This plugin is included in the Claude Code repository. The commands are automatically available when using Claude Code.

## Best Practices

### Using `/commit`
- Review the staged changes before committing
- Let the git-commit skill generate conventional commit messages
- Ensure ticket references are included (Refs: TICKET-ID)
- Use for routine commits during development

### Using `/commit-push`
- Use when you're ready to create a PR/MR
- Ensure all your changes are complete and tested
- Automatically creates branch, commits, pushes, and opens PR/MR
- Works with both GitHub (gh) and GitLab (glab)
- Use when you want to minimize context switching

### Using `/clean_gone`
- Run periodically to keep your branch list clean
- Especially useful after merging multiple PRs
- Safe to run - only removes branches already deleted remotely
- Helps maintain a tidy local repository

## Workflow Integration

### Quick commit workflow:
```bash
# Write code
/commit
# Continue development
```

### Feature branch workflow:
```bash
# Develop feature across multiple commits
/commit  # First commit
# More changes
/commit  # Second commit
# Ready to create PR
/commit-push
```

### Maintenance workflow:
```bash
# After several PRs are merged
/clean_gone
# Clean workspace ready for next feature
```

## Requirements

- Git must be installed and configured
- For `/commit-push`: GitHub CLI (`gh`) or GitLab CLI (`glab`) must be installed and authenticated (depending on your repository platform)
- Repository must be a git repository with a remote

## Troubleshooting

### `/commit` creates empty commit

**Issue**: No changes to commit

**Solution**:
- Ensure you have unstaged or staged changes
- Run `git status` to verify changes exist

### `/commit-push` fails to create PR/MR

**Issue**: `gh pr create` or `glab mr create` command fails

**Solution**:
- For GitHub: Install GitHub CLI: `brew install gh` (macOS) or see [GitHub CLI installation](https://cli.github.com/)
  - Authenticate: `gh auth login`
- For GitLab: Install GitLab CLI: `brew install glab` (macOS) or see [GitLab CLI installation](https://gitlab.com/gitlab-org/cli)
  - Authenticate: `glab auth login`
- Ensure repository has the appropriate remote (github.com or gitlab)

### `/clean_gone` doesn't find branches

**Issue**: No branches marked as [gone]

**Solution**:
- Run `git fetch --prune` to update remote tracking
- Branches must be deleted from the remote to show as [gone]

## Tips

- **Combine with other tools**: Use `/commit` during development, then `/commit-push` when ready
- **Use git-commit skill**: The skill enforces conventional commits with project-specific requirements (70-char limit, ticket references)
- **Regular cleanup**: Run `/clean_gone` weekly to maintain a clean branch list
- **Review before pushing**: Always review the commit message and changes before pushing

## Author

Anthropic (support@anthropic.com)

## Version

1.0.0
