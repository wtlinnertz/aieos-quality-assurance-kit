# How to Adapt This Kit

This kit provides the structure, rules, and prompts for quality assurance governance. Adapting it to your organization means filling in the content that is specific to your context — without modifying the governance structure.

---

## What to Adapt

### Organizational Principles

**Directory:** `docs/principles/`

This directory is initially empty. Populate it with your organization's QA policies. Example files you might create:

- **testing-standards.md** — What test types are required? What coverage thresholds apply? What constitutes sufficient evidence?
- **defect-management-policy.md** — Severity definitions, resolution SLAs, escalation criteria, acceptance criteria for shipping with known defects.
- **environment-management-policy.md** — Environment provisioning standards, data management, isolation requirements.

Replace the defaults with your organization's actual policies. Keep the structure (numbered sections, clear policy statements) but change the content to match your standards.

### Test Categories

The VP template includes integration, system, performance, and security test categories. If your organization uses additional categories (e.g., accessibility, compliance, chaos engineering), add them to the VP template and update the VP spec to include them in the test_scope_coverage gate.

### Coverage Thresholds

The VP spec requires measurable acceptance criteria for each test category. If your organization has standard coverage thresholds (e.g., 80% integration test coverage, p99 latency under 200ms), document them in your principles files and reference them during VP generation.

### Defect Severity Definitions

The TCR and QGR specs reference defect severity levels (critical, high, medium, low). If your organization uses different severity definitions, document them in a principles file and ensure the TCR prompt references your definitions.

---

## What Not to Adapt

### Specs

The specs define what makes an artifact valid. Do not soften hard gates to make artifacts easier to produce. If a hard gate is failing consistently, that usually means the artifact is incomplete — not that the gate is wrong.

If you genuinely need to add a hard gate (your organization requires something the spec does not check), add it. Do not remove existing hard gates.

### Validator Logic

Validators evaluate against specs. If a validator is producing unexpected results, check whether the spec accurately captures your requirements — and adjust the spec if needed, not the validator.

### Governance Model

`docs/governance-model.md` is a synchronized copy of the canonical governance model. Do not edit it. If you believe the governance model should change, update `aieos-governance-foundation/governance-model.md` and sync all kit copies.

---

## Adding Artifact Types

If your organization needs additional governed artifacts (e.g., an accessibility verification record, a compliance test record), follow the four-file system:

1. Write the spec first — define the hard gates before writing anything else
2. Write the validator — this forces you to verify the spec is evaluable
3. Write the template — structure only, no content rules
4. Write the prompt — generation behavior, references spec and template

Register the new artifact type in the playbook, index, and CLAUDE.md.

---

## Tool Bindings

This kit is tool-agnostic. Templates use generic placeholders for test tools, defect trackers, and CI/CD systems.

When adopting this kit, create a bindings document:

```
docs/bindings/
  test-framework-mapping.md    # Maps test categories to your test frameworks
  defect-tracker-mapping.md    # Maps defect fields to your issue tracker
  ci-cd-mapping.md             # Maps test execution to your CI/CD pipeline
```

Bindings are not governed artifacts — they have no spec, validator, or prompt. Update them when your tooling changes without touching the governed files.

---

## First-Time Setup Checklist

- [ ] Read `docs/playbook.md` fully before beginning
- [ ] Obtain a frozen ORD and supporting EEK artifacts (SAD, TDD, ACF, WDD)
- [ ] Review and populate `docs/principles/` with your organizational QA policies
- [ ] Complete and freeze the QAER
- [ ] Generate and freeze the VP
- [ ] Execute tests and collect evidence
- [ ] Generate and freeze TCR(s)
- [ ] Generate and freeze the QGR
- [ ] Hand off frozen QGR to the Release & Exposure Kit
