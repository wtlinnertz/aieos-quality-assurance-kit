# Prompt Evolution Log — Quality Assurance Kit

This log records every version change to every prompt in this kit. It is maintained by kit operators when a prompt version increments. Format defined in `aieos-governance-foundation/governance-model.md §16`.

Generation prompts produce governed artifacts. The QAER has no generation prompt (human-authored entry gate).

---

## Generation Prompts

| Prompt | From | To | Date | Triggered By | Change Summary | Approved By | Amendment |
|--------|------|----|------|-------------|----------------|-------------|-----------|
| vp-prompt.md | — | 1.0 | 2026-03-09 | Initial release | Initial version | — | — |
| vp-prompt.md | 1.0 | 1.1 | 2026-05-17 | ISSUE-03 gap closure (audit 2026-05-17) | Added compliance ordering check — halt instruction before VP generation when CER is in scope and SCK artifacts are not all Frozen. Closes gap where compliance initiatives could produce VP without frozen TM/SAR/CER/DAR. | Todd Linnertz | AM-001 |
| tcr-prompt.md | — | 1.0 | 2026-03-09 | Initial release | Initial version | — | — |
| qgr-prompt.md | — | 1.0 | 2026-03-09 | Initial release | Initial version | — | — |
