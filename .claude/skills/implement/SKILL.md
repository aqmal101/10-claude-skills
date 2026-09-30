---
name: implement
description: Implement one approved ticket end to end, following the spec, the design, and existing project conventions. Use when tickets/ contains a ready ticket and the code must actually be written. Triggers on "implement", "build this ticket", "do ticket N", "make it work", execute a ticket. Never starts work without a selected ticket.
---

# /implement

Phase: IMPLEMENTATION. One ticket at a time.

## Before coding

Read:

- `CLAUDE.md`
- `docs/spec.md`
- `docs/design.md` (if present)
- the selected ticket in `tickets/`
- the relevant existing source code

**Stop if no ticket was selected.** Do not implement from a bare request — route to `/to-tickets` instead.

## Rules

- Implement only the selected ticket. No adjacent improvements, no drive-by refactors.
- Investigate existing code before modifying it. Read before you write.
- Follow existing conventions — naming, structure, error handling, test layout. Match what is there, not what you prefer.
- Do not invent requirements. Missing requirement means the spec is wrong — report it, do not guess.
- Keep the implementation minimal. Smallest diff that satisfies acceptance criteria.
- Do not mark the ticket complete until every acceptance criterion is verified by running something.

## After implementation

Report:

- files changed
- implementation summary
- tests added or updated
- acceptance criteria status — each one, verified or not
- known issues and anything the ticket did not cover

## Workflow State

Append to `.workflow/state.md`:

```
| implement | implementation | <ISO date> | <ticket ID> |
```

## Next

`/test` for verification, then `/review` before the ticket is closed.
