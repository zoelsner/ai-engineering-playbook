# AI Engineering Starter Kit

Copy this directory into a new or existing repo when you want the playbook to become working project structure instead of reference material.

## Recommended Copy

```bash
cp -R starter-kit/. /path/to/your-repo/
```

Then adjust the placeholders in:

- `AGENTS.md`
- `CONTEXT.md`
- `docs/architecture.md`
- `docs/development-plan.md`
- `docs/design-quality.md`
- `.github/ISSUE_TEMPLATE/development-slice.md`
- `.github/pull_request_template.md`

## Optional Skill Symlinks

If you use multiple coding agents, keep one shared skill copy at `.agents/skills/` and add relative symlinks:

```bash
ln -s ../.agents/skills .claude/skills
ln -s ../.agents/skills .codex/skills
ln -s ../.agents/skills .pi/skills
```

Commit the symlinks if your tools support them in your environment.

## First Pass Checklist

- [ ] Replace placeholder project names and links.
- [ ] Define the repo's test, lint, typecheck, build, and E2E commands.
- [ ] Decide whether the red-test gate applies to all code changes or only product code.
- [ ] Decide the accessibility standard for UI work. Default recommendation: WCAG 2.2 AA for web UI.
- [ ] Add any project-specific exceptions to `AGENTS.md`.
- [ ] Turn on branch protection after the first CI run is green.
