# 10 Claude Skills — Software Development Workflow

A modular workflow for taking an idea to a shipped, documented feature using
Claude Code skills. Ten skills, one per phase. Each one is self-contained,
invokes independently, and names the next.

The problem it solves: Claude will happily write code for a vague request and
produce something plausible, wrong, and already merged by the time you notice.
These skills put a checkpoint before every irreversible step, and make each
phase leave an artifact on disk so the next one has something to reason about
instead of a conversation to remember.

---

## Install

Copy the `skills` directory into your project:

```
your-project/
└── .claude/
    └── skills/          ← copy the 10 folders here
```

Or, from a clone of this repo:

```bash
cp -r .claude/skills/* <your-project>/.claude/skills/
```

Restart Claude Code. Skills appear in the skill list and can be invoked as
`/<name>` or triggered automatically by the `description` field.

Requires Claude Code with skill support. No dependencies, no packages, no
network calls.

---

## The flow

```
IDEA → DISCOVERY → SPECIFICATION → DESIGN → PLANNING
     → IMPLEMENTATION → TESTING → REVIEW → DEBUGGING
     → SHIPPING → DOCUMENTATION
```

`/debug` branches off from any phase where something is broken, and returns to
where it left off. Everything else runs in order.

### The ten skills

| # | Skill | Phase | Does | Writes |
|---|---|---|---|---|
| 1 | `/grill-me` | Discovery | Interrogates a vague idea until the problem is understood | `docs/discovery.md` |
| 2 | `/to-spec` | Specification | Turns discovery into numbered, testable requirements | `docs/spec.md` |
| 3 | `/ui-design` | Design | UX, visual, responsive, accessibility direction | `docs/design.md` |
| 4 | `/to-tickets` | Planning | Breaks the spec into small, ordered, independent tickets | `tickets/NNN-*.md` |
| 5 | `/implement` | Implementation | Builds one ticket, following existing conventions | source code |
| 6 | `/test` | Testing | Verifies against acceptance criteria; writes missing tests | tests |
| 7 | `/review` | Review | Spec compliance + architecture + security + scope | findings |
| 8 | `/debug` | Debugging | Root cause, minimal fix, regression test | fix + test |
| 9 | `/ship` | Shipping | Pre-flight report, confirmation, deploy, verify | release |
| 10 | `/document` | Documentation | Syncs docs to what the code actually does | docs, ADRs |

`/ui-design` is optional — skip it for backend or library work. Every other
skill is part of the intended path.

---

## Using it

### Starting from an idea

```
/grill-me
```

Describe the idea in one or two sentences. The skill will ask about the
problem, users, goals, non-goals, workflows, roles, data, constraints, and
edge cases — one group of questions at a time, waiting for each answer. It will
challenge assumptions and name contradictions rather than accepting them.

When the problem is clear, it writes `docs/discovery.md` and lists what is
still an open question.

### Continuing

```
/to-spec        →  docs/spec.md
/ui-design      →  docs/design.md      (skip for backend)
/to-tickets     →  tickets/001-*.md, 002-*.md, ...
/implement tickets/001-auth.md
/test
/review
/ship
/document
```

You drive the order. The skills do not chain themselves. Each one ends by
naming the next.

### Invoking

Type `/` and the skill name appears. Or just ask in natural language — the
`description` field in each skill is written to trigger on the phrases a person
would actually use:

| Skill | Triggers on |
|---|---|
| `/grill-me` | "I have an idea for…", "not sure what to build", "help me think this through" |
| `/to-spec` | "write the spec", "turn this into a spec" |
| `/ui-design` | "design the UI", "what should this look like" |
| `/to-tickets` | "break this into tickets", "split into tasks", "plan this" |
| `/implement` | "implement ticket 3", "build this", "make it work" |
| `/test` | "write tests", "does it pass", "add coverage" |
| `/review` | "review this", "check my work", "before I merge" |
| `/debug` | "this is broken", "why does it fail", "it worked yesterday" |
| `/ship` | "ship it", "release", "deploy" |
| `/document` | "update the README", "docs are out of date" |

### Skipping is allowed

`/ui-design` can be skipped. `/test` and `/review` can be skipped for a
one-line change. But say so explicitly — a skipped step recorded as done is
worse than one recorded as skipped.

---

## How the skills constrain Claude

Each skill is written to stop a specific failure mode.

**`/grill-me` will not propose implementation.** A vague request is not a
licence to start coding. It asks until the problem is understood, and it
records what you do not know instead of guessing.

**`/to-spec` will not invent requirements.** Anything not traceable to
`docs/discovery.md` is marked `OPEN QUESTION` and blocks the gate. Every
requirement gets a stable `FR-*` ID so later phases can reference it.

**`/ui-design` refuses generic output.** No purple gradients, no default
Inter, no card grids without a reason. Every visual decision must state why it
fits this product. See the Anti-AI-Slop Rules section in that skill.

**`/to-tickets` will not add scope.** A needed requirement that is missing from
the spec is reported, not invented. Acceptance criteria are copied verbatim —
never weakened during decomposition.

**`/implement` refuses to start without a ticket.** It reads existing code
before writing, follows its conventions, keeps the diff minimal, and will not
report a ticket done until every acceptance criterion is verified by running
something.

**`/test` will not let a green suite pass as proof.** Every criterion maps to a
test. New tests are observed failing before being trusted. Pre-existing
failures are reported separately from regressions. Includes a red-green-refactor
cycle for behavior that does not exist yet, so it stands alone without an
external TDD skill.

**`/review` reports, it does not fix.** It is read-only unless you ask
otherwise. Every finding needs a concrete failure scenario — what input
produces what wrong result. Anything not confirmed is marked `PLAUSIBLE`
rather than presented as fact. It will not pad the list.

**`/debug` requires evidence before edits.** Reproduce first; if it will not
reproduce, it stops. One falsifiable hypothesis at a time, actively tried to
disprove. No symptom patches — a `try` block or a `sleep` will not pass. Ends
with a regression test.

**`/ship` stops before acting.** It reports what will ship, the migration
impact, the rollback plan, and the known risks — then waits for explicit
confirmation. It will not deploy a destructive change without you naming that
specific risk.

**`/document` will not document what does not exist.** Every claim is verified
against the code. Where code and spec disagree, it reports the divergence
rather than quietly picking a side.

---

## Gates

Some skills stop rather than produce something plausible.

| Skill | Stops when |
|---|---|
| `/to-spec` | An `OPEN QUESTION` affects a functional requirement |
| `/implement` | No ticket was selected |
| `/implement` | The ticket's acceptance criteria cannot be verified |
| `/debug` | The failure cannot be reproduced |
| `/ship` | Tests fail, build fails, or a BLOCKER is open |
| `/ship` | A destructive change is proposed without explicit confirmation |

`/review` is the other half: no ticket closes while a `BLOCKER` is open.

---

## Conventions the skills rely on

**Requirement IDs.** `docs/spec.md` numbers functional requirements `FR-1`,
`FR-2`, and so on. Tickets cite the IDs they satisfy, and `/review` checks
implemented behavior against them. This is what makes "does the code do what
we said" answerable.

**Acceptance criteria format.** Given / When / Then, one per feature, defined
in the spec and copied verbatim into the ticket. If a ticket cannot state how
it will be verified, it is not ready.

**`docs/design.md` is optional.** Everything that reads it treats it as
optional and skips the UX dimension when it is absent.

**`.workflow/state.md`.** Every skill appends one row to its History table and
updates the Current phase. This is a record of where the project is, not a
progress bar — if a step is skipped, it says so.

---

## File layout produced

```
your-project/
├── CLAUDE.md                  workflow rules for the agent
├── .workflow/
│   └── state.md               current phase + history
├── .claude/
│   └── skills/                the 10 skills
├── docs/
│   ├── discovery.md           ← /grill-me
│   ├── spec.md                ← /to-spec
│   └── design.md              ← /ui-design
├── tickets/
│   ├── INDEX.md               ← /to-tickets
│   ├── 001-*.md
│   └── 002-*.md
└── <your source and tests>
```

---

## Adapting it

The skills are plain Markdown. Read them before you use them, and edit them
for your project.

**Common edits:**
- Swap the output paths if you keep docs somewhere else.
- Add a stack section to `/test` and `/implement` — the framework, the runner
  command, the layout. This is the highest-value edit you can make; it removes
  the guesswork from every implementation run.
- Trim `/ui-design`'s anti-pattern list to what is actually wrong in your
  codebase.
- Add a `/security-review` skill if you handle untrusted input.

**Worth resisting:**
- Merging skills to make the list shorter. Each one is a checkpoint; two
  checkpoints in one skill is one checkpoint.
- Adding process skills you will not run. An unused skill is noise, and a noisy
  list makes the real ones easier to miss.
- Making them stricter than the project. A workflow nobody follows is worse
  than no workflow.

---

## Relationship to other skill packs

These ten are self-contained. They do not require a TDD plugin, a
brainstorming skill, or a planning framework — `/test` carries its own
red-green-refactor cycle, `/grill-me` its own questioning discipline, and
`/to-tickets` its own decomposition rules.

If you also have other skills installed, that is fine. The overlap is
deliberate and scoped:

| This pack | Overlaps with | Distinction |
|---|---|---|
| `/test` | `test-driven-development` | Adds acceptance-criteria mapping and coverage-gap reporting; includes the cycle rather than deferring it |
| `/review` | `code-review` | Gates on spec compliance by `FR-*`; requires a failure scenario per finding |
| `/debug` | `systematic-debugging` | Adds the "must reproduce first" stop and the mandatory regression test |

Invoke one or the other deliberately. Running both on the same code produces
two overlapping reports, not two independent ones.

---

## License

MIT — see [LICENSE](LICENSE). Use it, fork it, adapt it for your team.

Contributions welcome. If you extend a skill, the two things that matter most
are a `description` that triggers on how people actually talk, and a rule that
stops Claude doing the thing this skill exists to prevent. A skill with no
constraint is just a prompt.
