---
name: to-spec
description: Convert a validated discovery document into an implementation-ready specification with numbered requirements and Given/When/Then acceptance criteria. Use when docs/discovery.md exists and requirements must become precise, testable statements. Triggers on "write the spec", "to-spec", "turn this into a spec", requirements document, before planning tickets.
---

# /to-spec

Phase: SPECIFICATION. Input to `/to-tickets`.

## Read

- `docs/discovery.md`
- `CLAUDE.md`

## Produce

`docs/spec.md`

## Spec must contain

- Overview
- Problem
- Goals
- Non-goals
- Users and roles
- Functional requirements — numbered `FR-*`, each with observable behavior
- User flows
- Business rules
- Data requirements
- API requirements
- Error states
- Edge cases
- Acceptance criteria — Given / When / Then per feature
- Technical constraints

## Rules

- Do not invent requirements. Every statement traces to `docs/discovery.md`.
- Unresolved ambiguity is marked `OPEN QUESTION` and listed at the end. Never silently guessed.
- Every requirement gets a stable ID (`FR-1`, `FR-2`). Later skills reference these IDs.
- Every feature must have observable behavior — a reader must be able to tell whether it works.
- Every major feature must have acceptance criteria.
- No implementation code, no file structure, no framework choice. That belongs to `/to-tickets` and `/implement`.

## Gate

Do not proceed if any `OPEN QUESTION` affects a functional requirement. List them and stop.

## Workflow State

Append to `.workflow/state.md`:

```
| to-spec | specification | <ISO date> | docs/spec.md |
```

## Next

`/ui-design` first if the product has an interface, then `/to-tickets`.
