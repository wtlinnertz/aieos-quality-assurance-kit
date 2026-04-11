# Quality Assurance Principles

Version: v1.0

This document defines the organization's quality assurance standards and philosophy. It is input material for artifact generation within the Quality Assurance Kit — not a governed artifact. Adapt this file to reflect your organization's actual quality policies.

---

## 1. Verification Scope Matches Risk

- Higher-risk changes require deeper and broader testing; low-risk changes may use lighter verification.
- Risk classification must be explicit — derived from impact analysis, not assumed.
- Testing effort that is uniform regardless of risk wastes resources on low-risk changes and under-invests in high-risk ones.
- When risk is uncertain, treat the change as higher risk until evidence supports reclassification.

## 2. Evidence Over Assertion

- Quality claims must be backed by verifiable test results, not by statements that testing was performed.
- Every quality gate decision must reference specific test execution records, coverage metrics, or validation outputs.
- "We tested it" without evidence is not a quality signal — it is an unverified claim.
- Evidence must be reproducible: another party should be able to inspect the same results and reach the same conclusion.

## 3. Reproducibility

- Test campaigns must be repeatable — the same inputs and environment must produce the same results.
- Test environments, data, and execution steps must be documented sufficiently for independent reproduction.
- Non-reproducible test results are not valid evidence for quality gate decisions.
- When test results vary between executions, the variance itself is a finding that must be investigated before the quality gate can pass.

## 4. Independence

- Quality assessment must be independent from the development team that produced the artifact under test.
- Independence means the assessor has no incentive to pass an artifact that should fail.
- Self-validation (the same session or team that generated an artifact also validating it) is not independent assessment.
- When full organizational independence is not feasible, procedural independence (separate session, separate context) is the minimum acceptable standard.

## 5. Traceability

- Every test must trace to a requirement, acceptance criterion, or identified risk — tests without traceability are untethered from purpose.
- Every requirement or risk must have at least one corresponding test — requirements without test coverage are unverified.
- Traceability enables impact analysis: when a requirement changes, all affected tests can be identified and re-evaluated.
- Coverage gaps identified through traceability analysis must be addressed before the quality gate can pass.

## 6. Timeliness

- Quality gates must occur before release exposure, not after — finding defects in production is not quality assurance.
- Quality verification must be planned into the delivery timeline, not compressed into whatever time remains before a deadline.
- Late quality gates that are rushed to meet deadlines produce unreliable evidence and undermine the gate's purpose.
- When schedule pressure threatens quality gate thoroughness, the correct response is to escalate the schedule risk, not to reduce verification scope.
