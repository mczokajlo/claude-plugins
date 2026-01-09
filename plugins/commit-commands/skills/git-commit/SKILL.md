---
name: git-commit
description: Generate conventional commit messages following the Conventional Commits specification for git commits.
---

# Git Commit Message Generator

Generate structured, conventional commit messages that follow the Conventional Commits specification.

## Format

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Commit Types

| Type       | Purpose                 | Example                                      |
|------------|-------------------------|----------------------------------------------|
| `feat`     | New feature             | `feat(auth): add JWT authentication`         |
| `fix`      | Bug fix                 | `fix(api): handle null response from server` |
| `chore`    | Maintenance tasks       | `chore: update dependencies`                 |
| `docs`     | Documentation           | `docs: add API usage examples`               |
| `refactor` | Code refactoring        | `refactor(parser): simplify token handling`  |
| `test`     | Tests                   | `test: add unit tests for user service`      |
| `build`    | Rebuilt dependencies    | `build: rebuild after updating npm packages` |
| `perf`     | Performance improvement | `perf(db): optimize query execution`         |
| `style`    | Code formatting         | `style: apply consistent indentation`        |
| `ci`       | CI/CD changes           | `ci: add GitHub Actions workflow`            |
| `revert`   | Revert previous commit  | `revert commit abc123`                       |

## Structure Rules

1. **Type**: Required, lowercase, from the list above
2. **Scope**: Optional, describes codebase section in parentheses
3. **Description**: Required, brief summary in imperative mood
4. **Body**: Optional, detailed explanation after blank line
5. **Footer**: Optional, metadata like breaking changes or references

## Breaking Changes

Indicate breaking changes with either:
- Append `!` after type/scope: `feat(api)!: remove deprecated endpoint`
- Add footer: `BREAKING CHANGE: removed legacy API v1`

## Examples

### Simple Feature
```
feat(search): add fuzzy search algorithm

Implements Levenshtein distance for better match quality.
```

### Bug Fix with Scope
```
fix(validation): prevent empty email submission

Added client-side validation before form submission.
Ensures email field is not empty or whitespace-only.
```

### Breaking Change
```
feat(api)!: migrate to REST API v2

BREAKING CHANGE: API v1 endpoints removed. Clients must
upgrade to v2 endpoints with new authentication scheme.
```

### Chore
```
chore: update package dependencies

Updated all npm packages to latest stable versions.
```

### Multiple Paragraphs
```
refactor(core): restructure module architecture

Reorganized core modules for better separation of concerns.
Each module now has clear responsibility boundaries.

This change improves maintainability and makes the codebase
easier to navigate for new contributors.
```

## Validation Checklist

Before finalizing your commit message, verify:

- [ ] Type is one of the valid types (feat, fix, chore, etc.)
- [ ] Type is lowercase
- [ ] Scope (if used) is in parentheses and lowercase
- [ ] Description starts with lowercase letter
- [ ] Description uses imperative mood ("add" not "added" or "adds")
- [ ] Description is clear and concise
- [ ] Body (if present) is separated by blank line
- [ ] Footer (if present) is separated by blank line
- [ ] Breaking changes are clearly indicated

## Guidelines

**Description Best Practices:**
- Use imperative mood: "add feature" not "added feature"
- Be specific but concise
- Focus on what and why, not how
- Avoid vague terms like "fix stuff" or "update code"

**Body Best Practices:**
- Explain motivation for the change
- Contrast with previous behavior
- Describe side effects or consequences
- Keep lines readable (wrap at reasonable length)

**When to Use Body:**
- Change is not self-explanatory from description
- Multiple related changes need explanation
- Context or rationale should be documented
- Breaking changes need detailed explanation

**When to Skip Body:**
- Change is simple and obvious
- Description fully explains the change
- Self-documenting code changes

## Project-Specific Requirements

This project enforces additional requirements beyond standard conventional commits:

### 1. Maximum Line Length
- **All lines** must be maximum 70 characters (including whitespace)
- This applies to: description, body lines, and footer lines
- Use line breaks to wrap longer explanations

### 2. Required Footer
- **All commits** must include an issue/ticket reference footer
- Format: `Refs: TICKET-ID`
- Examples:
  - `Refs: JIRA-1234`
  - `Refs: #123`
  - `Refs: PROJ-456`
- This links commits to their corresponding issues/tickets

### 3. Preferred Commit Types (in order of frequency)
The project prefers these types in this order:
1. `feat:` - New feature
2. `fix:` - Bug fix
3. `refactor:` - Code refactoring (not `feat` or `fix`)
4. `test:` - Adding/updating tests
5. `chore:` - Maintenance tasks
6. `docs:` - Documentation updates
7. `build:` - Rebuilt dependencies
8. `perf:` - Performance improvements (not `feat` or `fix`)
9. `style:` - Code formatting changes
10. `ci:` - CI/CD changes
11. `revert` - Commit revert

### Updated Validation Checklist

In addition to the standard checklist above, also verify:
- [ ] All lines are maximum 70 characters (including whitespace)
- [ ] Footer includes required ticket reference (Refs: TICKET-ID)
- [ ] Commit type matches project preferences when applicable
