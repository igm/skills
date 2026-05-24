---
name: plan-execute
description: Implement a plan from a file (HTML or Markdown). Use when the user says "implement plan [path]", "run plan [path]", or "execute plan [path]" with an explicit file path to a plan in ~/.claude/plans/ or elsewhere. Use plan-create to generate plans first. The plan file must be provided — do not auto-discover.
---

# Plan Executor

Read a plan file, create tasks from its steps, and implement each step sequentially.

## Workflow

### 1. Read the plan

Read the plan file at the user-provided path. Extract:

- **Context** — background and motivation
- **Steps** — ordered implementation steps, typically in a table or numbered list
- **Dependencies** — which steps depend on others
- **Verification** — how to confirm correctness

### 2. Create tasks

For each implementation step in the plan, create a task with:

- **Subject**: concise action from the plan step (imperative form)
- **Description**: the plan step details, including target file(s) and what to change
- **Dependencies**: set `blockedBy` if the plan specifies step ordering

Skip creating tasks for verification-only steps — handle those during execution.

### 3. Implement steps

Work through tasks in order. For each task:

1. Mark it `in_progress`
2. **Read before editing** — always read the target file(s) to understand current state
3. Make the changes described in the plan step
4. If the step has independent sub-changes, make them in parallel
5. Mark it `completed` when done

If a step fails or reveals missing context:

- Re-read the plan to check if you misunderstood
- If the plan is ambiguous, stop and ask the user rather than guessing
- Do not skip steps or silently alter the plan

### 4. Verify

After all implementation steps:

1. Build the project (`go build .` or equivalent)
2. Run tests (`go test ./...` or equivalent)
3. Run linter (`golangci-lint run` or equivalent)
4. Regenerate fixtures if the project uses golden-file testing (`make fixtures` or equivalent)
5. Re-run tests after fixture regeneration

If verification fails, fix the issue and re-verify. Do not mark the task as done until it passes.

### 5. Summarize

After all steps are complete, provide a brief summary:

- What was changed (files + nature of changes)
- Test/lint results
- Any deviations from the plan and why

Do not commit unless the user asks.

## Guidelines

- Prefer `Edit` over `Write` for existing files
- Run `goimports` (or project-specific formatter) after editing Go files
- Preserve the project's existing code style and conventions
- If the plan references line numbers, treat them as approximate — always read the file first
- When the plan specifies "add field to struct", also update any constructors, tests, and fixtures that reference the struct
- Parallelize independent steps using multiple tool calls in a single response
