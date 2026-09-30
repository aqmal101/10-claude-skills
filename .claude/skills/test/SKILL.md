---
name: test
description: Verify an implementation against its ticket's acceptance criteria, close coverage gaps, and drive new behavior from failing tests. Use when code must be proven rather than assumed — after writing a feature, when a suite fails, or when asked to add tests. Triggers on "test this", "write tests", "does it pass", "verify", "check the acceptance criteria", "add coverage", "the test is failing". Self-contained — no TDD plugin required.
---

# /test

Phase: TESTING. Self-contained. Does not require any external test skill.

## Read

- `docs/spec.md`
- the selected ticket in `tickets/`
- the implementation and the existing test suite

## Mode A — verify what exists

Use when code is already written and its correctness is unproven.

1. Inspect the existing tests. Match their framework, layout, naming, and assertion style. **Do not introduce a second test framework.**
2. Run the suite first. Establish the baseline before adding anything. A suite that was already red is not your failure — record it before changing anything.
3. Map each acceptance criterion in the ticket to a test. Criteria with no test are the work.
4. Cover the edge cases and error states named in the spec.
5. Re-run. New tests must actually fail against a broken behavior. Prove it — temporarily invert the assertion or the input, watch it go red, then restore. A test never observed failing is not evidence of anything.
6. Run the full suite, not just the new tests. Report regressions separately from pre-existing failures.

## Mode B — drive new behavior

Use when behavior does not exist yet and the ticket is still in progress. Same skills, no TDD plugin needed.

1. **Red** — write one failing test for the next acceptance criterion. Run it. Confirm it fails, and confirm it fails *for the stated reason*, not from a typo or a broken import.
2. **Green** — write the minimum code to pass. Nothing more.
3. **Run the full suite.** The new test is worthless if it cost you an existing one.
4. **Refactor** — only now, with the suite green and re-run after every change.
5. Repeat per criterion.

Rules for Mode B:

- One test at a time. A batch of red tests gives you a batch of ambiguous failures.
- Never write the implementation first and back-fill the test. The test then only confirms what you already decided, and cannot fail the way the real bug would.
- The test describes the requirement, not your function names. `FR-3 rejects a coupon that expired yesterday`, not `test_applyDiscount_2`.

## Rules

- Test observable behavior, not implementation details. A test that breaks when you rename a private helper is testing the wrong thing.
- Every acceptance criterion needs a test, or an explicit note on why it does not need one. "It's obvious" is not a note.
- Do not mock the thing under test. Mocks verify your assumptions, not the code's behavior.
- Do not weaken, skip, or delete a test to get a green suite. If a test is genuinely wrong, say so and change it deliberately — do not bury it.
- Do not assert on log output, internal state, or private fields as a proxy for behavior.
- A passing suite is not proof of correctness. Report what remains unverified.

## Output

- exact command run
- passed / failed counts, split into new, pre-existing, and regression
- acceptance criteria → test mapping, with any gap named
- uncovered cases
- regressions, with the test name
- recommended fixes

## Workflow State

Append to `.workflow/state.md`:

```
| test | testing | <ISO date> | <test files> |
```

## Next

`/review`. If a test fails because the implementation is wrong, `/debug` — do not fix it inside this skill.
