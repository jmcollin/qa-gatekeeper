# PR comment — the evidence of the run, posted on the PR, always in English

Everything outside the fence is an instruction. The PR comment is the evidence of the run for the developers and reviewers of the PR. It complements the Phase 3 chat; it never replaces it.

## Approval and updates

1. Draft it only after a reply that asks for it (`post PR comment`, `fais le commentaire sur la PR`).
2. Show the draft: one line per section saying what it holds, and the full path of the file. Wait for a reply approving that draft. `ok` counts here, because the draft was just shown: this is an approval, not a waive, so the Waive rule of `SKILL.md` does not apply. Silence does not count.
3. Post it exactly as approved: `gh pr comment <id> -R <owner>/<repo> --body-file <file>`. Reply with the comment URL.
4. Write the comment ID, its `<n>` and the SHA under test in the working trace.
5. Later in the same thread, on the same SHA under test (a retest, a `Not verified` gap closed, a wording change asked), edit that comment, never post a second one. Keep its `<n>`. Refresh with this run: header token, TL;DR, Setup build proof, Summary, the cases that changed, `After merge`, `Not verified`, and the `QA run:` date. Start from the file; if it is gone, read the body back with `gh api repos/<owner>/<repo>/issues/comments/<comment id> --jq .body`.
   - A reply that asks for the edit (`update the PR comment`, `mets à jour le commentaire`) approves it: apply it, then say in the chat what changed.
   - A retest request alone does not: after the retest, show the changed lines and wait for approval.
   - Apply it with `gh api -X PATCH repos/<owner>/<repo>/issues/comments/<comment id> -F body=@<file>`.
6. New SHA under test (a fix pushed after the comment): it is a new run. New comment, `TEST #<n+1>`, through steps 1 to 3.
7. Comment ID lost: find the comment by its header and SHA in the PR comments (`gh api --paginate`). Comment deleted: ask before posting a new one.

## Before drafting

- `<n>`: read every PR comment (`gh api --paginate repos/<owner>/<repo>/issues/<id>/comments`). `<n>` = highest `## QA | TEST #k` found + 1, or 1 if none. An edit keeps the `<n>` of the comment it edits.
- Cases: every CUJ of the run, in plan order, renumbered `CASE #1`, `CASE #2`, … Statuses use the QA Note display mapping in `SKILL.md` (`OK`, `KO`, `BLOCKED`, `TO TEST`, `TO DISCUSS`).
- Write the draft to a file in the scratchpad. It is the `--body-file` of the post and of every later update.

## Body

Copy the content of the fence, without the fence lines. `[[ … ]]` is a choice: keep what applies, drop the brackets. Replace every `< >`.

```markdown
## QA | TEST #<n> | <GO|NO-GO|GO WITH RISKS>

**TL;DR**: <one line: the result against the baseline, then `<k> cases, <x> OK` plus the other statuses that occur>

**Testing environment**: <environment, one line>[[, compared with <baseline> ]]

<details>
<summary>Setup ↴</summary>

- <environment>: <URL>, <proof of the build under test, read on the environment: image tag, SHA, version string>
[[ - <baseline>: <URL>, <version> ]]
- <accounts by role label, never credentials>
- CI: <CI token from `SKILL.md`, one fact>
- DB: <checked (SELECT only) | not checked>

</details>

### 📊 Summary

**CASE #1 | <status>** | <Area>: <behaviour checked>
**CASE #2 | <status>** | <Area>: <behaviour checked>

### 📝 Tests

[[ OK case:
**CASE #<k> | OK | <Area>: <behaviour checked>**

<details>
<summary>Details ↴</summary>

**Preconditions**
- <data / account role / deploy state>

**Steps**
1. <step; step 1 carries the environment URL>
2. <step>

**Expected**
- <expected>

**Actual**
- <measured values with business IDs; a table when two environments or versions are compared, 4 columns max>
[[ - Side note (out of scope): <finding outside the diff, and why it is out of scope> ]]

</details>
  — any other status, not collapsed:
**CASE #<k> | <KO|BLOCKED|TO TEST|TO DISCUSS> | <Area>: <behaviour checked>**

**Preconditions** / **Steps** / **Expected**: as above.

**Actual**
- <observation>
[[ KO: - Severity: P<n>. Reproduced <n/m>. Suspect: `<path#Lxx>` or `<path>` (UNKNOWN line, <why>) ]]
[[ BLOCKED: - Unblocker: <data, access, deploy> ]]
[[ TO DISCUSS: - Question: <the decision the PM must take> ]] ]]

---

[[ only when the run has post-merge checks:
### ⏭️ After merge

**<Area>: <behaviour checked>** on <env URL>. Owner: <name, or the requester>
1. <step>
2. <step>

Expected: <expected> ]]

[[ only if a gap remains:
### ❌ Not verified

- <gap>: <why, and the risk> ]]

<sub>QA run: <YYYY-MM-DD> on [<short sha>](https://github.com/<owner>/<repo>/commit/<full sha>)</sub>
```

## Rules

- English only, whatever the chat language.
- Header token = the `<decision>` token of the Phase 3 chat.
- TL;DR and Testing environment: one line each. Everything else about the setup goes in `Setup ↴`.
- `OK` cases are collapsed under `Details ↴`. Other statuses stay open.
- After merge items are not cases: not in the TL;DR count, not in Summary.
- GitHub renders Markdown inside `<details>` only with a blank line after `</summary>` and before `</details>`.
- Facts only from this run, each tied to the SHA in the run marker. A check made in an earlier session or by someone else is cited as such, never as a result of this run.
- No credentials, tokens or session parameters: strip `_token=`, `token=` and the like from every URL.
- No adjectives, no em-dashes, no sentence that restates the diff. Never mention the assistant.
- The run marker is `QA run:`, distinct from any marker another tool writes in the PR description.
- Do not print this Rules section.
