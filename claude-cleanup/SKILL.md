---
name: claude-cleanup
description: >
  Refactor a CLAUDE.md file to follow progressive disclosure principles. Use when the user
  says "cleanup CLAUDE.md", "refactor CLAUDE.md", "organize instructions", "slim down
  CLAUDE.md", or wants to reduce context bloat in their project instructions.
---

# CLAUDE.md Cleanup

Refactor the CLAUDE.md file to follow progressive disclosure principles.

## Steps

1. **Find contradictions** — identify conflicting instructions, ask which to keep
2. **Extract essentials** — keep only what belongs in root CLAUDE.md:
   - One-sentence project description
   - Package manager (if not npm)
   - Non-standard build/typecheck commands
   - Anything relevant to every single task
3. **Group the rest** — organize into logical categories (e.g., TypeScript conventions,
   testing patterns, API design, Git workflow). Create a separate file for each group.
4. **Create the file structure** — output a minimal root CLAUDE.md with links to
   separate files, plus each file with its instructions. Suggest a docs/ folder structure.
5. **Flag for deletion** — identify instructions that are redundant, too vague, or
   overly obvious (e.g., "write clean code").
