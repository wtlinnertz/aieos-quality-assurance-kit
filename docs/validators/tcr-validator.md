# Test Campaign Record — Validator

This validator evaluates a generated Test Campaign Record (TCR) against `docs/specs/tcr-spec.md`. It is used in a separate AI session from the one that generated the TCR.

**Validator role:** Judge pass/fail. Do not suggest improvements, redesign content, or offer alternatives. Evaluate only what is explicitly present.

---

## Inputs Required

To run this validator, provide:
1. The generated TCR (full document)
2. `docs/specs/tcr-spec.md` (the spec — use this as the authoritative rules)

Do not use any other document as the source of truth for pass/fail criteria.

---

## Evaluation Procedure

Evaluate each hard gate in order. For each gate, apply the rules exactly as stated in the spec. Do not infer intent. Do not give partial credit. Ambiguity in the artifact is a failure condition — if you cannot determine whether a gate passes from what is explicitly present, the gate fails.

---

## Hard Gate Checks

### Gate 1: plan_execution

Check:
- VP ID is referenced in the document
- All VP tests in this campaign's scope are accounted for: either executed with results, or listed as a deviation with reason
- Campaign scope clearly states which VP test categories are covered
- Blocked or skipped tests have documented reasons

**Pass:** VP ID referenced; all in-scope tests accounted for; deviations documented with reasons.
**Fail:** VP ID not referenced; tests from VP scope not accounted for; blocked/skipped tests without reasons.

---

### Gate 2: evidence_completeness

Check:
- Every test result has a concrete evidence reference (log file, metric value, screenshot reference, or equivalent)
- No test is marked as "passed" without an evidence reference
- Failed tests have a failure description explaining what failed
- Evidence references are specific (not "see logs" or "test output")

**Pass:** All results have evidence references; failed tests have descriptions; evidence references are specific.
**Fail:** Results without evidence; "test passed" without reference; failed tests without failure description; vague evidence references.

---

### Gate 3: defect_tracking

Check:
- All defects found during the campaign are listed
- Each defect has: defect ID, description, severity (critical/high/medium/low), status (open/fixed/deferred/accepted), and affected component
- If no defects were found, an explicit "no defects found" statement is present
- Section is not empty without a statement

**Pass:** All defects listed with required fields; or explicit "no defects found" statement.
**Fail:** Defects found but not listed; defects without severity or status; empty section without statement.

---

### Gate 4: environment_verification

Check:
- The environment used is identified (matching VP §4 environment specification)
- Configuration details are present (versions, settings, data state)
- If the environment deviates from VP specification, the deviation is documented with impact assessment
- VP Environment Match field is present (Yes or No with deviation reference)

**Pass:** Environment identified; config details present; VP match confirmed or deviation documented.
**Fail:** Environment not identified; config details absent; deviation from VP not documented.

---

## Output Format

Produce a JSON result in exactly this format:

```json
{
  "status": "PASS | FAIL",
  "summary": "<one sentence verdict>",
  "hard_gates": {
    "plan_execution": "PASS | FAIL",
    "evidence_completeness": "PASS | FAIL",
    "defect_tracking": "PASS | FAIL",
    "environment_verification": "PASS | FAIL"
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
- Do not redesign or improve the TCR
- Do not evaluate content quality beyond what the spec requires
- Do not accept inferred information as equivalent to explicit content
- Evaluate only what is present in the document
