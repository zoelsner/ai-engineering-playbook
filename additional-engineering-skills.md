# Additional Engineering Skills & Approaches

> **Source:** Every skill, pattern, and quote in this document comes from [**mattpocock/skills**](https://github.com/mattpocock/skills) ("Skills for Real Engineers") by [Matt Pocock](https://github.com/mattpocock). All credit for the skills, methodologies, and engineering insights goes to him. This document is my synthesis and commentary — the originals are the canonical reference.
>
> Direct sources used: [README.md](https://github.com/mattpocock/skills/blob/main/README.md) · [CONTEXT.md (example)](https://github.com/mattpocock/skills/blob/main/CONTEXT.md) · [grill-with-docs](https://github.com/mattpocock/skills/blob/main/skills/engineering/grill-with-docs/SKILL.md) · [diagnose](https://github.com/mattpocock/skills/blob/main/skills/engineering/diagnose/SKILL.md) · [zoom-out](https://github.com/mattpocock/skills/blob/main/skills/engineering/zoom-out/SKILL.md) · [improve-codebase-architecture](https://github.com/mattpocock/skills/blob/main/skills/engineering/improve-codebase-architecture/SKILL.md) · [prototype](https://github.com/mattpocock/skills/blob/main/skills/engineering/prototype/SKILL.md) · [triage](https://github.com/mattpocock/skills/blob/main/skills/engineering/triage/SKILL.md) · [to-prd](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-prd/SKILL.md)
>
> The lightstrikelabs comparisons in this document refer to [lightstrikelabs/repo-analyzer-green](https://github.com/lightstrikelabs/repo-analyzer-green) — see [repo-best-practices.md](repo-best-practices.md) for the full credits there.

Companion to [repo-best-practices.md](repo-best-practices.md). Distilled from [mattpocock/skills](https://github.com/mattpocock/skills) ("Skills for Real Engineers"). The lightstrikelabs setup is a tightly integrated process framework. Matt's setup is the opposite: **small, composable skills you can pull in à la carte without buying a workflow**. Read together, they cover different muscles.

---

## Where This Fits

The lightstrikelabs playbook gives you the **scaffolding**: AGENTS.md as constitution, hooks as enforcement, slice workflow as rhythm. It assumes you'll adopt the whole thing.

Matt's skills add the **engineering discipline that runs inside that scaffolding** — the things you do once you're at the keyboard with an issue open and a feature to build. Most are usable in any repo regardless of whether you've adopted the lightstrikelabs setup.

The philosophical contrast is worth naming up front. From Matt's README:

> Approaches like GSD, BMAD, and Spec-Kit try to help by owning the process. But while doing so, they take away your control and make bugs in the process hard to resolve. These skills are designed to be small, easy to adapt, and composable.

So while lightstrikelabs says "follow the slice workflow," Matt says "here are eleven sharp tools, use whichever fits." Both are valuable. The lightstrikelabs approach gives you guardrails when you're building in a team. Matt's gives you precision tools when you're solo or want to opt into one practice without buying the rest.

---

## The Eleven New Approaches Worth Adding

### 1. Grilling-Before-Building (`/grill-with-docs`)

The single biggest pattern. Before any non-trivial code, run a structured **interview session** where the agent challenges your plan against the existing domain model and asks one question at a time until every branch of the decision tree is resolved.

Lightstrikelabs has `/distill-issue` for *capturing* a slice. Matt's `/grill-with-docs` runs *before* that — it's the alignment step, the part where you discover what you actually want. The skill body distills to:

- **Walk down the decision tree, one branch at a time.** Don't batch questions. Wait for feedback on each.
- **Challenge against the existing glossary.** If the user uses "cancellation" and CONTEXT.md defines it differently, call it out immediately.
- **Sharpen fuzzy language.** "You're saying 'account' — do you mean Customer or User?"
- **Stress-test with concrete scenarios.** Invent edge cases that force the user to be precise about boundaries.
- **Cross-reference with code.** If the user states how something works and the code disagrees, surface the contradiction.
- **Update CONTEXT.md inline** as terms get resolved. Don't batch.
- **Offer ADRs sparingly** (criteria below).

Why this matters: **misalignment is the #1 failure mode.** You and the agent think you agree until you see the diff. The grilling session is the cheap upfront cost that prevents the expensive rebuild.

The user already has `/grill-me` installed at user level — that's the lighter version. The "with-docs" variant adds the CONTEXT.md/ADR side-effects, which is the real magic.

### 2. CONTEXT.md — The Shared-Language Doc

This is **the single most important artifact Matt introduces** and the one most likely to be missing from your repos. It's a domain glossary that lives alongside `architecture.md`.

The example from mattpocock/skills' own CONTEXT.md:

```markdown
## Language

**Issue tracker**: The tool that hosts a repo's issues — GitHub Issues, Linear,
a local `.scratch/` markdown convention. Skills like `to-issues`, `to-prd`, and
`triage` read from and write to it.
_Avoid_: backlog manager, backlog backend, issue host

**Issue**: A single tracked unit of work inside an **Issue tracker** — a bug,
task, PRD, or slice produced by `to-issues`.
_Avoid_: ticket (use only when quoting external systems that call them tickets)

**Triage role**: A canonical state-machine label applied to an **Issue** during
triage (e.g. `needs-triage`, `ready-for-afk`). Each role maps to a real label
string via `docs/agents/triage-labels.md`.

## Relationships

- An **Issue tracker** holds many **Issues**
- An **Issue** carries one **Triage role** at a time

## Flagged ambiguities

- "backlog" was previously used to mean both the *tool* and the *body of work*
  — resolved: the tool is the **Issue tracker**.
```

Three structural pieces:
1. **Definitions** with explicit `_Avoid_:` for words that mean almost-the-same-thing.
2. **Relationships** between the defined terms (one X holds many Y).
3. **Flagged ambiguities** — old terms that got resolved into new ones, captured so the team doesn't drift back.

Matt's claim, which I find compelling:

> "There's a problem when a lesson inside a section of a course is made 'real' (i.e. given a spot in the file system)" → "There's a problem with the materialization cascade"
>
> This concision pays off session after session. It might be the single coolest technique in this repo.

The downstream effects:
- Variables, functions, files all named consistently using the shared language
- The codebase becomes easier for the agent to navigate
- The agent spends fewer tokens thinking, because it has a more concise language

Where lightstrikelabs has `docs/architecture.md` (system shape and conventions), CONTEXT.md is **about the words you use to describe the domain itself**. Both belong in a serious repo.

### 3. ADRs With Explicit Triggers

Every team writes ADRs eventually. Matt's contribution: **specific triggers** that tell you when an ADR is warranted and when it's noise.

Offer to create an ADR only when **all three** are true:

1. **Hard to reverse** — cost of changing your mind later is meaningful
2. **Surprising without context** — a future reader will wonder "why did they do it this way?"
3. **Result of a real trade-off** — there were genuine alternatives and you picked one for specific reasons

Skip if any is missing. This filter prevents the "ADR for everything" failure mode where the docs/adr directory becomes write-only.

The grilling skills produce ADRs as inline side-effects when these criteria hit during a conversation, not retrofitted later.

### 4. Disciplined Diagnosis: `/diagnose`

The lightstrikelabs setup has TDD baked in but doesn't say much about debugging hard bugs. Matt's `/diagnose` skill is a six-phase methodology where **Phase 1 is the entire skill**:

> **Phase 1 — Build a feedback loop.** This is the skill. Everything else is mechanical. If you have a fast, deterministic, agent-runnable pass/fail signal for the bug, you will find the cause. If you don't have one, no amount of staring at code will save you.

The taxonomy of feedback loops, ranked by preference:

1. Failing test at whatever seam reaches the bug
2. Curl/HTTP script against a running dev server
3. CLI invocation with fixture input, diffed against known-good snapshot
4. Headless browser script (Playwright/Puppeteer)
5. Replay a captured trace (real network request → replay through code path)
6. Throwaway harness (minimal subset, mocked deps, single function call)
7. Property/fuzz loop (1000 random inputs)
8. Bisection harness (`git bisect run` against an automated check)
9. Differential loop (same input, two versions, diff outputs)
10. HITL bash script (last resort, structured human-in-the-loop)

Then **iterate on the loop itself**: faster, sharper, more deterministic. A 30-second flaky loop is barely better than no loop. A 2-second deterministic loop is a debugging superpower.

Then the rest of the phases:
- **Phase 2: Reproduce.** Confirm the loop produces the *user's* failure, not a nearby one.
- **Phase 3: Hypothesize.** Generate **3–5 ranked hypotheses before testing any**. Each must be falsifiable with a stated prediction. Show the ranked list to the user — they often re-rank instantly with domain knowledge.
- **Phase 4: Instrument.** One probe per hypothesis. Tag every debug log with a unique prefix (`[DEBUG-a4f2]`) so cleanup is one grep. For perf bugs, *measure first*, fix second.
- **Phase 5: Fix + regression test.** Write the regression test *before* the fix — but only if there's a *correct seam*. If the only available seam is too shallow to lock down the real bug pattern, that itself is the finding (architecture is preventing the bug from being captured).

Why this is better than "just debug it": **it forces the agent past hypothesis-anchoring**. Generating 3–5 ranked hypotheses before testing one prevents the "first plausible idea wins" failure mode that wastes hours.

### 5. `/zoom-out` — The One-Line Skill

The entire skill body:

> I don't know this area of code well. Go up a layer of abstraction. Give me a map of all the relevant modules and callers, using the project's domain glossary vocabulary.

That's it. One sentence. Marked `disable-model-invocation: true` so it only runs when explicitly invoked.

Why it matters: **agents tunnel.** They see a function and start changing it without understanding how it fits. `/zoom-out` is the cheap reset button when you sense the agent is editing without context. Forces the perspective shift.

The "using the project's domain glossary vocabulary" piece is what makes it sharp — the map is in CONTEXT.md terms, not implementation jargon.

### 6. `/improve-codebase-architecture` — Preventive Design

Matt frames this as a recurring practice: **run it on your codebase every few days**. The premise:

> Most apps built with agents are complex and hard to change. Because agents can radically speed up coding, they also accelerate software entropy. Codebases get more complex at an unprecedented rate.

The skill looks for **deepening opportunities** — refactors that turn shallow modules into deep ones (Ousterhout's "Philosophy of Software Design" framing). The vocabulary is precise:

- **Module** — anything with an interface and an implementation
- **Interface** — everything a caller must know (types, invariants, error modes, ordering)
- **Depth** — leverage at the interface; deep = high leverage; shallow = interface as complex as implementation
- **Seam** — where an interface lives (use this, not "boundary")
- **Adapter** — a concrete thing satisfying an interface at a seam
- **Locality** — what maintainers get from depth: change, bugs, knowledge concentrated in one place

Two heuristics:

- **Deletion test:** imagine deleting the module. If complexity vanishes, it was a pass-through. If complexity reappears across N callers, it was earning its keep.
- **One adapter = hypothetical seam. Two adapters = real seam.** Premature abstraction is creating seams with one user.

The skill proposes candidates with `(files, problem, solution, benefits)` framing, **using CONTEXT.md vocabulary for the domain and architecture vocabulary for the code shape** — never mixing the two. ADR conflicts are surfaced only when friction is real enough to warrant revisiting; otherwise skipped.

This is preventive maintenance the lightstrikelabs setup doesn't have a slot for. Worth scheduling weekly.

### 7. `/prototype` — Throwaway Code That Answers a Question

A prototype isn't "early implementation." It's **throwaway code that answers one question**, and the question decides the shape:

- **"Does this state model feel right?"** → runnable terminal app that pushes the state machine through hard-to-reason cases
- **"What should this look like?"** → several radically different UI variations on a single route, switchable via URL param

Six universal rules:

1. **Throwaway from day one, clearly marked as such.** Locate it next to the real module so context is obvious, but name it so a casual reader sees it's not production.
2. **One command to run.** Whatever the existing task runner uses.
3. **No persistence by default.** State is in memory. (Persistence is what the prototype is *checking*, not what it depends on.)
4. **Skip the polish.** No tests, no error handling beyond runnable, no abstractions.
5. **Surface the state.** After every action, print or render the full relevant state.
6. **Delete or absorb when done.** Don't leave it rotting.

**The answer is the only thing worth keeping.** Capture it durably (commit message, ADR, issue, NOTES.md) along with the question it was answering.

This is a category that's missing from most agent setups. Useful when you're not sure whether your data model is right, or what the UI should feel like, and you want to find out without committing.

### 8. `/triage` — Issue Triage as a State Machine

Different problem from `/distill-issue`. `/distill-issue` *files* a clean issue from a fresh description. `/triage` *processes* the chaos that already exists in your tracker.

Two category roles + five state roles, each issue carries exactly one of each:

| Category | State |
|---|---|
| `bug` | `needs-triage` (initial) |
| `enhancement` | `needs-info` (waiting on reporter) |
| | `ready-for-agent` (fully specified) |
| | `ready-for-human` (needs human implementation) |
| | `wontfix` (will not be actioned) |

The triage flow:
1. **Gather context** — read full issue, parse prior triage notes, explore code in CONTEXT.md vocabulary, check `.out-of-scope/` for prior similar rejections.
2. **Recommend** category + state with reasoning. Wait for direction.
3. **Reproduce (bugs only)** — *before any grilling*, attempt repro. A confirmed repro makes a much stronger agent brief. Failed repro = strong `needs-info` signal.
4. **Grill (if needed)** — drop into a `/grill-with-docs` session.
5. **Apply outcome** — agent brief, human brief, info request, or wontfix.

Two cultural details that are worth borrowing:

- **AI disclaimer requirement.** Every comment posted by AI during triage **must** start with `> *This was generated by AI during triage.*` Transparency about who did what.
- **Out-of-scope knowledge base.** When an enhancement is rejected as wontfix, write the *reason* to `.out-of-scope/<topic>.md`, link from the closing comment, then close. Future triage sessions surface prior rejections automatically. This is **long-term memory for "we already said no to that"** — invaluable as a project ages.

### 9. `/to-prd` — Synthesis Without Interview

Subtle but important difference from `/distill-issue`:

- **`/distill-issue`** *gathers* context to file an issue
- **`/to-prd`** *assumes* the context exists in the conversation and **does NOT interview** — it just synthesizes

This is the right tool when you've just spent an hour grilling, prototyping, or planning, and you need the conversation turned into a structured PRD without re-asking questions you've already answered.

It also actively looks for **deep modules to extract** during synthesis. Sketches the major modules, checks with the user, asks which need tests. Then writes the PRD with:

- Problem Statement (user perspective)
- Solution (user perspective)
- **Long, numbered list of user stories** (`As <actor>, I want <feature>, so that <benefit>`)
- Implementation Decisions (modules, interfaces, schemas, contracts — not file paths or code, those rot fast)
- Testing Decisions (what makes a good test, which modules, prior art)
- Out of Scope
- Further Notes

Auto-applies `ready-for-agent` triage label since it's coming straight from a designed conversation.

When to use which: `/distill-issue` for "I have an idea, capture it." `/to-prd` for "we've already designed this in chat, file it." Both are useful; they're not duplicates.

### 10. `/setup-*-skills` — Config Bootstrap Pattern

Not a skill itself but a *meta-pattern*. The first thing you run in a new repo is a setup skill that asks:

- Which issue tracker? (GitHub / Linear / local files)
- What labels do you apply during triage?
- Where do you save docs we create?

It writes the answers to a config file the other skills consume. That solves the "skill needs project-specific config" problem cleanly without hardcoding assumptions.

If you build skills that depend on each other, this is the pattern: one bootstrap skill that runs first and configures the rest. The lightstrikelabs `/preflight` is auditing-focused (does this *exist*?). A setup skill is bootstrap-focused (let me *create* what's missing). They're complementary.

### 11. `/caveman` — Token Compression Mode

> Ultra-compressed communication mode. Cuts token usage ~75% by dropping filler while keeping full technical accuracy.

Productivity hack. Not for everyone — some tasks benefit from prose explanations — but worth knowing about for long-running sessions where context is filling up. Activates an explicit mode rather than constantly fighting the model's verbosity.

---

## Cultural Details Worth Stealing

A few things that aren't skills but are worth importing:

### Name the entropy problem out loud

Matt's framing in the README:

> Most apps built with agents are complex and hard to change. Because agents can radically speed up coding, they also accelerate software entropy. Codebases get more complex at an unprecedented rate. The fix is a radical new approach: caring about the design of the code.

Putting this in your AGENTS.md (or wherever) anchors the team. The "AI lets us move faster" narrative is incomplete without "...and complexity accumulates faster too, so design discipline matters more, not less."

### AI-generated content disclaimer

The triage requirement that AI-posted comments must lead with `> *This was generated by AI during triage.*` is a healthy cultural choice. Worth applying anywhere AI is operating semi-autonomously on shared artifacts (issues, PRs, docs).

### Skill framing as engineering literature

Matt's README explicitly cites Pragmatic Programmer, DDD, XP, Philosophy of Software Design as the grounding for each skill. Each "Why these skills exist" section opens with a relevant quote. Not just for vibes — it signals that the skills are encoding decades of engineering practice, not improvising.

If you build internal skills, citing the source material ("this skill encodes the red-green-refactor loop from XP") makes them more legitimate and easier to adopt.

---

## What to Add to Your Setup

In rough priority order (highest leverage first):

| | Add | Why |
|---|---|---|
| 1 | **CONTEXT.md** in any repo with non-trivial domain language | Concision compounds session over session. The single coolest technique. |
| 2 | **`/grill-with-docs`** as the alignment step before any non-trivial work | Prevents the #1 failure mode (misalignment) cheaply |
| 3 | **`/diagnose`** with its Phase 1 = "build a feedback loop" discipline | Stops the "stare at code, guess, repeat" debugging anti-pattern |
| 4 | **`/zoom-out`** as a one-liner to reset agent context when it's tunneling | Free, takes 30 seconds to install, used constantly |
| 5 | **ADR triggers** (hard-to-reverse + surprising-without-context + real trade-off) | Stops the "ADR for everything" noise that kills the practice |
| 6 | **`/improve-codebase-architecture`** scheduled weekly | Counters AI-accelerated entropy |
| 7 | **`/prototype` rules** when exploring design questions | Prototype shape ≠ implementation shape; throwaway-from-day-one |
| 8 | **`.out-of-scope/` knowledge base** for triaged rejections | Long-term memory for "we already said no" |
| 9 | **AI disclaimer convention** for AI-generated PR/issue comments | Transparency about agent participation |
| 10 | **`/triage` state machine** if your tracker is chaotic | Most useful in mature/active repos |
| 11 | **`/caveman` mode** for context-pressure long sessions | Niche but real |

The first four (CONTEXT.md, grill-with-docs, diagnose, zoom-out) are the highest leverage and lowest effort. Start there.

---

## Installation

Matt provides a one-liner installer:

```bash
npx skills@latest add mattpocock/skills
```

Then `/setup-matt-pocock-skills` to configure issue tracker + labels + doc location. You can pick a subset rather than all of them — they're designed to be independently usable.

If you want to mix-and-match with the lightstrikelabs setup: keep AGENTS.md as your constitution and the slice workflow as your rhythm, but add `/grill-with-docs`, `/diagnose`, `/zoom-out`, and a CONTEXT.md to any repo where domain language is starting to drift. They compose cleanly.

---

## References

- Source: [mattpocock/skills](https://github.com/mattpocock/skills)
- [grill-with-docs SKILL](https://github.com/mattpocock/skills/blob/main/skills/engineering/grill-with-docs/SKILL.md)
- [diagnose SKILL](https://github.com/mattpocock/skills/blob/main/skills/engineering/diagnose/SKILL.md)
- [improve-codebase-architecture SKILL](https://github.com/mattpocock/skills/blob/main/skills/engineering/improve-codebase-architecture/SKILL.md)
- [triage SKILL](https://github.com/mattpocock/skills/blob/main/skills/engineering/triage/SKILL.md)
- [Example CONTEXT.md](https://github.com/mattpocock/skills/blob/main/CONTEXT.md)
- [AI Hero Skills newsletter](https://www.aihero.dev/s/skills-newsletter)
- John Ousterhout, [A Philosophy of Software Design](https://www.amazon.com/Philosophy-Software-Design-2nd/dp/173210221X) — the grounding for `/improve-codebase-architecture`
