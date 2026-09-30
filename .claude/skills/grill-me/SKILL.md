---
name: grill-me
description: Interrogate a vague product idea until it becomes a well-defined problem. Use when the user proposes a feature, product, or project that is still fuzzy, ambiguous, or unvalidated, and before any spec or code is written. Triggers on "grill me", "help me think through this", "I have an idea for", "not sure what to build", brainstorming a new project.
---

# /grill-me

Phase: DISCOVERY. Input to `/to-spec`. Do not write production code.

## Purpose

Turn an ambiguous idea into a well-understood problem.

## Behavior

Do not propose implementation. Do not draft a spec.

Ask focused questions about:

- problem — what hurts today, and for whom
- target users
- goals and measurable success criteria
- non-goals
- core workflows
- user roles
- permissions
- data
- integrations
- constraints — deadline, stack, budget, team
- edge cases

## Rules

- Ask one logical group of questions at a time. Wait for the answer.
- Challenge assumptions explicitly. State what you are questioning and why.
- Identify contradictions between answers.
- Identify missing requirements rather than filling them yourself.
- Do not invent answers. If the user does not know, record it as an open question.
- Adapt depth to the idea. A one-line idea needs five questions, not fifty.
- Stop when further questions produce no new information.

## Output

Write `docs/discovery.md` containing:

1. Problem statement
2. Target users
3. Goals
4. Non-goals
5. Core workflows
6. Roles and permissions
7. Functional requirements
8. Non-functional requirements
9. Constraints
10. Assumptions
11. Open questions

## Workflow State

Append to `.workflow/state.md`:

```
| grill-me | discovery | <ISO date> | <file or doc touched> |
```

## Next

`/to-spec` — but only after open questions are resolved or explicitly accepted as risk.
