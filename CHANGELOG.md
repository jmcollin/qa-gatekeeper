# Changelog

## 2026-11-20

- Jira is optional. The QA Note has a mode per ticket, set at preflight: `JIRA` (Jira connected, ticket in Jira, project has the `QA Note` issue type: draft, then create after approval) or `DRAFT ONLY (<reason>)` (Jira not connected, ticket in another tracker, or no `QA Note` issue type: draft printed under the cut, nothing created). No ticket: no note.
- Jira not connected is `SKIPPED (not connected)`, never `MISSING`, and never blocks the gate. AC come from the PR description or are asked.
- The issue type is checked on the project metadata, never by a trial create. A failed create switches the ticket to `DRAFT ONLY (create failed: <error>)`: no retry with other fields or another issue type.
- Phase 3 template: the QA Note line above the cut has a `DRAFT ONLY` shape (`not created (<reason>)`, no create action).
- `assets/jira-qa-note.md` renamed `assets/qa-note.md`. Ticket keys may come from any tracker.
- Generic skill: no product- or company-specific context. Deployment-context rule replaces the single-shop rule. Payment-probe endpoints, env hosts and surfaces (front office / back office) described generically. QA Note examples use a fictional e-commerce scenario.

## 2026-10-12

- Phase 3 drafts one Jira QA Note per ticket (sub-task `QA Note`, English), shaped like the manual QA comment on PROJ-123: `TEST #n | <decision>`, environment, initial conditions, SUMMARY, then one `CASE #n` block per CUJ (preconditions, steps, expected, actual). Template in `assets/jira-qa-note.md`.
- The note is created only after a reply naming its ticket (`create QA note PROJ-123`). It is the only Jira write. Never a ticket comment.
- QA Note statuses are a display mapping (`PASS`→`OK`, `FAIL`→`KO`, `SKIPPED`→`TO TEST`, `BLOCKED`→`BLOCKED` or `TO DISCUSS`). The chat, trace and decision table keep their tokens.
- New `BLOCKED` cause `spec ambiguous`: AC or tickets contradict each other. Shown as `TO DISCUSS` with the question for the PM. A clear AC not met stays `FAIL`.
- Phase 1 CUJs carry their ticket and an English `<Area>: <behaviour>` title, so they copy into the note as written.
- Examples: `example-good.md` gains a QA Note built from PROJ-123 TEST #1, fixed to the rules (decision in the header, unblocker, PM question, side note). `example-bad.md` shows the raw comment (the QA's proposed format) and lists which parts break the gate rules if copied as-is. The `<Team> |` header prefix is not one of them; the skill just omits it.

## 2026-10-07

- Chat is the decision surface. PM message is at most 25 lines before the cut.
- Box-drawing tables and tables wider than 4 columns are forbidden in the chat.
- Templates are English. Copy the fenced block only. Example facts are fake.
- Status tokens have one source (`SKILL.md`). Cause stays out of the token.
- `SKIPPED (waived)` is an explicit user reply in this thread, recorded under the cut.
- Decision table: most severe row wins. P1 `BLOCKED` is covered. `UNKNOWN` rollback is `NO-GO` until accepted.
- Chrome is required for every planned UI CUJ, P2 included.
- Payment GET probes: integration envs and QA only. Preprod forbidden until PSP and mail are confirmed sandboxed.
- Description shortened. `argument-hint` matches the documented command, without `fast`.
- Decision table: rows for `SKIPPED` P0/P1 (`GO WITH RISKS`). `RED (unrelated)` has a fixed rule, no "if unsure". (Default row later changed to `NO-GO`, see below.)
- Waiving P0 evidence gives `GO WITH RISKS` at best, not `NO-GO`. Acceptance is an explicit reply naming the gap.
- Known rollback needs no acceptance. Only `UNKNOWN` rollback does.
- CI tokens live in `SKILL.md` only.
- Templates: copy the fence content, not the fence. Prose translated, tokens in English. `BLOCKED` CUJs listed under Blockers.
- Tool missing with env known: run what can run, then ask connect-or-waive, no tag.
- `path#Lxx` forbidden above the cut (was "first 25 lines").
- Inputs list QA and point to `.claude/rules/environments.md`.
- Payment probes on integration envs and QA: own test account only.
- Annex lists the Phase 1 invariants and their coverage.
- One Waive rule in `SKILL.md`, used by the gate, the templates, and the example: a waive names what it lifts and lifts only that cause. `go` / `ok` is not a waive. Accepted `BLOCKED` is now `SKIPPED (waived)`.
- `SKIPPED (user-approved)` needs a reply naming the CUJ.
- Decision table fails closed: no matching row is `NO-GO`. Explicit `GO` row. Missing CI token is `NO-GO`. A CUJ with no status blocks Phase 3.
- P1 workaround counts only if run in this session and it worked.
- Blocked template: real status per journey, no "all BLOCKED". Waive option no longer says `NO-GO`.
- Example: waiving Chrome leaves the env-behind P0 `BLOCKED`. Warning not to borrow its scenario.
- Path token goes on the PM header line. Missing `environments.md` means ask for the URL.
- Example: option (a) runs on the deployed SHA without waiving anything; CUJ-01 stays `BLOCKED`, cause `env behind`. The waive is a separate sentence. Picking an execution option is never a waive (rule in `SKILL.md`).
- One CI token per run, with a precedence order. Example no longer prints `CONFLICTING` and "no workflow ran" together.
- Phase 3 template: conditional lines in `[[ ]]` inside the fence (Blockers shapes, `Nothing blocking`, rollback variants), so a `GO` cannot carry an empty `BLOCKED` line or a rollback accepted by nobody.
