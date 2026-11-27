# Auditor annex

Use this under the `---` cut of the Phase 3 chat, or in `gate-report.md` if the user asked for the annex. Never above the cut. Never as a box-drawing table. Never as an 8-column table in the chat.

Status words and CI tokens are in `SKILL.md`. Do not invent another token.

If a 4-column recap fits under the cut, this is the only allowed grid:

| CUJ | Result | Severity | One sentence |
|---|---|---|---|
| CUJ-3 | FAIL | P0 | order created without payment |

Prefer bullets when a cell is longer than a few words.

## A. Context

- SHA / env / date
- `Path: FAST` or `Path: FULL` (same token as Phase 1)
- `Fast hint: honored | ignored — <veto item> | not passed`
- CI status (token from `SKILL.md`; for `RED (unrelated)`, name the check)
- Rollback
- Residual risks
- Known decisions applied: each dropped or downgraded point, with source, date, and the SHA it was re-checked on (`none` if none)
- Owner: named person, or the requester. Required when the decision is `GO WITH RISKS`
- Tests not run + why
- Gaps accepted by the user: the gap, and the reply that accepted it
- Post-merge checks: each one, the AC wording that names its env or moment, the deploy config showing that env is built from the base branch or a tag, and the pre-merge CUJ covering its behaviour
- Plan: each Phase 1 invariant, one bullet, with the CUJ that covers it or `not covered` + why

Do not cite ticket status. In the chat, do not repeat A as a second essay.

## B. Execution

One block per CUJ, in plan order:

```text
CUJ-01 · P0 · FAIL
Expected: …
Actual: …
Where: path#Lxx or path (UNKNOWN line — reason)
```

## C. Failures

Only if a FAIL exists. Under that CUJ block:

- steps, expected vs actual, URL
- HTTP / payload / console / SQL (SQL only if DB ON)
- reproducibility
- hypothesis tied to a diff hunk when one exists (`path#L42-L58`); otherwise hypothesis + `UNKNOWN line`
- fix or missing automated test

## D. Gaps

What was not tested (perf, a11y, mobile, rollback drill, DB when OFF, waived tools, orphan spec files) and the risk of each gap. In the chat this is the `Not verified` list. Post-merge checks are listed in A, never here nor under `Tests not run`.
