# Verification Plan — Validator

This validator evaluates a generated Verification Plan (VP) against `docs/specs/vp-spec.md`. It is used in a separate AI session from the one that generated the VP.

**Validator role:** Judge pass/fail. Do not suggest improvements, redesign content, or offer alternatives. Evaluate only what is explicitly present.

---

## Inputs Required

To run this validator, provide:
1. The generated VP (full document)
2. `docs/specs/vp-spec.md` (the spec — use this as the authoritative rules)

Do not use any other document as the source of truth for pass/fail criteria.

---

## Evaluation Procedure

Evaluate each hard gate in order. For each gate, apply the rules exactly as stated in the spec. Do not infer intent. Do not give partial credit. Ambiguity in the artifact is a failure condition — if you cannot determine whether a gate passes from what is explicitly present, the gate fails.

---

## Hard Gate Checks

### Gate 1: test_scope_coverage

Check:
- All four standard test categories are addressed: integration, system, performance, security
- For each category: scope description, specific areas, and out-of-scope items are present
- If a standard category is excluded, explicit justification is provided
- All QAER integration points are addressed in the integration test scope (check the QAER Integration Point Coverage table)

**Pass:** All four categories addressed (or excluded with justification); all QAER integration points covered.
**Fail:** Category missing without justification; QAER integration point not addressed; scope described without specifics.

---

### Gate 2: traceability

Check:
- Traceability matrix is present as a table
- Every test has at least one upstream reference (WDD work item, SAD section, or ACF constraint)
- Every QAER integration point has at least one test in the matrix
- Bidirectional mapping is complete (no orphaned tests, no uncovered integration points)

**Pass:** Matrix present; all tests traced; all integration points covered; bidirectional.
**Fail:** Matrix absent; tests without upstream reference; integration points without tests.

---

### Gate 3: environment_specification

Check:
- At least one test environment is specified
- Each included test category has an environment assignment
- Each environment specification includes: configuration details, data requirements, and isolation level
- If multiple categories share an environment, this is stated explicitly

**Pass:** Every included test category has an environment with config, data, and isolation specified.
**Fail:** Category without environment; environment without configuration details; data requirements not addressed.

---

### Gate 4: acceptance_criteria

Check:
- Each included test category has measurable pass/fail criteria
- Criteria are objective and specific — not qualitative
- Performance criteria include numeric thresholds with measurement methodology
- Security criteria include specific checks to pass
- Integration criteria include specific scenarios that must pass
- System criteria include end-to-end scenarios with expected outcomes

**Pass:** Measurable criteria present for every included category; criteria are specific and objective.
**Fail:** Category without criteria; qualitative criteria ("good performance"); criteria that cannot be objectively measured.

---

### Gate 5: compliance_sck_ordering (conditional)

Check the §Document Control section for one of two declarations:

**Path A — CER not in scope:**
- §Document Control contains the exact phrase: "CER not in scope — compliance ordering constraint N/A."
- Pass: phrase present and unambiguous.
- Fail: phrase absent, ambiguous, or qualified.

**Path B — CER in scope:**
- §Document Control must reference frozen TM, SAR, CER, and DAR artifact IDs with explicit Frozen status declarations.
- All four must be present: TM ID, SAR ID, CER ID, DAR ID.
- All four must be declared Frozen (not Draft, Approved, or Validated).
- Pass: all four IDs present and all four declared Frozen.
- Fail: any ID missing; any ID not declared Frozen; §Document Control contains no declaration (silence fails).

**Determining which path applies:** If the initiative has a CER in scope and §Document Control omits any declaration about compliance ordering, the gate fails. Do not assume CER is not in scope because it is not mentioned — silence is a failure.

---

## Output Format

Produce a JSON result in exactly this format:

```json
{
  "status": "PASS | FAIL",
  "summary": "<one sentence verdict>",
  "hard_gates": {
    "test_scope_coverage": "PASS | FAIL",
    "traceability": "PASS | FAIL",
    "environment_specification": "PASS | FAIL",
    "acceptance_criteria": "PASS | FAIL",
    "compliance_sck_ordering": "PASS | FAIL"
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
- Do not redesign or improve the VP
- Do not evaluate content quality beyond what the spec requires
- Do not accept inferred information as equivalent to explicit content
- Evaluate only what is present in the document
