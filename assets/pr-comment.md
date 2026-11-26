# PR comment — the evidence of the run, posted on the PR, always in English

Everything outside the fence is an instruction. The PR comment is the evidence of the run for the developers and reviewers of the PR. It complements the Phase 3 chat; it never replaces it. When it is drafted, the approval rule and the update rule are in `SKILL.md` (section PR comment). Do not restate them.

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
- GitHub renders Markdown inside `<details>` only with a blank line after `</summary>` and before `</details>`.
- Facts only from this run, each tied to the SHA in the run marker. A check made in an earlier session or by someone else is cited as such, never as a result of this run.
- No credentials, tokens or session parameters: strip `_token=`, `token=` and the like from every URL.
- No adjectives, no em-dashes, no sentence that restates the diff. Never mention the assistant.
- The run marker is `QA run:`, distinct from any marker another tool writes in the PR description.
- Do not print this Rules section.
