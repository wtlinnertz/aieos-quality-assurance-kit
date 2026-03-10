# Quality Gate Record

---

## 1. Document Control

| Field | Value |
|-------|-------|
| QGR ID | QGR-{PROJECT}-{NNN} |
| Date | {YYYY-MM-DD} |
| VP Reference | {VP-{PROJECT}-{NNN}} |
| QAER Reference | {QAER-{PROJECT}-{NNN}} |
| TCR References | {TCR-{PROJECT}-{NNN}, TCR-{PROJECT}-{NNN}, ...} |
| Status | Draft / Validated / Freeze Pending / Frozen |
| Governance Model Version | 1.0 |
| Prompt Version | {prompt version} |
| Spec Version | {spec version} |
| Principles Version | {principles file versions} |

---

## 2. TCR Summary

{For each frozen TCR:}

### {TCR ID}

| Field | Value |
|-------|-------|
| Campaign Scope | {VP test categories covered} |
| Overall Result | {summary: N passed, N failed, N blocked} |
| Key Findings | {notable results, significant failures, or important observations} |
| Defects Found | {count by severity} |

---

## 3. Coverage Assessment

### Overall Coverage

| Test Category | Tests Planned (VP) | Tests Executed (TCRs) | Coverage % | VP Target |
|---------------|-------------------|----------------------|-----------|-----------|
| Integration | {N} | {N} | {N%} | {VP acceptance threshold} |
| System | {N} | {N} | {N%} | {VP acceptance threshold} |
| Performance | {N} | {N} | {N%} | {VP acceptance threshold} |
| Security | {N} | {N} | {N%} | {VP acceptance threshold} |
| **Total** | **{N}** | **{N}** | **{N%}** | — |

### Coverage Gaps

{List tests not executed across all TCRs with reasons. Reference TCR §6 for details.}

| Test ID | Test Category | Reason Not Executed | TCR Reference |
|---------|--------------|-------------------|---------------|
| {T-NNN} | {category} | {reason} | {TCR ID §6} |

*If no coverage gaps, state: "All VP-planned tests were executed across all campaigns."*

---

## 4. Defect Assessment

### Defect Summary by Severity

| Severity | Total Found | Resolved | Accepted | Deferred | Open |
|----------|------------|----------|----------|----------|------|
| Critical | {N} | {N} | {N} | {N} | {N} |
| High | {N} | {N} | {N} | {N} | {N} |
| Medium | {N} | {N} | {N} | {N} | {N} |
| Low | {N} | {N} | {N} | {N} | {N} |
| **Total** | **{N}** | **{N}** | **{N}** | **{N}** | **{N}** |

### Critical and High Defect Disposition

{For each critical or high severity defect:}

| Defect ID | Severity | Status | Disposition Rationale |
|-----------|----------|--------|----------------------|
| {DEF-001} | {Critical/High} | {Resolved/Accepted} | {if resolved: verified fix reference; if accepted: specific rationale for acceptance} |

*If no critical or high defects, state: "No critical or high severity defects found."*

---

## 5. Risk Assessment

{Document residual risks from accepted defects, coverage gaps, or test limitations.}

| Risk | Source | Impact | Likelihood | Mitigation |
|------|--------|--------|-----------|-----------|
| {description} | {accepted defect / coverage gap / test limitation} | {impact assessment} | {high / medium / low} | {accept / mitigate / transfer} |

*If no residual risks, state: "No residual quality risks identified. Justification: {reason}."*

---

## 6. Quality Disposition

**Disposition: {PASS / CONDITIONAL / FAIL}**

### Evidence Basis

{Summarize the evidence supporting the disposition:}
- Coverage: {overall coverage assessment}
- Defects: {defect status summary}
- Acceptance Criteria: {VP acceptance criteria met/not met}

### Conditions (CONDITIONAL only)

{If CONDITIONAL, list specific conditions for proceeding:}
- {condition 1}
- {condition 2}

### Blocking Issues (FAIL only)

{If FAIL, list specific blocking issues that must be resolved:}
- {blocking issue 1}
- {blocking issue 2}

---

## 7. Completeness Checklist

Before freezing this record, confirm:

- [ ] All frozen TCRs referenced and summarized
- [ ] Coverage assessment derived from TCR data, broken down by VP test category
- [ ] All defects aggregated with current status
- [ ] All critical/high defects have disposition (resolved or accepted with rationale)
- [ ] Risk assessment addresses coverage gaps and accepted defects
- [ ] Quality disposition explicitly justified by evidence
- [ ] Conditions stated (if CONDITIONAL) or blocking issues listed (if FAIL)
