# Phase 2 — execution

Read this before the first CUJ. Status words and skip rules are in `SKILL.md`. Do not restate them.

## Order

P0 → P1 → P2. Never by page order.

Run every CUJ of the Phase 1 plan. A UI CUJ of any priority needs Chrome DevTools. If it is missing, that CUJ is `BLOCKED`, cause `tool missing`, including P2.

A CUJ that cannot be simulated from the browser: say why, propose another source (unit tests, a code path you can read), and ask before settling on it.

Before the Phase 3 chat, every Phase 1 row has a status from `SKILL.md`. Play any row still open, or ask about it.

## How to run one CUJ

Preconditions → action → collect exactly the evidence Phase 1 listed → assert.
Capture business IDs (order, customer, product, cart).

Signals, not footnotes: JS exceptions, unexpected 4xx/5xx, payload vs UI mismatch, missing email, DB mismatch (if ON).

`BLOCKED`: cause, unblocker, one precise ask, then continue anything not blocked.
Do not pollute the env. No bulk fixtures. No SQL writes. No sending, replying, forwarding, deleting, or labelling mail.

A UI screenshot alone is not a P0 PASS if Phase 1 required network (or DB, when ON).

## Severity

- P0 — critical journey, data loss, security, payment, ship blocker
- P1 — intended feature broken, workaround limited
- P2 — visible, non-blocking
- P3 — cosmetic / debt

## Payment probes

A direct GET on a payment entry, return or validation endpoint (PSP return URL, express-checkout entry, legacy payment controller) can create an order. That can also send order mail. It contradicts the no-mail rule if the env talks to a real PSP or a real inbox.

Allowed without asking, only to prove server-side payment guards:

- Integration environments
- QA

Only with a test account whose email you control. An integration database may be an import of real data: never probe as, or on behalf of, a customer account you did not create or receive for this run.

Record the created order id under data created by the run.

Forbidden, no exception, until someone confirms the PSP is sandboxed and order mail does not reach a real customer:

- Preprod
- Production
- Any env whose type is unknown — treat it as production

## Front-end conflicts

If a conflict is on committed front-end bundles, resolving the Git conflict is not enough. A full front-end rebuild is mandatory before execution.

## Suspect file

Under the cut only: `path#Lxx` or `path (UNKNOWN line — <reason>)`.
