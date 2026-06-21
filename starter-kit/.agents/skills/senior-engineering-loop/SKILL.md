---
name: senior-engineering-loop
description: Goal-driven senior engineering workflow. Use for end-to-end goals, production-grade implementation, unfamiliar repo refactors, root-cause debugging, performance optimization, UI components, publishable APIs, architecture/design review, or second-opinion gates.
---

# Senior Engineering Loop

Use this skill to turn an open-ended engineering request into a complete execution loop: understand the goal, inspect the real system, design, implement, validate real behavior, review, and iterate until the result meets the stated bar.

Default collaboration model: Codex drives the loop and remains responsible for the final decision. Claude, via `claude -p`, is a skeptical reviewer for pressure-testing plans, loops, features, code, and validation evidence when the cost of missing something is high.

## Operating Loop

1. State the goal and success bar in concrete terms.
2. Select the closest profile from `references/loop-catalog.md`.
3. Build context before changing code.
4. Make a plan proportional to risk.
5. Implement in coherent slices.
6. Validate the real thing after each meaningful step.
7. Review edge cases, maintainability, security, performance, accessibility, and user-facing behavior as relevant.
8. Iterate on failed validation or review findings before finalizing.
9. Finish with a concise summary, verification performed, and residual risks.

## Second-Opinion Gate

Use a second-opinion gate after designing an API, data model, architecture, security boundary, migration, or large refactor, and before committing to implementation when the cost of a bad design is high. Also use it before complex feature builds, when pressure-testing a broad loop, or after implementation as a code review pass.

If an external reviewer CLI such as `claude -p` is available and safe, send only the necessary non-secret design context:

```text
Act as a skeptical senior engineering reviewer. Review this plan, feature, code summary, or loop before Codex continues.

Goal:
{user goal}

Artifact under review:
{plan, feature shape, architecture, API routes, data model, code summary, test evidence, assumptions}

Please identify:
1. correctness or architecture risks
2. missing edge cases
3. simpler alternatives
4. validation and test gaps
5. the highest-impact changes before implementation
```

Treat reviewer output as critique, not authority. Codex should summarize what it accepted, what it rejected, and why. If external review is unavailable or unsafe, run the same checklist internally and say so.
