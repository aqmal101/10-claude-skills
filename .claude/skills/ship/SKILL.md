---
name: ship
description: Prepare, verify, and execute a release with a pre-flight report and a post-deploy check. Use when reviewed and tested work is ready to merge, tag, or deploy. Triggers on "ship it", "release", "deploy", "publish", "cut a release", "merge and deploy". Always reports before acting; never deploys destructive changes without explicit confirmation.
---

# /ship

Phase: SHIPPING. Report first, act second.

## Pre-flight check

- tests pass
- build succeeds
- no unresolved BLOCKER or IMPORTANT review findings
- environment variables present in the target environment
- database migrations — reversible? destructive? data loss possible?
- API and CORS configuration
- deployment configuration changed
- secrets not committed
- version bumped
- changelog and release notes written

## Before shipping, report

- what is being released
- target environment
- migration impact — including rollback plan
- known risks
- verification steps to run after deploy

**Then stop and wait for explicit confirmation.** Do not deploy, push, or merge before it.

## Rules

- Do not deploy destructive changes — a data-dropping migration, an irreversible API change — without explicit confirmation naming the specific risk.
- Never force-push a shared branch or bypass hooks and signing without being asked.
- A failed verification step after deploy is an incident, not a footnote. Report it as such.

## After deployment

- health check passes
- one critical workflow verified end to end, in the real environment
- release status recorded

## Output

Report:

- release contents
- environment
- migration result
- post-deploy verification results
- rollback path if something must be reverted

## Workflow State

Append to `.workflow/state.md`:

```
| ship | shipping | <ISO date> | <version or tag> |
```
