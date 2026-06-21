# AI Engineering Playbook

Three complementary playbooks for building software with coding agents (Claude Code, Codex CLI, pi). Distilled from three of the sharpest open setups I've seen — process, skills, and output format.

## The Docs

### [📋 repo-best-practices.md](./repo-best-practices.md)

A portable playbook for setting up a repo with **layered enforcement** — agent rules, hooks, CI, and branch protection that all reinforce the same constraints. Distilled from [lightstrikelabs/repo-analyzer-green](https://github.com/lightstrikelabs/repo-analyzer-green).

Covers:
- The `AGENTS.md` "Before Any Code" five-artifact gate
- `docs/architecture.md` as required reading
- Cross-agent skill layout (`.agents/skills/` with symlinks for Claude/Codex/pi)
- A senior engineering loop for goal-driven execution, real validation after each meaningful step, and second-opinion gates before high-cost designs
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

## How to Use These

If you're starting fresh: read **repo-best-practices.md** first, adopt the AGENTS.md + slice workflow + hooks, and add the senior engineering loop to keep agents working past "it runs" toward architecture, implementation, validation, and review. Then layer on the skills from **additional-engineering-skills.md** as you hit the situations they solve (alignment, debugging, design exploration). Once the workflow is humming, read **html-as-medium.md** to upgrade the artifacts each step produces.

If you already have a workflow: skip to **additional-engineering-skills.md** and pick the highest-leverage additions:

1. **CONTEXT.md** in any repo with non-trivial domain language
2. `/grill-with-docs` for alignment before non-trivial work
3. `/diagnose` for disciplined debugging
4. `/zoom-out` to reset agent context when it tunnels
5. `make-interfaces-feel-better` for frontend-heavy repos where "feels off" needs a concrete polish checklist

Then revisit **html-as-medium.md** for the three highest-leverage render upgrades: PR writeups as HTML, module maps as boxes-and-arrows, and visual design directions rendered side-by-side instead of described.

## Sources

- [lightstrikelabs/repo-analyzer-green](https://github.com/lightstrikelabs/repo-analyzer-green) — the integrated framework
- [mattpocock/skills](https://github.com/mattpocock/skills) — the composable skills
- [jakubkrehel/make-interfaces-feel-better](https://github.com/jakubkrehel/make-interfaces-feel-better) — frontend UI polish skill
- [Thariq Shihipar — HTML Effectiveness](https://thariqs.github.io/html-effectiveness/) — HTML as the agent's output medium
- [Theo on a Codex first loop](https://x.com/theo/status/2068595585121484866) and [Vox on senior-agent prompts](https://x.com/Voxyz_ai/status/2067237707483337118) — goal loops and second-opinion gates

## License

These docs synthesize public material from the source repos linked above. Treat them as personal notes; consult the originals for canonical guidance.
