# HTML as the Output Medium for Coding Agents

> **Source:** Every category, demo, and quote in this document comes from [**Thariq Shihipar's HTML Effectiveness guide**](https://thariqs.github.io/html-effectiveness/). All credit for the framing, the demos, and the engineering insight goes to him. This document is my synthesis of how those patterns slot into the workflow described in [repo-best-practices.md](repo-best-practices.md) and [additional-engineering-skills.md](additional-engineering-skills.md).
>
> Direct source: [thariqs.github.io/html-effectiveness](https://thariqs.github.io/html-effectiveness/) — 20 worked demos across 9 categories.

Companion to [repo-best-practices.md](repo-best-practices.md) (the integrated process framework) and [additional-engineering-skills.md](additional-engineering-skills.md) (the composable skills). Those docs cover *how* to work with an agent. This doc covers *what the agent's output should look like* — and specifically, when HTML beats markdown.

---

## Where This Fits

Lightstrikelabs gives you the scaffolding. Matt Pocock gives you the engineering discipline. Thariq's contribution is one level up from both: **the artifact the agent produces in the first place**.

Most agent output today is markdown dumped into a chat window. That works for prose. It fails the moment information is **spatial, motion-based, or interactive** — a diff is spatial, an easing curve is motion, a triage board is interactive. Markdown flattens all three.

The reframing in one sentence:

> HTML *is* the medium your design system ships in, so it's the natural format for talking about it. Tokens become swatches, components become contact sheets, and the artifact can be fed straight back into the next prompt.

The same logic generalizes — diffs become annotated diffs, modules become boxes-and-arrows, status reports become charts. The agent already knows HTML. The unlock is *asking it to use HTML when the information is wrong-shaped for prose*.

---

## The Nine Categories

Each category answers one question: **"when prose fails, what shape should the answer take instead?"**

### 1. Exploration & Planning

When you don't know what you want yet, ask the agent to fan out across several directions and lay them next to each other so you can point at one — instead of reading three sequential walls of text and trying to hold them all in your head.

- **Three code approaches** — side-by-side comparison of three ways to solve the same problem, trade-offs called out inline
- **Visual design directions** — a handful of layout/palette options rendered live so you can react to them, not imagine them
- **Implementation plan** — milestones on a timeline, a data-flow diagram, inline mockups, the risky code, and a risk table

The pattern: **comparison + commitment**. Fan out, pick, then turn the pick into a plan the implementer can read.

This is the natural output of `/grill-with-docs` and `/to-prd` once the conversation is settled. The PRD is an artifact; an HTML PRD with a timeline and inline mockups is a *better* artifact than the markdown version.

### 2. Code Review & Understanding

> Diffs and call-graphs are spatial information; markdown flattens them.

- **Annotated pull request** — diff rendered with margin notes, severity tags, and jump links — easier to scan than scrolling a terminal
- **PR writeup for reviewers** — the author's side: motivation, before/after, file-by-file tour with the *why*, where to focus the review
- **Module map** — an unfamiliar package drawn as boxes and arrows, with the hot path highlighted and entry points listed

This is what `/zoom-out` should produce. Matt's one-line skill says "give me a map of all the relevant modules and callers, using the project's domain glossary vocabulary." That map is **not prose**. It's nodes and edges. Render it.

The PR writeup pattern is also the natural pair with the lightstrikelabs PR template — the markdown checklist proves the gates were hit; the HTML writeup gives the reviewer a *tour* of the change.

### 3. Design

> HTML *is* the medium your design system ships in.

- **Living design system** — colors, type scale, spacing tokens pulled from a repo and rendered as swatches you can copy from
- **Component variants** — every size, state, and intent of one component on a single sheet for review

If you have a CONTEXT.md describing your domain, you should also have a `design-system.html` (or similar) describing your tokens and components. Both serve the same purpose: **make the language concrete so the agent uses it consistently**.

### 4. Prototyping

> Motion and interaction can't be described, only felt.

- **Animation sandbox** — the transition in isolation, with sliders for duration and easing, so you can tune it before wiring it in
- **Clickable flow** — four screens linked together, enough fidelity to feel whether the interaction is right

This is exactly Matt's `/prototype` skill, but rendered. His six rules still apply (throwaway from day one, one command to run, no persistence, skip the polish, surface the state, delete or absorb when done). The HTML demo *is* the throwaway code. The "answer" you keep is the captured decision in your ADR or NOTES.md, not the prototype itself.

### 5. Illustrations & Diagrams

> Inline SVG gives the agent a real pen.

- **SVG figure sheet** — diagrams for a blog post, drawn inline so they can be tweaked and copied out one by one
- **Annotated flowchart** — a deploy pipeline drawn as a real flowchart — click any step to see what runs, timings, and failure paths

The unlock is realizing the agent can *draw*, not just describe. For incident reports, architecture diagrams, request-path explainers — let the agent emit SVG instead of asking you to imagine the picture from prose.

### 6. Decks

> A handful of `<section>` tags and twenty lines of JS is a slide deck.

- **Arrow-key slide deck** — a short presentation as one HTML file, left/right to navigate, no build step

Point the agent at a Slack thread or a design doc and get something you can arrow-key through in a meeting — no Keynote, no export step.

### 7. Research & Learning

> An explainer with collapsible sections, tabbed code samples and a glossary in the margin reads very differently from the same words dumped linearly.

- **How a feature works** — TL;DR box, collapsible request-path steps, tabbed config snippets, an FAQ
- **Concept explainer** — a domain concept (e.g. consistent hashing) taught with a live interactive demo, a comparison table, and a hover-linked glossary

This is the natural output of any deep research session. If your `/grill-with-docs` produces 4,000 words of prose, ship it as an HTML explainer instead. Collapsibles, tabs, and inline glossary lookups change reading from a chore into a tool you actually navigate.

### 8. Reports

> Recurring documents — status updates, post-mortems — benefit most from a bit of structure and color.

- **Weekly status** — what shipped, what slipped, a small chart, formatted for Monday-morning skim
- **Incident timeline** — post-mortem with minute-by-minute timeline, log excerpts, follow-up checklist

Recurring artifacts (status, post-mortems, weeklies) get skimmed harder than one-offs. A small chart and a colored timeline turn something people skim into something they actually read.

### 9. Custom Editing Interfaces

> Sometimes it's hard to describe what you want in a text box. Ask for a throwaway editor for the exact thing you're working on — and always end with an export button that turns whatever you did in the UI back into something you can paste into the agent or commit.

- **Ticket triage board** — drag thirty tickets across Now / Next / Later / Cut, then copy the final ordering out as markdown
- **Feature flag editor** — toggles grouped by area, dependency warnings when a prerequisite is off, a "copy diff" button for just the changed keys
- **Prompt tuner** — editable template with variable slots highlighted; sample inputs on the right re-render live as you type

The pattern is the most novel one in the guide: **the agent builds you a UI for the specific decision you're making, then exports your decisions back as text the agent can consume next turn.** You stay in the loop; the loop gets tighter.

The triage board is a direct upgrade to Matt's `/triage` skill. Today, `/triage` walks you through issues one at a time in chat. With this pattern, the agent emits an HTML board, you drag, you click "export," and paste the JSON back. The state machine still runs — the human-in-the-loop step is just dramatically faster.

---

## Where Each Pattern Fits in the Existing Workflow

| Lightstrikelabs / Matt step | HTML pattern |
|---|---|
| `/distill-issue` output | Status report card + risk table |
| `/grill-with-docs` outcome | Concept explainer or feature explainer |
| `/to-prd` artifact | Implementation plan (timeline + mockups + risk table) |
| PR description | Annotated diff + PR writeup |
| `/zoom-out` map | Module map (boxes and arrows) |
| `/diagnose` Phase 4 instrumentation results | Annotated flowchart of the failing path |
| `/prototype` (UI question) | Visual design directions; clickable flow |
| `/prototype` (state question) | Custom editing interface that surfaces state |
| UI state matrix | Component state contact sheet |
| Accessibility acceptance | Keyboard/focus/contrast evidence report |
| `/triage` board | Ticket triage editor with export |
| `/improve-codebase-architecture` candidates | Module map showing shallow vs deep modules |
| Weekly recap, post-mortem | Status report, incident timeline |

The point of this table: **most of the steps in the existing playbook produce text artifacts that would be sharper as HTML artifacts.** This isn't a new workflow — it's a render upgrade for the workflow you already have.

---

## What to Steal First

In rough priority order:

| | Add | Why |
|---|---|---|
| 1 | **PR writeup HTML** for non-trivial PRs | Reviewers actually read it; markdown PR descriptions get skimmed |
| 2 | **Module map HTML** as the output of `/zoom-out` | Boxes and arrows beat prose for understanding callers and dependencies |
| 3 | **Visual design directions** when there's any UI question | Three rendered options beat three paragraphs describing options |
| 4 | **Annotated flowchart** for incident reports and diagnose findings | The shape of the failure is spatial — show it |
| 5 | **Custom editing interface** for any task that's "go through 30 of these and pick" | Triage, flag editing, prompt tuning — drag-drop + export beats one-at-a-time chat |
| 6 | **Implementation plan** as HTML (timeline + risk table + mockups) | Replaces the markdown PRD when the slice is non-trivial |
| 7 | **Concept explainer** for new domain terms added to CONTEXT.md | Collapsibles + glossary make onboarding doc much more usable |
| 8 | **SVG figure sheet** for architecture and request-path docs | Inline SVG > "imagine a diagram here" |
| 9 | **Status report HTML** if you write a weekly | Small charts and color make it actually get read |
| 10 | **Slide deck** when going from chat to a meeting | Skip Keynote entirely |

The first three are the biggest leverage. They cost the agent one extra turn and they change the artifact from "wall of text" to "thing you can scan in 15 seconds."

---

## Operational Notes

A few things to standardize if you adopt this seriously:

- **Single-file HTML, no build step.** Self-contained, openable from disk, pasteable into a chat as an attachment. Same constraint as Thariq's demos.
- **Always add an export button** for any HTML that captures user decisions. The output of the editor needs to flow back into the agent — JSON, markdown, or a diff.
- **Render in `docs/artifacts/` (or similar) and `.gitignore` it by default.** These are conversational artifacts, not source of truth. Promote individual ones to `docs/` if they earn it (a real architecture diagram, the actual design system).
- **Avoid secrets and unnecessary external scripts.** HTML artifacts are easy to share, so keep credentials, private data, and surprise network calls out by default.
- **Cite the agent inline.** Matt's AI-disclaimer convention applies — if the artifact will be shared, say what produced it.

---

## References

- Source: [Thariq Shihipar — HTML Effectiveness](https://thariqs.github.io/html-effectiveness/) — the worked demos, with full HTML for each
- Companion docs in this repo: [repo-best-practices.md](repo-best-practices.md) · [additional-engineering-skills.md](additional-engineering-skills.md)
