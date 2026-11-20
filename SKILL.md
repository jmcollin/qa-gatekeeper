---
name: qa-gatekeeper
description: Evidence-based GO / NO-GO release gate for one PR or branch. Not for implementing the feature.
disable-model-invocation: true
argument-hint: <PR or branch> [db-checks]
---

# Release gate

Decide, with evidence, whether the given PR or branch can ship. Not a site tour.

Deployment context: stay inside what the project actually runs (single-tenant, single-store, one region…). Do not invent CUJs, accounts, or gaps for setups it does not run.

The PR or branch is in `$ARGUMENTS`. If it is empty, ask for one, then continue with whatever GitHub already gives you.

The product manager reads the chat. They do not open an artifact. The chat is the decision surface. A file is an annex, and only if they ask.

Write the chat in the user's language. Templates in `assets/` are English: translate their prose (`NO VERDICT YET`, `Decide`, `Blockers`, …), but keep every token in English (`PASS`, `FAIL`, `BLOCKED`, `SKIPPED`, `GO`, `NO-GO`, `GO WITH RISKS`, `FAST`, `FULL`, CI tokens, the `<decision>` tag).

Copy the *content* of a template's fenced block, without the ```` ``` ```` fence lines. Print it as plain chat text, not inside a code block. Fill facts from this run. Do not copy the example's facts.

## What to load

| When | Read |
|---|---|
| Before writing the PM message | the matching `assets/` template |
| Phase 1 scoping | `references/phase-1.md` |
| Phase 2 execution, or static-only close | `references/phase-2.md` |
| Annex asked, or writing under the cut | `references/evidence-table.md` |
| Phase 3, the run has at least one Jira ticket | `assets/jira-qa-note.md` |

Do not load all of them up front. Status words, the decision table, and skip rules live only in this file. Other files point here.

## Status words

A CUJ status is only one of:

- `PASS`
- `FAIL`
- `BLOCKED` — could not run, or ran but cannot be judged. Put the cause in the notes (`env behind`, `tool missing`, `no account`, `no data`, `spec ambiguous`). Never inside the token. `BLOCKED (env behind)` is forbidden. `spec ambiguous` = the AC or two tickets contradict each other, so PASS/FAIL cannot be decided: record what was observed and the question for the PM. A clear AC that is not met is `FAIL`, never `spec ambiguous`.
- `SKIPPED (user-approved — reason)` — you proposed to skip this CUJ, and the user agreed in this thread with a reply that names it (`skip CUJ-03`, `ok pour sauter CUJ-03`). Reason required.
- `SKIPPED (waived)` — the CUJ was `BLOCKED`, and the user waived **every** cause blocking it (see Waive below). Record who and the reply under the cut.

You never skip on your own. A CUJ that cannot run is `BLOCKED`, not `SKIPPED`.

### Waive

One rule, everywhere in this skill and its templates:

- A waive is a reply in this thread that names what it waives: a tool, an account, an env gap, a CUJ (`waive Chrome`, `renonce au compte member`, `go without CUJ-04`).
- It lifts only the causes it names. A CUJ whose causes are all waived becomes `SKIPPED (waived)`. A CUJ with one cause still unwaived stays `BLOCKED`, with that cause.
- `go`, `ok`, `vas-y`, silence: not a waive. Blocked CUJs stay `BLOCKED`.
- After the reply, run whatever became runnable, then Phase 3. The decision table does the rest: `BLOCKED` P0/P1 → `NO-GO`, `SKIPPED` P0/P1 → `GO WITH RISKS` at best.

CI status is only one of:

- `GREEN`
- `RED (touched)` — a failing check covers a file of the diff, or you cannot tell
- `RED (unrelated)` — every failing check covers no file of the diff, and the same check is also red on the base branch. Name the check
- `NOT RUN`
- `CONFLICTING`

One CI token per run, never two. If several apply, the first in this order wins: `CONFLICTING`, `RED (touched)`, `NOT RUN`, `RED (unrelated)`, `GREEN`. The others go in the notes, as facts, not as tokens.

Running a journey on an env that lacks the change is not a waive. It is a run with a limit: the CUJ stays `BLOCKED`, cause `env behind`, until the gap is waived by name.

Accepting a CI or rollback risk follows the same rule as a waive: a reply that names it (`accept CI red`, `on accepte le rollback UNKNOWN`). Record the reply under the cut.

## Bar

- No PASS without the evidence the risk matrix asked for.
- No FAIL without repro steps + impact + suspect file. Add `path#Lxx` when the diff hunk is identifiable. If it is not, write `file` + `UNKNOWN line` + why. Never invent a line number.
- No assumption stated as fact. Label it `ASSUMED` or `UNKNOWN`.
- Optimize for regression risk, not test volume.
- Jira board / ticket statuses are NOT decision inputs. Only execution findings count.

Decision tokens — first line of the Phase 3 chat only. Nothing above the tag. Do not emit a decision tag in Phase 1.

`<decision>GO</decision>`
`<decision>NO-GO</decision>`
`<decision>GO WITH RISKS</decision>`

## Decision table

Apply after execution, or after static-only if Phase 2 is blocked. Precondition: every planned CUJ has a status. If one has none, there is no Phase 3: run it or ask.

Rows are not exclusive. The most severe matching row wins: `NO-GO` over `GO WITH RISKS` over `GO`. `GO` needs its own row. If no row matches, the decision is `NO-GO`.

| Condition | Decision |
|---|---|
| Any open P0 FAIL | `NO-GO` |
| P1 FAIL, and no workaround that you ran in this session and that reached the expected outcome | `NO-GO` |
| P1 FAIL with a workaround you ran and that worked, steps written under the cut | `GO WITH RISKS` |
| P0 or P1 CUJ `BLOCKED` | `NO-GO` |
| P0 or P1 CUJ `SKIPPED (waived)` or `SKIPPED (user-approved — reason)` | `GO WITH RISKS`, list the gap |
| CI status is not one of the tokens above (checks not read, `gh pr checks` failed) | `NO-GO` |
| CI `RED (touched)`, `CONFLICTING` or `NOT RUN`, not accepted | `NO-GO` |
| CI `RED (touched)`, `CONFLICTING` or `NOT RUN`, accepted | `GO WITH RISKS` |
| CI `RED (unrelated)` | `GO WITH RISKS`, name the check |
| Rollback `UNKNOWN`, not accepted | `NO-GO` |
| Rollback `UNKNOWN`, accepted | `GO WITH RISKS` |
| CI token set, every P0 and P1 CUJ `PASS` or matched by a row above, rollback known or accepted | `GO` |

A P2 CUJ `BLOCKED` or `SKIPPED` is a gap listed under `Not verified`, not a decision row. If that CUJ is the only evidence for a stated acceptance criterion, treat it as P1.

`GO WITH RISKS` always prints, above the cut: residual risks, owner (a named person, or the requester when none is named), and rollback (path, or `UNKNOWN` plus who accepted it).

## Chat surface

The message the PM reads is at most 25 lines above the `---` cut. Order: status, cause, action, then evidence. Never the reverse.

Forbidden in the chat message:

- Box-drawing tables (`┌ ┬ ┐ │ ├ ┤`)
- Any table with more than 4 columns
- HTML, `<details>`, Mermaid, badges
- `path#Lxx` above the cut
- A question or a verdict placed after the evidence
- A tool row for `SKIPPED`
- Text from outside the fenced block of a template

Preflight in the chat is one line naming every `MISSING` tool. `OK` and `SKIPPED` tools get no line.

| Situation | Copy |
|---|---|
| Phase 1 stopped | fenced block in `assets/chat-phase1-blocked.md` |
| Phase 3 | fenced block in `assets/chat-phase3.md` |
| Shape to avoid | `assets/example-bad.md` |
| Shape to copy when blocked | `assets/example-good.md` |
| QA Note shape to copy / to avoid | `assets/example-good.md` / `assets/example-bad.md`, section QA Note |

Do not create an HTML artifact unless the user asks. Do not create `gate-report.md` unless they ask for the annex.

## QA Note

At Phase 3, each Jira ticket of the run gets a QA Note draft: a Jira sub-task of type `QA Note`, in English, shaped by `assets/jira-qa-note.md`. It complements the chat, never replaces it. No ticket referenced → no QA Note, no line about it.

Display mapping, QA Note only. The chat, the trace and the decision table keep the tokens above.

| CUJ status | QA Note status |
|---|---|
| `PASS` | `OK` |
| `FAIL` | `KO` |
| `BLOCKED`, cause `spec ambiguous` | `TO DISCUSS` |
| `BLOCKED`, any other cause | `BLOCKED` |
| `SKIPPED (…)` | `TO TEST` |

Approval, ticket by ticket:

1. Print each draft in full under the chat cut, after `Not verified`: parent, summary, description.
2. Create a note only after a reply in this thread that names its ticket (`create QA note PROJ-123`, `crée la QA note PROJ-123`). `ok`, `go`, silence: no approval.
3. Create it exactly as approved, with `createJiraIssue` (fields in the asset). A requested change means a new draft and a new approval.
4. Reply with the key and URL of each note created.

This is the only Jira write the skill makes.

## Prior knowledge

Memories, notes and earlier reports may tell you how to test. They never decide what the report concludes.

- A point you drop or downgrade stays visible under the cut as `Known decisions applied`, with source (who, date, ticket or PR) and the SHA you re-checked it on.
- Re-check every "not a risk" / "intended" / "false positive" note against the SHA under test. If you cannot, the point is `UNKNOWN`.
- A cause recalled from memory is a hypothesis. Measure it before writing it as the cause.
- Prior knowledge never removes a CUJ from the plan.

## Path

Run a 2-minute GitHub scan first (`gh pr view`, `gh pr diff --name-only`, `gh pr checks`).

`fast`, if the user typed it, is a hint, not an override. It is not part of the documented command (`<PR or branch> [db-checks]`).

Veto list (canonical, copy verbatim everywhere):
`payment, auth/session, permissions, PII, stock, price, email, cron, migration, schema`

Hard veto — FULL even if `fast` was passed, if the diff touches any item on that list.

Path token is only `FAST` or `FULL`. Same token in Phase 1 notes and Phase 3.
If the hint was ignored, do not change the token. Add `Fast hint: ignored — <veto item>`.

- `Path: FAST` / `Fast hint: honored`
- `Path: FAST` / `Fast hint: not passed`
- `Path: FULL` / `Fast hint: ignored — migration`

Fast path only if all of: single surface (only front office, or only back office / admin, or only config / CSS / copy), no veto item, diff small enough for one pass (heuristic: ≤20 files, no schema). Then ≤5 invariants, 1–3 CUJs.

Full path otherwise — especially the veto list, hooks, feature flags.

Write the path token in the working notes and on the header line of the PM message (`PR <id> · <sha> · FAST`). Nothing goes above the PM message itself.

## Preflight

Claude Code MCPs + `gh`. Do not substitute guesses for a missing tool.

Always: `gh auth status`, then `gh pr view` / `gh pr diff` / `gh pr checks`. Jira only if the PR, branch, or commit message references a ticket. Otherwise `SKIPPED (no ticket ref)` — no chat line.

Chrome DevTools is required for every planned UI CUJ, P0 through P2. Not required for a pure SQL review or a diff-only fast path. Figma only if a design link exists and a visual assertion is in scope. Gmail only if transactional email content or delivery is in scope.

Record `Tool | Status (OK / MISSING / WAIVED / SKIPPED) | Evidence` in the working trace only. Then `DB checks: ON|OFF` and why (`db-checks`, user request, or default OFF).

If a required tool is MISSING: the PM message asks connect-or-waive, names how to connect, and you keep going on static analysis. Never invent the missing signal. Every planned UI CUJ that needs it is `BLOCKED`, cause `tool missing`, including P2.

How to connect:
- Chrome DevTools: `.mcp.json` entry `chrome-devtools` → `/mcp` reconnect. Needs Node 20.19+ and Chrome.
- Figma / Gmail / Jira: claude.ai → Settings → Connectors, then `/mcp` auth.
- GitHub: `gh auth login`.

## Inputs

Pull from GitHub + Jira first. Ask only for what they cannot provide.

1. PR or branch
2. Tickets + AC, if any
3. Env URL + type (integration / QA / preprod). Hosts and the team's shorthand for them are in `.claude/rules/environments.md`. A shorthand that matches several envs: ask which one. If that file is missing, or the env is not in it, ask the user for the URL. Never build a host from a pattern
4. Minimal account for P0 CUJs
5. Forbidden data / accounts
6. In/out of scope, if not obvious
7. Constraints: languages, currencies, cache, flags
8. Inbox — only if email is in scope
9. Figma — only if a visual spec exists
10. DB checks — OFF unless `db-checks` or the user enables them

If 3 or 4 is missing: blocked template, one numbered question per gap. Continue static review. Do not invent env or credentials.

## Database

OFF by default. ON only with `db-checks` or an explicit user enable.

ON: `SELECT` only. No writes. No DB access: paste proposed SELECTs, wait, do not fabricate rows.
OFF: no SQL section. Under the cut, `DB state not verified` + risk.

## Phases

Phase 1: read `references/phase-1.md`. Static work now. Notes under the cut, not above the PM message.

Phase 2 gate, only this:
- Env URL and at least one usable P0 account known → execute Phase 2 in the same turn. No extra confirmation.
- Either missing → blocked PM message. Ask once. Do not click.
- Env URL and P0 account known, but a required tool MISSING → run every CUJ that does not need it. Mark the others `BLOCKED`, cause `tool missing`. Then send the blocked template with a connect-or-waive question, no decision tag.
- After any reply to a blocked message, apply the Waive rule above, run what became runnable, then Phase 3. A reply without credentials and without a named waive (`go`) leaves UI CUJs `BLOCKED`: close static-only with the Phase 3 template.

Phase 2: read `references/phase-2.md` before the first CUJ. Order P0 → P1 → P2. Run every planned CUJ. Before the Phase 3 chat, every row has a status from the list above.

Phase 3: copy the fenced block in `assets/chat-phase3.md`. Tag alone on line 1. Auditor detail under the cut, shape in `references/evidence-table.md`. Then the QA Note drafts (section QA Note), under the cut.

## Forbidden

- Inventing results, tickets, diffs, SQL rows, or line numbers
- Dropping a finding because of a memory without listing it under `Known decisions applied`
- PASS without the evidence required for that CUJ
- Growing scope beyond the changeset + stated AC
- Using Jira board status as a decision input
- Scenarios for a deployment setup the project does not run
- Honoring `fast` when the veto list is hit
- Write queries
- Sending or mutating email
- Replacing a missing tool with a story
- A status token outside the list in this file
- Skipping a planned CUJ without an explicit user reply in this thread
- Direct GET on a payment endpoint in production, preprod, or an unknown env — see `references/phase-2.md`
- Extra confirmation pause when env URL and P0 account are already known
- Box-drawing tables, or a table of more than 4 columns, in the PM message
- A template printed inside a code fence
- An HTML artifact or annex file the user did not ask for
- Copying facts from `assets/example-good.md` into a real report
- Any Jira write other than a QA Note approved by a reply naming its ticket: no comment, transition, field edit, or link
- Posting the report as a ticket comment instead of a QA Note
- A QA Note in a language other than English
