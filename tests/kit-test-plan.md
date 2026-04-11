# Quality Assurance Kit — Test Plan

This document contains the structural integrity checks and flow scenario tests for the Quality Assurance Kit. These tests verify that the kit is complete, internally consistent, and capable of producing valid artifacts.

---

## Structural Integrity Checks

Structural checks verify that the kit's files are present, properly named, and internally consistent. These checks do not require AI — they are verifiable by inspection.

### S-01: Four-File Completeness

**Check:** Every governed artifact type has exactly four files: spec, template, prompt, validator.

| Artifact Type | Spec | Template | Prompt | Validator |
|---------------|------|----------|--------|-----------|
| QAER | docs/specs/qaer-spec.md | docs/artifacts/qaer-template.md | *(human-authored — no prompt)* | docs/validators/qaer-validator.md |
| VP | docs/specs/vp-spec.md | docs/artifacts/vp-template.md | docs/prompts/vp-prompt.md | docs/validators/vp-validator.md |
| TCR | docs/specs/tcr-spec.md | docs/artifacts/tcr-template.md | docs/prompts/tcr-prompt.md | docs/validators/tcr-validator.md |
| QGR | docs/specs/qgr-spec.md | docs/artifacts/qgr-template.md | docs/prompts/qgr-prompt.md | docs/validators/qgr-validator.md |

**Expected result:** All files present.

*Note: The QAER is human-authored and does not have a generation prompt. This is consistent with the entry gate pattern (see RRK Service Reliability Entry Record, REK Release Entry Record). The spec, template, and validator constitute three of the four files; the fourth is intentionally absent for human-authored entry gates.*

---

### S-02: Hard Gate Count Alignment

**Check:** Each spec's declared hard gate count matches the validator's gate checks.

| Artifact Type | Spec Hard Gates | Validator Gates |
|---------------|----------------|----------------|
| QAER | 5 | 5 |
| VP | 4 | 4 |
| TCR | 4 | 4 |
| QGR | 5 | 5 |

**Expected result:** Counts match for all four artifact types.

---

### S-03: Hard Gate Name Alignment

**Check:** Gate names in specs match gate names in validators (exact string match for JSON output field names).

| Artifact | Spec Gate Names | Validator Gate Names |
|----------|----------------|---------------------|
| QAER | document_control, upstream_reference, qa_owner, test_infrastructure, integration_points | document_control, upstream_reference, qa_owner, test_infrastructure, integration_points |
| VP | test_scope_coverage, traceability, environment_specification, acceptance_criteria | test_scope_coverage, traceability, environment_specification, acceptance_criteria |
| TCR | plan_execution, evidence_completeness, defect_tracking, environment_verification | plan_execution, evidence_completeness, defect_tracking, environment_verification |
| QGR | tcr_completeness, disposition_justified, defect_assessment, coverage_assessment, risk_assessment | tcr_completeness, disposition_justified, defect_assessment, coverage_assessment, risk_assessment |

**Expected result:** All gate names match exactly.

---

### S-04: Prompt-to-Spec Reference Integrity

**Check:** Each generation prompt references the correct spec and template. No prompt inlines content rules.

| Prompt | References Spec | References Template | Inlines Rules? |
|--------|----------------|--------------------|----|
| vp-prompt.md | docs/specs/vp-spec.md | docs/artifacts/vp-template.md | No |
| tcr-prompt.md | docs/specs/tcr-spec.md | docs/artifacts/tcr-template.md | No |
| qgr-prompt.md | docs/specs/qgr-spec.md | docs/artifacts/qgr-template.md | No |

**Expected result:** All prompts reference correct spec and template; no inlined rules.

---

### S-05: Validator-to-Spec Reference Integrity

**Check:** Each validator references its spec as the source of truth. Validators do not reference prompts.

| Validator | References Spec | References Prompt? |
|-----------|-----------------|-------------------|
| qaer-validator.md | docs/specs/qaer-spec.md | No |
| vp-validator.md | docs/specs/vp-spec.md | No |
| tcr-validator.md | docs/specs/tcr-spec.md | No |
| qgr-validator.md | docs/specs/qgr-spec.md | No |

**Expected result:** All validators reference the correct spec; none reference prompts.

---

### S-06: Template Section Alignment

**Check:** Each template's section headings match the required sections listed in the corresponding spec.

| Artifact | Spec Required Sections | Template Sections |
|----------|----------------------|-------------------|
| QAER | Document Control, Upstream Reference, QA Owner, Test Infrastructure Confirmation, Integration Points, Completeness Checklist, Freeze Declaration | §1-§7 (all present) |
| VP | Document Control, Test Scope, Traceability Matrix, Test Environments, Acceptance Criteria, Risks and Assumptions, Completeness Checklist | §1-§7 (all present) |
| TCR | Document Control, Campaign Summary, Test Results, Environment Verification, Defect Summary, Coverage Assessment, Deviations | §1-§7 (all present) |
| QGR | Document Control, TCR Summary, Coverage Assessment, Defect Assessment, Risk Assessment, Quality Disposition, Completeness Checklist | §1-§7 (all present) |

**Expected result:** All template sections match spec required sections.

---

## Flow Scenario Tests

Flow scenarios verify that the kit's artifacts, when produced in order with appropriate inputs, pass validation. These tests require AI execution.

---

### F-00: Normal Flow — All Tests Pass

**Scenario:** Receive a frozen ORD with EEK artifacts → complete QAER → generate VP → execute all tests (all pass, no defects) → generate TCR → generate QGR with PASS disposition.

**Setup:**
- Provide: a frozen ORD with complete EEK artifacts (SAD with 3 integration points, TDD with test strategy, ACF with performance/security constraints, WDD with work items)
- Complete QAER manually using the QAER template
- Validate QAER → freeze → generate VP → validate → freeze
- Provide test execution evidence: all tests pass, no defects, all environments as specified
- Generate TCR → validate → freeze
- Generate QGR → validate → freeze

**Expected outcomes:**
- QAER: all 5 gates PASS
- VP: all 4 gates PASS; all 3 SAD integration points addressed; traceability complete
- TCR: all 4 gates PASS; 100% coverage; no defects; environment verified
- QGR: all 5 gates PASS; disposition is PASS; no residual risks

**Key gate to verify:** QGR Gate 2 (disposition_justified) — confirm PASS is supported by evidence: all tests pass, no defects, full coverage.

---

### F-01: Flow with Defects — CONDITIONAL Disposition

**Scenario:** Some tests fail, defects found. High severity defect accepted with rationale. QGR produces CONDITIONAL disposition.

**Setup:**
- Use frozen VP from F-00
- Provide test execution evidence: 2 integration tests fail; 1 high defect found (accepted with business justification); 1 medium defect found (deferred)
- Generate TCR with failures and defects
- Validate TCR → freeze
- Generate QGR with defect status showing accepted high defect

**Expected outcomes:**
- TCR: all 4 gates PASS; failures documented with evidence; defects listed with severity and status
- QGR: all 5 gates PASS; disposition is CONDITIONAL; conditions state the accepted high defect; risk assessment documents residual risk from accepted defect

**Key gate to verify:** QGR Gate 3 (defect_assessment) — confirm high defect has specific acceptance rationale (not "low risk").

---

### F-02: Flow with Critical Defect — FAIL Disposition

**Scenario:** Critical defect found during testing. QGR produces FAIL disposition.

**Setup:**
- Use frozen VP from F-00
- Provide test execution evidence: critical defect found in integration test; test blocked by the defect; coverage gap in affected area
- Generate TCR with critical defect and blocked tests
- Validate TCR → freeze
- Generate QGR with open critical defect

**Expected outcomes:**
- TCR: all 4 gates PASS; critical defect documented; blocked tests documented with reason
- QGR: all 5 gates PASS; disposition is FAIL; blocking issues list the critical defect; coverage gap documented in risk assessment

**Key gate to verify:** QGR Gate 2 (disposition_justified) — confirm FAIL is the only valid disposition when an open critical defect exists. A PASS or CONDITIONAL with an open critical defect would fail this gate.

---

## Notes

- All structural checks (S-01 through S-06) should be verified before running flow scenarios.
- Flow scenarios F-00 through F-02 cover the three disposition outcomes (PASS, CONDITIONAL, FAIL).
- Additional scenarios may be added as new patterns are identified in production use.
