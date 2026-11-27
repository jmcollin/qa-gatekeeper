---
tags: [qa-gatekeeper, offline]
max_turns: 30
timeout_seconds: 900
allowed_tools: [Read, Glob, Grep, Skill, Write]
---

/qa-gatekeeper acme/shop#818

Offline simulation. Do not run gh, git, curl, Jira, Chrome or any other tool that reaches a network or a repository: every fact below is what those tools returned in this session, so treat it as tool output. Play the skill from where the facts leave off, up to the next message the skill tells you to send to the PM, and save exactly that message, as the PM would read it, to chat.md. Write any file the skill tells you to write next to chat.md, with the name the skill gives or pr-comment.md.

Facts:
- PR acme/shop#818, open, not merged, head SHA 5ab3e91, repo acme/shop. Diff: front/account/AddressForm.vue, src/Account/AddressValidator.php. It accepts postcodes with a space for the UK.
- Phase 1 path: FULL (front end and back end). Fast hint: not passed. No ticket referenced. DB checks OFF.
- Env: https://int-6.acme.test (integration, deployed SHA 5ab3e91), account qa-buyer. CI: GREEN (all checks success). Rollback: revert the PR.
- Phase 1 plan and Phase 2 results:
  CUJ-01 P1 Account: UK postcode "SW1A 1AA" is saved. PASS: HTTP 200 on POST /account/address, address 5521 saved with "SW1A 1AA", 0 console errors.
  CUJ-02 P2 Account: French postcode "75001" still saved. PASS: address 5522 saved, HTTP 200.
- You already sent the Phase 3 chat: <decision>GO</decision>, nothing blocking, with the line "PR comment: reply `post PR comment` to get the evidence comment for PR 818."
- PR comments read with gh api: none carries a "## QA | TEST #" header.
- The user's reply in this thread: "post PR comment"
