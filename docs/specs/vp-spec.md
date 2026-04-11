# Verification Plan — Specification

Version: v1.0

The Verification Plan (VP) defines the test scope beyond unit tests for an initiative entering quality assurance. It covers integration tests, system tests, performance tests, and security tests. It is derived from the SAD integration points, TDD test strategy, and ACF constraints.

The VP is the authoritative document that defines what must be tested, how it will be tested, and what constitutes a pass for each test category. All Test Campaign Records (TCRs) are executed against this plan.

---

## What This Artifact Is Not

- **Not a test case document.** The VP defines test scope, categories, and acceptance criteria — not individual step-by-step test procedures. Detailed test cases are execution artifacts outside QAK governance.
- **Not a test result.** The VP defines what to test; the TCR documents what was tested and the results.
- **Not a unit test plan.** Unit tests are the responsibility of EEK. The VP covers integration, system, performance, and security testing.

---

## Purpose

The VP serves three roles:

1. **Scope definition** — Enumerates all verification activities required beyond unit tests, ensuring no integration point or constraint is left unverified
2. **Traceability map** — Maps verification activities to upstream requirements (WDD work items, SAD integration points, ACF constraints) so coverage can be assessed
3. **Acceptance criteria** — Defines measurable pass/fail criteria for each test category so the TCR has objective standards to report against

---

## Upstream Dependencies

- Frozen QAER — confirms readiness and identifies integration points
- SAD — integration points and component boundaries
- TDD — test strategy and unit test coverage baseline
- ACF — performance, security, and compliance constraints
- WDD — work items for traceability

**Optional inputs (if SCK adopted):**
- Frozen Threat Model (TM) from SCK — informs security test scenarios in the VP's security test category
- Frozen Security Assessment Record (SAR) from SCK — informs security verification scope and specific checks to include

These SCK inputs are optional. The VP must function without them — the security test category uses ACF security constraints as the baseline when SCK artifacts are not available.

---

## Required Sections

1. Document Control
2. Test Scope
3. Traceability Matrix
4. Test Environments
5. Acceptance Criteria
6. Risks and Assumptions
7. Completeness Checklist

---

## Content Rules

### Document Control
**Rules**
- VP ID must be present (format: VP-{PROJECT}-{NNN})
- Date must be present
- Initiative reference must be present (QAER ID and ORD ID)
- Version must be stated

**Failure Examples**
- Missing VP ID
- QAER ID or ORD ID not referenced

### Test Scope
**Rules**
- Each test category must be listed: integration, system, performance, security
- For each category: scope description, specific areas to test, and out-of-scope items must be stated
- All QAER integration points must be addressed in the integration test scope
- If a standard test category is not applicable, explicit justification must be provided
- Additional test categories (e.g., accessibility, compliance) may be included but do not replace the four standard categories

**Failure Examples**
- Test category missing without justification
- QAER integration point not addressed in integration scope
- Scope described as "all components" without specifics
- Out-of-scope not stated

### Traceability Matrix
**Rules**
- Each test area must trace to at least one upstream requirement: WDD work item, SAD integration point, or ACF constraint
- Traceability must be bidirectional: every QAER integration point must have at least one test; every test must trace to at least one requirement
- Format: table with columns for Test ID, Test Description, Upstream Reference (WDD/SAD/ACF), and Test Category

**Failure Examples**
- Tests listed without upstream references
- QAER integration points with no corresponding tests
- Traceability matrix absent

### Test Environments
**Rules**
- Each test category must specify the environment where tests will execute
- Environment specification must include: environment name, configuration details, data requirements, and isolation level
- If multiple categories share an environment, this must be stated explicitly
- Environment availability must be confirmed or contingency documented

**Failure Examples**
- Environment not specified for a test category
- Configuration details absent (just an environment name)
- Data requirements not addressed

### Acceptance Criteria
**Rules**
- Each test category must have measurable pass/fail criteria
- Integration test criteria: specific integration scenarios that must pass; coverage threshold if applicable
- System test criteria: end-to-end scenarios with expected outcomes
- Performance test criteria: numeric thresholds (latency, throughput, resource usage) with measurement methodology
- Security test criteria: specific checks to pass (vulnerability scan clean, authentication verified, authorization enforced)
- Criteria must be objective — "acceptable performance" is not measurable; "p99 latency < 200ms under 100 concurrent users" is measurable

**Failure Examples**
- "All tests pass" without specifying what tests
- Qualitative criteria ("good performance," "secure enough")
- Missing criteria for a test category
- Criteria that cannot be objectively measured

### Risks and Assumptions
**Rules**
- At least one risk or assumption must be documented
- Each risk must have a mitigation approach
- Assumptions about test data, environment availability, or third-party dependencies must be stated

**Failure Examples**
- Section empty
- Risks listed without mitigations

---

## Format Requirements

- VP ID must follow the standard format
- All test categories must use consistent naming (integration, system, performance, security)
- Traceability matrix must be a table, not prose
- Acceptance criteria must include numeric values where applicable

---

## Completeness Rules

- All four standard test categories addressed or explicitly excluded with justification
- All QAER integration points mapped to integration tests
- Traceability matrix present with bidirectional mapping
- Acceptance criteria present for every included test category
- At least one test environment specified

---

## Relationship Rules

- The VP gates TCR generation — no TCR may be generated until the VP is frozen
- The VP does not replace the TDD — the TDD covers unit test strategy; the VP covers verification beyond unit tests
- TCRs must reference the VP and report results against VP acceptance criteria
- The QGR assesses coverage against VP scope — tests not in the VP are not counted toward coverage

---

## Hard Gates

1. **test_scope_coverage** — All four standard test categories (integration, system, performance, security) addressed or explicitly excluded with justification; all QAER integration points addressed in integration test scope
2. **traceability** — Traceability matrix present; every QAER integration point has at least one test; every test traces to at least one upstream requirement (WDD, SAD, or ACF)
3. **environment_specification** — At least one test environment specified with configuration details, data requirements, and isolation level for each included test category
4. **acceptance_criteria** — Measurable pass/fail criteria present for each included test category; criteria are objective and specific (numeric thresholds where applicable)
