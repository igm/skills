---
name: plan-create
description: >
  Explore the codebase and produce a rich HTML implementation plan for a feature, refactor,
  performance improvement, or architectural change. Use when the user wants to plan or design
  a change and says things like "create a plan", "plan this", "I want to improve/add/fix X",
  "make a plan for X", or "design the approach for X". Outputs an HTML file (not markdown) at
  .claude/plans/SLUG.html where SLUG is derived from what is being planned. Does NOT implement
  — plan only. Use plan-execute to implement the generated plan.
---

# Plan

Explore the codebase, understand the intent, then produce a self-contained HTML plan document
that is scannable and immediately actionable.

## Workflow

### Phase 1: Explore

Before asking anything, understand the current state:

1. Launch up to 3 Explore agents in parallel targeting:
   - The code directly relevant to what's being planned
   - Related components, data structures, integration points
   - Existing patterns, utilities, and conventions to reuse

2. Read CLAUDE.md and any relevant docs/ for project conventions.

**Questions answerable by reading code must not be asked to the user.**

### Phase 2: Interview

Ask at most 2–3 focused questions with 2–4 concrete options each and a clear recommendation.
Cover scope, key tradeoffs, and any ambiguity that exploration couldn't resolve. Skip entirely
if the intent is already unambiguous.

For complex/ambiguous plans, the user can invoke grill-me for a deeper interview before
returning to write the plan.

### Phase 3: Write the HTML Plan

**File naming:** derive a short, descriptive kebab-case slug from what is being planned.
Save to `.claude/plans/<slug>.html` — never `.md`.

Good slug examples:
- "plan performance improvements" → `performance-improvements.html`
- "plan auth refactor" → `auth-refactor.html`
- "plan watchlist concurrency" → `watchlist-concurrency.html`

**Start from the template** at `.claude/skills/plan-create/assets/plan-template.html`
— read it, then adapt it: replace all placeholder comments, add/remove sections as needed,
fill in real content. Do not copy boilerplate sections that don't apply.

**Anchor links:** Step cards use `id="phase-N-step-M"`, phase sections use `id="phase-N"`.
Each phase `<h2>` and `<h3>` must include an `<a class="anchor" href="#ID">#ID</a>` that reveals
on hover so readers can copy-permalink to any phase or step.

Checkboxes use native `<input type="checkbox" class="step-check">` wrapped in a `<label>` inside
`<article><header>` and `<input type="checkbox" class="verify-check">` in checklist `<li>` items.
CSS `:has()` handles the done/strikethrough state — no JavaScript needed. Checked state is
session-only (no persistence).

**HTML requirements:**

- Use semantic HTML elements styled by Pico CSS (loaded from CDN in template) — no custom classes
  beyond `.step-check`, `.verify-check`, and `.anchor`; no inline styles
- Structure: `<main>` → `<section id="phase-N">` → `<article id="phase-N-step-M">` per step
- Badges: `<span data-badge="info|safe|warn|risk">` (CSS in template handles colors)
- Tags/labels: `<kbd>tag-name</kbd>`
- Context block: `<aside>` with `<h4>Context</h4>`
- Every step must have: file path(s) in `<code>`, concrete approach, measurable outcome
- Verification checklist at the end with specific, runnable checks
- Recommended execution order when multiple changes are involved

**Visual & interactive elements (encouraged):**

- **Inlined SVG:** Use for visualizing data flows, component interaction diagrams, architecture
  overviews, state machines, or any concept that benefits from a visual representation. Keep SVGs
  self-contained and styled with CSS variables from the design system.
- **Tables:** Use for structured comparisons (before/after, alternatives, file inventories,
  API signatures, dependency matrices). Prefer tables over verbose prose for tabular data.
- **JavaScript:** Use for progressive disclosure (collapsible sections, toggleable detail panels),
  interactive filtering of step lists, dynamic tab switching, or any behavior that helps readers
  focus on what's relevant without losing context. Keep JS minimal and scoped.
- **Syntax highlighting:** Use **[highlight.js](https://highlightjs.readthedocs.io/en/latest/readme.html)**
  (loaded from CDN — already included in the template) for all code blocks. Wrap snippets in
  `<pre><code class="language-LANG">` where LANG is the language identifier (e.g., `language-go`,
  `language-html`, `language-bash`, `language-yaml`, `language-sql`, `language-js`). After writing
  all content, call `hljs.highlightAll()` to activate highlighting on every `<pre><code>` block.
  For change descriptions showing diffs, use `language-diff` which natively renders `+`/`-` prefixes
  with appropriate colors. Never leave code blocks unhighlighted — every `<pre><code>` must have a
  language class.

**Plan content rules:**

- One recommended approach, not a menu of alternatives
- Include file paths and line numbers
- Reference existing functions/utilities to reuse
- Concise enough to scan in 2 minutes, detailed enough to execute without re-reading code
- Context section: why this change, what prompted it, intended outcome
- No implementation — plan only

## Review & Execution

After plan-execute finishes, use plan-validate to verify implementation against the plan.

The plan is an immutable reference document — content (titles, descriptions, file paths,
approach details, verification items) must never be modified during review unless the user
explicitly requests an update. Only step completion state (`data-step-id` checkboxes) is mutable.

## Output

After writing the file, tell the user:
- The full file path
- A one-sentence summary of what the plan covers
- How to open it: `open .claude/plans/<slug>.html`
