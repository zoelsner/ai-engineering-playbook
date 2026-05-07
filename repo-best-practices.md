# Repo-as-a-System: Best Practices for Agent-Driven Development

> **Source:** Every pattern in this document comes from [**lightstrikelabs/repo-analyzer-green**](https://github.com/lightstrikelabs/repo-analyzer-green). All credit for the patterns, examples, file structures, and design decisions goes to that repo's authors. This document is my synthesis and commentary — the originals are the canonical reference.
>
> Direct sources used: [AGENTS.md](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/AGENTS.md) · [docs/architecture.md](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/docs/architecture.md) · [docs/skills.md](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/docs/skills.md) · [docs/agent-compat.md](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/docs/agent-compat.md) · [docs/commit-messages.md](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/docs/commit-messages.md) · [.github/ISSUE_TEMPLATE/development-slice.md](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/.github/ISSUE_TEMPLATE/development-slice.md) · [.github/pull_request_template.md](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/.github/pull_request_template.md) · [.github/workflows/ci.yml](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/.github/workflows/ci.yml) · [lefthook.yml](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/lefthook.yml) · [.claude/settings.json](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/.claude/settings.json) · [.agents/skills/](https://github.com/lightstrikelabs/repo-analyzer-green/tree/main/.agents/skills)

A portable playbook distilled from [lightstrikelabs/repo-analyzer-green](https://github.com/lightstrikelabs/repo-analyzer-green). Drop the patterns here into any new repo and you get a setup where humans and coding agents (Claude Code, Codex CLI, pi) can collaborate with mechanical guardrails, traceable history, and a real architecture doc.

---

## The Big Idea

Most repos treat process as documentation that nobody reads. This repo treats it as **layered enforcement** — the same rule appears in (1) docs, (2) chat-visible artifacts the agent must produce, (3) local hooks, and (4) CI/branch protection. When the agent forgets, the hook blocks the edit. When the hook is skipped, CI blocks the merge. When CI is wrong, a "Recent Misses" log captures the failure and tightens the rule.

Five components do all the work:

1. **`AGENTS.md`** — agent constitution, with a hard "Before Any Code" gate
2. **`docs/architecture.md`** — required reading, single source of architectural truth
3. **`.agents/skills/`** — shared skills (`distill-issue`, `start-slice`, `preflight`)
4. **Three-surface red-green enforcement** — pre-tool hook, pre-commit hook, CI
5. **Issue + PR templates that mirror each other** — same fields, same shape

---

## 1. `AGENTS.md` — The Agent Constitution

A single file that every coding agent loads automatically (Claude Code, Codex CLI, and pi all read it). `CLAUDE.md` is just `@AGENTS.md` so they stay in sync.

### The "Before Any Code" gate

The single highest-value pattern. Before editing any non-test file, the agent must post **five chat-visible artifacts**:

1. **Linked issue** — GitHub issue URL for this slice. No issue → run `/distill-issue` first.
2. **Branch** — `git status` excerpt confirming a feature branch (not `main`).
3. **Preflight** — output of a `/preflight` script that audits the workspace (auth, hooks, runtime versions, etc).
4. **Failing test (RED)** — `vitest run <file>` output showing a FAIL block, produced **before** any non-test edit.
5. **Architecture/plan check** — one-line confirmation that `docs/architecture.md` and `docs/development-plan.md` were skimmed for conflicts.

If any are missing, **stop and ask**. The chat transcript itself is the audit trail — reviewers can scroll back and see all five.

### The "Recent Misses" log

At the bottom of `AGENTS.md`, a running log of process failures with date + one-sentence summary + link to the corrective PR. Example from the source repo:

> **2026-04-26 — PDF export shipped without observing a red test.** The TDD rule was read and rationalized away as "small UI change"... Lesson: classify-and-skip is the failure mode; produce the chat-visible RED test artifact every time.

This is the institutional memory that prevents the same mistake twice. Every miss becomes a tightened gate (which is how mechanical hooks #105 and #114 came to exist).

### Other AGENTS.md sections worth copying verbatim

- **Scope discipline** — one issue per PR, one PR per issue, no opportunistic cleanup bundled in.
- **Type-safety bans** — `as any`, `: any`, `<any>`, `as unknown as` are blockers, not suggestions. Unavoidable escapes need a `// type-escape:` marker.
- **No barrel files** — direct imports only, no `index.ts`. Keeps the dep graph explicit and bundler-friendly.
- **Test placement rules** — colocated `*.test.ts` for domain; `test/` for harness/fixtures/integration; `e2e/` for browser.
- **Commit hygiene** — Conventional Commits, body explains *why*, `Issue: #N` and `PR: #N` references, no amends during normal work.

---

## 2. `docs/architecture.md` — The Required-Reading Doc

A doc that:
- Agents **must** skim before any code/test/tooling/architecture change
- Must be **updated in the same PR** if architecture changes
- Becomes the answer to "why is this structured this way?"

### What goes in it

- **Architectural thesis** — one paragraph stating the core shape of the system (e.g., "domain-first, frameworks/providers/storage are adapters around a tested core").
- **Stack choices with rationale** — not just "we use Next.js," but "Next.js because we need browser UI + API + server-side analysis; the domain stays independent of Next."
- **Folder structure** — concrete tree showing `src/domain/`, `src/application/`, `src/infrastructure/`, `src/app/`, `test/`, `e2e/`, with rules about which layer can import which.
- **Scaffolding order** — numbered list of the first 10 things to do on a fresh repo. Tooling and CI **before** product code.
- **TypeScript posture** — `strict: true`, `noUncheckedIndexedAccess: true`, `exactOptionalPropertyTypes: true`, ban `any`.
- **Boundary enforcement rules** — `src/domain` cannot import `src/app`/`src/infrastructure`/Next/SDKs/DB clients. Documented now, mechanically enforced later via a dependency-cruiser-style script.
- **Definition of Done** — explicit checklist (lint/typecheck/tests/build/E2E all pass, docs updated, no boundary violations, PR title is Conventional, CI green).
- **Exception registers** — type escapes, lint disables, skipped tests, boundary deviations all tracked openly with reason + owner + expiration.
- **Foundational E2E contract** — one named E2E test that protects the main workflow, deterministic, no paid model calls, no real network.

### Why this matters

When an agent reaches for an "easy" cast or barrel file, the architecture doc is the receipt that says: *we already considered this, here's what we chose and why*. Without it, every session re-litigates the basics.

---

## 3. `.agents/skills/` — Shared Skills With Cross-Agent Compat

All project-level skills live in **one** directory:

```
.agents/skills/<name>/
├── SKILL.md          # required entry point with YAML frontmatter
├── scripts/          # helper scripts the agent invokes
├── references/       # detailed docs loaded on demand
└── assets/           # fixtures, templates, images
```

Pi reads this path natively. Claude Code and Codex reach it via **relative symlinks** committed to git:

```
.claude/skills  ->  ../.agents/skills
.codex/skills   ->  ../.agents/skills
.pi/skills      ->  ../.agents/skills
```

**Write the skill once, all three agents see it.** Don't put skill content directly under `.claude/skills/` or `.codex/skills/` — those are symlinks, not directories.

### The three skills the source repo ships

These compose into a complete "start a slice" flow:

#### `distill-issue` — "I have a description, file an issue"

Turns a chat description of a slice into a properly-structured GitHub issue using the project's template (Problem / Intended Outcome / Non-Goals / Acceptance Criteria / Test Expectations / Architecture Impact / Blockers). The skill ships a small renderer script that takes JSON in, prints the issue body Markdown out, and the agent runs `gh issue create` with the result.

The renderer doesn't call `gh` itself — it prints the body so the human can review the draft before it lands. This is the right tradeoff: automate the boilerplate, keep the human in the loop for the publishing step.

#### `start-slice` — "I have an issue, start working on it"

After `distill-issue` returns a URL, this skill bootstraps the slice in one command:

1. `git fetch origin main`
2. `git worktree add ../worktrees/<issue#>-<slug> -b <issue#>-<slug> origin/main`
3. `pnpm install --frozen-lockfile` in the new worktree
4. Runs `/preflight`

It refuses to proceed if the current branch isn't `main`, the working tree is dirty, the slug isn't lowercase-hyphenated, or the issue number isn't a positive integer. Always supports `--dry-run` so the agent can paste the plan into chat for sanity-check before anything runs.

The worktree pattern is the secret sauce: multiple slices in flight without `git checkout` thrashing, no `node_modules` collisions across branches.

#### `preflight` — "Is this workspace actually set up?"

Audits a fresh clone for fork-day setup gaps:

| Check | What it verifies |
|---|---|
| CI workflow files | `.github/workflows/` exists with at least one `.yml` |
| Claude red-green hook | `.claude/settings.json` parses and declares `hooks.PreToolUse` |
| lefthook git hooks | `.git/hooks/pre-commit` looks like a lefthook-managed hook |
| git remote origin | `git remote get-url origin` returns a URL |
| Node version | satisfies `engines.node` from `package.json` |
| pnpm version | satisfies `engines.pnpm` from `package.json` |
| Vercel project link | `.vercel/project.json` is present (skipped if N/A) |
| `gh` auth status | `gh auth status` exits 0 |
| main branch protection | `gh api .../branches/main/protection` returns 200 |

Each check returns `pass` / `fail` / `skip` with a remediation hint. Output goes straight into chat as the "Before Any Code" item-3 artifact. Exit `1` if anything fails.

### SKILL.md frontmatter rules

```markdown
---
name: my-skill
description: One sentence on what this skill does AND when to use it. The model decides whether to load it from this string — be specific.
---
```

- `name` must match the parent directory exactly, lowercase + digits + hyphens, 1–64 chars, no leading/trailing/consecutive hyphens.
- `description` up to 1024 chars. **The description is what the model sees in its system prompt** — the difference between "Helps with searches" (bad) and "Web search and content extraction via the Brave Search API. Use when looking up documentation, current facts, or any web content" (good) is the difference between a skill that triggers and one that gets ignored.
- Document **when to use** the skill near the top of the body, not just *how*.

### Skill review checklist

Before merging a new skill PR:

- [ ] Name matches parent directory and conforms to naming rules
- [ ] Description is specific enough that the model knows when to use it
- [ ] Body documents *when to use* near the top, not just *how*
- [ ] Helper scripts live under the skill directory, not `scripts/` at repo root
- [ ] If the skill runs scripts with branching logic, those scripts have tooling tests under `test/tooling/`
- [ ] No secrets, tokens, or environment-specific paths checked in
- [ ] Verified to load in at least Claude Code; ideally Codex and pi too

---

## 4. Three-Surface Red-Green Enforcement

The single most clever pattern in the repo. The same TDD rule lives in **one script** (`scripts/red-green-gate.ts`) that's invoked from three places:

### Surface 1: Claude Code PreToolUse hook

`.claude/settings.json`:
```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write|MultiEdit",
        "hooks": [
          { "type": "command", "command": "node \"$CLAUDE_PROJECT_DIR/scripts/red-green-gate.ts\"" }
        ]
      }
    ]
  }
}
```

When the agent tries to Edit/Write/MultiEdit, the hook runs first. If it edits a non-test source file without a recent edit to the colocated `*.test.ts`, the hook blocks the edit.

### Surface 2: Codex CLI hook

`.codex/config.toml`:
```toml
[[hooks.PreToolUse]]
[[hooks.PreToolUse.hooks]]
type = "command"
command = "node scripts/red-green-gate.ts"
```

Same script, different config syntax. Codex requires the project to be marked trusted in `~/.codex/config.toml`:
```toml
[projects."/abs/path/to/repo"]
trust_level = "trusted"
```

### Surface 3: lefthook pre-commit

`lefthook.yml`:
```yaml
pre-commit:
  commands:
    red-green-gate:
      run: node scripts/red-green-gate.ts --staged
    type-escape-check:
      run: pnpm run type-escape:check
    barrel-files-check:
      run: pnpm run barrel-files:check

commit-msg:
  commands:
    conventional-commit:
      run: pnpm run commit-message:check -- --file {1}
```

Same script with `--staged` flag, scanning what's about to commit. If the agent skips the in-session hook (or edits via a different tool), the commit gate catches it.

### The escape hatch

When a change is genuinely test-irrelevant (tooling config, docs, etc), the agent adds a marker comment:

```ts
// red-green:exempt — commit hook configuration is tooling-only
```

The gate respects the marker, but the marker is greppable, reviewable, and explicit. No silent bypasses.

### Why three surfaces

- **Hooks can be skipped** (`--no-verify`, etc) — they're convenience, not authority
- **CI is authoritative** — branch protection requires green CI before merge
- **Local hooks fail fast** — catch the mistake in seconds, not on a CI round-trip

The overlap is the point. Every agent + every commit path hits the same gate.

---

## 5. Issue + PR Templates That Mirror Each Other

### `.github/ISSUE_TEMPLATE/development-slice.md`

```markdown
---
name: Development Slice
about: Plan a coherent implementation slice
title: ""
labels: ""
assignees: ""
---

## Problem
What problem are we solving?

## Intended Outcome
What should be true when this is complete?

## Non-Goals
What should this issue not include?

## Acceptance Criteria
- [ ]

## Test Expectations
What tests should be added or updated?

## Architecture Impact
Does this affect domain boundaries, adapters, schemas, CI, or docs?

## Blockers Or Dependencies
What must be decided or completed first?
```

### `.github/pull_request_template.md`

```markdown
## Linked Issue
Closes #<issue-number>

## Before Any Code Checklist
- [ ] Linked issue (above)
- [ ] Branch (not `main`)
- [ ] Preflight output
- [ ] Failing test (RED) observed before any non-test edit
- [ ] Architecture/plan check against `docs/architecture.md`

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
- [ ] Foundational E2E, if relevant

Commands run:
```text
```

## Architecture And Docs
- [ ] This follows `docs/architecture.md`
- [ ] Architecture docs were updated, if architecture changed
- [ ] No domain/application/infrastructure boundary violations were introduced

## Type Safety
- [ ] Runtime data is parsed or narrowed instead of cast
- [ ] No `as any`, `: any`, `<any>`, or `as unknown as` was introduced
- [ ] Any unavoidable type escape uses a documented `type-escape:` marker

## Risk And Rollback
What can go wrong, and how can this be rolled back?

## Dependencies
List new dependencies and explain why each is needed.
```

The mirror matters: an issue describes *what we're going to do*, the PR confirms *we did exactly that*. Same fields, same shape, same review effort on both sides.

---

## 6. CI as the Merge Gate

A single workflow that enforces everything locally + branch protection that requires it to be green.

`.github/workflows/ci.yml`:
```yaml
name: CI
on:
  pull_request:
    branches: [main]
  push:
    branches: [main]

permissions:
  contents: read

jobs:
  quality-gate:
    runs-on: ubuntu-latest
    timeout-minutes: 20
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v4
        with: { version: 10.29.3 }
      - uses: actions/setup-node@v4
        with:
          node-version-file: .node-version
          cache: pnpm

      - name: Check PR title  # validates Conventional Commit format on the squash-merge title
        if: github.event_name == 'pull_request'
        env:
          PR_TITLE: ${{ github.event.pull_request.title }}
        run: node scripts/check-commit-message.ts --message "$PR_TITLE"

      - run: pnpm install --frozen-lockfile
      - run: pnpm exec playwright install --with-deps chromium
      - run: pnpm run ci  # = pnpm check && pnpm build && pnpm test:e2e
```

Where `pnpm run ci` boils down to:
```json
{
  "check": "pnpm lint && pnpm format:check && pnpm typecheck && pnpm test",
  "ci": "pnpm check && pnpm build && pnpm test:e2e"
}
```

### Branch protection settings

- Require pull requests before merging
- Require CI green checks before merging
- Require branches to be up to date when practical
- Disallow direct pushes to `main`
- Admin bypass disabled by default

**Hooks are convenience. CI is authority.** That separation is what makes the three-surface enforcement actually trustworthy.

---

## 7. Conventional Commits, Enforced Three Ways

The repo enforces `<type>(scope): <imperative summary>` commit subjects in three places:

1. **`commit-msg` lefthook** runs `pnpm run commit-message:check --file {1}` against every commit
2. **Local script** the developer can run: `pnpm run commit-message:check -- --message "feat(chat): add follow-up"`
3. **CI step** validates the PR title (which becomes the squash-merge subject)

Allowed types: `feat`, `fix`, `docs`, `test`, `refactor`, `perf`, `build`, `ci`, `chore`, `revert`.

Non-trivial commits include a body via second `-m`:
```bash
git commit -m "feat(chat): model targeted follow-up conversations" \
  -m "Add conversation targets for report dimensions, findings, caveats, and evidence items.

Issue: #32
PR: #58"
```

The `Issue: #N` and `PR: #N` trailers make `git blame` actually useful years later.

---

## 8. Type-Safety Discipline

The repo bans these patterns and enforces it with a script (`pnpm run type-escape:check`) wired into pre-commit + CI:

- `as any`
- `: any`
- `<any>`
- `as unknown as`

Unavoidable escapes require a marker comment near the assertion:
```ts
// type-escape: upstream library type is incorrect; value is validated by ReviewerAssessmentSchema before use
```

The escape-hatch checker requires the marker. Without it, the staged file fails the gate.

Companion rules baked into `tsconfig.json`:
- `strict: true`
- `noUncheckedIndexedAccess: true`
- `exactOptionalPropertyTypes: true`
- `allowJs: false`
- Avoid `skipLibCheck` unless a dep forces it

Preferred alternatives to assertions:
- Zod parsing for external data
- Type guards for local runtime checks
- Discriminated unions for domain state
- Generic constraints for reusable helpers
- Exhaustive switches for variants

---

## 9. The Slice Workflow, End to End

Here's how a feature gets built in this repo. This is the rhythm worth importing wholesale:

```
1. User describes a slice in chat
   ↓
2. Agent runs /distill-issue
   → produces issue body via renderer script
   → agent runs `gh issue create` with the body
   → returns issue URL → posted in chat as "Before Any Code" item 1
   ↓
3. Agent runs /start-slice --issue 143 --slug workflow-skills --dry-run
   → user sanity-checks plan
   → agent re-runs without --dry-run
   → fetches main, creates worktree, installs deps, runs preflight
   → preflight output → posted in chat as items 2 + 3
   ↓
4. Agent skims docs/architecture.md and docs/development-plan.md
   → posts one-line conflict check → item 5
   ↓
5. Agent writes a failing test FIRST
   → runs `pnpm vitest run <file>` → captures FAIL output → item 4
   → only THEN can the PreToolUse hook be satisfied for non-test edits
   ↓
6. Agent implements the slice
   → red-green-gate.ts allows non-test edits because colocated test was just edited
   → type-escape, barrel-files, lint all run on commit
   ↓
7. Agent commits with Conventional Commit format
   → commit-msg hook validates subject
   ↓
8. Agent pushes, opens PR with template
   → CI runs: lint + format + typecheck + tests + build + E2E
   → PR title checked against Conventional Commits
   → branch protection requires green
   ↓
9. Reviewer checks the five "Before Any Code" boxes against the chat transcript
   → if any are missing, PR is rejected even if code is correct
```

Every step has a mechanical guard. The agent literally cannot skip a step without leaving a trace.

---

## 10. The Cross-Agent Layout

If you want this to work across Claude Code, Codex CLI, and pi:

| Concern | Pattern |
|---|---|
| Skills | One copy at `.agents/skills/<name>/`, symlinks at `.claude/skills`, `.codex/skills`, `.pi/skills` |
| Agent rules | Single `AGENTS.md`; `CLAUDE.md` is `@AGENTS.md`; all three agents read AGENTS.md natively |
| Hooks | Same script (`scripts/red-green-gate.ts`); different config files per agent (`.claude/settings.json`, `.codex/config.toml`, `.pi/extensions/...`) |
| Trust gating | Codex requires explicit `trust_level = "trusted"` in `~/.codex/config.toml` per project |

The script reads `$CLAUDE_PROJECT_DIR` if set, otherwise falls back to `process.cwd()` — works without explicit env wiring across agents.

---

## What to Steal First

If you're starting a new repo and want the highest-leverage subset:

1. **`AGENTS.md` with the "Before Any Code" five-artifact gate** — this alone catches 80% of the pathological agent failure modes
2. **`docs/architecture.md` as required reading** — kills the "let me just cast this real quick" instinct
3. **Issue + PR templates that mirror each other** — a 30-minute setup that pays back forever
4. **Conventional Commits enforced by commit-msg hook + CI on PR titles** — your future self doing `git blame` will thank you
5. **Lefthook with red-green + type-escape + format checks** — fast feedback before commit

Skip-for-later (good ideas but bigger lift):

- The full three-surface red-green script (start with just the lefthook version, add the PreToolUse hook later)
- Cross-agent skill symlinks (only matters if you actually use multiple agents)
- The `start-slice` worktree dance (overkill for solo work)
- Foundational E2E + Playwright (only when the product workflow is stable enough to be worth protecting)

The genius of this setup isn't any single piece — it's that **every rule appears in the place where the rule can be enforced**, and the docs explain *why* so future agents (and humans) don't re-litigate the basics.

---

## References

- Source: [lightstrikelabs/repo-analyzer-green](https://github.com/lightstrikelabs/repo-analyzer-green)
- [AGENTS.md](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/AGENTS.md)
- [docs/architecture.md](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/docs/architecture.md)
- [docs/skills.md](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/docs/skills.md)
- [docs/agent-compat.md](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/docs/agent-compat.md)
- [docs/commit-messages.md](https://github.com/lightstrikelabs/repo-analyzer-green/blob/main/docs/commit-messages.md)
- [Agent Skills specification](https://agentskills.io/specification)
