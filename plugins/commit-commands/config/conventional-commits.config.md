# Conventional Commit Configuration

This configuration defines the conventional commit format and validation rules for all commit commands and agents.

## Format Specification

### Standard Format

**Simple commit:**
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

<longer description>

Refs: <task-id>
```

## Mandatory Rules

1. **Type**: REQUIRED, must be lowercase, must be one of the valid types listed below
2. **Colon + Space**: REQUIRED after type or scope: `": "`
3. **Description**: REQUIRED, must start with lowercase letter, imperative mood (command form: add/fix/change, not past tense: added/fixed/changed), no period at end
4. **Scope**: OPTIONAL, must be lowercase if present, wrapped in parentheses
5. **Body**: OPTIONAL, separated from description by blank line, starts with capital letter
6. **Task Reference**: REQUIRED if task context detected in branch name, format `Refs: <TASK-ID>`

## Valid Commit Types

Only these types are allowed:

- **feat**: New feature or functionality added
  - Creating new features
  - Adding capabilities
  - Extending existing features

- **fix**: Bug fix in functionality
  - Resolving defects
  - Correcting errors
  - Patching issues

- **refactor**: Code changes that don't affect functionality or fix bugs
  - Restructuring code
  - Improving code organization
  - Simplifying logic without changing behavior

- **docs**: Documentation changes only
  - README updates
  - API documentation
  - Code comments
  - OpenAPI specs

- **build**: Build system or dependency changes
  - Webpack configuration
  - npm/yarn dependencies
  - Build scripts
  - Asset compilation

- **test**: Test additions or modifications
  - Adding new tests
  - Updating existing tests
  - Test infrastructure changes

- **perf**: Performance improvements
  - Optimization changes
  - Speed improvements
  - Resource usage reduction
  - No functionality changes

- **style**: Code formatting and style
  - Whitespace changes
  - Semicolons
  - Line breaks
  - Code formatting only

- **chore**: Maintenance and tooling
  - Package updates
  - Tool configuration
  - Housekeeping tasks

- **ci**: CI/CD configuration and scripts
  - GitHub Actions
  - GitLab CI
  - Jenkins configuration
  - NOT for fixing CI issues (use `fix` instead)

- **revert**: Revert a previous commit
  - Format: `revert: <original commit message>`
  - Include reference to reverted commit

## Task Reference Detection

### Decision Tree

**Task Reference Rule:**
- **IF** branch name contains pattern `[A-Z]+-\d+` (e.g., PA-1234, PROJ-567) → Task reference **MANDATORY**
- **ELSE** (branch is main, develop, or has no task ID) → Task reference **OPTIONAL**

### Branch Name Patterns

- `feature/<TASK-ID>-*` (e.g., `feature/PA-1234-add-auth`)
- `fix/<TASK-ID>-*` (e.g., `fix/PA-5678-memory-leak`)
- `refactor/<TASK-ID>-*` (e.g., `refactor/PROJ-999-cleanup`)
- `<PROJECT>-<NUMBER>` anywhere in branch name (e.g., `PA-1234`, `PROJ-456`)
- Pattern: `[A-Z]+-\d+` (uppercase letters, hyphen, numbers)

### Task Reference Format

When task reference is required:
```
<type>: <description>

<optional longer description>

Refs: <TASK-ID>
```

The `Refs:` line must:
- Start with capital "R"
- Be on its own line
- Be preceded by a blank line (separated from description or body)
- Match the task ID from the branch name

## Forbidden Patterns

These commit messages are REJECTED:

### Non-Descriptive Commits

❌ `fix: ci` - Too vague, doesn't explain what was fixed
❌ `ci: fix` - Too vague, doesn't explain what changed
❌ `chore: updates` - Too generic, what was updated?
❌ `refactor: changes` - Too generic, what changed?
❌ `fix: bug` - Too vague, which bug?

### Code Review Artifacts

❌ `refactor: changes after CR` - Not descriptive of actual changes
❌ `fix: pr comments` - Not descriptive of actual changes
❌ `fix: address review feedback` - Not descriptive of actual changes
❌ `chore: address review` - Not descriptive of actual changes

### Vague Descriptions

❌ Any description with fewer than 3 meaningful words
❌ Generic verbs without objects (e.g., "fix: update", "feat: add")
❌ Descriptions that don't explain what changed or why

### Format Violations

❌ Uppercase type: `Fix: bug` (should be `fix:`)
❌ Uppercase scope: `feat(Auth): feature` (should be `feat(auth):`)
❌ Uppercase description start: `feat: Add feature` (should be `feat: add feature`)
❌ Trailing period: `feat: add feature.` (should be `feat: add feature`)
❌ Missing colon: `feat add feature` (should be `feat: add feature`)
❌ Missing space after colon: `feat:add feature` (should be `feat: add feature`)

### Wrong Tense/Mood

❌ Past tense: `feat: added feature` (should be `feat: add feature`)
❌ Past tense: `fix: fixed bug` (should be `fix: resolve bug`)
❌ Continuous: `feat: adding feature` (should be `feat: add feature`)
❌ Continuous: `fix: fixing bug` (should be `fix: resolve bug`)

## Format Rules Summary

### Type Rules
- Must be lowercase
- Must be from valid types list
- No spaces in type

### Scope Rules (if present)
- Must be lowercase
- Wrapped in parentheses
- No spaces inside parentheses
- Describes component/area (e.g., `api`, `auth`, `parser`, `ui`)

### Description Rules
- Must start with lowercase letter
- Must be imperative mood (command form: "add", "fix", "change")
- No period at the end
- Should be specific and descriptive
- Minimum 3 meaningful words

**Meaningful words** are words with semantic content, excluding:
- Articles: a, an, the
- Prepositions: in, on, at, to, from, for
- Conjunctions: and, or, but

Example: "fix: add a test" → 2 meaningful words (add, test)
Example: "feat: add user authentication" → 3 meaningful words (add, user, authentication)

### Body Rules (if present)
- Separated from description by blank line
- Starts with capital letter
- Can be multiple paragraphs
- Explains the "why" not the "what"

### Task Reference Rules (if required)
- Separated from body by blank line
- Format: `Refs: <TASK-ID>`
- Capital "R" in "Refs"
- Must match task ID from branch name
- Can reference multiple tasks: `Refs: PA-1234, PA-5678`

## Examples

### ✅ Valid Commits

**Simple commit:**
```
feat: add user authentication endpoint
```

**With scope:**
```
feat(auth): implement jwt token validation
```

**With body:**
```
fix: resolve memory leak in event listeners

Remove event listeners when components unmount to prevent
memory accumulation during navigation
```

**With task reference:**
```
feat: add oauth2 authentication flow

Implements OAuth2 authorization code flow with PKCE for
secure third-party authentication

Refs: PA-1234
```

**Complex example:**
```
refactor(api): extract validation middleware

Centralizes request validation logic into reusable middleware
functions, reducing duplication across route handlers

Refs: PROJ-5678
```

**Multiple task references:**
```
fix: resolve race condition in data sync

Adds mutex lock to prevent concurrent writes to shared state

Refs: PA-1234, PA-5678
```

**Revert commit:**
```
revert: add experimental feature

This reverts commit abc123def456

Refs: PA-9999
```

### ❌ Invalid Commits

**Uppercase type:**
```
Fix: bug in parser
# Should be: fix: resolve null pointer exception in json parser
```

**Past tense:**
```
feat: added new feature
# Should be: feat: add new feature
```

**Continuous tense:**
```
fix: fixing memory leak
# Should be: fix: resolve memory leak in event handlers
```

**Uppercase description:**
```
feat: Add authentication
# Should be: feat: add authentication
```

**Trailing period:**
```
feat: add feature.
# Should be: feat: add feature
```

**Missing colon:**
```
feat add feature
# Should be: feat: add feature
```

**Non-descriptive (forbidden pattern):**
```
fix: ci
# Should be: fix: resolve docker build timeout in ci pipeline
# or: ci: increase build timeout to 10 minutes
```

**Code review artifact (forbidden pattern):**
```
refactor: changes after CR
# Should be: refactor(api): extract error handling logic
```

**Wrong case scope:**
```
feat(API): add endpoint
# Should be: feat(api): add endpoint
```

**Missing task reference (when on feature/PA-1234-auth branch):**
```
feat: add authentication
# Should be:
# feat: add authentication
#
# Refs: PA-1234
```

**Vague description:**
```
chore: updates
# Should be: chore: update eslint to version 8.5
```

## Validation Checklist

Use this checklist to validate commit messages:

- [ ] Type is valid (from the allowed list)
- [ ] Type is lowercase
- [ ] Colon and space after type/scope
- [ ] Description starts with lowercase
- [ ] Description is imperative mood
- [ ] Description has no trailing period
- [ ] Scope is lowercase (if present)
- [ ] Body starts with capital letter (if present)
- [ ] Blank line separates description and body (if body present)
- [ ] Task reference included if branch has task ID
- [ ] Task reference format is correct: `Refs: <TASK-ID>`
- [ ] No forbidden patterns detected
- [ ] Description is specific (not vague)
- [ ] Minimum 3 meaningful words in description

## Error Messages

When validation fails, provide clear, actionable error messages:

**Format:**
```
❌ Commit message validation failed:

Issue: [specific problem]
Rule: [which rule was violated]
Your message: "[actual message]"

Fix: [specific suggestion with example]
```

**Example error message:**
```
❌ Commit message validation failed:

Issue: Type must be lowercase
Rule: Format Rules - Type must be lowercase
Your message: "Fix: resolve parser error"

Fix: Change "Fix" to "fix":
fix: resolve parser error
```

## References

- [Conventional Commits Specification](https://www.conventionalcommits.org/en/v1.0.0/)
- [Angular Commit Guidelines](https://github.com/angular/angular/blob/main/CONTRIBUTING.md)
- [Git Trailers Documentation](https://git-scm.com/docs/git-interpret-trailers)
