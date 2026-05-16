---
spec: ironclad/conformance
version: 0.0.1
status: draft
parent: ironclad
references:
  - ../iron-law.md
  - ../detectors.schema.json
  - ../GOVERNANCE.md
---

# Conformance

## [CLAIM]

Test fixtures for declaring Ironclad Level N conformance.

## [STRUCTURE]

| level | file | scope | status |
|---|---|---|---|
| L1 | `level-1.md` | 6 static stages (Type · Lint · Drift · Commit · Arch · Secret) | v0.0.1 drafted |
| L2 | `level-2.md` | 2 mocked logic stages (Unit · Cov) | TBD |
| L3 | `level-3.md` | 3 empirical stages (Smoke · Perf · Visual) | TBD |
| L4 | `level-4.md` | 2 HITL stages (Audit · UAT) | TBD |

## [DECLARATION_FORMAT]

External tool declares conformance in its own README/docs:

```
ironclad-spec: <spec version>
iron-law:      L<N>
detectors:     <subset, e.g. "5/19" or "[UNTESTED_AC, STATUS_DRIFT, ...]">
ears:          <full | partial | none>
```

## [TEST_PROCEDURE]

```
1. Tool implements stage(s) at target Level N.
2. Tool runs the fixtures defined in level-<N>.md.
3. All hard-stage fixtures pass + soft-stage fixtures report → tool MAY declare iron-law: L<N>.
4. Phase B+ external auditor MAY re-run for independent verification.
```

Cumulative rule applies: declaring `iron-law: L3` requires passing L1 + L2 + L3 fixtures.

## [FIXTURE_FORMAT]

Each stage fixture follows a uniform structure:

```
### [STAGE_<N>.<M>] <Name>
- Pass case:    <minimal artifact that should pass the stage>
- Fail case:    <minimal artifact that should fail the stage>
- Pass signal:  <tool output indicating pass — exit code 0 or detector severity != error>
- Fail signal:  <tool output indicating fail>
```

## [LIMITATIONS] v0.0.1

- Only Level 1 fixtures provided. L2-L4 TBD.
- No automated test runner yet — tools self-verify; Phase B+ may add audit tooling.
- Fixtures describe scenarios, not language-specific code (each tool maps to its own stack).

