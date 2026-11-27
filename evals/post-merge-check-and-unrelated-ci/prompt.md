---
tags: [qa-gatekeeper, offline]
max_turns: 30
timeout_seconds: 900
allowed_tools: [Read, Glob, Grep, Skill, Write]
---

/qa-gatekeeper acme/shop#815

Offline simulation. Do not run gh, git, curl, Jira, Chrome or any other tool that reaches a network or a repository: every fact below is what those tools returned in this session, so treat it as tool output. Play the skill from where the facts leave off, up to the next message the skill tells you to send to the PM, and save exactly that message, as the PM would read it, to chat.md.

Facts:
- PR acme/shop#815, open, not merged, head SHA 71e0b3d. Diff: src/Upload/ZipValidator.php, front/upload/UploadForm.vue, tests/Upload/ZipValidatorTest.php. It rejects ZIP files over 50 MB with a clear error.
- No ticket referenced. AC from the PR description: (1) on the integration env, uploading a ZIP over 50 MB shows the error 'File too large (max 50 MB)'; (2) before the prod tag, check the same upload on preprod.
- Deploy config read at main: .github/workflows/cd.yml deploys preprod only on tags v*, never from a PR. The PR does not touch it.
- gh pr checks: build success, unit success, e2e-legacy failure. e2e-legacy only runs tests/legacy/**, which the diff does not touch. On main, e2e-legacy is also failing on the last 5 runs.
- Env: https://int-2.acme.test (integration, deployed SHA 71e0b3d). P0 account: qa-seller. Chrome DevTools OK, gh OK.
- Rollback: revert the PR.
- Phase 1 path: FULL (front end and back end). Fast hint: not passed.
- Phase 1 plan: CUJ-01 P1 Upload: a 60 MB ZIP is rejected with the error on int-2; CUJ-02 P1 Upload: a 10 MB ZIP still uploads on int-2. You classified AC (2) as a post-merge check (evidence: PR not merged, AC says 'before the prod tag' and 'preprod', cd.yml at main builds preprod from tags only).
- Phase 2: CUJ-01 PASS (error shown, HTTP 413, 0 console errors). CUJ-02 PASS (upload 201, file listed).
- Go to Phase 3.
