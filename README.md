# AI Engineering Playbook

Two complementary playbooks for building software with coding agents (Claude Code, Codex CLI, pi). Distilled from two of the sharpest open setups I've seen.

## The Docs

### [📋 repo-best-practices.md](./repo-best-practices.md)

A portable playbook for setting up a repo with **layered enforcement** — agent rules, hooks, CI, and branch protection that all reinforce the same constraints. Distilled from [lightstrikelabs/repo-analyzer-green](https://github.com/lightstrikelabs/repo-analyzer-green).

Covers:
- The `AGENTS.md` "Before Any Code" five-artifact gate
- `docs/architecture.md` as required reading
- Cross-agent skill layout (`.agents/skills/` with symlinks for Claude/Codex/pi)
- Three-surface red-green enforcement (PreToolUse hook + lefthook + CI)
- Issue + PR templates that mirror each other
- Conventional Commits enforcement at three layers
- The full slice workflow: `/distill-issue` → `/start-slice` → `/preflight` → red test → implementation
- TypeScript-specific bans (`as any`, `as unknown as`) with marker-based escape hatches

This is the **integrated process framework** — adopt the whole thing, get a system where the agent can't skip steps without leaving a trace.

### [🛠️ additional-engineering-skills.md](./additional-engineering-skills.md)

A complement focused on **small composable skills** that work in any repo regardless of whether you've adopted the integrated framework above. Distilled from [mattpocock/skills](https://github.com/mattpocock/skills) ("Skills for Real Engineers").

Covers eleven approaches the integrated framework doesn't have:
- `CONTEXT.md` as a shared-language doc (domain glossary as a first-class artifact)
- `/grill-with-docs` — interview-before-build that updates docs/ADRs inline
- ADR triggers (hard-to-reverse + surprising-without-context + real trade-off)
- `/diagnose` — disciplined six-phase debugging where Phase 1 is *always* "build a feedback loop"
- `/zoom-out` — one-line skill that forces system-map perspective
- `/improve-codebase-architecture` — periodic preventive design pass against AI-accelerated entropy
- `/prototype` — throwaway code that answers ONE question, with six rules to keep it from becoming production debt
- `/triage` as a state machine + `.out-of-scope/` knowledge base for past rejections
- `/to-prd` vs `/distill-issue` — synthesis vs gathering, both useful
- The setup-skill bootstrap pattern for skills that share config
- AI-disclaimer conventions for AI-generated PR/issue comments

## How to Use These

If you're starting fresh: read **repo-best-practices.md** first, adopt the AGENTS.md + slice workflow + hooks, then layer on the skills from **additional-engineering-skills.md** as you hit the situations they solve (alignment, debugging, design exploration).

If you already have a workflow: skip to **additional-engineering-skills.md** and pick the highest-leverage additions:

1. **CONTEXT.md** in any repo with non-trivial domain language
2. `/grill-with-docs` for alignment before non-trivial work
3. `/diagnose` for disciplined debugging
4. `/zoom-out` to reset agent context when it tunnels

## Sources

- [lightstrikelabs/repo-analyzer-green](https://github.com/lightstrikelabs/repo-analyzer-green) — the integrated framework
- [mattpocock/skills](https://github.com/mattpocock/skills) — the composable skills

## License

These docs synthesize public material from the source repos linked above. Treat them as personal notes; consult the originals for canonical guidance.
