# Quality Gate Record — Validator

This validator evaluates a generated Quality Gate Record (QGR) against `docs/specs/qgr-spec.md`. It is used in a separate AI session from the one that generated the QGR.

**Validator role:** Judge pass/fail. Do not suggest improvements, redesign content, or offer alternatives. Evaluate only what is explicitly present.

---

## Inputs Required

To run this validator, provide:
1. The generated QGR (full document)
2. `docs/specs/qgr-spec.md` (the spec — use this as the authoritative rules)

Do not use any other document as the source of truth for pass/fail criteria.

---

## Evaluation Procedure

Evaluate each hard gate in order. For each gate, apply the rules exactly as stated in the spec. Do not infer intent. Do not give partial credit. Ambiguity in the artifact is a failure condition — if you cannot determine whether a gate passes from what is explicitly present, the gate fails.

---

## Hard Gate Checks

### Gate 1: tcr_completeness

Check:
- All frozen TCRs for this initiative are referenced in the QGR
- Each TCR is summarized with: TCR ID, campaign scope, overall result, key findings
- No TCR is omitted from the summary
- If only one TCR exists, it is still summarized

**Pass:** All TCRs referenced and summarized with required fields.
**Fail:** TCR omitted; TCR listed without summary; TCR results misrepresented.

---

### Gate 2: disposition_justified

Check:
- Quality disposition is one of: PASS, CONDITIONAL, FAIL
- Disposition is explicitly supported by evidence from TCR summary, coverage assessment, and defect assessment
- PASS: verify no open critical/high defects exist in defect assessment; coverage targets met
- CONDITIONAL: verify conditions are stated; accepted risks are documented
- FAIL: verify blocking issues are listed
- Disposition does not contradict the evidence (e.g., PASS with open critical defects)

**Pass:** Disposition stated; evidence supports the disposition; no contradictions.
**Fail:** Disposition not stated; disposition contradicts evidence; PASS with open critical/high defects; CONDITIONAL without conditions; FAIL without blocking issues.

---

### Gate 3: defect_assessment

Check:
- All defects from all TCRs are aggregated
- Defects are categorized by severity (critical, high, medium, low)
- Current status is stated for each severity category
- All critical and high severity defects are either: resolved (fixed and verified), accepted with specific rationale, or identified as blocking issues
- Acceptance rationale for critical/high defects is specific — not "low risk" or "will fix later"
- Defect aggregate is consistent with TCR defect summaries

**Pass:** All defects aggregated; critical/high defects addressed with specific disposition; consistent with TCRs.
**Fail:** Defects missing from aggregate; critical/high defects without disposition; vague acceptance rationale; inconsistency with TCRs.

---

### Gate 4: coverage_assessment

Check:
- Coverage is stated as executed/planned with percentages
- Coverage is broken down by VP test category
- Coverage gaps are identified with specific tests not executed and reasons
- Coverage data is derived from TCR coverage assessments (not independently calculated)

**Pass:** Coverage stated with VP breakdown; gaps identified; data consistent with TCRs.
**Fail:** Coverage not stated; not broken down by category; gaps not identified; coverage inconsistent with TCRs.

---

### Gate 5: risk_assessment

Check:
- Residual risks from accepted defects, coverage gaps, or test limitations are documented
- Each risk has: description, impact assessment, likelihood, and mitigation approach
- If accepted defects exist in §4, corresponding risks are documented in §5
- If coverage gaps exist in §3, corresponding risks are documented in §5
- If no residual risks exist, explicit "no residual risks" statement with justification is present

**Pass:** Risks documented with required fields; accepted defects and coverage gaps reflected; or explicit "none" with justification.
**Fail:** Accepted defects exist but no corresponding risk; coverage gaps exist but not in risk assessment; risks without impact assessment; empty section without statement.

---

## Output Format

Produce a JSON result in exactly this format:

```json
{
  "status": "PASS | FAIL",
  "summary": "<one sentence verdict>",
  "hard_gates": {
    "tcr_completeness": "PASS | FAIL",
    "disposition_justified": "PASS | FAIL",
    "defect_assessment": "PASS | FAIL",
    "coverage_assessment": "PASS | FAIL",
    "risk_assessment": "PASS | FAIL"
  },
  "blocking_issues": [
    {
      "gate": "<gate_name>",
      "description": "<what specifically failed>",
      "location": "<section or field where the failure is>"
    }
  ],
  "warnings": [
    {
      "description": "<non-blocking observation>",
      "location": "<section>"
    }
  ],
  "completeness_score": "<0-100>"
}
```

**Interpretation rules:**
- Any gate failure → `"status": "FAIL"`
- `blocking_issues` lists exactly the failures — no additional content
- `warnings` are non-blocking; they do not affect status
- `completeness_score` is advisory; it does not override gate results
- If all gates pass, `blocking_issues` is an empty array

---

## Validator Constraints

- Do not suggest how to fix failures
- Do not redesign or improve the QGR
- Do not evaluate content quality beyond what the spec requires
- Do not accept inferred information as equivalent to explicit content
- Evaluate only what is present in the document
