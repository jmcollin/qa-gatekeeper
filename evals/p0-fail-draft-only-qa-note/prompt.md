---
tags: [qa-gatekeeper, offline]
max_turns: 30
timeout_seconds: 900
allowed_tools: [Read, Glob, Grep, Skill, Write]
---

/qa-gatekeeper acme/shop#816

Offline simulation. Do not run gh, git, curl, Jira, Chrome or any other tool that reaches a network or a repository: every fact below is what those tools returned in this session, so treat it as tool output. Play the skill from where the facts leave off, up to the next message the skill tells you to send to the PM, and save exactly that message, as the PM would read it, to chat.md.

Facts:
- PR acme/shop#816, open, not merged, head SHA e88f210. Diff: src/Checkout/ConfirmController.php (hunk lines 42-58 changed), src/Checkout/GuestCustomerFactory.php. It lets a guest confirm an order without creating an account.
- gh pr checks: all checks success.
- Ticket SHOP-90 referenced. SHOP-90 lives in Linear, not in Jira. Jira is not connected.
- Env: https://int-5.acme.test (integration, deployed SHA e88f210). Guest checkout needs no account; P0 account qa-buyer exists for CUJ-02. Chrome DevTools OK, gh OK.
- Rollback: revert the PR.
- Phase 1 path: FULL (checkout and payment). Fast hint: not passed.
- Phase 1 plan (all routed to SHOP-90):
  CUJ-01 P0 Checkout: a guest can confirm and pay an order
  CUJ-02 P1 Checkout: a logged-in customer still confirms an order
- Phase 2:
  CUJ-01 FAIL: as a guest, cart with 1 product, fill address, click Confirm. POST /checkout/confirm returns HTTP 500, console 'TypeError: customer is null', no order created. Reproduced 3 times out of 3.
  CUJ-02 PASS: order 20311 created, HTTP 200, 0 console errors.
- Go to Phase 3.
