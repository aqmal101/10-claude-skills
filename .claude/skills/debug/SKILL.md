---
name: debug
description: Find and fix the root cause of a failure, crash, or wrong result using evidence at every step, then prevent recurrence with a test. Use when something is broken, a test fails unexpectedly, or behavior does not match the spec. Triggers on "debug", "this is broken", "why does it fail", "it works on my machine", "it throws", stack trace, flaky, intermittent, "it worked yesterday", regression. Not for building new features.
---

# /debug

Phase: DEBUGGING. Self-contained. Evidence before edits — always.

## Process

1. **Reproduce.** Get a reliable, repeatable failure with the exact command, input, and observed output. If you cannot reproduce it, stop and say so. Do not debug a ghost. Ask what changed, when it last worked, and whether it is consistent.
2. **Gather evidence.** Read the actual error message and the full stack trace — the top frame is often a victim, not the cause. Find the input that triggered it. Read the surrounding code and its recent changes. `git log`, `git bisect`, and `git diff` are usually faster than reasoning.
3. **State one hypothesis** as a falsifiable claim: "X is wrong because Y." Not a list of suspects.
4. **Try to disprove it.** Pick the cheapest observation that would kill the hypothesis. If it survives, discard it — do not keep arguing for it — and form the next one.
5. **Identify the root cause** — why the failure exists, not the line where it surfaced. A stack trace shows where it broke, never why.
6. **Explain** the cause in plain language *before* changing anything. If you cannot explain it, you have not found it yet.
7. **Fix minimally.** The smallest change that removes the cause. No refactoring, no cleanup, no reformatting while debugging.
8. **Verify.** Re-run the original reproduction from step 1. It must pass.
9. **Prevent.** Add or update the test that would have caught this. A fix with no regression test is not finished.

## Rules

- Do not patch symptoms. Suppressing an error, widening a `try`, adding a `sleep`, or bumping a timeout hides the failure until it returns somewhere worse.
- Do not change more than one thing at a time. Batched changes hide which one worked.
- Do not rewrite unrelated code, even if it is ugly. Note it; do not fix it here.
- Do not declare success without re-running the original reproduction.
- Do not delete, skip, or weaken a failing test to make the suite pass. If the test is wrong, say so and fix it deliberately.
- Do not guess. Read the code. Read the error. Evidence over intuition.
- A fix with no regression test is not a complete fix.

## Output

- exact reproduction steps and command
- root cause, explained in plain terms
- the fix, and why this fix rather than another
- the test that now prevents it
- anything else the investigation turned up, reported but not acted on

## Workflow State

Append to `.workflow/state.md`:

```
| debug | debugging | <ISO date> | <root cause in one line> |
```

## Next

`/test` to confirm no regressions, then back to `/implement` for the remaining ticket work.
