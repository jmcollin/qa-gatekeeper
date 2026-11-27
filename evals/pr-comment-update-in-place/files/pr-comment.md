## QA | TEST #1 | GO WITH RISKS

**TL;DR**: UK postcodes with a space are saved; the French postcode check is waived: 2 cases, 1 OK, 1 TO TEST

**Testing environment**: integration int-6

<details>
<summary>Setup ↴</summary>

- int-6: https://int-6.acme.test, deployed SHA 5ab3e91
- Account: qa-buyer
- CI: GREEN
- DB: not checked

</details>

### 📊 Summary

**CASE #1 | OK** | Account: UK postcode "SW1A 1AA" is saved
**CASE #2 | TO TEST** | Account: French postcode "75001" still saved

### 📝 Tests

**CASE #1 | OK | Account: UK postcode "SW1A 1AA" is saved**

<details>
<summary>Details ↴</summary>

**Preconditions**
- qa-buyer account

**Steps**
1. On https://int-6.acme.test/account/address as qa-buyer, enter "SW1A 1AA" and save

**Expected**
- Address saved

**Actual**
- HTTP 200 on POST /account/address, address 5521 saved with "SW1A 1AA", 0 console errors

</details>

**CASE #2 | TO TEST | Account: French postcode "75001" still saved**

**Preconditions** / **Steps** / **Expected**: on https://int-6.acme.test/account/address as qa-buyer, enter "75001" and save; address saved.

**Actual**
-

---

### ❌ Not verified

- CUJ-02 French postcode: Chrome DevTools missing, waived. Risk: a regression on non-UK postcodes.

<sub>QA run: 2026-11-27 on [5ab3e91](https://github.com/acme/shop/commit/5ab3e91)</sub>
