---
spec: ironclad/iron-law
version: 0.0.2
status: draft
parent: ironclad
references:
  - detectors.schema.json
  - ears.md
  - GOVERNANCE.md
---

# Iron Law

## [CLAIM]

A 4-tier cumulative quality verification, where each higher tier requires the previous tier passed plus stronger evidence.

## [LEVELS]

| level | name | environment | industry analog | llm cost / feature |
|---|---|---|---|---|
| L1 | Base Static | tooling only (no runtime) | SAST + linter + secret scanner | 0 |
| L2 | Mocked Logic | runtime, mocked externals | unit + coverage (ISTQB) | 0 |
| L3 | Full Empirical | runtime, real environment | integration + system + e2e | 0-1 (result interpretation) |
| L4 | HITL | human review | acceptance + audit | 0 (LLM), 1 (human) |

## [STAGES]

13 stages distributed across 4 levels.

| stage | level | name | alias (industry) | strictness | speed | pass criteria |
|---|---|---|---|---|---|---|
| stage_1.1 | L1 | Type | Type Check / SAST | hard | fast | type checker exit 0, no errors |
| stage_1.2 | L1 | Lint | Static Analysis | hard | fast | linter exit 0, no errors |
| stage_1.3 | L1 | Drift | Consistency Check (Ironclad) | hard | fast | all detectors (see `detectors.schema.json`) return severity != error |
| stage_1.4 | L1 | Commit | VCS Hygiene · Clean Tree | hard | fast | `git diff --quiet && git diff --cached --quiet` |
| stage_1.5 | L1 | Arch | Architectural Conformance | hard | fast | dependency direction valid, no layer violation |
| stage_1.6 | L1 | Secret | Secret Scanning | hard | fast | no API key / token / credential pattern in tracked files |
| stage_2.1 | L2 | Unit | Unit Test (ISTQB) | hard | mid | test runner exit 0, all unit tests pass (mocked externals) |
| stage_2.2 | L2 | Cov | Code Coverage | soft | mid | coverage >= threshold (project-defined) |
| stage_3.1 | L3 | Smoke | System Integration Probe | hard | heavy | per-AC smoke proof: real environment, observable result |
| stage_3.2 | L3 | Perf | Performance Test | soft | heavy | within `performance_budget` (project-defined) |
| stage_3.3 | L3 | Visual | Visual Regression Test (VRT) | hard | heavy | per-AC visual proof (VRT match or human-approved baseline) |
| stage_4.1 | L4 | Audit | Evidence + Code Review | hard | manual | reviewer signs evidence sufficiency AND code/test change per AC |
| stage_4.2 | L4 | UAT | User Acceptance Test | hard | manual | human confirms intent alignment per AC |

Notes:
- `stage_3.1 Smoke` subsumes integration + system + basic e2e levels of the ISTQB hierarchy — Ironclad unifies these at AC granularity (per-AC real-env proof).
- `stage_3.3 Visual` is N/A (auto-pass) when feature has no UI surface.
- `stage_4.1 Audit` is two-fold per AC: (a) evidence sufficiency, (b) code/test review. The same human signs both — breaks AI-self-certification.

## [STAGE_COUNT_RATIONALE]

The 13-stage count is load-bearing — neither padded nor minimal.

| level | stages | rationale |
|---|---|---|
| L1 | 6 | static checks decompose into distinct tool surfaces — combining is loss of clarity |
| L2 | 2 | mocked logic = unit + coverage (ISTQB primitives for isolated testing) |
| L3 | 3 | functional + non-functional + visual (3 dimensions of empirical proof) |
| L4 | 2 | mechanical (evidence audit) + qualitative (intent alignment) |

**Absent stages and where they live:**

| absent | location |
|---|---|
| Integration / System / E2E | stage_3.1 Smoke (intentionally unified) |
| Format / Style | stage_1.2 Lint |
| Regression | re-run of stage_2.1 Unit + stage_3.1 Smoke |
| Mutation Testing | out of standard scope (project decision) |
| Fuzz Testing | out of standard scope (project decision) |

A 14th stage would either duplicate an existing stage's surface or split semantics for marginal gain. A 12-stage version would collapse Type/Lint, Smoke/Perf, or Audit/UAT — each separation is load-bearing.

## [CUMULATIVE_RULE]

```
L(N) can be attempted only if L(N-1) passed.
Within a level, all hard stages must pass; soft stages must report (pass | fail | n/a).
```

## [EVIDENCE_RULE]

Declared evidence (kind != `gate_run` / `gate_auto_run`) per mode required for completion:

| mode | declared_evidence_min | max_level_enforced |
|---|---|---|
| tutorial | 1 | L1 |
| prototype | 1 | L2 |
| product | 3 | L3 |
| regulated | 3 | L4 |

`--hotfix-reason` allows product/regulated to complete with 1 declared evidence; the reason is auto-recorded as `kind=hotfix` evidence (audit preserved).

## [CONFORMANCE]

Mode-to-level mapping is diagonal (not a matrix):

```
tutorial   conforms to L1
prototype  conforms to L2
product    conforms to L3
regulated  conforms to L4
```

External tools can declare conformance as: `iron-law: L<N>`.

`risk-aware override`: a feature flagged `entities[].sensitive` or in security/payment area auto-elevates one level (e.g., product feature with sensitive=true → L4 enforced).

## [LLM_COST_PROFILE]

| level | tooling | llm calls / feature |
|---|---|---|
| L1 | type checker, linter, drift detector, git, arch lint, secret scan | 0 |
| L2 | test runner, coverage tool | 0 |
| L3 | smoke harness, perf tool, visual diff | 0-1 (interpretation only; deterministic pass/fail skips it) |
| L4 | human reviewer | 0 (LLM cost), 1 human review |

A feature passing all four levels incurs at most 1 LLM call. Combined with `[ARTIFACTS].detectors.schema.json` constant-cost graph pruning, total LLM cost per feature is O(1).

## [HISTORY]

The 4-tier structure derives from harness-boot's pre-Ironclad 7 flat gates (`gate_0..gate_5, gate_perf`):

| pre-Ironclad gate | ironclad stage | change |
|---|---|---|
| gate_0 (tests) | stage_2.1 Unit | reclassified |
| gate_1 (typecheck) | stage_1.1 Type | reclassified |
| gate_2 (lint) | stage_1.2 Lint | reclassified |
| gate_3 (coverage) | stage_2.2 Cov | reclassified |
| gate_4 (commit) | stage_1.4 Commit | reclassified |
| gate_5 (smoke) | stage_3.1 Smoke | reclassified |
| gate_perf | stage_3.2 Perf | reclassified |
| (none) | stage_1.3 Drift | promoted from `harness check` |
| (none) | stage_1.5 Arch | new |
| (none) | stage_1.6 Secret | new |
| (none) | stage_3.3 Visual | new |
| (none) | **L4 (HITL)** | **invented — anti AI-self-certification** |

Honest naming: L1-L3 = reclassification + 3 new stages. L4 is the original invention. The 4-tier shape aligns with industry graded standards (SLSA: 4 levels, PCI DSS: 4 levels, CMM: 5 levels) for partial conformance.

## [LIMITATIONS] v0.0.2

- Self-healing automation (3-retry, auto-rollback) is aspirational; not part of this spec yet.
- Conformance test suite (`conformance/level-{N}/`) not yet provided.
- `stage_3.3 Visual` baseline approval procedure tool-defined, not standardized.

