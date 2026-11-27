# Changelog

## 2026-11-27

- New `references/troubleshooting.md`, loaded when a tool errors, an env does not answer, or a result looks stale: CDN "origin down" pages on non-production envs, deployments scaled to zero, NXDOMAIN after a deploy, CDN cache on unversioned assets, translation caches, non-production envs without production assets, the `get_css_styles` crash, SSO-only test accounts. Diagnostics only: dropping a point still goes through `Known decisions applied`, and scaling an env up or purging a cache needs a reply in the thread.
- New `BLOCKED` cause `env down`: the env does not answer (shutdown, scaled to zero). Distinct from `env behind`, where the env answers without the change.
- Frontmatter: `metadata.author` and `metadata.tags`. `disable-model-invocation` and `argument-hint` stay top-level: Claude Code reads them there.
- `evals/`: offline scenario cases for the decision table and the chat surface, in the `claude plugin eval` layout.
- Phase 3 template: the `Not verified` line itself says a post-merge check never goes there (seen once in an eval run despite the rule in `SKILL.md`).
- Phase 3 template: the same guard on the `Residual risks` line, and the `Blockers` heading always prints, `Nothing blocking` included (both seen missing in eval runs).
- Examples are opened only when a template leaves the shape unclear, never by default.
- Evals: cases for creating an approved QA Note in Jira and for updating the PR comment in place; a `Blockers` heading grader.
- Changing an env (scale up, cache purge, redeploy, data fix) needs a reply naming the action and the env (new Forbidden line); troubleshooting points to it.
- Troubleshooting entries diagnose, never decide: a 521 means a shutdown only inside the documented window; a failed second DNS check is `env down`; a recalled cause is measured first.
- `references/troubleshooting.md` also loads when a test login fails or a UI CUJ is about to fail while the env has the PR HEAD.
- QA Note parent check (sub-task or epic → ask) moves back before the draft, so the approved parent is the real one; `qa-note-jira.md` no longer restates the summary rule.
- Lazy loading: the PR comment procedure (approval, update in place, `gh` commands) moves from `SKILL.md` to `assets/pr-comment.md`, loaded when the user asks for the comment or for an update. The Jira fields and HTML body move from `assets/qa-note.md` to `assets/qa-note-jira.md`, loaded only after the reply that approves a note in `JIRA` mode.

## 2026-11-26

- A ticket gets a QA Note draft only when at least one of its CUJs is not `PASS` (`FAIL`, `BLOCKED` or `SKIPPED`). All `PASS`: no draft and no line in the chat, in every mode. The draft still lists every CUJ of the ticket, `PASS` ones included.
- PR comment: when the target is a GitHub PR, Phase 3 offers an evidence comment (`post PR comment`), whatever the QA Note mode. Template in `assets/pr-comment.md`: `## QA | TEST #n | <decision>`, one-line TL;DR and environment, setup under `Setup ↴`, Summary, one `CASE #n` per CUJ with preconditions, steps, expected and actual (OK cases collapsed), `Not verified` only if a gap remains, `QA run:` marker. Drafted after a reply asking for it, posted after approval of the draft. Later in the same thread, on the same SHA, the same comment is edited (`PATCH`, same `TEST #n`, header, setup, summary and `QA run:` refreshed); a retest shows the changed lines before the edit. A new SHA under test is a new comment.
- Post-merge checks: an AC step that targets an env deployed only from the base branch or a release tag (preprod, production, "before the prod tag") is no longer a CUJ, so it can no longer force `SKIPPED` and `GO WITH RISKS`. Classified in Phase 1 only, on three conditions written in the trace: PR not merged, AC names the env or moment, deploy config read at the base branch proves it. Its behaviour keeps a pre-merge CUJ on an integration env (same steps, at least P1), whose status drives the decision. Never run by the skill, never under `Not verified`. Listed on an `After merge` line in the chat and the blocked message, in the PR comment, and in the QA Note when the ticket gets one (HTML Jira body included). A result, a user reply or an env that is behind never moves a step between CUJ and post-merge check.

## 2026-11-20

- QA Note matches the team's manual QA comment layout. One content, two renderings: the markdown draft printed under the cut (what the user approves) and, in `JIRA` mode, an HTML Jira body (`contentFormat: html`) with info panels for environment, initial conditions and summary, a note panel for TESTS, purple `CASE #n` labels, coloured statuses, an `ACTUAL RESULT` panel per status (success / error / note / warning) and OK cases collapsed under `DETAILS ↴`. The Jira body adds markup, never content.
- Statuses stay `OK`, `KO`, `BLOCKED`, `TO TEST`, `TO DISCUSS` (no `NOK`); `TO TEST` stays reserved for user-approved skips, a case not run for lack of data is `BLOCKED`.
- Draft gains a `BRANCH NAME` line and a `LOCAL 🖥️` environment type. `None` is no longer an allowed initial-conditions value.
- KO result condensed: `Severity: P<n>. Reproduced <n/m>.` and `Suspect: <file> (<hypothesis>)`; URL and evidence go in the observation.
- Optional header prefix `<PROJECT LABEL> | `, only when the user names it in the thread.
- Fix: `createJiraIssue` takes `issueType`, not `issueTypeName`.

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
