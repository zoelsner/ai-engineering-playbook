# Architecture

This is required reading before code, test, tooling, or architecture changes.

## Thesis

State the core shape of the system in one paragraph.

Example: The domain layer owns product rules. Frameworks, providers, storage, and UI are adapters around that tested core.

## Stack Choices

| Choice | Why | Trade-Off |
|---|---|---|
| | | |

## Folder Structure

```text
src/
  domain/
  application/
  infrastructure/
  app/
test/
e2e/
docs/
```

## Boundaries

- `domain` must not import framework, UI, database, SDK, or network modules.
- `application` coordinates domain behavior and ports.
- `infrastructure` implements adapters for storage, providers, network, and platform services.
- `app` owns routing, rendering, composition, and delivery.

## Testing Strategy

- Domain tests colocated with domain modules.
- Integration tests cover adapter and application behavior.
- E2E tests protect the main user workflow.
- No paid model calls, real credentials, or nondeterministic network calls in default CI.

## Definition of Done

- [ ] Lint passes.
- [ ] Format check passes.
- [ ] Typecheck passes.
- [ ] Unit/application tests pass.
- [ ] Build passes.
- [ ] E2E passes when relevant.
- [ ] Architecture docs updated when architecture changed.
- [ ] No boundary violations or undocumented type escapes.
- [ ] PR title follows Conventional Commits.

## Exception Registers

Track exceptions openly with reason, owner, and expiration.

### Type Escapes

| File | Reason | Owner | Expires |
|---|---|---|---|
| | | | |

### Skipped Tests

| Test | Reason | Owner | Expires |
|---|---|---|---|
| | | | |

### Boundary Deviations

| File | Reason | Owner | Expires |
|---|---|---|---|
| | | | |
