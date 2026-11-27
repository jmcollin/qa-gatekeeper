---
tags: [qa-gatekeeper, offline]
max_turns: 30
timeout_seconds: 900
allowed_tools: [Read, Glob, Grep, Skill, Write]
---

/qa-gatekeeper acme/shop#813

Offline simulation. Do not run gh, git, curl, Jira, Chrome or any other tool that reaches a network or a repository: every fact below is what those tools returned in this session, so treat it as tool output. Play the skill from where the facts leave off, up to the next message the skill tells you to send to the PM, and save exactly that message, as the PM would read it, to chat.md. Instead of calling createJiraIssue, write the exact arguments you would pass to it, as JSON, to create-call.json next to chat.md. The call returns key SHOP-101 and URL https://acme.atlassian.net/browse/SHOP-101.

Facts:
- PR acme/shop#813, head SHA 9b17e05, branch feat/coupon-trim. Ticket SHOP-77 in Jira (project SHOP), Jira connected, the project has the issue type QA Note. JQL parent = SHOP-77 AND issuetype = "QA Note" returned no issue. QA Note mode: SHOP-77 JIRA.
- Env: https://int-2.acme.test (integration int-2), account qa-buyer. Path FULL. DB checks OFF.
- You already sent the Phase 3 chat: <decision>GO WITH RISKS</decision>, with the line "QA Note SHOP-77: draft below. Reply `create QA note SHOP-77` to create it in Jira." The draft under the cut was:
  Summary: QA note SHOP-77 - TEST #1: coupon API trim OK, cart box trim to test
  CASE #1 | OK | API: coupon code with surrounding spaces accepted. Step 1: POST https://int-2.acme.test/api/cart/coupon with "  SAVE10 " as qa-buyer. Expected: code accepted. Actual: HTTP 200, coupon.code "SAVE10", discount 10.00.
  CASE #2 | TO TEST | Cart: coupon box trims the pasted code. Step 1: on https://int-2.acme.test/cart as qa-buyer, paste " SAVE10 " in the coupon box and apply. Expected: trimmed code sent, coupon applied. Actual: empty.
- The user's reply in this thread: "create QA note SHOP-77"
