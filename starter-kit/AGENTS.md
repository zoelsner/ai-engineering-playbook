# Agent Operating Rules

This file is the repo constitution for coding agents and humans working with them. Keep it short enough to be read, strict enough to matter, and updated whenever the process misses.

## Before Any Code

Before editing any non-test product code, post these artifacts in chat:

1. Linked issue or task URL for the slice.
2. Branch confirmation from `git status --short --branch`.
3. Preflight result or the closest repo-specific setup check.
4. Failing test or reproduction output observed before the fix.
5. Architecture/plan check against `docs/architecture.md` and `docs/development-plan.md`.

If any item is missing, stop and explain what is missing.

### Docs-Only Path

For docs-only changes, use this lighter gate instead:

1. Scope: name the docs being changed and the intended reader.
2. Source: list any upstream sources or say "no external source."
3. Consistency: check whether README, templates, and related docs need the same change.
4. Quality: run markdown/link checks when available.
5. Review: scan for stale counts, contradictions, broken relative links, and unclear reuse/license claims.

## Senior Engineering Loop

For non-trivial work:

```text
Goal: {task or agreed spec}

Continue until the architecture, implementation, tests, review, and final result meet the bar, not merely until the code runs. After every meaningful step, validate real behavior with the best available evidence.
```

Use the closest profile from `.agents/skills/senior-engineering-loop/references/loop-catalog.md`: baseline, production build, unfamiliar repo refactor, debugging, performance, UI component, API, or second-opinion review.

## Second-Opinion Gate

Default model:

- Codex is the driver for context gathering, planning, implementation, validation, and final judgment.
- Claude is the reviewer when a second opinion would raise quality.
- The user stays accountable for the decision. Reviewer output is critique, not authority.

Before implementing high-cost API, architecture, data model, migration, security, or large refactor decisions, get a skeptical second opinion when safe. Also use `claude -p` to pressure-test complex feature plans, broad loops, and post-implementation code review summaries.

Useful checkpoints:

1. Plan pressure test before implementation.
2. Loop pressure test when the work is broad or fuzzy.
3. Code review pass after implementation and local validation.

If external review is unavailable or unsafe, run the same critique internally and state that it was internal.

## UI Acceptance

For UI work, issue and PR acceptance criteria must cover:

- Keyboard navigation.
- Visible focus.
- Accessible names for controls.
- Error descriptions and recovery paths.
- Contrast and target size.
- No color-only meaning.
- Reduced motion.
- Mobile/narrow widths.
- Loading, empty, partial, error, offline/slow, disabled/permission, long-content, high-contrast states.

Use `docs/design-quality.md` for the full checklist.

## Scope Discipline

- One issue per PR.
- One coherent slice per PR.
- No opportunistic cleanup unless it directly supports the slice.
- Update docs in the same PR when behavior, architecture, workflow, or terminology changes.

## Type and Test Discipline

- Prefer parsers, type guards, and discriminated unions over assertions.
- Avoid `as any`, `: any`, `<any>`, and `as unknown as`.
- Unavoidable escapes need a nearby `type-escape:` marker with a reason and expiration.
- Add regression coverage for bug fixes.
- Keep test placement consistent with the repo's existing pattern.

## Commit Discipline

Use Conventional Commits:

```text
type(scope): imperative summary
```

Allowed types: `feat`, `fix`, `docs`, `test`, `refactor`, `perf`, `build`, `ci`, `chore`, `revert`.

Non-trivial commits should explain why in the body and include issue/PR references when available.

## Recent Misses

Record process failures here. Every miss should name the date, the failure, and the correction.

- YYYY-MM-DD: Example miss. Correction: linked PR or rule update.
