# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A library of Claude Code skills — modular packages in `<skill-name>/SKILL.md` format that extend Claude's capabilities. `~/.claude/skills` is a symlink to this repo, so skills here are live.

## Skill Structure

Every skill is a directory containing at minimum a `SKILL.md` with YAML frontmatter:

```
skill-name/
├── SKILL.md          # required: frontmatter (name, description) + instructions
├── scripts/          # executable code run directly, not loaded into context
├── references/       # docs loaded into context on demand
└── assets/           # templates/files used in output, not loaded into context
```

Frontmatter allowed fields: `name`, `description`, `license`, `allowed-tools`, `metadata`. No other fields.

## Development Workflow

**Initialize a new skill:**
```bash
python skill-dev/scripts/init_skill.py <skill-name> --path .
```

**Validate a skill:**
```bash
python skill-dev/scripts/quick_validate.py <skill-name>/
```

**Package a skill for distribution** (validates first, then creates `<skill-name>.skill` zip):
```bash
python skill-dev/scripts/package_skill.py <skill-name>/
```

## Key Conventions

- Skill `description` in frontmatter is the triggering mechanism — make it comprehensive with concrete "when to use" phrases. Keep `name` hyphen-case, max 64 chars; `description` max 1024 chars.
- SKILL.md body should stay under 500 lines. Move verbose content to `references/` files and link to them from SKILL.md.
- Never create README.md, CHANGELOG.md, or other auxiliary docs inside skill directories — only files an AI agent needs to do the job.
- Commit messages use Conventional Commits (`feat`, `fix`, `docs`, `refactor`, `chore`, etc.). Do NOT add `Co-Authored-By: Claude` trailers.
- Reference design patterns: `skill-dev/references/workflows.md` (sequential/conditional flows), `skill-dev/references/output-patterns.md` (templates and examples).
