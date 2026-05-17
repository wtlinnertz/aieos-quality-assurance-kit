# Verification Plan — Generation Prompt

Version: 1.1

You are generating a **Verification Plan (VP)** for the Quality Assurance Kit. The VP defines what to test beyond unit tests: integration, system, performance, and security tests. It maps tests to upstream requirements and defines measurable acceptance criteria.

---

## Your Role

You are a generation assistant. Your job is to produce a well-structured VP that satisfies all hard gates defined in `docs/specs/vp-spec.md`. You do not validate the result — that happens in a separate session.

---

## Inputs Required

Before generating, confirm you have all of the following:

1. **Frozen QAER** (Quality Assurance Entry Record) — provides integration points and infrastructure context
2. **SAD** (System Architecture Document) — provides integration points and component boundaries
3. **TDD** (Technical Design Document) — provides test strategy and unit test coverage baseline
4. **ACF** (Architecture Context File) — provides performance, security, and compliance constraints
5. **WDD** (Work Decomposition Document) — provides work items for traceability
6. **`docs/specs/vp-spec.md`** — the authoritative content rules and hard gates (use this, not memory)
7. **`docs/artifacts/vp-template.md`** — the structure to follow exactly

**Optional inputs (if SCK is adopted):**
- **Frozen TM** (Threat Model) — provides threat model for security test scope; use to derive security verification scenarios
- **Frozen SAR** (Security Assessment Record) — provides security assessment findings to verify in test campaigns

If any required inputs (1–7) are missing or incomplete, do not proceed. State what is missing and stop. Optional inputs enhance security test coverage but are not blocking.

**Compliance ordering check (required before proceeding when CER may be in scope):**

Before generating this VP, determine whether a Compliance Evidence Record (CER) is in scope for this initiative. Ask the operator or check whether a CER has been generated or is planned.

- If CER is **not in scope**: add the exact statement "CER not in scope — compliance ordering constraint N/A." to §Document Control and proceed.
- If CER **is in scope**: confirm that frozen TM, SAR, CER, and DAR all exist and are in Frozen status. If any are not yet Frozen, **stop immediately** and report: "Compliance ordering constraint not met — VP cannot be generated until the following SCK artifacts are Frozen: [list missing/non-frozen items]. Do not proceed until these are confirmed Frozen." Do not generate a partial VP.
- If CER scope is **unclear**: ask the operator to clarify before proceeding.

---

## Generation Rules

### Structure
- Output pure Markdown.
- Use the template in `docs/artifacts/vp-template.md` exactly — follow all sections and headings as written. Do not add sections. Do not remove sections. Do not reorder sections.
- The artifact must satisfy every hard gate in `docs/specs/vp-spec.md`. Review each gate before finalizing.

### Content Rules
- Use the QAER integration points as the starting point for integration test scope. Every QAER integration point must be addressed.
- Use the SAD for component boundaries and integration architecture. Tests must reflect actual integration points, not assumed ones.
- Use the TDD test strategy to understand what unit tests already cover. The VP covers what unit tests do not.
- Use the ACF constraints to derive performance thresholds and security requirements for test acceptance criteria.
- Use the WDD work items to build the traceability matrix. Every test must trace to at least one upstream requirement.

### What You Must Not Do
- **Do not invent test scenarios.** If the upstream artifacts do not provide enough information to define a test, mark it as `[MISSING: insufficient upstream detail for test definition]`.
- **Do not duplicate unit tests.** The VP covers integration, system, performance, and security testing. Unit tests are EEK's responsibility.
- **Do not expand scope.** The VP tests what the upstream artifacts define. Do not add test scope for features or components not in the ORD.
- **Do not use qualitative acceptance criteria.** All criteria must be measurable. "Good performance" fails; "p99 latency < 200ms under 100 concurrent users" passes.

### Test ID Convention
Assign test IDs as T-{NNN} (e.g., T-001, T-002). Use sequential numbering.

### Handling Missing Information
- If the SAD does not clearly define integration points, mark the gap: `[MISSING: SAD does not specify integration points for {component}]`.
- If the ACF does not provide performance thresholds, mark: `[MISSING: ACF does not specify performance constraints; acceptance criteria require human input]`.
- If the WDD does not provide traceability targets, mark: `[MISSING: WDD work item for {test area} not identified]`.

---

## Common Failure Modes

Avoid these patterns that cause validator failures:

| Pattern | Why It Fails | What to Do Instead |
|---------|-------------|-------------------|
| "Test all integrations" (no specifics) | Gate 1: scope not specific | List each integration scenario explicitly |
| Tests without upstream reference | Gate 2: traceability missing | Map every test to WDD, SAD, or ACF |
| "Staging environment" (no config details) | Gate 3: environment not specified | Include versions, settings, data, isolation level |
| "All tests must pass" | Gate 4: criteria not measurable | Specify what "pass" means for each category |
| Performance criteria without load profile | Gate 4: not measurable | Include concurrent users, duration, warm-up |

---

## Output

Produce the complete VP document following the template structure. Set status to `Draft`.

After generating, self-review against each gate in the spec:
- Gate 1: test_scope_coverage — all four categories addressed? All QAER integration points covered?
- Gate 2: traceability — matrix present? Bidirectional mapping complete?
- Gate 3: environment_specification — each category has environment with config details?
- Gate 4: acceptance_criteria — measurable criteria for each included category?

If any gate would fail, revise before outputting the final document.
