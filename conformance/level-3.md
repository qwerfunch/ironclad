---
spec: ironclad/conformance/level-3
version: 0.0.1
status: draft
parent: ironclad/conformance
references:
  - ../iron-law.md
---

# Level 3 (Full Empirical) — Conformance Fixtures

## [CLAIM]

3 fixtures for validating Level 3 (Full Empirical) stage implementation in an external tool.

## [SCOPE]

Level 3 = 3 stages, real runtime environment.

| stage | name | strictness | iron-law.md ref |
|---|---|---|---|
| stage_3.1 | Smoke | hard | per-AC smoke proof, real environment |
| stage_3.2 | Perf | soft | within `performance_budget` |
| stage_3.3 | Visual | hard | per-AC visual proof (VRT match) |

**Prerequisite**: Level 1 + Level 2 (all 8 stages) MUST pass.

`stage_3.1 Smoke` subsumes integration + system + basic e2e (per iron-law.md).
`stage_3.3 Visual` is N/A (auto-pass) when feature has no UI surface.

## [FIXTURES]

### [STAGE_3.1] Smoke

- **Pass case**: feature deployed in real environment (or test environment matching production); each AC produces an observable result (log line, HTTP response, state change).
- **Fail case**: one AC's smoke check returns an unexpected result (e.g., 500 instead of 200, missing log line).
- **Pass signal**: per-AC smoke check returns expected result for every AC ID in the feature.
- **Fail signal**: ≥ 1 AC smoke check returns unexpected result OR no observable signal.

### [STAGE_3.2] Perf

- **Pass case**: measured performance within declared `performance_budget` (e.g., LCP < 2.5s, p95 latency < 500ms, bundle < 200KB).
- **Fail case**: measurement exceeds at least one budget dimension.
- **Pass signal**: all budget dimensions within threshold; no regression vs previous recorded baseline beyond noise.
- **Fail signal**: ≥ 1 budget dimension exceeded OR statistically significant regression.

`stage_3.2 Perf` is `soft` — over-budget reports but does not block completion if recorded as `kind=perf_regression` evidence (paired with `kind=perf_resolved` to clear).

### [STAGE_3.3] Visual

- **Pass case**: rendered UI matches stored baseline image (per AC) within VRT tolerance, OR human approved current rendering as new baseline.
- **Fail case**: rendered UI differs from baseline beyond tolerance AND no human approval for new baseline.
- **Pass signal**: VRT diff ≤ tolerance for every AC's visual proof.
- **Fail signal**: VRT diff > tolerance AND no `harness visual-approve` (or equivalent) for new baseline.

For features without UI (`ui_surface.present: false`), stage_3.3 reports N/A (auto-pass).

## [CONFORMANCE_DECLARATION]

A tool MAY declare `iron-law: L3` after:

1. Passing all Level 1 + Level 2 fixtures (cumulative rule).
2. Implementing all 3 Level 3 stages.
3. Running 3 fixtures (pass case AND fail case for each).
4. Confirming pass signals fire for pass cases AND fail signals fire for fail cases.
5. Recording result in tool's CHANGELOG or audit log.

## [LIMITATIONS] v0.0.1

- `performance_budget` dimensions and thresholds are project-defined.
- VRT tolerance (px diff, ssim threshold) is project-defined.
- "Real environment" boundary is project-defined (production-equivalent staging vs ephemeral test env).
- Multi-platform visual (iOS/Android/desktop) is one fixture per platform; not separated into sub-stages.
