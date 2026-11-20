# Phase 3 — copy the content of the fenced block only

Everything outside the fence is an instruction. Do not print it. Do not print the fence lines either: line 1 of the chat is the `<decision>` tag.

Inside the fence, `[[ … ]]` is a choice: keep only the line or lines that apply, drop the brackets and the instruction. Nothing in `[[ ]]` or `< >` reaches the chat as written.

```text
<decision>{GO|NO-GO|GO WITH RISKS}</decision>

**{GO|NO-GO|GO WITH RISKS}** — PR <id> — <short sha> — FAST|FULL

<one or two sentences: business impact. No file path here.>

Blockers
[[ one line per P0/P1 CUJ whose status is FAIL, BLOCKED or SKIPPED, in plan order, each in one of these shapes:
- P0 — CUJ-<n> — FAIL: <finding, one sentence>
- P0 — CUJ-<n> — BLOCKED: <every cause still open>
- P1 — CUJ-<n> — SKIPPED (waived): <gap>. Waived by <who>: "<their reply>"
- P1 — CUJ-<n> — SKIPPED (user-approved — <reason>). Approved by <who>: "<their reply>"
  — or, if there is none —
Nothing blocking. <remaining P2s in one line, or: no P2> ]]

Residual risks: <none, or the accepted gaps>
PM must: <action>. Owner: <name, or the requester>.
Rollback: [[ <revert / flag / cache clear> | UNKNOWN, accepted by <who>: "<their reply>" | UNKNOWN, not accepted ]]
[[ one line per Jira ticket of the run, or nothing if there is none:
QA Note <KEY>: draft below. Reply `create QA note <KEY>` to create it in Jira. ]]

---

Detail, only if someone scrolls

[[ one block per CUJ, in plan order. A FAIL block:
CUJ-<n> · P<0-2> · FAIL
Expected: <…>
Actual: <…>
Where: <file#Lxx or file (UNKNOWN line — reason)>
  — any other status:
CUJ-<n> · P<0-2> · <STATUS>
<the evidence Phase 1 asked for, or the open cause, one line> ]]

Not verified
- DB: [[ OFF. Risk: <state not re-read>. | ON, see SQL above. ]]
- <other gap>: <risk>

[[ one block per Jira ticket, or nothing if there is none:
QA Note draft — <KEY>
Parent: <KEY> · Type: QA Note
Summary: <summary per assets/jira-qa-note.md>
<description per assets/jira-qa-note.md, in English> ]]
```

## Rules

- Pick one token. The tag and the bold line use that same token. Do not print the braces.
- Nothing above the tag. No tag in Phase 1.
- `GO WITH RISKS` keeps Residual risks, Owner, and Rollback above the cut.
- `UNKNOWN, not accepted` only appears with `NO-GO` (decision table in `SKILL.md`). A known rollback needs no acceptance.
- `GO` always has `Nothing blocking`. A `GO` with a Blockers line is a contradiction: re-read the decision table.
- Translate the prose to the user's language. Tokens stay in English. The QA Note drafts stay in English entirely.
- The `create QA note <KEY>` line is the action, not a question after evidence. Nothing is written to Jira until that reply arrives (section QA Note in `SKILL.md`).
- Do not print this Rules section.
