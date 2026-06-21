## Linked Issue

Closes #<issue-number>

## Before Any Code Checklist

- [ ] Linked issue or documented docs-only scope
- [ ] Branch is not `main`
- [ ] Preflight or setup check posted
- [ ] Failing test/reproduction observed before non-test product code edits, or docs-only path used
- [ ] Architecture/plan check against `docs/architecture.md` and `docs/development-plan.md`

## Scope

What coherent slice does this PR complete?

## Non-Goals

What is intentionally out of scope?

## Test Evidence

- [ ] Lint
- [ ] Format check
- [ ] Typecheck
- [ ] Unit/application tests
- [ ] Build
- [ ] E2E, if relevant
- [ ] Accessibility checks, if UI changed
- [ ] State matrix covered, if UI changed
- [ ] Markdown/link checks, if docs changed

Commands run:

```text
```

## Architecture And Docs

- [ ] This follows `docs/architecture.md`
- [ ] Architecture docs were updated, if architecture changed
- [ ] Context/domain language was updated, if terminology changed
- [ ] No boundary violations were introduced

## Accessibility And States

Required for UI work. Write `N/A` for backend/docs-only changes.

- [ ] Keyboard flow works
- [ ] Focus states are visible
- [ ] Interactive controls have accessible names
- [ ] Errors are associated with fields and recoverable
- [ ] Contrast and target sizes meet the project bar
- [ ] Reduced-motion and mobile/narrow states were considered
- [ ] Loading, empty, partial, error, disabled/permission, and long-content states are covered or explicitly non-applicable

## Type Safety

- [ ] Runtime data is parsed or narrowed instead of cast
- [ ] No `as any`, `: any`, `<any>`, or `as unknown as` was introduced
- [ ] Any unavoidable type escape uses a documented `type-escape:` marker

## Risk And Rollback

What can go wrong, and how can this be rolled back?

## Dependencies

List new dependencies and explain why each is needed.
