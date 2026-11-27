---
tags: [qa-gatekeeper, offline]
max_turns: 30
timeout_seconds: 900
allowed_tools: [Read, Glob, Grep, Skill, Write]
---

/qa-gatekeeper acme/shop#813

Offline simulation. Do not run gh, git, curl, Jira, Chrome or any other tool that reaches a network or a repository: every fact below is what those tools returned in this session, so treat it as tool output. Play the skill from where the facts leave off, up to the next message the skill tells you to send to the PM, and save exactly that message, as the PM would read it, to chat.md.

Facts:
- PR acme/shop#813, open, not merged, head SHA 9b17e05. Diff: src/Api/CouponController.php, src/Cart/CouponApplier.php, front/cart/CouponBox.vue. It lets a coupon code be pasted with surrounding spaces.
- gh pr checks: all checks success.
- Ticket SHOP-77 referenced in the PR title. Jira connected, SHOP-77 is in Jira, its project has the issue type QA Note. AC: (1) a code with leading or trailing spaces is accepted by the API, (2) the cart coupon box trims the code before sending it.
- Env: https://int-2.acme.test (integration, deployed SHA 9b17e05). P0 account: qa-buyer.
- Rollback: revert the PR, no migration.
- Preflight: gh OK, Jira OK, Chrome DevTools MISSING.
- Phase 1 path: FULL (price is on the veto list). Fast hint: not passed.
- Phase 1 plan you wrote (all routed to SHOP-77):
  CUJ-01 P0 API: POST /api/cart/coupon accepts "  SAVE10 " (API, no browser needed)
  CUJ-02 P1 Cart: coupon box trims the pasted code (UI)
  CUJ-03 P2 Cart: an invalid code still shows the error message (UI)
- Phase 2 so far: CUJ-01 run with curl as qa-buyer: HTTP 200, response JSON coupon.code = "SAVE10", discount 10.00. CUJ-02 and CUJ-03 BLOCKED, cause tool missing.
- You sent the blocked message asking to connect Chrome DevTools or waive it.
- The user's reply in this thread (from Marie, the requester): "waive Chrome, I can't install it today"
