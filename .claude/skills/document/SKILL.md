---
name: document
description: Synchronize project documentation with what the code actually does, and keep ADRs for decisions that were not obvious. Use after implementation or release when docs have drifted, when onboarding a new contributor, or when a decision needs recording. Triggers on "document this", "update the README", "write an ADR", "docs are out of date", "onboarding", architecture overview. Never documents functionality that does not exist.
---

# /document

Phase: DOCUMENTATION. Reflects reality; does not create intent.

## Read first

- the actual implementation — every claim below must be verified against code
- the spec — to know what was intended

If code and spec disagree, report the divergence. Do not paper over it and do not silently pick a side.

## Update only what is stale

- README — setup, run, project purpose
- architecture overview — the real structure, not an idealized one
- API documentation — from the actual routes and payloads
- setup and environment documentation
- database documentation — real schema, real migrations
- deployment documentation
- ADR — for a decision that was non-obvious and would be re-litigated without context
- changelog

## Rules

- Every documented behavior must be verified to exist. Never document aspirational functionality.
- Do not create documentation nobody asked for. A new doc needs a reader with a task in mind.
- Match the depth to the audience — onboarding docs for a new contributor, not for the person who wrote the code.
- Prefer updating an existing doc over creating a second one on the same subject.
- No screenshots or examples that were not actually produced.

## Output

List what changed and what is still undocumented.

## Workflow State

Append to `.workflow/state.md`:

```
| document | documentation | <ISO date> | <files updated> |
```
