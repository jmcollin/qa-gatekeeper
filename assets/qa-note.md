# QA Note — one per ticket with a CUJ not PASS, always in English

Everything outside the fences is an instruction. The QA Note is the QA report attached to one ticket. In `JIRA` mode it is created as a Jira sub-task of type `QA Note` under the ticket; in `DRAFT ONLY` mode it is only printed, for the user to copy into their tracker. It complements the Phase 3 chat; it never replaces it. The modes, the approval rule and the status mapping are in `SKILL.md` (section QA Note). Do not restate them. A filled draft (PROJ-123) is in `assets/example-good.md`, section QA Note; the shape to avoid is in `assets/example-bad.md`, section QA Note.

The note has one content and two renderings:

- **Draft** (fence 1, markdown): what is printed under the chat cut, in every mode. This is what the user approves.
- **Jira body** (fence 2, HTML): `JIRA` mode only. The approved draft, rendered with Jira panels, colours and collapsed OK cases. Same words, same order, same cases: rendering adds markup, never content.

## Before drafting

- `<n>`: in `JIRA` mode, read the ticket's existing QA Notes (JQL `parent = <KEY> AND issuetype = "QA Note"`); `<n>` = highest `TEST #k` found in their summaries + 1, or 1 if none carries one. In `DRAFT ONLY` mode, use 1 unless the user gave a number, and say so in the line above the cut.
- Cases: the CUJs that cover this ticket, in plan order, renumbered `CASE #1`, `CASE #2`, … for this note. A CUJ covering two tickets appears in both notes.
- Header prefix: `<PROJECT LABEL> | ` before `TEST #<n>` only when the user named a project label for the note in this thread (e.g. `SCV PROJECT`). Otherwise no prefix. Never invent one from the epic or the repo name.

## Jira fields (`JIRA` mode only)

In `DRAFT ONLY` mode, print only the parent, the summary below, and the draft.

`createJiraIssue` arguments:

- `projectKey`: the ticket's project
- `issueType`: `QA Note`
- `parent`: the ticket key. If the ticket is itself a sub-task or an epic, ask which issue to attach it to
- `summary`: `QA note <KEY> - TEST #<n>: <one-line outcome>`, e.g. `QA note PROJ-123 - TEST #1: Free returns badge OK, badge ordering blocked, bundle scope to discuss`
- `contentFormat`: `html`
- `description`: the Jira body (fence 2), filled from the approved draft
- Nothing else: no assignee, labels, priority, transition

## Draft

Copy the content of the fence, without the fence lines. `[[ … ]]` is a choice: keep what applies, drop the brackets. Replace every `< >`.

```markdown
## **[[ <PROJECT LABEL> | ]]TEST #<n> | <GO|NO-GO|GO WITH RISKS>**

### **TESTING ENVIRONMENT: <INTEGRATION (<env name>) 🌐|QA 🌐|PREPROD 🌐|LOCAL 🖥️>**

<environment URL>

**BRANCH NAME:** `<branch>`

### **INITIAL CONDITIONS/EXTRA INFO:**

_<PR <id> · <short sha> · FAST|FULL · accounts by role label (never credentials) · flags · DB checks ON|OFF>_

### **📊 SUMMARY**

**CASE #1 | <OK|KO|BLOCKED|TO TEST|TO DISCUSS>**
**CASE #2 | <…>**

### **📝 TESTS**

**CASE #1 | <OK|KO|BLOCKED|TO TEST|TO DISCUSS> | <Area>: <behaviour checked>**

**PRECONDITIONS:**

* _<data / account role required>_

### **STEPS:**

1. <step; step 1 carries the environment URL>
2. <step>

**EXPECTED RESULT:**

* <expected>

**ACTUAL RESULT:**

[[ OK:
* <observation that proves it, with business IDs (product, order, customer), measured values, languages or breakpoints checked>
* Note: <only if checked against a proxy, e.g. no data for the exact precondition>
  — KO:
* <observation, with URL and evidence: HTTP status / payload / console / SQL if DB ON>
* Severity: P<0-3>. Reproduced <n/m | once>.
* Suspect: <path#Lxx | path (UNKNOWN line — reason)> (<root-cause hypothesis tied to a diff hunk, or UNKNOWN>)
  — BLOCKED:
* <what was tried, with IDs/URLs, and why it cannot be verified here>
* Unblocker: <data, access, deploy>
  — TO TEST:
*
  — TO DISCUSS:
* <what was observed>
* <conflicting tickets / AC, e.g. "PROJ-122 stores the flag per product, PROJ-123 AC refers to the selected variant">
* Question: <the decision the PM must take> ]]
[[ only if a defect outside the scope was seen during this case:
* Side note (pre-existing, out of scope): <one sentence> ]]

---

[[ only when this ticket has post-merge checks:
### **⏭️ AFTER MERGE**

**<Area>: <behaviour checked>**

1. <step; step 1 carries the environment URL>
2. <step>

**EXPECTED RESULT:**

* <expected>

**OWNER:** <name, or the requester> ]]
```

## Jira body (`JIRA` mode only)

Render the approved draft into this HTML. Emit the fragment only, without the fence lines, no `<html>`/`<body>`. Same `[[ ]]` and `< >` rules as the draft. `{STATUS}` is a coloured status span from the table below.

```html
<h2><strong>[[ <PROJECT LABEL> | ]]TEST #<n> | </strong><span style="color: <decision colour>"><strong><GO|NO-GO|GO WITH RISKS></strong></span></h2>
<div data-type="panel-info"><h3><strong>TESTING ENVIRONMENT: <INTEGRATION (<env name>) 🌐|QA 🌐|PREPROD 🌐|LOCAL 🖥️></strong></h3><pre><code><environment URL></code></pre><p><strong>BRANCH NAME: </strong><code><branch></code></p></div>
<div data-type="panel-info"><h3><strong>INITIAL CONDITIONS/EXTRA INFO:</strong></h3><p><em><initial conditions line></em></p></div>
<div data-type="panel-info"><h3><strong>📊 SUMMARY</strong></h3><p><span style="color: #403294"><strong>CASE #1</strong></span><strong> | </strong>{STATUS}<br><span style="color: #403294"><strong>CASE #2</strong></span><strong> | </strong>{STATUS}</p></div>
<div data-type="panel-note"><h3><strong>📝 TESTS</strong></h3></div>
<p><span style="color: #403294"><strong>CASE #1</strong></span><strong> | </strong>{STATUS}<strong> | <Area>: <behaviour checked></strong></p>
[[ OK case: wrap everything below the case title, up to the closing </div> of ACTUAL RESULT, in
<details><summary>DETAILS ↴</summary> … </details>
  — any other status: no <details> ]]
<p><strong>PRECONDITIONS:</strong></p><ul><li><p><em><precondition></em></p></li></ul>
<h3><strong>STEPS:</strong></h3><ol><li><p><step, URLs as <a href="URL">URL</a>></p></li></ol>
<div data-type="panel-info"><p><strong>EXPECTED RESULT:</strong></p><ul><li><p><expected></p></li></ul></div>
<div data-type="<actual panel>"><p><strong>ACTUAL RESULT:</strong></p><ul><li><p><one bullet of the draft per item></p></li></ul></div>
<hr>
[[ only when the draft has an AFTER MERGE block:
<div data-type="panel-note"><h3><strong>⏭️ AFTER MERGE</strong></h3></div>
<p><strong><Area>: <behaviour checked></strong></p>
<h3><strong>STEPS:</strong></h3><ol><li><p><step, URLs as <a href="URL">URL</a>></p></li></ol>
<div data-type="panel-info"><p><strong>EXPECTED RESULT:</strong></p><ul><li><p><expected></p></li></ul></div>
<p><strong>OWNER:</strong> <name, or the requester></p> ]]
```

| Value | Colour (`style="color: …"`) | ACTUAL RESULT panel | OK collapsed |
|---|---|---|---|
| `OK` | `#36b37e` | `panel-success` | yes, in `<details>` |
| `KO` | `#ff5630` | `panel-error` | no |
| `BLOCKED` | `#bf2600` | `panel-note` | no |
| `TO TEST` | `#97a0af` | `panel-note` | no |
| `TO DISCUSS` | `#ff991f` | `panel-warning` | no |
| `GO` (header) | `#36b37e` | — | — |
| `GO WITH RISKS` (header) | `#ff991f` | — | — |
| `NO-GO` (header) | `#ff5630` | — | — |
| `CASE #n` label | `#403294` | — | — |

Status span: `<span style="color: <colour>"><strong><STATUS></strong></span>`.

HTML limits (Jira rejects or flattens otherwise): no `<details>` or table inside a panel; a `<details>` may hold panels. No column layouts. Escape `<`, `>`, `&` and `'` in text (`&lt;`, `&gt;`, `&amp;`, `&#039;`). An empty TO TEST actual result is one empty `<li><p></p></li>`.

## Rules

- English only, whatever the chat language.
- Header token = the `<decision>` token of the Phase 3 chat. Same token, never another.
- One block per case, `---` (draft) or `<hr>` (Jira body) after each. SUMMARY lists every case, in order.
- The Jira body says exactly what the approved draft says. A difference is a new draft and a new approval.
- The PM-facing chat stays the decision surface. Do not copy the note above the chat cut. Never print the HTML in the chat.
- Facts only from this run. Never copy facts from an example.
- Do not print this Rules section.
