---
tags: [qa-gatekeeper, offline]
max_turns: 30
timeout_seconds: 900
allowed_tools: [Read, Glob, Grep, Skill, Write]
---

/qa-gatekeeper acme/shop#814 fast

Offline simulation. Do not run gh, git, curl, Jira, Chrome or any other tool that reaches a network or a repository: every fact below is what those tools returned in this session, so treat it as tool output. Play the skill from where the facts leave off, up to the next message the skill tells you to send to the PM, and save exactly that message, as the PM would read it, to chat.md.

Facts:
- PR acme/shop#814, open, not merged, head SHA c03d7aa. Diff (3 files): modules/stripe/src/WebhookHandler.php, modules/stripe/config/services.yml, modules/stripe/tests/WebhookHandlerTest.php. It retries a webhook once when the order lock is busy.
- gh pr checks: all checks success.
- Ticket SHOP-80 referenced. Jira connected, project has the QA Note issue type. AC: a webhook received while the order is locked is processed after one retry, with no duplicate payment record.
- Env: https://int-1.acme.test (integration, deployed SHA c03d7aa). P0 account: qa-buyer. Chrome DevTools OK, gh OK, Jira OK.
- Rollback: revert the PR, no migration.
- Phase 1 plan (all routed to SHOP-80):
  CUJ-01 P0 Payment: webhook during a lock is processed after one retry
  CUJ-02 P0 Payment: no duplicate payment record
  CUJ-03 P1 Checkout: normal card checkout still completes
- Phase 2 results, every one with the evidence Phase 1 asked for:
  CUJ-01 PASS: log shows retry 1 then processed, order 10442 state paid.
  CUJ-02 PASS: payments list for order 10442 has one row.
  CUJ-03 PASS: order 10443 paid, confirmation page, 0 console errors, HTTP 200 on /checkout/confirm.
- Every planned CUJ has a status. Go to Phase 3.
