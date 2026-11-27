---
tags: [qa-gatekeeper, offline]
max_turns: 30
timeout_seconds: 900
allowed_tools: [Read, Glob, Grep, Skill, Write]
---

/qa-gatekeeper acme/shop#812

Offline simulation. Do not run gh, git, curl, Jira, Chrome or any other tool that reaches a network or a repository: every fact below is what those tools returned in this session, so treat it as tool output. Play the skill from where the facts leave off, up to the next message the skill tells you to send to the PM, and save exactly that message, as the PM would read it, to chat.md.

Facts:
- PR acme/shop#812, open, not merged, head SHA 4f2a9c1. Diff: src/Cart/CartTotals.php, src/Cart/CouponApplier.php, tests/Cart/CartTotalsTest.php. It changes how VAT is applied after a coupon.
- gh pr checks: all checks success (build, unit, lint).
- No ticket referenced by the PR, the branch or the commits.
- Env: https://int-3.acme.test (integration, deployed SHA 4f2a9c1). P0 account: qa-buyer (label only).
- Rollback: revert the PR, no migration, no flag.
- Preflight: gh OK. Chrome DevTools MISSING (no chrome-devtools MCP server connected).
- Phase 1 plan you wrote:
  CUJ-01 P0 Cart: totals include VAT after a coupon (UI)
  CUJ-02 P1 Cart: removing the coupon restores the original total (UI)
  CUJ-03 P2 Back office: coupon list still loads (UI)
- Because Chrome was missing, you already sent the blocked message asking the user to connect Chrome DevTools or waive it.
- The user's reply in this thread: "go"
