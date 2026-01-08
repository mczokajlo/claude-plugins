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
3. Examines recent commit messages to match your repository's style
4. Drafts an appropriate commit message
5. Stages relevant files
6. Creates the commit

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
- Automatically drafts commit messages following conventional commit format
- **Automatic verification** before committing (validates all rules, provides fixes)
- Strict validation of commit message format (type, description, task references)
- Detects and requires task references from branch names
- Rejects forbidden patterns and non-descriptive commits
- Avoids committing files with secrets (.env, credentials.json)
- Provides clear error messages with suggested fixes

#### Automatic Verification

All commits created by `/commit` and `/commit-push` are automatically verified before creation:

✅ **Pre-Commit Validation**
- Validates commit message before creating the commit
- Checks all conventional commit rules
- Provides detailed feedback on issues found
- Offers automatically fixed versions

🔧 **Automatic Fixes**
- Corrects case issues (uppercase → lowercase)
- Fixes tense/mood (past/continuous → imperative)
- Suggests line breaks for long lines
- Adds missing task references
- Replaces vague descriptions with suggestions

**Workflow:**
1. You run `/commit` or `/commit-push`
2. Command drafts commit message from changes
3. Verification agent validates the message
4. If issues found:
   - Shows what's wrong
   - Provides fixed version
   - Asks if you want to use the fix
5. If valid or you approve fix:
   - Creates the commit

### `/commit-push`

Complete workflow command that commits, pushes, and creates a pull request in one step.

**What it does:**
1. Creates a new branch (if currently on main)
2. Stages and commits changes with an appropriate message
3. Pushes the branch to origin
4. Creates a pull request using `gh pr create`
5. Provides the PR URL

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
# - Commit your changes
# - Push to remote
# - Open a PR with summary and test plan
# - Give you the PR URL to review
```

**Features:**
- Analyzes all commits in the branch (not just the latest)
- Creates commits following strict conventional commit format
- **Automatic verification** before committing (validates all rules, provides fixes)
- Validates commit messages before pushing
- Creates comprehensive PR descriptions with:
  - Summary of changes (1-3 bullet points)
  - Test plan checklist
  - Claude Code attribution
- Handles branch creation automatically
- Multi-platform support: GitHub (`gh`) and GitLab (`glab`)

**Requirements:**
- GitHub CLI (`gh`) must be installed and authenticated
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

## Installation

This plugin is included in the Claude Code repository. The commands are automatically available when using Claude Code.

## Best Practices

### Using `/commit`
- Review the staged changes before committing
- Let Claude analyze your changes and match your repo's commit style
- Trust the automated message, but verify it's accurate
- Use for routine commits during development

### Using `/commit-push`
- Use when you're ready to create a PR
- Ensure all your changes are complete and tested
- Claude will analyze the full branch history for the PR description
- Review the PR description and edit if needed
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

## Conventional Commit Format

All commits created by this plugin follow the [Conventional Commits](https://www.conventionalcommits.org/) specification with **strict enforcement**. Invalid commit messages will be rejected with clear error messages.

### Format

**Simple:**
```
<type>: <description>
```

**With scope:**
```
<type>(scope): <description>
```

**With body and task reference:**
```
<type>: <description>

<optional longer description>

Refs: <task-id>
```

### Valid Types

- **feat**: New feature or functionality
- **fix**: Bug fix
- **refactor**: Code refactoring (no functionality change)
- **docs**: Documentation changes
- **build**: Build system or dependencies
- **test**: Test additions or modifications
- **perf**: Performance improvements
- **style**: Code formatting (no logic change)
- **chore**: Maintenance and tooling
- **ci**: CI/CD configuration
- **revert**: Revert previous commit

### Validation Rules

All commit messages are validated against these rules:

✅ **Format Requirements:**
- Type must be lowercase and from the valid types list
- Colon and space required after type: `": "`
- Description must start with lowercase letter
- Description must be imperative mood (e.g., "add", "fix", not "added", "fixing")
- No trailing period on description
- Scope must be lowercase (if present)
- Each line must not exceed 70 characters (title, body, task reference)

✅ **Task References:**
- Automatically detected from branch names
- Required when branch contains task ID (e.g., `feature/PA-1234-*`, `fix/PROJ-567-*`)
- Format: `Refs: <TASK-ID>`
- Supports patterns: `PA-1234`, `PROJ-999`, `[A-Z]+-\d+`

❌ **Rejected Patterns:**
- Non-descriptive: `fix: ci`, `chore: updates`, `fix: bug`
- Code review artifacts: `refactor: changes after CR`, `fix: pr comments`
- Wrong case: `Fix: bug`, `feat(Auth): feature`
- Wrong tense: `feat: added feature`, `fix: fixing bug`
- Vague descriptions (fewer than 3 meaningful words)

### Examples

**✅ Valid:**

Simple commit:
```
feat: add user authentication endpoint
```

With scope:
```
feat(auth): implement jwt token validation
```

With body:
```
fix: resolve memory leak in event listeners

Remove event listeners when components unmount to prevent
memory accumulation during navigation
```

With task reference (required on `feature/PA-1234-*` branch):
```
feat: add oauth2 authentication flow

Implements OAuth2 authorization code flow with PKCE for
secure third-party authentication

Refs: PA-1234
```

Complex example:
```
refactor(api): extract validation middleware

Centralizes request validation logic into reusable middleware
functions, reducing duplication across route handlers

Refs: PROJ-5678
```

**❌ Invalid:**

```
Fix: bug                    # Uppercase type
feat: Added feature         # Past tense
fix: ci                     # Non-descriptive, forbidden pattern
refactor: changes after CR  # Forbidden pattern
feat: Add feature           # Uppercase description
feat: add feature.          # Trailing period
feat add feature            # Missing colon
feat: add comprehensive user authentication system with oauth2 and jwt  # Line too long (73 chars)
```

### Validation Process

When you run `/commit` or `/commit-push`:

1. **Context Gathering**: Analyzes git status, diff, branch name, and recent commits
2. **Message Generation**: Creates a commit message following conventional commit format
3. **Validation**: Checks all format rules, task references, and forbidden patterns
4. **Error Handling**: If validation fails, provides specific error with suggested fix
5. **Commit Creation**: If validation passes, creates the commit

### Error Messages

Validation failures provide clear, actionable feedback:

```
❌ Commit message validation failed:

Issue: Type must be lowercase
Rule: Format Rules - Type must be lowercase
Your message: "Fix: resolve parser error"

Fix: Change "Fix" to "fix":
fix: resolve parser error
```

### Task Reference Detection

Task references are automatically detected from your branch name:

| Branch Name | Task ID Detected | Reference Required |
|-------------|------------------|-------------------|
| `feature/PA-1234-auth` | PA-1234 | Yes |
| `fix/PROJ-567-bug` | PROJ-567 | Yes |
| `main` | None | No |
| `develop` | None | No |
| `refactor/PA-999-cleanup` | PA-999 | Yes |

When a task ID is detected, the commit message **must** include:
```
Refs: <TASK-ID>
```

### Configuration

Commit message rules are defined in:
```
plugins/commit-commands/config/conventional-commits.config.md
```

You can customize:
- Valid commit types
- Task reference detection patterns
- Forbidden patterns
- Format rules
- Error messages

### Line Length Tips

**Keeping lines under 70 characters:**
- Break long titles into title + body format
- Use concise, specific language
- Move technical details to the body

**Example:**
```
✅ feat: add user authentication

Implements OAuth2 authorization code flow with PKCE for
secure third-party authentication
```

### Benefits

✅ **Consistency**: All commits follow the same format
✅ **Searchability**: Easy to find commits by type
✅ **Automation**: Enables automatic changelog generation
✅ **Semantic Versioning**: Supports automated version bumps
✅ **Traceability**: Links commits to tasks/issues
✅ **Quality**: Prevents vague or non-descriptive commits

## Multi-Platform Support

The `/commit-push` command supports both GitHub and GitLab repositories:

| Platform | CLI Tool | Command |
|----------|----------|---------|
| GitHub | `gh` | `gh pr create` |
| GitLab | `glab` | `glab mr create` |

The platform is automatically detected from your git remote URL.

### GitLab Installation

```bash
brew install glab
glab auth login
```

## Requirements

- Git must be installed and configured
- For GitHub repositories: GitHub CLI (`gh`) installed and authenticated
- For GitLab repositories: GitLab CLI (`glab`) installed and authenticated
- Repository must be a git repository with a remote

## Troubleshooting

### `/commit` creates empty commit

**Issue**: No changes to commit

**Solution**:
- Ensure you have unstaged or staged changes
- Run `git status` to verify changes exist

### `/commit-push` fails to create PR

**Issue**: `gh pr create` or `glab mr create` command fails

**Solution**:
- For GitHub:
  - Install GitHub CLI: `brew install gh` (macOS) or see [GitHub CLI installation](https://cli.github.com/)
  - Authenticate: `gh auth login`
  - Ensure repository has a GitHub remote
- For GitLab:
  - Install GitLab CLI: `brew install glab` (macOS) or see [GitLab CLI installation](https://gitlab.com/gitlab-org/cli)
  - Authenticate: `glab auth login`
  - Ensure repository has a GitLab remote

### `/clean_gone` doesn't find branches

**Issue**: No branches marked as [gone]

**Solution**:
- Run `git fetch --prune` to update remote tracking
- Branches must be deleted from the remote to show as [gone]

## Tips

- **Combine with other tools**: Use `/commit` during development, then `/commit-push` when ready
- **Let Claude draft messages**: The commit message analysis learns from your repo's style
- **Regular cleanup**: Run `/clean_gone` weekly to maintain a clean branch list
- **Review before pushing**: Always review the commit message and changes before pushing

## Author

Anthropic (support@anthropic.com)

## Version

1.0.0
