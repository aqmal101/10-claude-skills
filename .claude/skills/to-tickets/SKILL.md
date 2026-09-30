---
name: to-tickets
description: Break an approved specification and design into small, independently understandable, dependency-ordered executable tickets. Use when docs/spec.md and docs/design.md are settled and work must be scheduled and delegated. Triggers on "to-tickets", "break this into tickets", "split into tasks", planning, backlog, work breakdown.
---

# /to-tickets

Phase: PLANNING. Input to `/implement`.

## Read

- `docs/spec.md`
- `docs/design.md` (if present)

## Produce

`tickets/NNN-<slug>.md` — one file per ticket.

## Each ticket must contain

- ID and title
- objective — one sentence
- context — links to the `FR-*` requirement IDs it satisfies
- dependencies — other ticket IDs
- files or modules likely affected
- implementation requirements
- acceptance criteria — Given / When / Then, copied from the spec
- testing requirements

## Rules

- One ticket, one outcome. Do not mix unrelated concerns.
- Prefer small. If a ticket cannot be described in one sentence, split it.
- Every ticket must be independently understandable — a reader must not need a meeting to know what to build.
- Preserve dependency order. Record it as ticket IDs, not prose.
- Do not add requirements. A needed requirement is missing from the spec — report it, do not invent it.
- Do not specify implementation beyond naming likely files. Design decisions belong to `/implement`.
- Every ticket must have acceptance criteria. No criteria, no ticket.
- Reuse the spec's acceptance criteria verbatim. Do not weaken them during decomposition.

## Output

Also produce `tickets/INDEX.md` with the ordered list and the dependency graph.

## Workflow State

Append to `.workflow/state.md`:

```
| to-tickets | planning | <ISO date> | tickets/ |
```

## Next

`/implement` — one ticket at a time.
