# Quality Assurance Entry Record

---

## 1. Document Control

| Field | Value |
|-------|-------|
| QAER ID | QAER-{PROJECT}-{NNN} |
| Date | {YYYY-MM-DD} |
| Initiative Summary | {1-2 sentences identifying the initiative and the scope of quality verification} |
| Status | Draft / Validated / Freeze Pending / Frozen |
| Governance Model Version | 1.0 |
| Prompt Version | N/A |
| Spec Version | {spec version} |
| Principles Version | {principles file versions} |

---

## 2. Upstream Reference

**Operational Readiness Document ID:** {ORD-{PROJECT}-{NNN}}

**ORD Status:** {Frozen}

### EEK Artifacts

| Artifact | ID | Status |
|----------|----|--------|
| Operational Readiness Document (ORD) | {ORD-{PROJECT}-{NNN}} | {Frozen} |
| System Architecture Document (SAD) | {SAD-{PROJECT}-{NNN}} | {Frozen} |
| Technical Design Document (TDD) | {TDD-{PROJECT}-{NNN}} | {Frozen} |
| Architecture Context File (ACF) | {ACF-{PROJECT}-{NNN}} | {Frozen} |
| Work Decomposition Document (WDD) | {WDD-{PROJECT}-{NNN}} | {Frozen} |

---

## 3. QA Owner

| Field | Value |
|-------|-------|
| Name | {named individual — not a team} |
| Contact | {channel, email, or equivalent} |
| Scope | {which initiative or component this person owns for this verification} |

---

## 4. Test Infrastructure Confirmation

### Test Environments

| Environment | Purpose | Readiness Status | Notes |
|-------------|---------|-----------------|-------|
| {environment name} | {integration / performance / staging / etc.} | Ready / Provisioning (target: YYYY-MM-DD) / Not Available (blocker: ...) | |
| {environment name} | {purpose} | {status} | |

### Test Data

{Confirm test data availability. Document any gaps or dependencies.}

### Test Tooling

| Tool | Purpose |
|------|---------|
| {tool name} | {test framework / test runner / monitoring / etc.} |
| {tool name} | {purpose} |

---

## 5. Integration Points

{Extract from SAD. For each integration point that requires cross-component verification:}

| # | Components | Interface Type | Verification Concern | SAD Reference |
|---|-----------|---------------|---------------------|---------------|
| 1 | {component A} ↔ {component B} | {API / event / shared data / etc.} | {what could go wrong at this boundary} | {SAD §N or heading} |
| 2 | {component A} ↔ {component C} | {interface type} | {verification concern} | {SAD reference} |

*If the SAD identifies no integration points requiring cross-component verification, state: "No cross-component integration points identified. Justification: {reason}."*

---

## 6. Completeness Checklist

Before freezing this record, confirm:

- [ ] QAER ID, date, and initiative summary present
- [ ] ORD ID referenced and status confirmed as Frozen
- [ ] SAD, TDD, ACF, WDD listed with IDs and confirmed as Frozen
- [ ] Named QA owner with contact information and scope
- [ ] At least one test environment listed with readiness status
- [ ] Test data availability addressed
- [ ] Test tooling listed
- [ ] Integration points extracted from SAD (or explicit "none" with justification)

---

## 7. Freeze Declaration

By freezing this record, the QA owner confirms:
- The upstream ORD is in Frozen status
- All required EEK artifacts (SAD, TDD, ACF, WDD) are available and in Frozen status
- Test infrastructure is confirmed ready or provisioning gaps are documented
- Integration points from the SAD have been identified for cross-component verification
- This record is complete and authorizes VP generation to begin

**Frozen by:** {name}
**Freeze date:** {YYYY-MM-DD}
**Status:** Frozen
