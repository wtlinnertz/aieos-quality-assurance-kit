# Quality Assurance Kit — Documentation Index

This kit governs verification campaigns, integration/system testing, and pre-release quality gates. It is Layer 9 of the AIEOS system.

---

## Start Here

| Document | Purpose |
|----------|---------|
| `playbook.md` | End-to-end process definition — read this first |
| `how-to-use-with-ai.md` | AI session setup and tool guidance |
| `how-to-adapt.md` | Organizational adoption guidance |
| `governance-model.md` | AIEOS structural rules (reference) |

---

## Artifact Governing Files

### Step 0 — Quality Assurance Entry Record (QAER)

| File | Location | Purpose |
|------|----------|---------|
| Spec | `specs/qaer-spec.md` | Content rules and 5 hard gates |
| Template | `artifacts/qaer-template.md` | Entry record structure |
| Prompt | *(human-authored — no generation prompt)* | — |
| Validator | `validators/qaer-validator.md` | Pass/fail evaluation |

### Step 1 — Verification Plan (VP)

| File | Location | Purpose |
|------|----------|---------|
| Spec | `specs/vp-spec.md` | Content rules and 4 hard gates |
| Template | `artifacts/vp-template.md` | VP structure |
| Prompt | `prompts/vp-prompt.md` | Generation instructions |
| Validator | `validators/vp-validator.md` | Pass/fail evaluation |

### Step 2 — Test Campaign Record (TCR)

| File | Location | Purpose |
|------|----------|---------|
| Spec | `specs/tcr-spec.md` | Content rules and 4 hard gates |
| Template | `artifacts/tcr-template.md` | TCR structure |
| Prompt | `prompts/tcr-prompt.md` | Generation instructions |
| Validator | `validators/tcr-validator.md` | Pass/fail evaluation |

### Step 3 — Quality Gate Record (QGR)

| File | Location | Purpose |
|------|----------|---------|
| Spec | `specs/qgr-spec.md` | Content rules and 5 hard gates |
| Template | `artifacts/qgr-template.md` | QGR structure |
| Prompt | `prompts/qgr-prompt.md` | Generation instructions |
| Validator | `validators/qgr-validator.md` | Pass/fail evaluation |

---

## Principles

| File | Purpose |
|------|---------|
| `principles/` | Organizational QA policy (initially empty; populate with your organization's testing standards) |

---

## Examples

`examples/` — Worked examples (initially empty; to be populated with a complete QA flow)

---

## Guides

| Document | Purpose |
|----------|---------|
| `entry-from-eek.md` | Boundary briefing when arriving from the Engineering Execution Kit |

---

## Tests

`tests/kit-test-plan.md` — S-01 to S-06 structural checks + F-00 to F-02 flow scenarios
