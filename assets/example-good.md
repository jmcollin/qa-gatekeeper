# Good — Phase 1 blocked

Copy the shape. Facts below are fake.

Picking an execution option is never a waive. A waive is its own sentence, naming the cause it lifts (Waive rule in `SKILL.md`). Do not reuse the PR id, SHAs, account labels, or dates in a real report.

The scenario (SSO login, Vue 2 bundles, `?finalize`, organization page) only illustrates the shape. Do not borrow its CUJs, risks, causes, or options for another PR: derive them from that PR's diff.

```text
NO VERDICT YET — I need 2 answers

PR #0000 · aaaa111 · FULL
The branch conflicts with master. The integration env is on bbbb222.
The login fix is not deployed there.
Chrome DevTools is not connected: no journey was run.

Decide
1. Chrome: reconnect it (/mcp → chrome-devtools → Reconnect, Node 20.19+ and Chrome).
   Or say "waive Chrome": CUJ-02 to CUJ-05 become SKIPPED (waived).
   CUJ-01 stays BLOCKED by the SHA until the env gap is fixed (b) or waived.
   If every cause is waived, the report is GO WITH RISKS at best.
2. Env is behind. Integration deploys fire on pull_request only, no manual run.
   (a) I run the login journey on bbbb222 and record what I see. That does not
       test the fix: CUJ-01 stays BLOCKED, cause env behind, and the report notes it.
   (b) You merge master and rebuild the Vue 2 bundles first.
   To accept the gap instead, say "waive the env gap". The gap only matters
   for a logged-out visitor with ?finalize. If Chrome is reconnected, I run CUJ-01
   on bbbb222 with that gap listed as accepted. If Chrome is waived too,
   CUJ-01 becomes SKIPPED (waived).

I will not run the blocked journeys until 1 and 2 are answered.

---

Context, if you scroll

CI: CONFLICTING. Behind master, conflicts in committed Vue 2 bundles.

Planned journeys
- CUJ-01 P0 owner account — SSO login, no loop, external return_to neutralized.
  BLOCKED: Chrome missing, env behind.
- CUJ-02 P1 owner account — org page, Manage button, nothing editable.
  BLOCKED: Chrome missing.
- CUJ-03 P1 member account — read only, owner email, no Manage.
  BLOCKED: Chrome missing.
- CUJ-04 P1 orphan account — empty screen + billing button.
  BLOCKED: Chrome missing.
- CUJ-05 P2 — served copy matches the branch. i18n cache is not cleared on deploy.
  BLOCKED: Chrome missing.

Organization page files have not changed since the env SHA.
Only the login journey is env-behind.

Rollback: branch is not merged, nothing to revert for this gate.
In prod, revert the merge; migration down() effect is UNKNOWN.

DB: not verified. Figma and Gmail: out of scope.
```

# Good — QA Note

Copy the shape. The scenario (a "Free 30-day returns" badge in the "Delivery & returns" block of a product page) is fictional, adapted to the rules of `assets/qa-note.md`. The ticket keys, PR id, SHA, product names and IDs are fake. Do not copy cases, IDs or findings into another note: derive them from that run.

What makes it good:

- Header carries the decision token, not `TBD`. Here `NO-GO`: CASE #7 and #13 are P1 `BLOCKED`, cause `spec ambiguous`, and the decision table gives `NO-GO` until the PM answers or waives them by name.
- Environment named by type and env name, initial conditions give PR, SHA, path, account roles and DB checks.
- Every `TO TEST` is a `SKIPPED (user-approved — reason)` CUJ; the reason stays in the chat, the case keeps an empty actual result.
- `OK` checked against a proxy says so in a `Note:`. `BLOCKED` gives what was tried and the unblocker. `TO DISCUSS` gives the conflicting tickets and one question.
- A defect outside the scope is one `Side note`, never a case.
- No header prefix: nobody named a project label in the thread.
- Shortened: SUMMARY lists all 13 cases; only one case per shape is shown below. A real note shows every case.
- This is the draft printed under the cut. In `JIRA` mode the same words are rendered into the HTML Jira body of `assets/qa-note-jira.md`: info panels, coloured statuses, `ACTUAL RESULT` panel by status, CASE #1, #2 and #8 collapsed under `DETAILS ↴`.

Parent: PROJ-123 · Type: QA Note
Summary: QA note PROJ-123 - TEST #1: Free returns badge OK, badge ordering blocked, variant rule and bundles to discuss

```markdown
## **TEST #1 | NO-GO**

### **TESTING ENVIRONMENT: INTEGRATION (int 4) 🌐**

https://shop-int4.example.com/en/

**BRANCH NAME:** `feat/free-returns-badge`

### **INITIAL CONDITIONS/EXTRA INFO:**

_PR #0000 · aaaa111 · FULL · visitor (logged out), back-office admin account not provided · DB checks OFF_

### **📊 SUMMARY**

**CASE #1 | OK**
**CASE #2 | OK**
**CASE #3 | BLOCKED**
**CASE #4 | TO TEST**
**CASE #5 | OK**
**CASE #6 | TO TEST**
**CASE #7 | TO DISCUSS**
**CASE #8 | OK**
**CASE #9 | OK**
**CASE #10 | OK**
**CASE #11 | OK**
**CASE #12 | TO TEST**
**CASE #13 | TO DISCUSS**

### **📝 TESTS**

**CASE #1 | OK | PDP Delivery & returns: Product with Free returns = YES shows the Free 30-day returns badge**

**PRECONDITIONS:**

* _Product with Free returns = YES exists on the environment_

### **STEPS:**

1. Open the PDP on https://shop-int4.example.com/en/ of a product flagged Free returns
2. Scroll to the 'Delivery & returns' block
3. Verify the 'Guarantees' section

**EXPECTED RESULT:**

* 'Free 30-day returns' is displayed in the 'Guarantees' section of the 'Delivery & returns' block.

**ACTUAL RESULT:**

* 'Free 30-day returns' displayed in 'Guarantees' (first line) on Ceramic Mug (id 1041), Linen Apron (1042) and Oak Board (1043): all 3 products returned by the 'Free returns' listing filter.

---

**CASE #2 | OK | PDP Delivery & returns: Free returns badge uses the same visual treatment as the Express shipping badge**

**PRECONDITIONS:**

* _Product with Free returns = YES and Express shipping exists_

### **STEPS:**

1. Open the PDP on https://shop-int4.example.com/en/ of a product with both Free returns and Express shipping
2. Scroll to the 'Delivery & returns' block
3. Compare the Free returns line with 'Express shipping available' (icon, font, colour, spacing)

**EXPECTED RESULT:**

* Free returns line has the same icon, typography, colour and spacing as the Express shipping line, matching the Figma mock-up.

**ACTUAL RESULT:**

* Free returns line uses the same component as the other guarantee lines: green 'check' icon rgb(89,175,112), 'body-default' 14px/400 rgb(29,29,27), same row layout and spacing, no strikethrough or opacity.
* Note: no product with Express shipping on int4; compared against the sibling lines of the same list, not the Express shipping line.

---

**CASE #3 | BLOCKED | PDP Delivery & returns: Free returns badge is placed above the Express shipping badge**

**PRECONDITIONS:**

* _Product with Free returns = YES and Express shipping exists_

### **STEPS:**

1. Open the PDP on https://shop-int4.example.com/en/ of a product with both Free returns and Express shipping
2. Scroll to the 'Guarantees' section
3. Verify the order of the lines

**EXPECTED RESULT:**

* 'Free 30-day returns' is displayed directly above 'Express shipping available'.

**ACTUAL RESULT:**

* No product with both Free returns and Express shipping on int4. Express shipping products found by search ('Express Kettle' 2001, 'Express Lamp' 2002) redirect to the homepage.
* Unblocker: one product flagged both Free returns and Express shipping on int4.

---

**CASE #4 | TO TEST | PDP Delivery & returns: Product with Free returns = NO shows no returns badge**

**PRECONDITIONS:**

* _Product with Free returns = NO exists_

### **STEPS:**

1. Open the PDP on https://shop-int4.example.com/en/ of a product not flagged Free returns
2. Scroll to the 'Delivery & returns' block
3. Verify no returns-related badge is displayed

**EXPECTED RESULT:**

* No Free returns badge at all: no greyed-out text, no strikethrough text, no error icon.

**ACTUAL RESULT:**

*

---

**CASE #7 | TO DISCUSS | PDP Delivery & returns: Free returns badge reflects the selected variant**

**PRECONDITIONS:**

* _Product with several variants; the block states its information is based on the selected variant_

### **STEPS:**

1. Open the PDP on https://shop-int4.example.com/en/ of the product
2. Select each variant in turn and check the 'Delivery & returns' block
3. Compare the badge with the Free returns value of that variant / product

**EXPECTED RESULT:**

* Free returns badge is consistent with the Free returns value applying to the selected variant.

**ACTUAL RESULT:**

* Block states 'Info based on selected variant' and the badge is shown for every variant, consistent with the listing filter.
* PROJ-122 stores Free returns per product (not per variant, unlike Express shipping); PROJ-123 AC and tech notes refer to the selected variant.
* Question: does the badge follow the product flag or the selected variant?

---

**CASE #8 | OK | PDP Delivery & returns: Free returns badge is translated in all supported languages**

**PRECONDITIONS:**

* _Product with Free returns = YES exists_

### **STEPS:**

1. Open the product PDP on https://shop-int4.example.com/ in EN, FR, ES, IT and PL
2. Scroll to the 'Guarantees' section in each language
3. Verify the badge wording

**EXPECTED RESULT:**

* Free returns badge is translated in each language (no English fallback, no raw translation key).

**ACTUAL RESULT:**

* Ceramic Mug (1041): EN 'Free 30-day returns', FR 'Retours gratuits sous 30 jours', ES 'Devoluciones gratuitas en 30 días', IT 'Resi gratuiti entro 30 giorni', PL 'Darmowy zwrot w ciągu 30 dni'. No raw key, no fallback.
* Side note (pre-existing, out of scope): ES line 'nvío exprés disponible' is missing its leading 'E'.

---

**CASE #13 | TO DISCUSS | PDP Delivery & returns: Bundle with Free returns = YES shows the badge**

**PRECONDITIONS:**

* _Bundle with Free returns = YES exists_

### **STEPS:**

1. Open the PDP on https://shop-int4.example.com/en/ of a bundle flagged Free returns
2. Scroll to the 'Delivery & returns' block
3. Verify the 'Guarantees' section

**EXPECTED RESULT:**

* 'Free 30-day returns' is displayed for the bundle as for single products.

**ACTUAL RESULT:**

* Bundle 'Breakfast Set' (3050) is returned by the bundles listing filter 'Free returns' (1 result), but its 'Delivery & returns' block has no 'Guarantees' section and no badge (only Shipping, About this bundle).
* PROJ-122 lets bundles declare Free returns; PROJ-123 AC only mentions products.
* Question: must bundles display the Free returns badge on their PDP?

---
```
