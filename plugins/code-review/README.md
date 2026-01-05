# Code Review Plugin

Automated code review for pull requests using multiple specialized agents with confidence-based scoring to filter false positives.

## Overview

The Code Review Plugin automates pull request review by launching multiple agents in parallel to independently audit changes from different perspectives. It uses confidence scoring to filter out false positives, ensuring only high-quality, actionable feedback is posted.

## Commands

### `/code-review`

Performs automated code review on a pull request using multiple specialized agents.

**What it does:**
1. Checks if review is needed (skips closed, draft, trivial, or already-reviewed PRs)
2. Gathers relevant CLAUDE.md guideline files from the repository
3. Summarizes the pull request changes
4. Launches 4 parallel agents to independently review:
   - **Agents #1 & #2**: Audit for CLAUDE.md compliance
   - **Agent #3**: Scan for obvious bugs in changes
   - **Agent #4**: Analyze git blame/history for context-based issues
5. Scores each issue 0-100 for confidence level
6. Filters out issues below 80 confidence threshold
7. Posts review comment with high-confidence issues only

**Usage:**
```bash
/code-review
```

**Example workflow:**
```bash
# On a PR branch, run:
/code-review

# Claude will:
# - Launch 4 review agents in parallel
# - Score each issue for confidence
# - Post comment with issues ≥80 confidence
# - Skip posting if no high-confidence issues found
```

**Features:**
- Multiple independent agents for comprehensive review
- Confidence-based scoring reduces false positives (threshold: 80)
- CLAUDE.md compliance checking with explicit guideline verification
- Bug detection focused on changes (not pre-existing issues)
- Historical context analysis via git blame
- Automatic skipping of closed, draft, or already-reviewed PRs
- Links directly to code with full SHA and line ranges

**Review comment format:**
```markdown
## Code review

Found 3 issues:

1. Missing error handling for OAuth callback (CLAUDE.md says "Always handle OAuth errors")

https://github.com/owner/repo/blob/abc123.../src/auth.ts#L67-L72

2. Memory leak: OAuth state not cleaned up (bug due to missing cleanup in finally block)

https://github.com/owner/repo/blob/abc123.../src/auth.ts#L88-L95

3. Inconsistent naming pattern (src/conventions/CLAUDE.md says "Use camelCase for functions")

https://github.com/owner/repo/blob/abc123.../src/utils.ts#L23-L28
```

**Confidence scoring:**
- **0**: Not confident, false positive
- **25**: Somewhat confident, might be real
- **50**: Moderately confident, real but minor
- **75**: Highly confident, real and important
- **100**: Absolutely certain, definitely real

**False positives filtered:**
- Pre-existing issues not introduced in PR
- Code that looks like a bug but isn't
- Pedantic nitpicks
- Issues linters will catch
- General quality issues (unless in CLAUDE.md)
- Issues with lint ignore comments

## Multi-Platform Support

This plugin supports GitHub, GitLab, and optionally Jira for issue tracking:

| Platform | CLI Tool | PR/MR Commands             | Issue Commands              |
|---------|---------|----------------------------|-----------------------------|
| GitHub  | `gh`    | `gh pr view/diff/comment`  | `gh issue view/list`        |
| GitLab  | `glab`  | `glab mr view/diff/note`   | `glab issue view/list`      |
| Jira    | `acli`  | N/A                        | `acli jira issue view/list` |

The platform is automatically detected from your git remote URL.

### Jira Integration (Optional)

To use Jira for issue tracking instead of the platform's native issues:
1. Install: `brew install acli`
2. Authenticate: `acli jira auth login`
3. Create a `.jira` file in your repository root

### Code Link Formats

| Platform | Format                                                  |
|----------|---------------------------------------------------------|
| GitHub | `https://github.com/owner/repo/blob/SHA/path#L10-L15`   |
| GitLab | `https://gitlab.com/owner/repo/-/blob/SHA/path#L10-15`  |

## Installation

This plugin is included in the Claude Code repository. The command is automatically available when using Claude Code.

## Best Practices

### Using `/code-review`
- Maintain clear CLAUDE.md files for better compliance checking
- Trust the 80+ confidence threshold - false positives are filtered
- Run on all non-trivial pull requests
- Review agent findings as a starting point for human review
- Update CLAUDE.md based on recurring review patterns

### When to use
- All pull requests with meaningful changes
- PRs touching critical code paths
- PRs from multiple contributors
- PRs where guideline compliance matters

### When not to use
- Closed or draft PRs (automatically skipped anyway)
- Trivial automated PRs (automatically skipped)
- Urgent hotfixes requiring immediate merge
- PRs already reviewed (automatically skipped)

## Workflow Integration

### Standard PR review workflow:
```bash
# Create PR with changes
/code-review

# Review the automated feedback
# Make any necessary fixes
# Merge when ready
```

### As part of CI/CD:
```bash
# Trigger on PR creation or update
# Automatically posts review comments
# Skip if review already exists
```

## Requirements

- Git repository with GitHub or GitLab integration
- For GitHub: GitHub CLI (`gh`) installed and authenticated
- For GitLab: GitLab CLI (`glab`) installed and authenticated
- (Optional) For Jira: Atlassian CLI (`acli`) installed and authenticated
- CLAUDE.md files (optional but recommended for guideline checking)

## Troubleshooting

### Review takes too long

**Issue**: Agents are slow on large PRs

**Solution**:
- Normal for large changes - agents run in parallel
- 4 independent agents ensure thoroughness
- Consider splitting large PRs into smaller ones

### Too many false positives

**Issue**: Review flags issues that aren't real

**Solution**:
- Default threshold is 80 (already filters most false positives)
- Make CLAUDE.md more specific about what matters
- Consider if the flagged issue is actually valid

### No review comment posted

**Issue**: `/code-review` runs but no comment appears

**Solution**:
Check if:
- PR is closed (reviews skipped)
- PR is draft (reviews skipped)
- PR is trivial/automated (reviews skipped)
- PR already has review (reviews skipped)
- No issues scored ≥80 (no comment needed)

### Link formatting broken

**Issue**: Code links don't render correctly in GitHub

**Solution**:
Links must follow this exact format:
```
https://github.com/owner/repo/blob/[full-sha]/path/file.ext#L[start]-L[end]
```
- Must use full SHA (not abbreviated)
- Must use `#L` notation
- Must include line range with at least 1 line of context

### CLI not working

**Issue**: `gh` or `glab` commands fail

**Solution**:
- For GitHub:
  - Install GitHub CLI: `brew install gh` (macOS) or see [GitHub CLI installation](https://cli.github.com/)
  - Authenticate: `gh auth login`
  - Verify repository has GitHub remote
- For GitLab:
  - Install GitLab CLI: `brew install glab` (macOS) or see [GitLab CLI installation](https://gitlab.com/gitlab-org/cli)
  - Authenticate: `glab auth login`
  - Verify repository has GitLab remote

## Tips

- **Write specific CLAUDE.md files**: Clear guidelines = better reviews
- **Include context in PRs**: Helps agents understand intent
- **Use confidence scores**: Issues ≥80 are usually correct
- **Iterate on guidelines**: Update CLAUDE.md based on patterns
- **Review automatically**: Set up as part of PR workflow
- **Trust the filtering**: Threshold prevents noise

## Configuration

### Adjusting confidence threshold

The default threshold is 80. To adjust, modify the command file at `commands/code-review.md`:
```markdown
Filter out any issues with a score less than 80.
```

Change `80` to your preferred threshold (0-100).

### Customizing review focus

Edit `commands/code-review.md` to add or modify agent tasks:
- Add security-focused agents
- Add performance analysis agents
- Add accessibility checking agents
- Add documentation quality checks

## Technical Details

### Agent architecture
- **2x CLAUDE.md compliance agents**: Redundancy for guideline checks
- **1x bug detector**: Focused on obvious bugs in changes only
- **1x history analyzer**: Context from git blame and history
- **Nx confidence scorers**: One per issue for independent scoring

### Scoring system
- Each issue independently scored 0-100
- Scoring considers evidence strength and verification
- Threshold (default 80) filters low-confidence issues
- For CLAUDE.md issues: verifies guideline explicitly mentions it

### Platform integration
Uses platform-specific CLI (`gh` for GitHub, `glab` for GitLab) for:
- Viewing PR/MR details and diffs
- Fetching repository data
- Reading git blame and history
- Posting review comments/notes

## Author

Boris Cherny (boris@anthropic.com)

## Version

1.0.0
