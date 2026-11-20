# Phase 1 — scoping

Read this when starting the static review. Status words, skip rules, and the Phase 2 gate are in `SKILL.md`. Do not restate them.

Working notes go under the chat cut, or in the trace. The PM message is the fenced block in `assets/chat-phase1-blocked.md` when blocked, otherwise a one-line progress note, then Phase 2.

## 1. Changeset

- Files by surface: front office, back office / admin, API, SQL, config, emails, cron, permissions
- Migrations: purpose, tables, idempotency, existing-data risk, rollback
- Side effects: cache, indexes, framework hooks / events, SSO, CDN, flags, emails
- CI: one CI token from `SKILL.md`. For `RED (unrelated)`, check the failing check on the base branch too
- SHA check: verify if the env SHA contains the PR HEAD. Touched files not deployed → those CUJs are `BLOCKED`, cause `env behind`. Do not put the cause inside the status token.

In the PM message, CI and the SHA gap are one or two sentences. The file list stays under the cut.

## 2. Invariants and risks

Fast: 3–5 rows. Full: 5–12 rows.

Working-trace columns only. Do not render this grid in the chat:

`Surface | Invariant / risk | P0–P2 | Required evidence | Blast radius`

Blast radius examples: one language vs all, customer group, flag off by default, front office vs back office.

Under the cut, one bullet, not a 5-column table:

`- P0 · SSO login · Owner, Member, and orphan land on my-account with no loop. Evidence: redirect chain, 0 console errors. Blast: all customers.`

## 3. CUJs

Only journeys the diff can break or the ticket claims.

Working trace, not the chat grid:

`ID | ticket | title | actor+role | preconditions | steps | expected | P0–P2 + why (file, + #Lxx if known) | evidence to collect`

- `ticket`: the Jira key the CUJ covers, or `none`. It routes the CUJ to that ticket's QA Note (`assets/jira-qa-note.md`).
- `title`: `<Area>: <behaviour checked>`, in English, e.g. `PDP Delivery & returns: Product with Free returns = NO shows no returns badge`.
- Preconditions, steps and expected: in English too, numbered steps, env URL in step 1. They are copied into the QA Note as written.
- Cover the AC first, then regressions, then out-of-scope guards (`the X is not displayed elsewhere`) when the AC fences the scope.

Evidence is specific: `HTTP 200 + cart JSON has X`, `no console error on checkout`, `mail subject/body/link`, `SELECT order state`. Do not demand console+network+UI on a non-UI CUJ.

Under the cut, one line:

`- CUJ-01 · P0 · owner account · SSO login, no loop, external return_to neutralized. Evidence: network chain, console.`

## 4. Access

Minimum roles for P0/P1 only. What each account must and must not do.
Name the single account that unblocks P0s if one is enough.
Do not print credentials. Account labels are enough.

## 5. SQL plan

Only if DB checks ON: objective, tables, assertion, proposed `SELECT`.
OFF: one line under the cut, `DB: not verified`.

## 6. Rollback

Full path: how this reverts — flag, revert PR, cache clear, migration down or not. If unknown, say `UNKNOWN`.
Fast path: one line.
This feeds the decision table in `SKILL.md`. If Phase 1 is blocked, rollback is one line under the cut.
