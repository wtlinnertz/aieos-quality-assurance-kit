# Test Campaign Record — Specification

Version: v1.0

The Test Campaign Record (TCR) documents the execution of the Verification Plan: test results, environment used, defects found, and coverage achieved. It is an evidence artifact — it captures what was tested and what the results were, not what should have been tested.

One or more TCRs may be generated per VP. Each TCR covers a specific test campaign (e.g., integration tests, performance tests). Collectively, all TCRs for an initiative must cover the full VP scope.

---

## What This Artifact Is Not

- **Not a Verification Plan.** The TCR documents execution results; it does not define what to test. Test scope is defined in the VP.
- **Not a defect report.** The TCR references defects found during testing but is not the primary defect tracking mechanism. Defects are tracked in the organization's defect management system.
- **Not a quality disposition.** The TCR reports evidence; the QGR interprets the evidence and declares a disposition.

---

## Purpose

The TCR serves three roles:

1. **Evidence record** — Captures concrete test execution evidence (results, metrics, logs) so that the QGR has factual basis for disposition
2. **Coverage accounting** — Maps executed tests back to the VP to assess what percentage of the verification plan was completed
3. **Defect inventory** — Lists all defects found during the campaign with severity, status, and resolution information

---

## Upstream Dependencies

- Frozen VP — the plan this campaign executes against
- Test execution evidence — results, logs, screenshots, metrics from test execution
- Defect records — defects found during testing

---

## Required Sections

1. Document Control
2. Campaign Summary
3. Test Results
4. Environment Verification
5. Defect Summary
6. Coverage Assessment
7. Deviations

---

## Content Rules

### Document Control
**Rules**
- TCR ID must be present (format: TCR-{PROJECT}-{NNN})
- Date must be present
- VP reference must be present (VP ID)
- Campaign scope must be stated (which VP test categories this TCR covers)

**Failure Examples**
- Missing TCR ID
- VP ID not referenced
- Campaign scope absent

### Campaign Summary
**Rules**
- Campaign dates (start and end) must be stated
- Campaign scope must describe which VP test categories were executed
- Test execution team must be identified
- Overall campaign result must be stated (all tests passed, N failures, N blocked)

**Failure Examples**
- Campaign dates missing
- Scope described as "everything" without specifying VP categories
- Execution team not identified

### Test Results
**Rules**
- Results must be organized by VP test category
- For each test: test ID (matching VP traceability matrix), result (pass/fail/blocked/skipped), and evidence reference must be present
- Evidence must be concrete: log references, metric values, screenshots, or equivalent — not assertions ("test passed")
- Failed tests must include failure description
- Blocked or skipped tests must include reason

**Failure Examples**
- Results not organized by VP category
- Tests listed as "passed" without evidence reference
- Failed tests without failure description
- Blocked tests without reason

### Environment Verification
**Rules**
- The environment used must be identified (matching VP environment specification)
- Configuration details must be stated (versions, settings, data state)
- Any deviation from the VP-specified environment must be documented with impact assessment
- Confirmation that tests ran in the specified environment, not a substitute

**Failure Examples**
- Environment not identified
- Configuration details absent
- Deviation from VP environment not documented

### Defect Summary
**Rules**
- All defects found during the campaign must be listed
- Each defect must have: defect ID, description, severity (critical/high/medium/low), status (open/fixed/deferred/accepted), and affected component
- If no defects were found, this must be explicitly stated (not an empty section)
- Severity must follow consistent definitions (reference organizational defect policy if available)

**Failure Examples**
- Defects found but not listed
- Defects listed without severity
- Defects listed without status
- Empty section (no explicit "no defects found" statement)

### Coverage Assessment
**Rules**
- Coverage must be stated as: tests executed / tests planned (from VP)
- Coverage must be broken down by VP test category
- If coverage is below 100%, the gap must be explained (which tests were not executed and why)
- Coverage is a factual report, not a judgment — the QGR interprets coverage

**Failure Examples**
- Coverage stated as a percentage without denominator
- Coverage not broken down by category
- Coverage gaps not explained

### Deviations
**Rules**
- Any deviation from the VP must be documented: tests added, tests removed, scope changed, environment changed
- Each deviation must have a reason and impact assessment
- If no deviations occurred, this must be explicitly stated

**Failure Examples**
- Deviations occurred but not documented
- Deviations without reason
- Section empty without explicit "no deviations" statement

---

## Format Requirements

- TCR ID must follow the standard format
- Test results must be in table format for clarity
- Defect summary must be in table format
- Coverage must be expressed as executed/planned with percentages

---

## Completeness Rules

- All VP test categories covered by this campaign must have results
- All tests executed must have evidence references
- All defects must have severity and status
- Coverage assessment must account for all VP tests in scope

---

## Relationship Rules

- The TCR references a specific frozen VP by ID
- Multiple TCRs may reference the same VP (one per campaign)
- All TCRs for an initiative must collectively cover the full VP scope
- The QGR aggregates TCR results — TCRs must be frozen before QGR generation

---

## Hard Gates

1. **plan_execution** — All VP tests in this campaign's scope are accounted for: executed with results, or deviation documented with reason; VP ID referenced
2. **evidence_completeness** — Every test result has a concrete evidence reference (log, metric, screenshot); no assertions without evidence; failed tests have failure descriptions
3. **defect_tracking** — All defects found are listed with defect ID, severity, status, and affected component; or explicit "no defects found" statement
4. **environment_verification** — Tests ran in the VP-specified environment (or deviation documented with impact assessment); environment configuration details present
