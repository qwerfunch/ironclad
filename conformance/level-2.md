---
spec: ironclad/conformance/level-2
version: 0.0.1
status: draft
parent: ironclad/conformance
references:
  - ../iron-law.md
---

# Level 2 (Mocked Logic) — Conformance Fixtures

## [CLAIM]

2 fixtures for validating Level 2 (Mocked Logic) stage implementation in an external tool.

## [SCOPE]

Level 2 = 2 stages, runtime with mocked externals.

| stage | name | strictness | iron-law.md ref |
|---|---|---|---|
| stage_2.1 | Unit | hard | test runner exit 0, mocked externals |
| stage_2.2 | Cov | soft | coverage >= project threshold |

**Prerequisite**: Level 1 (all 6 stages) MUST pass before attempting Level 2.

## [FIXTURES]

### [STAGE_2.1] Unit

- **Pass case**: a module with ≥ 1 unit test; all tests pass; external I/O (network, disk, DB) mocked.
- **Fail case**: same module with one test deliberately broken (e.g., assertion fails).
- **Pass signal**: test runner exits 0; reported pass count = total test count.
- **Fail signal**: test runner exits non-zero OR reported fail count ≥ 1.

### [STAGE_2.2] Cov

- **Pass case**: source + tests yielding line coverage ≥ project-defined threshold (e.g., 80%).
- **Fail case**: remove tests until coverage drops below threshold.
- **Pass signal**: coverage tool reports measured value ≥ threshold.
- **Fail signal**: coverage tool reports measured value < threshold.

`stage_2.2 Cov` is `soft` (per iron-law.md) — coverage drop reports but does not block completion if recorded as `kind=cov_below` evidence with rationale.

## [CONFORMANCE_DECLARATION]

A tool MAY declare `iron-law: L2` after:

1. Passing all Level 1 fixtures (cumulative rule).
2. Implementing both Level 2 stages.
3. Running both fixtures (pass case AND fail case).
4. Confirming pass signals fire for pass cases AND fail signals fire for fail cases.
5. Recording result in the tool's CHANGELOG or audit log.

## [LIMITATIONS] v0.0.1

- Threshold for Cov is project-defined; spec does not prescribe a fixed number.
- "Mocked externals" boundary (what counts as external) is project-defined.
- Property-based and mutation testing are out of scope (project decision).
