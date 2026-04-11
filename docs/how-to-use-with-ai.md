# How to Use This Kit with AI

This guide explains how to set up AI sessions for each step in the Quality Assurance Kit workflow. Follow the session setup instructions precisely. Incorrect session setup is the most common cause of poor artifact quality.

---

## Core Discipline

**One artifact per session.** Do not generate multiple artifacts in the same session.

**Separate generation and validation.** Always validate in a new session. Never ask the AI that generated an artifact to validate it. This produces self-validation bias.

**Include full frozen documents.** Do not summarize upstream artifacts. Provide the complete document.


## QAER. Human-Authored (No AI Generation Session)

The QAER is human-authored. Do not use AI to complete it. Complete the template yourself using information from the frozen ORD and supporting EEK artifacts.

**Validation session setup:**
```
Documents to provide:
1. The completed QAER (full document)
2. docs/specs/qaer-spec.md

Prompt:
"Validate this Quality Assurance Entry Record against the QAER spec.
Use only the spec as the source of truth for pass/fail criteria.
Do not suggest improvements. Judge only what is explicitly present.
Output JSON using the format defined in the validator."
```


## VP. Generation Session

**Session setup:**
```
Documents to provide:
1. Frozen QAER (full document)
2. SAD (System Architecture Document. Full document, for integration points)
3. TDD (Technical Design Document. Full document, for test strategy)
4. ACF (Architecture Context File. Full document, for constraints)
5. WDD (Work Decomposition Document. Full document, for traceability)
6. docs/specs/vp-spec.md
7. docs/artifacts/vp-template.md

Prompt:
"Generate a Verification Plan using the provided inputs.
Follow the prompt in docs/prompts/vp-prompt.md.
Use the template exactly. Satisfy all hard gates in the spec.
Ensure all SAD integration points are addressed.
Map tests to WDD work items for traceability.
Do not invent test cases. Mark any missing information
with [MISSING: reason]. Output pure Markdown."
```

**After generation:** Review the VP. Confirm:
- All SAD integration points have corresponding test cases
- Acceptance criteria are measurable (numeric thresholds, specific conditions)
- Test environments are specified with enough detail to reproduce
- Tests trace to requirements via WDD work items

**Validation session setup:**
```
Documents to provide:
1. The generated VP (full document)
2. docs/specs/vp-spec.md

Prompt:
"Validate this Verification Plan against the VP spec.
Use only the spec as the source of truth for pass/fail criteria.
Do not suggest improvements. Judge only what is explicitly present.
Output JSON using the format defined in docs/validators/vp-validator.md."
```


## TCR. Generation Session

**Session setup:**
```
Documents to provide:
1. Test execution evidence (test results, logs, screenshots, metrics)
2. Frozen VP (the plan being executed)
3. Defect records (defects found with severity and status)
4. Environment details (configuration, versions, test data)
5. docs/specs/tcr-spec.md
6. docs/artifacts/tcr-template.md

Prompt:
"Generate a Test Campaign Record for this test campaign using the provided evidence.
Follow the prompt in docs/prompts/tcr-prompt.md.
Use the template exactly. Satisfy all hard gates in the spec.
Account for all VP tests. Document deviations for any tests not executed.
Do not invent test results. Mark missing evidence
with [MISSING: reason]. Output pure Markdown."
```

**After generation:** Review the TCR. Confirm:
- Test results match the execution evidence
- All VP tests are accounted for (executed or deviation documented)
- Defects are accurately recorded with correct severity
- Environment details match what was actually used

**Validation session setup:**
```
Documents to provide:
1. The generated TCR (full document)
2. docs/specs/tcr-spec.md

Prompt:
"Validate this Test Campaign Record against the TCR spec.
Use only the spec as the source of truth for pass/fail criteria.
Do not suggest improvements. Judge only what is explicitly present.
Output JSON using the format defined in docs/validators/tcr-validator.md."
```


## QGR. Generation Session

**Session setup:**
```
Documents to provide:
1. All frozen TCRs for this initiative (full documents)
2. Frozen VP (for coverage assessment)
3. Defect status summary (all defects with current status)
4. docs/specs/qgr-spec.md
5. docs/artifacts/qgr-template.md

Prompt:
"Generate a Quality Gate Record using the provided inputs.
Follow the prompt in docs/prompts/qgr-prompt.md.
Use the template exactly. Satisfy all hard gates in the spec.
Ensure the disposition is justified by evidence from the TCRs.
Account for all critical/high defects.
Document residual risks. Output pure Markdown."
```

**After generation:** Review the QGR. Confirm:
- All TCRs are referenced
- Disposition is supported by the evidence (not aspirational)
- All critical/high defects are accounted for (resolved or accepted with rationale)
- Coverage meets VP targets
- Residual risks are documented with impact assessment

**Validation session setup:**
```
Documents to provide:
1. The generated QGR (full document)
2. docs/specs/qgr-spec.md

Prompt:
"Validate this Quality Gate Record against the QGR spec.
Use only the spec as the source of truth for pass/fail criteria.
Do not suggest improvements. Judge only what is explicitly present.
Output JSON using the format defined in docs/validators/qgr-validator.md."
```


## Troubleshooting

**Validator returns FAIL on multiple gates**
Check that the generation session included all required inputs. Missing inputs are the most common cause of multi-gate failures.

**Integration points not covered**
Ensure the SAD was provided in full to the VP generation session. If the SAD has changed since the QAER was frozen, the QAER may need to be re-entered.

**Defects not properly categorized**
Provide clear severity definitions in your organization's principles files. The TCR prompt uses severity as a key classification. Ambiguous severity definitions cause downstream qgr failures.

**QGR disposition seems incorrect**
Review the TCR evidence. The QGR disposition is derived from TCR results and defect status. If the evidence does not support the desired disposition, the evidence is the issue, not the QGR.

**Coverage gaps in TCR**
Document all tests not executed with specific reasons. The QGR must account for these gaps in its risk assessment. A PASS disposition requires all required tests to be executed or gaps to have documented justification.
