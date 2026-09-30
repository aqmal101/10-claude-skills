---
name: review
description: Review code for spec compliance, architecture fit, security, performance, and scope discipline, then classify findings by severity. Use after tests pass and before a ticket is closed, a branch merges, or a release ships. Triggers on "review this", "code review", "check my work", "is this good", "before I merge", "look over this diff". Spec-compliance gate, not a general diff review. Reports findings and does not edit code.
---

# /review

Phase: REVIEW. Self-contained. Read-only by default — findings, not fixes.

## Read

- `docs/spec.md` — the requirements, by `FR-*` ID
- `docs/design.md` (if present) — the intended UX and visual behavior
- the ticket under review — its scope and acceptance criteria
- the implementation and the diff

If `docs/spec.md` is missing, say so. Without it this is an opinion, not a review, and the Spec compliance dimension cannot run.

## Dimensions

### Spec compliance
The dimension that makes this skill different from a generic diff review.
- Does each implemented behavior trace to a specific `FR-*`? Name the ID.
- Is any implemented behavior **not** in the spec? Unrequested behavior is a finding, not a bonus.
- Are the ticket's acceptance criteria all verified, and by what?
- Does behavior match the spec's error states and edge cases, or only the happy path?

### Architecture
- Does it fit the existing structure, or was a new pattern introduced?
- Is there a new abstraction with one implementation, a factory for one product, or a config for a value that never changes?
- Did it duplicate something the codebase already has?

### Maintainability
- Will the next reader understand it without you in the room?
- Naming, dead code, hidden coupling, unexplained cleverness.
- Comments that explain *why* rather than restating *what*.

### Security
Only issues **introduced by this change**.
- Injection — SQL, command, template, path.
- Authz on every new endpoint or handler. Authentication is not authorization.
- Secrets in code, config, logs, or fixtures.
- Unvalidated input at a trust boundary — user input, headers, third-party payloads.
- Unsafe deserialization, unconstrained file paths, SSRF on user-supplied URLs.
- Weak or hand-rolled crypto.

### Performance
Obvious and demonstrable, not speculative.
- N+1 queries; work in a loop that could be hoisted.
- A missing index on a new query path.
- Unbounded input — a list, upload, or query with no limit.
- Blocking work on a request path.

### UX
- Does the implementation follow `docs/design.md`?
- Any state the design did not cover — empty, loading, error, or permission-denied?
- Keyboard reachable, focus order sane, labels present?
- Does it error on a user-facing path in a way that shows nothing to the user?

### Scope
- Did the change include anything the ticket did not ask for?
- Speculative config, dead scaffolding, comments about future plans?
- Unrelated formatting churn that will make the real diff hard to read?

## Rules

- **Report, do not fix.** Only edit code if the user explicitly asks.
- Every finding needs a concrete failure scenario: what input or state produces what wrong result. "Consider extracting this" is not a finding.
- No finding without a location and a consequence.
- **Verify before reporting.** A suspected bug you did not confirm is marked `PLAUSIBLE`, not confirmed. Say which one it is.
- Do not report style preferences the project's existing code does not follow. Consistency beats your taste.
- Do not report something the linter or the type checker already catches. Read the output of those tools if they exist.
- Do not pad the list. Three real findings beat thirty speculative ones.
- Findings you cannot connect to a spec requirement, a code location, or a failure mode belong in `NOTE` at most — or nowhere.

## Output

Findings ordered most severe first. Each one:

```
[SEVERITY] <file>:<line> — <one-sentence claim>
Failure scenario: <concrete input/state → wrong result>
VERIFIED | PLAUSIBLE
```

Severity:

- `BLOCKER` — wrong behavior, data loss, security hole. Blocks the ticket.
- `IMPORTANT` — will cause a defect or costly rework, not immediately.
- `MINOR` — real, low impact.
- `NOTE` — worth knowing, no action required.

State explicitly if no findings survive verification. Do not pad.

## Workflow State

Append to `.workflow/state.md`:

```
| review | review | <ISO date> | <reviewed scope> |
```

## Next

Fix `BLOCKER` and `IMPORTANT`, then `/ship`. Do not close a ticket with an open BLOCKER.
