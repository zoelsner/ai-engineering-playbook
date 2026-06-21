# AI Engineering Playbook

A portable playbook and starter kit for building software with coding agents (Claude Code, Codex CLI, pi). Distilled from sharp open setups across process, skills, frontend polish, accessibility, and output format.

## Start Here

If you want to apply the playbook to a repo, copy [`starter-kit/`](./starter-kit/) first. It contains working templates for `AGENTS.md`, `CONTEXT.md`, architecture docs, issue/PR templates, docs-quality CI, and a project-level `senior-engineering-loop` skill.

Then run the docs-quality checks, fill in the placeholders, and read the reference docs below when you want to understand why each piece exists.

## The Docs

### [📋 repo-best-practices.md](./repo-best-practices.md)

A portable playbook for setting up a repo with **layered enforcement** — agent rules, hooks, CI, and branch protection that all reinforce the same constraints. Distilled from [lightstrikelabs/repo-analyzer-green](https://github.com/lightstrikelabs/repo-analyzer-green).

Covers:
- The `AGENTS.md` "Before Any Code" five-artifact gate
- `docs/architecture.md` as required reading
- Cross-agent skill layout (`.agents/skills/` with symlinks for Claude/Codex/pi)
- A senior engineering loop for goal-driven execution, Codex-as-driver implementation, real validation after each meaningful step, and `claude -p` second-opinion gates before high-cost designs
- Accessibility acceptance criteria and UI state matrices in issue/PR templates
- Three-surface red-green enforcement (PreToolUse hook + lefthook + CI)
- Issue + PR templates that mirror each other
- Conventional Commits enforcement at three layers
- The full slice workflow: `/distill-issue` → `/start-slice` → `/preflight` → red test → implementation
- TypeScript-specific bans (`as any`, `as unknown as`) with marker-based escape hatches

This is the **integrated process framework** — adopt the whole thing, get a system where the agent can't skip steps without leaving a trace.

### [🛠️ additional-engineering-skills.md](./additional-engineering-skills.md)

A complement focused on **small composable skills** that work in any repo regardless of whether you've adopted the integrated framework above. Mostly distilled from [mattpocock/skills](https://github.com/mattpocock/skills) ("Skills for Real Engineers"), plus Jakub Krehel's `make-interfaces-feel-better` frontend polish skill.

Covers twelve approaches the integrated framework doesn't have:
- `CONTEXT.md` as a shared-language doc (domain glossary as a first-class artifact)
- `/grill-with-docs` — interview-before-build that updates docs/ADRs inline
- ADR triggers (hard-to-reverse + surprising-without-context + real trade-off)
- `/diagnose` — disciplined six-phase debugging where Phase 1 is *always* "build a feedback loop"
- `/zoom-out` — one-line skill that forces system-map perspective
- `/improve-codebase-architecture` — periodic preventive design pass against AI-accelerated entropy
- `/prototype` — throwaway code that answers ONE question, with six rules to keep it from becoming production debt
- `make-interfaces-feel-better` — frontend polish checks for typography, surfaces, motion, hit areas, and transition performance
- `/triage` as a state machine + `.out-of-scope/` knowledge base for past rejections
- `/to-prd` vs `/distill-issue` — synthesis vs gathering, both useful
- The setup-skill bootstrap pattern for skills that share config
- AI-disclaimer conventions for AI-generated PR/issue comments

### [🎛️ design-quality-stack.md](./design-quality-stack.md)

A routing layer for frontend/design work with agents. It explains how accessibility acceptance criteria, state matrices, `make-interfaces-feel-better`, Impeccable, and HTML artifacts fit together.

Covers:
- WCAG 2.2 AA as the default web UI bar unless a project chooses otherwise
- State matrices as required UI requirements, not polish
- What evidence to ask agents for: screenshots, keyboard walkthroughs, accessibility scans, state stories, and polish tables
- When to use lightweight UI polish versus a fuller Impeccable-style design operating system
- How HTML artifacts make visual decisions easier to review

### [🖼️ html-as-medium.md](./html-as-medium.md)

A render upgrade for the workflow you already have. Distilled from [Thariq Shihipar's HTML Effectiveness guide](https://thariqs.github.io/html-effectiveness/) — 20 worked demos that show when HTML beats markdown as the agent's output format.

Covers nine situations where prose flattens information that's actually spatial, motion-based, or interactive:
- **Exploration** — three code approaches or design directions side by side, then an implementation plan with timeline + risk table
- **Code review** — annotated diffs, PR writeups, module maps as boxes-and-arrows
- **Design** — design system tokens as swatches, component variants as contact sheets
- **Prototyping** — animation sandboxes (motion has to be felt) and clickable flows
- **Illustrations** — inline SVG figures and annotated flowcharts (the agent can draw)
- **Decks** — arrow-key HTML slides, no Keynote, no build step
- **Research** — feature/concept explainers with collapsibles, tabs, glossary
- **Reports** — weekly status with charts, incident timelines
- **Custom editing interfaces** — throwaway editors (triage board, flag editor, prompt tuner) that export decisions back as text the agent can consume

The bridge to the other two docs: **most workflow steps already produce text artifacts that would be sharper as HTML.** This isn't a new workflow — it's a better artifact for `/zoom-out`, `/to-prd`, `/triage`, PR descriptions, and incident reports.

## Repo Structure

- [`starter-kit/`](./starter-kit/) — copyable project structure and templates
- [`.github/workflows/docs-quality.yml`](./.github/workflows/docs-quality.yml) — markdown and link checks for this repo
- [`.markdownlint-cli2.jsonc`](./.markdownlint-cli2.jsonc) — local/CI markdown lint config
- [`.lychee.toml`](./.lychee.toml) — link checker config with exclusions for volatile sources
- [`ATTRIBUTION.md`](./ATTRIBUTION.md) — source and reuse notes

## How to Use These

If you're starting fresh: copy **starter-kit/** into the target repo, fill in the placeholders, then read **repo-best-practices.md** to understand the AGENTS.md + slice workflow + hooks. Add the senior engineering loop, use Codex as the driver for context, implementation, validation, and final judgment, and use `claude -p` as a second-opinion reviewer when pressure-testing plans, features, code, or loops. Make accessibility/state matrices part of every UI issue and PR. Then layer on the skills from **additional-engineering-skills.md** as you hit the situations they solve (alignment, debugging, design exploration). For UI work, use **design-quality-stack.md** as the routing guide. Once the workflow is humming, read **html-as-medium.md** to upgrade the artifacts each step produces.

If you already have a workflow: skip to **additional-engineering-skills.md** and pick the highest-leverage additions:

1. **CONTEXT.md** in any repo with non-trivial domain language
2. `/grill-with-docs` for alignment before non-trivial work
3. `/diagnose` for disciplined debugging
4. `/zoom-out` to reset agent context when it tunnels
5. `make-interfaces-feel-better` for frontend-heavy repos where "feels off" needs a concrete polish checklist

Then revisit **design-quality-stack.md** and **html-as-medium.md** for the highest-leverage UI/render upgrades: state matrices, accessibility evidence, PR writeups as HTML, module maps as boxes-and-arrows, and visual design directions rendered side-by-side instead of described.

## Sources

- [lightstrikelabs/repo-analyzer-green](https://github.com/lightstrikelabs/repo-analyzer-green) — the integrated framework
- [mattpocock/skills](https://github.com/mattpocock/skills) — the composable skills
- [jakubkrehel/make-interfaces-feel-better](https://github.com/jakubkrehel/make-interfaces-feel-better) — frontend UI polish skill
- [Thariq Shihipar — HTML Effectiveness](https://thariqs.github.io/html-effectiveness/) — HTML as the agent's output medium
- [Theo on a Codex first loop](https://x.com/theo/status/2068595585121484866) and [Vox on senior-agent prompts](https://x.com/Voxyz_ai/status/2067237707483337118) — goal loops and second-opinion gates
- [W3C WCAG overview](https://www.w3.org/WAI/standards-guidelines/wcag/) — accessibility reference

## License

These docs synthesize public material from the source repos linked above. Treat them as personal notes and starter-kit scaffolding; consult the originals for canonical guidance. See [ATTRIBUTION.md](./ATTRIBUTION.md) before reusing this as a public package.
