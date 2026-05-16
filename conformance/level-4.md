---
spec: ironclad/conformance/level-4
version: 0.0.1
status: draft
parent: ironclad/conformance
references:
  - ../iron-law.md
---

# Level 4 (HITL) — Conformance Fixtures

## [CLAIM]

2 fixtures for validating Level 4 (Human-in-the-Loop) stage implementation. Unlike L1-L3, fixtures here are **checklist procedures**, not automated pass/fail cases — because L4 verifies human judgment, not tool behavior.

## [SCOPE]

Level 4 = 2 stages, manual human review.

| stage | name | strictness | iron-law.md ref |
|---|---|---|---|
| stage_4.1 | Audit | hard | reviewer signs evidence + code review per AC |
| stage_4.2 | UAT | hard | human confirms intent alignment per AC |

**Prerequisite**: Level 1 + Level 2 + Level 3 (all 11 automatable stages) MUST pass.

L4 conformance verifies that the **tool provides procedural infrastructure** for human review — not that the human's verdict was correct.

## [FIXTURES]

### [STAGE_4.1] Audit — Reviewer Checklist

For each AC in the feature, the reviewer MUST verify and sign:

```
[ ] (a) Evidence sufficiency
    - At least 1 declared evidence (kind != gate_run / gate_auto_run) exists for this AC.
    - Evidence kind matches AC nature (test for behavior, manual_check for UX, etc.).
    - Evidence timestamp is recent (within feature work window).

[ ] (b) Code/test review
    - Code change implements the AC as written (not adjacent).
    - Test asserts the AC's stated condition/response (per ears.md grammar).
    - No obvious self-certification (e.g., LLM-generated test for LLM-generated code without independent verification).

[ ] (c) Anti-cheating
    - LLM-authored evidence is tagged (`author: llm` or equivalent).
    - Reviewer's sign-off (this checklist) is distinct from LLM author.
```

**Pass signal**: tool records the reviewer's identity + timestamp + per-AC checkmarks for all 3 items × all ACs.
**Fail signal**: any AC has ≥ 1 unchecked item OR reviewer identity missing OR reviewer == LLM author.

### [STAGE_4.2] UAT — Intent Alignment Checklist

For the feature as a whole, the user (or product owner) MUST verify and sign:

```
[ ] (a) Behavior matches intent
    - When using the feature in its intended scenario, the result aligns with the original goal (not just the literal AC text).

[ ] (b) No surprise
    - Feature does what user expects; no hidden side effects, no unrelated changes.

[ ] (c) Acceptable trade-offs
    - Where the implementation chose between options (e.g., latency vs cost, simplicity vs flexibility), the user accepts the choice.
```

**Pass signal**: tool records user's identity + timestamp + checkmarks for all 3 items.
**Fail signal**: any item unchecked OR user identity missing OR feature flagged for revision.

## [TOOL_REQUIREMENTS]

To claim `iron-law: L4`, the tool MUST provide:

1. **Identity tracking**: separate identifier for human reviewer vs LLM author per evidence/sign-off.
2. **Per-AC sign-off surface**: UI or CLI that lets reviewer mark each AC's checklist items.
3. **Audit trail**: timestamped record of all checklist activity (immutable or append-only).
4. **Anti-self-certification guard**: tool refuses to accept reviewer sign-off when reviewer identity matches LLM author.

These are **infrastructure requirements**, not behavioral fixtures.

## [CONFORMANCE_DECLARATION]

A tool MAY declare `iron-law: L4` after:

1. Passing all Level 1 + Level 2 + Level 3 fixtures (cumulative).
2. Implementing both Audit (stage_4.1) and UAT (stage_4.2) checklist infrastructure.
3. Demonstrating one end-to-end run of both checklists on a real feature (with real human signer ≠ LLM author).
4. Documenting the four [TOOL_REQUIREMENTS] satisfied.
5. Recording in tool's CHANGELOG.

## [LIMITATIONS] v0.0.1

- L4 verifies **infrastructure**, not **judgment quality**. A signer's quality is out of standard scope.
- "Independent verification" (in [STAGE_4.1] item (b)) is project-defined; recommended: reviewer did not author the LLM prompt that generated code/tests.
- No automated way to verify checklist items are not rubber-stamped — relies on signer integrity + audit trail review.
- Multi-reviewer workflows (2+ signers, quorum) are out of MVP scope; single signer sufficient for L4 declaration.
