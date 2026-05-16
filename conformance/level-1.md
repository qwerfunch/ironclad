---
spec: ironclad/conformance/level-1
version: 0.0.1
status: draft
parent: ironclad/conformance
references:
  - ../iron-law.md
  - ../detectors.schema.json
---

# Level 1 (Base Static) — Conformance Fixtures

## [CLAIM]

6 fixtures for validating Level 1 (Base Static) stage implementation in an external tool.

## [SCOPE]

Level 1 = 6 static stages, all `hard` strictness, all `fast` speed. No runtime required.

| stage | name | iron-law.md ref |
|---|---|---|
| stage_1.1 | Type | type checker exit 0 |
| stage_1.2 | Lint | linter exit 0 |
| stage_1.3 | Drift | all detectors severity != error |
| stage_1.4 | Commit | clean working tree |
| stage_1.5 | Arch | no layer violation |
| stage_1.6 | Secret | no credential pattern |

## [FIXTURES]

### [STAGE_1.1] Type

- **Pass case**: a source file with valid type annotations matching the project's type system.
- **Fail case**: same file with one assignment of `string` to a `number`-typed variable.
- **Pass signal**: type checker exits 0, zero error diagnostics.
- **Fail signal**: type checker exits non-zero OR ≥ 1 error diagnostic.

### [STAGE_1.2] Lint

- **Pass case**: a source file conforming to the project's lint configuration.
- **Fail case**: same file with one rule violation (e.g., unused variable, missing semicolon if enforced).
- **Pass signal**: linter exits 0, zero error-level findings.
- **Fail signal**: linter reports ≥ 1 error-level finding.

### [STAGE_1.3] Drift

- **Pass case**: spec.yaml and source tree consistent — every `features[].modules[]` entry has a corresponding file; every tracked source file is mapped to a feature.
- **Fail case**: introduce one unmapped source file (`UNMAPPED_ARTIFACT` detector triggers).
- **Pass signal**: all 19 detectors return severity != error (per `detectors.schema.json`).
- **Fail signal**: ≥ 1 detector returns severity `error`.

### [STAGE_1.4] Commit

- **Pass case**: working tree clean (`git diff --quiet && git diff --cached --quiet`).
- **Fail case**: working tree has one unstaged modification.
- **Pass signal**: both diff checks return exit 0.
- **Fail signal**: either diff check returns non-zero.

### [STAGE_1.5] Arch

- **Pass case**: import graph respects declared layer order (e.g., UI → service → data, no reverse edge).
- **Fail case**: introduce one import that reverses the layer order (e.g., data layer imports from UI).
- **Pass signal**: arch lint reports zero violations.
- **Fail signal**: arch lint reports ≥ 1 violation.

### [STAGE_1.6] Secret

- **Pass case**: no tracked file contains API key / token / password pattern.
- **Fail case**: introduce one tracked file with a line like `API_KEY = "sk_live_..."`.
- **Pass signal**: secret scanner reports zero matches.
- **Fail signal**: secret scanner reports ≥ 1 match.

## [CONFORMANCE_DECLARATION]

A tool MAY declare `iron-law: L1` conformance after:

1. Implementing all 6 stages (or marking irrelevant ones as N/A with reason).
2. Running the 6 fixtures (each as pass case AND fail case).
3. Confirming all pass signals fire for pass cases AND all fail signals fire for fail cases.
4. Recording the result in the tool's CHANGELOG or audit log.

## [LIMITATIONS] v0.0.1

- Fixtures are language/stack-agnostic descriptions. Each tool maps to its own implementation.
- No machine-readable harness yet — tools self-run the 6 fixtures.
- N/A allowance: stage_1.5 Arch is N/A only if project has < 2 layers; stage_1.4 Commit is N/A only if no VCS used.
- Phase B+ may add tighter requirements (e.g., specific severity thresholds for detector subsets).

