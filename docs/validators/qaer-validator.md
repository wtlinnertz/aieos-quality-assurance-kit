# Quality Assurance Entry Record — Validator

This validator evaluates a completed Quality Assurance Entry Record (QAER) against `docs/specs/qaer-spec.md`. It is used in a separate AI session from the one that completed the QAER.

**Validator role:** Judge pass/fail. Do not suggest improvements, redesign content, or offer alternatives. Evaluate only what is explicitly present.

---

## Inputs Required

To run this validator, provide:
1. The completed QAER (full document)
2. `docs/specs/qaer-spec.md` (the spec — use this as the authoritative rules)

Do not use any other document as the source of truth for pass/fail criteria.

---

## Evaluation Procedure

Evaluate each hard gate in order. For each gate, apply the rules exactly as stated in the spec. Do not infer intent. Do not give partial credit. Ambiguity in the artifact is a failure condition — if you cannot determine whether a gate passes from what is explicitly present, the gate fails.

---

## Hard Gate Checks

### Gate 1: document_control

Check:
- QAER ID is present and matches the format `QAER-{PROJECT}-{NNN}`
- Date is present (any standard date format is acceptable)
- Initiative summary is present and contains at least 1-2 sentences (not blank, not "TBD")

**Pass:** All three fields present and non-empty with required content.
**Fail:** Any field absent, blank, or "TBD."

---

### Gate 2: upstream_reference

Check:
- An ORD ID is present and matches the format `ORD-{PROJECT}-{NNN}` (or similar structured ID)
- ORD status is stated as **Frozen** (not Draft, not Validated, not Approved)
- The following EEK artifacts are listed with IDs and confirmed as Frozen: SAD, TDD, ACF, WDD
- All four EEK artifacts must be present with structured IDs and Frozen status

**Pass:** All sub-checks pass.
**Fail:** ORD ID absent; ORD status not Frozen; any of SAD, TDD, ACF, WDD missing or not confirmed as Frozen.

---

### Gate 3: qa_owner

Check:
- A named individual is present (a person's name — not a team name, not a role title like "QA Team" or "Test Lead")
- Contact information is present (channel, email, or equivalent)
- Scope is stated (which initiative or component this person owns for this verification)

**Pass:** All three fields present for a named individual.
**Fail:** "Team" or role title listed instead of a person's name; contact absent; scope absent.

---

### Gate 4: test_infrastructure

Check:
- At least one test environment is listed with a readiness status (Ready, Provisioning with target date, or Not Available with blocker)
- Test data availability is addressed (confirmed available or gaps documented)
- Test tooling is listed (at least one tool with its purpose)

**Pass:** At least one environment with readiness status; test data addressed; tooling listed.
**Fail:** No environments listed; readiness status absent; test data not addressed; tooling not listed.

---

### Gate 5: integration_points

Check:
- Integration points are extracted from the SAD
- Each integration point identifies: components involved, interface type (API, event, shared data, etc.), and verification concern
- SAD reference is cited for each integration point (section number or heading)
- If no integration points exist, explicit "none" statement with justification is present

**Pass:** At least one integration point with all required fields and SAD reference; or explicit "none" with justification.
**Fail:** Section empty without justification; integration points without SAD reference; components or interface types not specified; verification concerns absent.

---

## Output Format

Produce a JSON result in exactly this format:

```json
{
  "status": "PASS | FAIL",
  "summary": "<one sentence verdict>",
  "hard_gates": {
    "document_control": "PASS | FAIL",
    "upstream_reference": "PASS | FAIL",
    "qa_owner": "PASS | FAIL",
    "test_infrastructure": "PASS | FAIL",
    "integration_points": "PASS | FAIL"
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
- Do not redesign or improve the QAER
- Do not evaluate content quality beyond what the spec requires
- Do not accept inferred information as equivalent to explicit content
- Evaluate only what is present in the document
