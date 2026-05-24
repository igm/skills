---
name: plan-validate
description: >
  Validate code changes against a plan file to verify complete and correct implementation.
  Use after plan-execute completes, or when the user says "review against the plan",
  "verify implementation", "check if the plan was followed", or "validate the changes".
  Reads the plan HTML, compares against the git diff, and reports step-by-step
  completeness, deviations, and issues. Also triggers when reviewing a PR or branch
  that has an associated plan in .claude/plans/.
---

# Code Review — Plan-to-Code Verification

Review code changes against a plan to verify correct and complete implementation.

## Workflow

### 1. Locate the plan

Check for a plan file in `.claude/plans/`. If the user provides a path, use it.
Otherwise, find the most recent plan matching the current work.

### 2. Get the changes

Choose the appropriate diff:

- In-progress work: `git diff HEAD`
- Branch vs main: `git diff main...HEAD`
- Staged only: `git diff --staged`

### 3. Compare implementation against plan

For each plan step, check:

1. **File match** — does the diff touch the files the step specifies?
2. **Approach match** — does the implementation follow the described approach?
3. **Completeness** — fully implemented, partial, or missing?
4. **Correctness** — bugs, edge cases, style issues?

Rate each step:

| Rating | Meaning |
|--------|---------|
| Done | Fully implemented, matches plan |
| Partial | Some work done but incomplete |
| Deviated | Implemented differently from plan |
| Missing | No changes found |

### 4. Report

**Summary**: X/Y steps complete, Z deviations, N issues.

**Per-step**: Rating + 1–2 sentence explanation for each.

**Issues**: Bugs, missing error handling, style inconsistencies.

**Deviations**: Where implementation diverges — explain what changed and whether it's intentional.

### 5. Update plan state

After reporting, if any steps are rated "Done", ask the user for confirmation before touching the file:

> "Mark N steps as done in the plan file? (Y/n)"

On confirmation, read the plan HTML, identify all checkable elements (step cards, checklist items, or anything similar), and add `checked` to whichever ones correspond to passing steps. Use judgment — the structure may vary between plans.

- **Never** modify plan content (titles, descriptions, file paths) — only add/remove the `checked` attribute on checkboxes.
- If implementation diverges from plan, flag for discussion rather than rewriting the plan.

## Composition

- Use after `/plan-execute` to validate the implementation
- Use alongside `/verify` to also run the app and check behavior
