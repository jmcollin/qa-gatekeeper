# Phase 1 blocked — copy the content of the fenced block only

Everything outside the fence is an instruction. Do not print it. Do not print the fence lines either.

Also used when env and account are known but a required tool is missing. Then some CUJs already ran: each journey line carries its real status.

```text
NO VERDICT YET — I need N answers

PR #<id> · <short sha> · FAST|FULL
<one or two sentences: why the runs cannot start. CI and SHA gap go here, not in a table.>

Decide
1. <question. Named options. For a waive option: which cause it lifts, which CUJs become SKIPPED (waived), which stay BLOCKED by another cause.>
2. <next question, only if one is needed.>

I will not run the blocked journeys until these are answered.

---

Context, if you scroll

<CI in one line. Deployed SHA vs HEAD. Touched files not on the env.>

Planned journeys
- CUJ-01 P0 <actor> — <expected, one sentence>. <STATUS>: <cause or evidence, one line>.
- CUJ-02 P1 <actor> — <expected>. <STATUS>: <cause or evidence>.

<One line on what is already on the env.>

Rollback: <one line, or UNKNOWN>.

DB: not verified.
<MISSING tools, one line. No line for SKIPPED.>
```

## Rules

- Replace every `<placeholder>`. Do not leave a placeholder in the chat.
- N is the number of Decide items.
- Cause stays out of the status token. Write `BLOCKED`, then the cause. A CUJ blocked by two causes names both.
- `<STATUS>` is the real status of that CUJ (`PASS`, `FAIL`, `BLOCKED`). Do not write "all BLOCKED" if one ran.
- No box-drawing table. No table wider than 4 columns.
- Translate the prose to the user's language. Tokens stay in English.
- A waive option follows the Waive rule in `SKILL.md`: it lifts only the cause it names. If every cause of a P0/P1 is waived, the report is `GO WITH RISKS` at best. If one cause stays, that CUJ stays `BLOCKED` and the report is `NO-GO`.
