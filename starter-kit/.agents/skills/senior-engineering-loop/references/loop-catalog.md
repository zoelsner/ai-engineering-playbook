# Loop Catalog

Use the smallest loop that covers the task. Combine loops when the user asks for broad work such as "make this production-ready and fast."

## Baseline Goal Loop

Use for most engineering tasks.

```text
Goal: {task or agreed spec}

Continue until the architecture, implementation, tests, review, and final result meet the bar, not merely until the code runs. After every meaningful step, validate real behavior with the best available evidence.
```

Checklist:

1. Define success criteria.
2. Inspect the relevant system context.
3. Plan the smallest complete path.
4. Implement in coherent slices.
5. Validate after each slice.
6. Review and iterate.
7. Report what changed and what was verified.

## Production Build Loop

Use when building a feature, service, tool, or app that should be shippable.

Cover requirements, non-goals, edge cases, architecture, error handling, security, observability, tests, and handoff.

## Unfamiliar Repo Refactor Loop

Use when inheriting a codebase, cleaning structure, or refactoring.

Map entry points, core modules, ownership boundaries, state/data flow, and test coverage before changing structure. Refactor only where the payoff is real.

## Senior Debugging Loop

Use for bugs, regressions, failing tests, incidents, or unexplained behavior.

Reproduce the issue, isolate root cause, fix the cause rather than the symptom, add regression coverage, and verify the original failure no longer occurs.

## Performance Loop

Use for speed, memory, scalability, slow UI, slow APIs, expensive queries, or unnecessary renders.

Define the metric and workload, measure or infer a baseline with evidence, optimize the smallest high-impact path, then re-measure.

## Production UI Component Loop

Use for reusable frontend components, forms, dashboards, and interactive UI.

Cover existing design conventions, props and composition, loading/empty/error/disabled states, responsive layout, text fitting, keyboard interaction, focus, labels, and accessibility evidence.

## Publishable API Loop

Use for backend routes, webhooks, SDK-facing APIs, and service boundaries.

Cover resource model, routes, validation, authentication, authorization, idempotency, pagination, rate limits, error model, controller/service/data boundaries, and tests.

## Second-Opinion Review Loop

Use after design and before implementation for high-impact API, architecture, migration, security, or refactor decisions. Also use when pressure-testing a complex feature plan, a broad execution loop, or a post-implementation code review.

Codex remains the driver. Claude, via `claude -p`, is the reviewer when available and safe. Incorporate useful critique, reject mismatched advice, then implement, validate, or revise the loop.

Never send secrets, credentials, private production data, or unnecessary proprietary context to an external reviewer.
