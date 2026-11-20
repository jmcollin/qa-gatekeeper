# Bad — do not reproduce

A message that opens on a tool table, then a 5-column risk matrix, then 7-column CUJs, and puts the question at the bottom.

Do not:

- draw Unicode grids (`┌ ┬ ┐`)
- put a table of more than 4 columns in the chat
- print the full preflight, including SKIPPED tools
- place the question or the verdict after the evidence
- put `path#Lxx` above the cut
- print instructions that sat outside a template fence

Real shape to avoid (shortened):

```text
I cannot run the tests yet: I need your answer on two points (at the bottom).
Tools
┌─────────┬─────────┬──────────┐
│ Tool    │ Status  │ Evidence │
└─────────┴─────────┴──────────┘
1. Changeset
… ten paragraphs …
2. Invariants and risks
┌─────────┬─────────┬──────┬─────────┬─────────┐
│ Surface │ Risk    │ Pri  │ Evidence│ Radius  │
└─────────┴─────────┴──────┴─────────┴─────────┘
3. Journeys (CUJ)
┌────┬────────┬────────┬────────┬────────┬────────┬────────┐
… 7 columns …
Two answers I need:
1. …
2. …
```

The PM must not have to scroll to learn that a choice is required.

# Bad — QA Note

The manual QA comment on PROJ-123 (TEST #1) is the format our QA proposed, and the source of this template. Copied as-is by the skill, though, some of its parts break the gate rules. The fixed version is in `assets/example-good.md`, section QA Note.

The `<Team> |` prefix in its header is not a fault: it is part of the QA's proposal. The skill simply does not print it.

Do not:

- post the report as a ticket comment: it is a `QA Note` sub-task, created after a reply naming the ticket
- put `TBD` in the header: the skill always has a decision, `TEST #<n> | <decision token>`
- label an integration env `STAGING`: `INTEGRATION (<env name>)`, `QA` or `PREPROD`
- write `None` under initial conditions when a PR, SHA, accounts and DB mode exist
- leave a `BLOCKED` case without its unblocker
- leave a `TO DISCUSS` case without the conflicting tickets and one question
- mark a case `TO TEST` that was not `SKIPPED` with an approval naming it: an unrun planned CUJ is `BLOCKED`
- turn an out-of-scope defect into a case
- translate the note: it stays in English whatever the chat language

Real shape to avoid (shortened):

```text
## **Team | TEST #1 | TBD**

### **TESTING ENVIRONMENT: STAGING 🌐**

### **INITIAL CONDITIONS/EXTRA INFO:**

_None_

**CASE #3 | BLOCKED | PDP Delivery & returns: Free returns badge is placed above the Express shipping badge**
…
**ACTUAL RESULT:**

* No product with both Free returns and Express shipping on int4. Position relative to the Express shipping line cannot be verified.

**CASE #13 | TO DISCUSS | PDP Delivery & returns: Bundle with Free returns = YES shows the badge**
…
* PROJ-123 AC only mentions products – to confirm whether bundles should display the badge.
```
