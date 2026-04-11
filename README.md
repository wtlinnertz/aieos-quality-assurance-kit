# aieos-quality-assurance-kit

**Layer 9 of the AIEOS system — Quality Assurance**

This kit governs verification campaigns, integration/system testing, and pre-release quality gates. It receives a frozen Operational Readiness Document (ORD) from the Engineering Execution Kit and produces a Quality Gate Record that declares quality disposition, clearing the path to the Release & Exposure Kit or returning the initiative to engineering.

## What this kit does

The Engineering Execution Kit (Layer 4) produces an ORD that declares the system is operationally ready. But "operationally ready" isn't the same as "quality verified." This kit fills that gap:

- **Verification planning** — What integration, system, performance, and security tests are needed beyond unit tests?
- **Test campaign execution** — What evidence proves the tests were run and what were the results?
- **Defect tracking** — What defects were found, how severe are they, and what is their status?
- **Quality disposition** — Does the evidence support proceeding to release, proceeding with conditions, or returning to engineering?

## Artifact types

This kit produces three governed artifact types plus an entry gate:

| Step | Artifact | Purpose |
|------|----------|---------|
| 0 | Quality Assurance Entry Record (QAER) | Entry gate: confirms ORD is frozen, QA ownership accepted, test infrastructure ready |
| 1 | Verification Plan (VP) | Defines integration, system, performance, and security test scope derived from SAD, TDD, and ACF |
| 2 | Test Campaign Record (TCR) | Evidence artifact documenting test execution results, environment, defects, and coverage |
| 3 | Quality Gate Record (QGR) | Terminal artifact declaring quality disposition: PASS, CONDITIONAL, or FAIL |

Each governed artifact type has exactly four governing files: spec, template, prompt, validator.

## Quick start

1. Read `docs/playbook.md` — the complete process definition
2. Read `docs/how-to-use-with-ai.md` — session setup and AI tool guidance
3. See `examples/` — worked examples (initially empty; to be populated)

## Repository structure

```
docs/
  principles/          # Organizational QA policy (input material)
  specs/               # Content rules and hard gates per artifact type
  artifacts/           # Templates and intake forms
  prompts/             # AI generation prompts
  validators/          # Quality gate definitions
  playbook.md          # End-to-end process definition
  index.md             # Documentation entry point
  how-to-adapt.md      # Organizational adoption guidance
  how-to-use-with-ai.md # AI tool usage guide
  governance-model.md  # AIEOS structural rules (reference)
  entry-from-eek.md    # Boundary briefing from Engineering Execution Kit
examples/              # Worked examples
tests/
  kit-test-plan.md     # Structural integrity checks and flow scenarios
CLAUDE.md              # AI operating instructions
```

## AIEOS layer

| Layer | Kit | Status |
|-------|-----|--------|
| 2. Product Intelligence | `aieos-product-intelligence-kit` | Built |
| 4. Engineering Execution | `aieos-engineering-execution-kit` | Built |
| **9. Quality Assurance** | **`aieos-quality-assurance-kit`** | **Built** |
| 5. Release & Exposure | `aieos-release-exposure-kit` | Built |
| 6. Reliability & Resilience | `aieos-reliability-resilience-kit` | Built |
| 7. Insight & Evolution | `aieos-insight-evolution-kit` | Built |
| 8. Operational Diagnostics | `aieos-operational-diagnostics-kit` | Built |

See `aieos-governance-foundation/docs/layer-model.md` for the full layer model.
