# Design Quality

Use this file for UI work. Accessibility and state coverage are acceptance criteria, not late polish.

## Accessibility Bar

Default to WCAG 2.2 AA for web UI unless the product has a stricter or explicitly different standard.

- Keyboard navigation reaches every interactive control in a sensible order.
- Focus states are visible and not color-only.
- Icon-only controls have accessible names.
- Form errors identify the field, describe the problem, and remain available to assistive tech.
- Text/background contrast meets the project bar.
- Meaning is never conveyed by color alone.
- Touch/click targets are large enough for reliable input.
- Motion respects `prefers-reduced-motion`.
- Content survives zoom, text resizing, and narrow/mobile widths.

## State Matrix

| State | What To Decide |
|---|---|
| Loading | Skeleton, spinner, optimistic shell, or no placeholder |
| Empty | Useful next action or explanation |
| Partial data | What remains trustworthy and what is hidden or caveated |
| Error | Recovery path, retry, support/debug detail, and safe copy |
| Offline / slow network | Stale data, retry, or blocking UI |
| Disabled / permission denied | Why action is unavailable and how to resolve it |
| Long text / many items | Wrapping, truncation, pagination, virtualization, overflow |
| Mobile / narrow viewport | Navigation, density, sticky controls, content priority |
| Reduced motion | Crossfade or instant equivalent for animated flows |
| High contrast / forced colors | Borders, focus, icons, charts, and status indicators still read |

## Polish Pass

Run `make-interfaces-feel-better` when the UI technically works but feels unfinished.

Report changes in a table:

| Principle | Before | After |
|---|---|---|
| Typography | | |
| Surfaces | | |
| Motion | | |
| Performance | | |

## Evidence

- Desktop screenshot:
- Mobile screenshot:
- Keyboard walkthrough:
- Accessibility scan:
- Reduced motion / high contrast check:
- State coverage:
