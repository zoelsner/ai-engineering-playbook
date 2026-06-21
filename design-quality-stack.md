# Design Quality Stack for Agent-Built UI

This doc routes the design practices that are split across the rest of the playbook. Use it when a repo has UI work and the agent needs a concrete definition of "good enough."

The order matters: accessibility and states are acceptance criteria, polish is a review pass, and richer design tools are for cases where static prose is too weak.

## The Stack

| Layer | Use When | Output |
|---|---|---|
| Product context | The UI depends on domain language, user intent, or policy | `CONTEXT.md` terms and a short task summary |
| State matrix | Any page or component has data, permissions, async work, or responsive behavior | Loading, empty, partial, error, offline, disabled, long-content, mobile, reduced-motion, and high-contrast decisions |
| Accessibility bar | Any user-facing UI changes | Keyboard path, visible focus, accessible names, error descriptions, contrast, target size, reduced motion, no color-only meaning |
| Component evidence | Reusable UI, forms, dashboards, or complex states | Screenshots, Storybook stories, Playwright flows, or HTML contact sheets for every meaningful state |
| `make-interfaces-feel-better` | The UI works but feels rough | Typography, surfaces, motion, hit areas, optical alignment, and transition-performance polish table |
| Impeccable | You need a fuller design operating system, not just polish | Product/design docs, detector rules, live browser iteration, and broader anti-slop review loop |
| HTML artifacts | A visual decision is hard to judge in prose | Side-by-side design directions, annotated diffs, module maps, animation sandboxes, or clickable flows |

## Accessibility Acceptance Criteria

Default to WCAG 2.2 AA for web UI unless the product has a stricter or explicitly different standard.

- Keyboard navigation reaches every interactive control in a sensible order.
- Focus states are visible and not color-only.
- Icon-only controls have accessible names.
- Form errors name the field, describe the problem, and remain available to assistive tech.
- Text and UI contrast meet the project bar.
- Meaning is not conveyed by color alone.
- Touch and click targets are large enough for reliable input.
- Motion respects `prefers-reduced-motion`.
- Content works at narrow widths, zoom, and text resizing.

## State Matrix Template

| State | Decision |
|---|---|
| Loading | Skeleton, spinner, optimistic shell, or no placeholder |
| Empty | Useful next action or explanation |
| Partial data | What remains trustworthy and what is hidden or caveated |
| Error | Recovery path, retry behavior, support/debug detail, and safe copy |
| Offline / slow network | Stale data, retry, or blocking UI |
| Disabled / permission denied | Why action is unavailable and how to resolve it |
| Long text / many items | Wrapping, truncation, pagination, virtualization, overflow |
| Mobile / narrow viewport | Navigation, density, sticky controls, content priority |
| Reduced motion | Crossfade or instant equivalent for animated flows |
| High contrast / forced colors | Borders, focus, icons, charts, and status indicators still read |

## Evidence To Ask Agents For

Use the checks that match the repo. Do not cargo-cult all of them into every project.

- Browser screenshots for desktop and mobile states.
- Keyboard-only walkthrough notes.
- Accessibility scan output, such as axe through Playwright or the repo's chosen tooling.
- Storybook stories or an HTML contact sheet for state variants.
- Reduced-motion and high-contrast screenshots when motion or color encodes meaning.
- A `Before` / `After` polish table when running `make-interfaces-feel-better`.

## When To Reach For Impeccable

Use `make-interfaces-feel-better` when the interface mostly exists and needs polish.

Use Impeccable when the work needs a more complete design loop: product brief, design brief, detector rules, browser iteration, and a persistent quality bar across a frontend project.

Do not install Impeccable automatically. Make it an intentional repo decision because it changes the workflow more than a lightweight skill does.
