---
tags: [qa-gatekeeper, offline]
max_turns: 30
timeout_seconds: 900
allowed_tools: [Read, Glob, Grep, Skill, Write]
---

/qa-gatekeeper acme/shop#817

Offline simulation. Do not run gh, git, curl, Jira, Chrome or any other tool that reaches a network or a repository: every fact below is what those tools returned in this session, so treat it as tool output. Play the skill from where the facts leave off, up to the next message the skill tells you to send to the PM, and save exactly that message, as the PM would read it, to chat.md.

Facts:
- PR acme/shop#817, open, not merged, head SHA 2d9c4e8. Diff (2 files): front/theme/css/product.css, front/theme/css/product-mobile.css. It fixes the product price wrapping on mobile.
- gh pr checks: all checks success. No ticket referenced.
- Env: https://int-4.acme.test (integration). The environments file says integration envs are shut down every night from 20:00 to 08:00 Europe/Paris. P0 account: qa-buyer. Chrome DevTools OK, gh OK.
- Rollback: revert the PR.
- Phase 1 path: FAST (CSS only). Fast hint: not passed.
- Phase 1 plan: CUJ-01 P1 Product page: the price stays on one line at 375 px; CUJ-02 P2 Product page: desktop layout unchanged.
- Phase 2: it is 21:40 Europe/Paris. Opening https://int-4.acme.test/product/12 in Chrome returns a Cloudflare 521 'Web server is down' page. Same for the home page. The deployed SHA cannot be read.
