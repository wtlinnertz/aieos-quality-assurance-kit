# Quality Gate Record — Specification

Version: v1.0

The Quality Gate Record (QGR) is the terminal artifact of the Quality Assurance Kit. It aggregates Test Campaign Record results, coverage data, and defect status to declare a quality disposition: PASS (proceed to REK), CONDITIONAL (proceed with noted risks), or FAIL (return to EEK).

The QGR is the downstream boundary contract for the Release & Exposure Kit. Its §Quality Disposition section is the handoff point.

---

## What This Artifact Is Not

- **Not a test result.** The QGR interprets test results documented in TCRs; it does not contain raw test data.
- **Not a risk register.** The QGR documents residual quality risks relevant to the release decision; it is not a comprehensive risk management artifact.
- **Not a release authorization.** The QGR declares quality readiness; the release decision is made in the Release & Exposure Kit.

---

## Purpose

The QGR serves three roles:

1. **Evidence aggregation** — Synthesizes all TCR results, coverage data, and defect status into a single quality assessment
2. **Disposition declaration** — Provides a clear PASS/CONDITIONAL/FAIL judgment with supporting evidence
3. **Boundary contract** — Serves as the quality clearance input for the Release & Exposure Kit entry

---

## Upstream Dependencies

- All frozen TCRs for this initiative
- Frozen VP — for coverage assessment
- Defect status summary — all defects with current status

---

## Required Sections

1. Document Control
2. TCR Summary
3. Coverage Assessment
4. Defect Assessment
5. Risk Assessment
6. Quality Disposition
7. Completeness Checklist

---

## Content Rules

### Document Control
**Rules**
- QGR ID must be present (format: QGR-{PROJECT}-{NNN})
- Date must be present
- VP reference must be present (VP ID)
- All TCR IDs must be listed
- Initiative reference must be present (QAER ID)

**Failure Examples**
- Missing QGR ID
- VP ID or TCR IDs not referenced
- QAER ID not referenced

### TCR Summary
**Rules**
- Each TCR must be summarized: TCR ID, campaign scope, overall result, key findings
- All TCRs for this initiative must be included — no TCR may be omitted
- If only one TCR exists, it must still be summarized in this section

**Failure Examples**
- TCR not listed that was frozen for this initiative
- TCR listed without summary of results
- TCR results misrepresented (contradicts TCR content)

### Coverage Assessment
**Rules**
- Overall coverage must be stated: total tests executed / total tests planned (from VP)
- Coverage must be broken down by VP test category
- Coverage targets from VP acceptance criteria must be referenced
- Coverage gaps must be identified with specific tests not executed and reason
- Coverage assessment must be derived from TCR data, not independently calculated

**Failure Examples**
- Coverage stated without VP reference
- Coverage not broken down by category
- Coverage gaps not identified
- Coverage numbers inconsistent with TCR coverage assessments

### Defect Assessment
**Rules**
- All defects from all TCRs must be aggregated
- Defects must be categorized by severity: critical, high, medium, low
- Current status of each defect must be stated: open, fixed, deferred, accepted
- All critical and high severity defects must have one of: resolved (fixed and verified), accepted with documented rationale, or identified as a blocking issue
- If critical or high defects are accepted (not resolved), the acceptance rationale must be specific — not "low risk" or "will fix later"

**Failure Examples**
- Defects from a TCR not included in the aggregate
- Critical defect listed as "accepted" without specific rationale
- Defect status not current (shows old status from TCR rather than current status)
- Severity categories inconsistent with TCR severity assignments

### Risk Assessment
**Rules**
- Residual risks from incomplete coverage, accepted defects, or test limitations must be documented
- Each risk must have: description, impact assessment, and likelihood
- Mitigation approach must be stated for each risk (accept, mitigate, transfer)
- If no residual risks exist, this must be explicitly stated with justification

**Failure Examples**
- Accepted defects exist but risk section says "no risks"
- Coverage gaps exist but risk section does not address them
- Risks listed without impact assessment
- Empty section without explicit "no residual risks" statement

### Quality Disposition
**Rules**
- Disposition must be one of: PASS, CONDITIONAL, FAIL
- Disposition must be explicitly supported by the evidence in the preceding sections
- PASS: all VP acceptance criteria met; no open critical or high defects; coverage targets met
- CONDITIONAL: VP acceptance criteria mostly met; accepted risks documented in §Risk Assessment; conditions for proceeding stated
- FAIL: VP acceptance criteria not met; or open critical/high defects without acceptance rationale; or coverage gaps without justification; blocking issues listed
- If CONDITIONAL: specific conditions must be stated (what the release team must acknowledge or address)
- If FAIL: specific blocking issues must be listed with what must be resolved before re-entry

**Failure Examples**
- Disposition stated without evidence support
- PASS with open critical defects
- CONDITIONAL without stated conditions
- FAIL without specific blocking issues
- Disposition contradicts the evidence (e.g., PASS when coverage is below VP target without justification)

---

## Format Requirements

- QGR ID must follow the standard format
- Disposition must be in bold and clearly visible
- Defect summary must be in table format
- Coverage must include both absolute numbers and percentages

---

## Completeness Rules

- All TCRs referenced and summarized
- Coverage assessment derived from TCR data
- All defects aggregated with current status
- Risk assessment addresses coverage gaps and accepted defects
- Disposition explicitly justified

---

## Relationship Rules

- The QGR is the downstream boundary contract for the Release & Exposure Kit
- A frozen QGR with PASS or CONDITIONAL disposition clears the initiative for REK entry
- A frozen QGR with FAIL disposition blocks REK entry and returns the initiative to EEK
- The QGR does not replace TCRs — TCRs remain the evidence record; the QGR is the interpretation

---

## Hard Gates

1. **tcr_completeness** — All frozen TCRs for this initiative are referenced and summarized; no TCR omitted
2. **disposition_justified** — Quality disposition (PASS/CONDITIONAL/FAIL) is explicitly supported by evidence from TCR summary, coverage assessment, and defect assessment; disposition does not contradict the evidence
3. **defect_assessment** — All critical and high severity defects are either resolved (fixed and verified), accepted with specific rationale, or identified as blocking issues; defect aggregate is consistent with TCR defect summaries
4. **coverage_assessment** — Coverage is stated as executed/planned broken down by VP test category; coverage gaps identified with specific tests and reasons; coverage derived from TCR data
5. **risk_assessment** — Residual risks from accepted defects, coverage gaps, or test limitations are documented with impact and mitigation; or explicit "no residual risks" with justification
