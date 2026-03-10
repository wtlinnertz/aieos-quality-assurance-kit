# CLAUDE.md — Quality Assurance Kit

## What This Repository Is

This is the **Quality Assurance Kit** — an AIEOS kit that governs verification campaigns, integration/system testing, and pre-release quality gates. It is Layer 9 of the AIEOS system. It receives a frozen Operational Readiness Document (ORD) from the Engineering Execution Kit and governs verification planning, test campaign execution, defect tracking, and quality disposition before the initiative proceeds to the Release & Exposure Kit.

## Repository Structure

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
tests/                 # Structural integrity checks
```

## Artifact Types

This kit produces three governed artifact types plus an entry gate:

1. **Verification Plan (VP)** — Defines integration, system, performance, and security test scope. Derived from SAD integration points, TDD test strategy, and ACF constraints.
2. **Test Campaign Record (TCR)** — Evidence artifact documenting test execution results, environment used, defects found, and coverage achieved.
3. **Quality Gate Record (QGR)** — Terminal artifact declaring quality disposition: PASS (proceed to REK), CONDITIONAL (proceed with noted risks), or FAIL (return to EEK).

Each artifact type has exactly four governing files: spec, template, prompt, validator.

Plus one entry gate:

- **Quality Assurance Entry Record (QAER)** — Step 0 gate (human-authored). Confirms the ORD is frozen, names the QA owner, confirms test infrastructure is ready, and identifies integration points from SAD that require cross-component verification. Validated against `qaer-spec.md` before VP generation begins.

## Key Rules

- **Specs are the source of truth** — prompts and validators reference specs, never inline rules
- **Validators judge, they do not help** — no suggestions, no redesign
- **Freeze before promote** — ORD must be frozen before QAER; QAER must be frozen before VP generation; VP must be frozen before TCR generation; TCR must be frozen before QGR generation
- **Separate generation and validation** — different AI sessions to prevent self-validation bias
- **No scope expansion** — downstream artifacts must not expand scope beyond upstream
- **No inferred information** — mark missing information explicitly, do not fill gaps
- **Governance model sync** — `docs/governance-model.md` is a synchronized copy of `aieos-governance-foundation/governance-model.md` (canonical authority). Do not edit kit copy directly; update `aieos-governance-foundation` first, then sync all kit copies to match exactly. See governance-model.md §15 for versioning and change protocol.
- **Engagement Record** — QAK maintains the Layer 9 section of the project's ER. Add artifact IDs as they freeze. See `docs/playbook.md §Maintaining the Engagement Record` and `aieos-governance-foundation/docs/engagement-record-spec.md`.

## Artifact Flow

```
Step 0: Quality Assurance Entry Record → validate → freeze
Step 1: Verification Plan → generate from QAER + SAD + TDD + ACF
        → validate → freeze
Step 2: Test Campaign Record → generate from VP execution evidence
        → validate → freeze
Step 3: Quality Gate Record → generate from TCR(s) + VP + defect status
        → validate → freeze → handoff to Layer 5 (REK)
```

## Boundary Contracts

- **Upstream:** Receives a frozen ORD from the Engineering Execution Kit (Layer 4). The ORD confirms operational readiness; QAK requires the frozen ORD plus SAD (integration points), TDD (test strategy), ACF (constraints), and WDD (work items for traceability). See `docs/entry-from-eek.md` for the boundary briefing.
- **Downstream (Layer 5):** Produces a frozen QGR that the Release & Exposure Kit (Layer 5) uses as quality clearance for release entry. The QGR §Quality Disposition section is the downstream boundary contract — it provides the disposition (PASS/CONDITIONAL/FAIL), residual risks, and any conditions for proceeding.

## File Naming

| Type | Pattern |
|------|---------|
| Spec | `{type}-spec.md` |
| Template | `{type}-template.md` |
| Prompt | `{type}-prompt.md` |
| Validator | `{type}-validator.md` |

## When Working on This Kit

- Read the playbook (`docs/playbook.md`) for the full process definition
- Read the governance model (`docs/governance-model.md`) for structural rules
- Check `docs/how-to-use-with-ai.md` for session setup instructions
- Reference `examples/` for worked examples

## Building or Auditing AIEOS Kits

- `aieos-governance-foundation/docs/kit-structure-standard.md` — compliance checklist for building and auditing kits
- `aieos-governance-foundation/docs/philosophy.md` — design rationale for governance model decisions
- `aieos-governance-foundation/docs/layer-model.md` — layer model and kit registry
