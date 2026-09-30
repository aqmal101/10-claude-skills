---
name: ui-design
description: Produce an intentional UX and visual design direction for a specified product. Use when docs/spec.md describes screens, flows, or user-facing behavior and a design direction is needed before implementation. Triggers on "design the UI", "ui-design", "what should this look like", visual direction, design system, styling direction. Not for backend or system architecture.
---

# /ui-design

Phase: DESIGN. Covers UX, visual, responsive, accessibility. Not architecture.

## Read

- `docs/spec.md`
- `docs/discovery.md`
- `CLAUDE.md`

## Produce

`docs/design.md`

## Must cover

### UX

- information architecture
- navigation model
- user flows
- interaction patterns
- empty states
- loading states
- error states
- success states

### Visual

- design principles
- visual personality
- typography — chosen face with a reason, not a default
- color system — semantic roles, not just swatches
- spacing scale
- visual hierarchy
- component language
- iconography
- imagery
- motion

### Responsive

- mobile, tablet, desktop behavior for every layout decision

### Accessibility

- contrast
- keyboard navigation
- focus states
- semantic structure
- readable typography

## Anti-AI-Slop Rules

Do not default to:

- generic SaaS dashboards
- predictable card grids
- excessive rounded cards
- purple gradients
- Inter / Roboto / system fonts without justification
- arbitrary glassmorphism
- decorative gradients with no purpose
- excessive shadows
- meaningful-sounding but purposeless animation
- generic hero sections

Every major visual decision must state the reason and connect it to the product context. Prefer contextual design over fashionable design.

## Output

`docs/design.md`:

1. Product personality
2. UX principles
3. Information architecture
4. User flows
5. Visual direction
6. Typography
7. Color system
8. Layout and spacing system
9. Components
10. Interaction patterns
11. Responsive behavior
12. Accessibility
13. Anti-patterns to avoid
14. Design rationale

## Workflow State

Append to `.workflow/state.md`:

```
| ui-design | design | <ISO date> | docs/design.md |
```

## Next

`/to-tickets`.
