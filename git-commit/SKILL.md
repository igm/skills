---
name: git-commit
description: >
  Commit changes to the current branch using conventional commit format. Use when the user
  says "commit", "commit changes", "make a commit", or "git commit". Runs tests first if
  the project has a test suite, groups unrelated changes into separate commits.
---

# Git Commit

Commit staged changes. If nothing is staged, stage and commit all changes.

## Pre-commit

Run tests if the project has a test suite. Fix any failures before committing.

## Grouping

If changes span unrelated concerns, split into multiple commits — one per concern.

## Commit Message Format

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

### Types

| Type | Purpose |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, semicolons, etc. |
| `refactor` | Neither feature nor fix |
| `perf` | Performance improvements |
| `test` | Adding or updating tests |
| `build` | Build system or dependencies |
| `ci` | CI/CD changes |
| `chore` | Other changes |

### Guidelines

- Imperative mood ("add" not "added")
- First line under 72 chars
- Reference issues in footer: `Closes #123`
- Body explains WHY, not just WHAT
- Do NOT include `Co-Authored-By: Claude` or any AI co-author trailer
