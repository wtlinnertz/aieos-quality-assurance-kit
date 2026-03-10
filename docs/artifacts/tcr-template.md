# Test Campaign Record

---

## 1. Document Control

| Field | Value |
|-------|-------|
| TCR ID | TCR-{PROJECT}-{NNN} |
| Date | {YYYY-MM-DD} |
| VP Reference | {VP-{PROJECT}-{NNN}} |
| Campaign Scope | {which VP test categories this TCR covers} |
| Status | Draft / Validated / Freeze Pending / Frozen |
| Governance Model Version | 1.0 |
| Prompt Version | {prompt version} |
| Spec Version | {spec version} |
| Principles Version | {principles file versions} |

---

## 2. Campaign Summary

| Field | Value |
|-------|-------|
| Campaign Start Date | {YYYY-MM-DD} |
| Campaign End Date | {YYYY-MM-DD} |
| Campaign Scope | {VP test categories executed in this campaign} |
| Execution Team | {names or roles of team members who executed tests} |
| Overall Result | {all tests passed / N passed, N failed, N blocked, N skipped} |

---

## 3. Test Results

### {Test Category — e.g., Integration Tests}

| Test ID | Test Description | Result | Evidence Reference | Notes |
|---------|-----------------|--------|-------------------|-------|
| {T-001} | {description} | Pass / Fail / Blocked / Skipped | {log ref / metric value / screenshot ref} | {failure description if Fail; reason if Blocked/Skipped} |
| {T-002} | {description} | {result} | {evidence} | {notes} |

{Repeat for each test category covered by this campaign.}

---

## 4. Environment Verification

| Field | Value |
|-------|-------|
| Environment Used | {environment name, matching VP §4} |
| Configuration | {versions, settings, data state — matching VP specification} |
| VP Environment Match | {Yes / No — if No, see Deviations §7} |

{Confirm the environment matches the VP specification. If any deviation occurred, document it in §7 Deviations.}

---

## 5. Defect Summary

| Defect ID | Description | Severity | Status | Affected Component |
|-----------|-------------|----------|--------|--------------------|
| {DEF-001} | {description} | Critical / High / Medium / Low | Open / Fixed / Deferred / Accepted | {component name} |
| {DEF-002} | {description} | {severity} | {status} | {component} |

*If no defects were found, state: "No defects found during this campaign."*

---

## 6. Coverage Assessment

### Coverage by Test Category

| Test Category | Tests Planned (VP) | Tests Executed | Coverage % |
|---------------|-------------------|----------------|-----------|
| {category} | {N} | {N} | {N%} |
| {category} | {N} | {N} | {N%} |
| **Total** | **{N}** | **{N}** | **{N%}** |

### Coverage Gaps

{If coverage is below 100%, list specific tests not executed and reason:}

| Test ID | Reason Not Executed |
|---------|-------------------|
| {T-NNN} | {reason} |

*If all planned tests were executed, state: "All planned tests in scope were executed. No coverage gaps."*

---

## 7. Deviations

{Document any deviations from the VP: tests added, tests removed, scope changed, environment changed.}

| Deviation | Reason | Impact Assessment |
|-----------|--------|------------------|
| {description} | {reason} | {impact on results} |

*If no deviations occurred, state: "No deviations from the Verification Plan."*
