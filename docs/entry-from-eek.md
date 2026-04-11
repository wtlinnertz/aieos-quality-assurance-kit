# Entry from Engineering Execution Kit (EEK)

**You are here because:** The Operational Readiness Document (ORD) is frozen and the initiative is ready for quality verification before release.

**What you must bring:**

| Artifact | Status Required | Where to Find It |
|----------|----------------|-----------------|
| Operational Readiness Document (ORD-{PROJECT}-{NNN}) | Frozen | EEK `docs/sdlc/` in the consuming project |
| System Architecture Document (SAD) | Frozen | EEK `docs/sdlc/` in the consuming project |
| Technical Design Document (TDD) | Frozen | EEK `docs/sdlc/` in the consuming project |
| Architecture Context File (ACF) | Frozen | EEK `docs/sdlc/` in the consuming project |
| Work Decomposition Document (WDD) | Frozen | EEK `docs/sdlc/` in the consuming project |

**First artifact to produce in this kit:** Quality Assurance Entry Record (QAER) — human-authored, no prompt

**Where to start:** `docs/playbook.md` → "Step 0 — Quality Assurance Entry Gate"

**What changes at this boundary:**

- You shift from engineering execution to quality verification. The focus moves from "does it work?" (unit tests, code review) to "does it work together?" (integration, system, performance, security).
- The artifact owner changes: the QA owner is now the primary accountable party. This may be a different person than the engineering lead.
- The test scope expands beyond unit tests to cover integration points, system behavior, performance under load, and security posture.
- The timeline is bounded: QAK produces a disposition (PASS/CONDITIONAL/FAIL), not an indefinite operational state.

**Common mistakes at this transition:**

| Mistake | How to Avoid |
|---------|--------------|
| Treating QAER as optional because "we already have test results" | The QAER is the formal entry gate that establishes the QA owner and confirms test infrastructure is ready. Unit test results from EEK are inputs, not substitutes. |
| Skipping SAD integration point identification | The VP depends on QAER identifying which integration points need cross-component verification. Without this, the VP will have coverage gaps. |
| Generating VP without the full set of EEK artifacts | VP generation requires SAD, TDD, ACF, and WDD. Missing any of these produces an incomplete verification plan. |
| Assuming unit test coverage is sufficient | Unit tests verify individual components. QAK verifies that components work together, under load, and securely. These are different verification concerns. |

**If you arrived here without a complete upstream artifact:**

Stop. Return to EEK, complete and freeze the ORD, and ensure SAD, TDD, ACF, and WDD are available. QAK cannot establish a meaningful verification plan without the full set of EEK artifacts. A pointer to incomplete artifacts does not satisfy the entry gate.

---

*For the full entry flow, see `docs/playbook.md`.*
