---
name: grill-me
description: >
  Interview the user relentlessly about a plan or design until reaching shared understanding,
  resolving each branch of the decision tree. Use when the user wants to stress-test a plan,
  get grilled on their design, or says "grill me", "interview me", "stress test this",
  "challenge my assumptions", or "poke holes in this".
---

# Grill Me

Interview the user relentlessly about every aspect of the plan or design until reaching a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one.

## Rules

- **Ask one question at a time** — wait for the answer before proceeding to the next.
- **Provide your recommended answer** upfront for each question.
- **Explore the codebase** instead of asking when a question can be answered by reading code.
- **Follow dependency chains** — don't ask about downstream decisions until upstream ones are settled.
- **Keep going** until there are no unresolved branches left in the design tree.
- **Summarize all decisions** before finishing.
