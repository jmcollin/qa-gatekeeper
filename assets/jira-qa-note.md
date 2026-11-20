# Jira QA Note — one per ticket, always in English

Everything outside the fence is an instruction. The QA Note is a Jira sub-task of type `QA Note` under the ticket. It complements the Phase 3 chat; it never replaces it. The approval rule and the status mapping are in `SKILL.md` (section QA Note). Do not restate them. A filled note (PROJ-123) is in `assets/example-good.md`, section QA Note; the shape to avoid is in `assets/example-bad.md`, section QA Note.

## Before drafting

- Read the ticket's existing QA Notes: JQL `parent = <KEY> AND issuetype = "QA Note"`. `<n>` = highest `TEST #k` found in their summaries + 1, or 1 if none carries one.
- Cases: the CUJs that cover this ticket, in plan order, renumbered `CASE #1`, `CASE #2`, … for this note. A CUJ covering two tickets appears in both notes.

## Jira fields

- `projectKey`: the ticket's project
- `issueTypeName`: `QA Note`
- `parent`: the ticket key. If the ticket is itself a sub-task or an epic, ask which issue to attach it to
- `summary`: `QA note <KEY> - TEST #<n>: <one-line outcome>`, e.g. `QA note PROJ-123 - TEST #1: Free returns badge OK, badge ordering blocked, bundle scope to discuss`
- `contentFormat`: `markdown`
- `description`: the content of the fence below
- Nothing else: no assignee, labels, priority, transition

## Description

Copy the content of the fence, without the fence lines. `[[ … ]]` is a choice: keep what applies, drop the brackets. Replace every `< >`.

```markdown
## **TEST #<n> | <GO|NO-GO|GO WITH RISKS>**

### **TESTING ENVIRONMENT: <INTEGRATION (<env name>)|QA|PREPROD> 🌐**

<environment URL>

### **INITIAL CONDITIONS/EXTRA INFO:**

_<PR <id> · <short sha> · FAST|FULL · accounts by role label (never credentials) · flags · DB checks ON|OFF — or: None>_

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
* <observation>
* Severity: P<0-3>
* URL: <url>
* Evidence: <HTTP status / payload / console / SQL if DB ON>
* Reproducibility: <always | n/m | once>
* Suspect file(s): <path#Lxx | path (UNKNOWN line — reason)>
* Root-cause hypothesis: <tied to a diff hunk, or UNKNOWN>
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
```

## Rules

- English only, whatever the chat language.
- Header token = the `<decision>` token of the Phase 3 chat. Same token, never another.
- One block per case, `---` after each. SUMMARY lists every case, in order.
- The PM-facing chat stays the decision surface. Do not copy the note above the chat cut.
- Facts only from this run. Never copy facts from an example.
- Do not print this Rules section.
