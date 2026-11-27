# QA Note in Jira — `JIRA` mode only, after the reply that approves the note

Load this only to create an approved QA Note in Jira. The draft and its rules are in `assets/qa-note.md`; the approval rule is in `SKILL.md` (section QA Note). Do not restate them.

## Jira fields (`JIRA` mode only)

`createJiraIssue` arguments:

- `projectKey`: the ticket's project
- `issueType`: `QA Note`
- `parent`: the parent of the approved draft (`assets/qa-note.md`, Before drafting)
- `summary`: the summary of the approved draft (`assets/qa-note.md`)
- `contentFormat`: `html`
- `description`: the Jira body below, filled from the approved draft
- Nothing else: no assignee, labels, priority, transition

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

- The Jira body says exactly what the approved draft says. A difference is a new draft and a new approval.
- Never print the HTML in the chat.
- Do not print this Rules section.
