# Software Development Workflow

This environment uses a modular software development workflow.

## Available Skills

Skills live in `.claude/skills/` and are invoked as `/<name>`.

1. /grill-me — discovery
2. /to-spec — specification
3. /ui-design — design
4. /to-tickets — planning
5. /implement — implementation
6. /test — testing
7. /review — review
8. /debug — debugging
9. /ship — shipping
10. /document — documentation

Skills run one at a time, in this order. Each names the next.

```
IDEA → DISCOVERY → SPECIFICATION → DESIGN → PLANNING
     → IMPLEMENTATION → TESTING → REVIEW → DEBUGGING
     → SHIPPING → DOCUMENTATION
```

`/debug` branches off from any phase where something is broken, and returns to it.

## Core Principle

Do not jump directly into implementation when requirements are unclear.

Prefer:

IDEA
→ DISCOVERY
→ SPECIFICATION
→ DESIGN
→ PLANNING
→ IMPLEMENTATION
→ TESTING
→ REVIEW
→ DEBUGGING
→ SHIPPING
→ DOCUMENTATION

## Modular Execution

Skills are independent.

Do NOT automatically execute all skills.

Only execute the skill explicitly requested by the user unless
the current skill's contract requires another skill.

## Engineering Rules

- Read relevant project files before making claims about the codebase.
- Do not invent requirements.
- Do not modify unrelated code.
- Follow the existing project architecture and conventions.
- Prefer simple solutions over unnecessary abstraction.
- Keep changes small and reviewable.
- Do not implement features that are not specified.
- Preserve existing behavior unless the specification requires a change.

## Design Quality

Avoid generic AI-generated interfaces.

Design must be:
- context-specific
- intentional
- consistent
- accessible
- responsive
- visually distinctive

Do not default to common AI UI patterns without design justification.

## Workflow State

When a project uses the workflow system, maintain:

`.workflow/state.md`

Every skill appends one row to its History table and updates Current on
completion. If a step is skipped, say so explicitly rather than leaving the
state stale — a stale state file is worse than none.
