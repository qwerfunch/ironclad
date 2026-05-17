---
spec: ironclad
version: 0.0.23
status: draft
license: MIT
reference_implementation: https://github.com/qwerfunch/harness-boot
inspired_by:
  - "SLSA (graded supply-chain integrity)"
  - "WCAG (graded accessibility conformance)"
  - "OpenAPI (machine-readable spec + reference impl)"
---

# Ironclad Standard

## [CLAIM]

> A standard defining graded, falsifiable consistency among Spec, Code, and Tests.

falsifiable: the standard is negated if any one of the three pieces is found to mismatch yet pass Level N. See `falsifications/` for the registry of counter-examples.

Rationale specific to LLM-assisted authoring (self-certification cycle barrier · LLM-free verification budget): see `iron-law.md` `[AI_ERA_RATIONALE]`.

## [DIMENSIONS]

| dim | pieces | role |
|---|---|---|
| measured | Spec · Code · Tests | targets of consistency measurement |
| mechanism | SSoT · Iron Law · Drift Detection | enforcement tools (Trinity of Integrity) |

## [ARTIFACTS]

| file | content | version |
|---|---|---|
| `iron-law.md` | Level 1-4 · pass criteria · conformance | v0.0.6 |
| `detectors.schema.json` | 19 detector contract (JSON Schema + catalog) | v0.0.3 |
| `ears.md` | EARS AC grammar (5 patterns + BNF) | v0.0.2 |
| `GOVERNANCE.md` | RFC procedure · BDFL · adoption | v0.3 |
| `conformance/` | Level test suite (L1-L4) | v0.0.1 |
| `falsifications/` | Counter-example registry (operationalizes falsifiability) | v0.0.2 |

## [VERSIONING]

- major: breaking change (Level def · detector contract · BNF)
- minor: additive (new detector · new stage)
- patch: doc · example
- independent from harness-boot version

## [CONFORMANCE]

External tools declare conformance as:

```
iron-law: L<N>           # Level 1, 2, 3, or 4
detectors: <subset>      # e.g., "5/19" or list of detector IDs
ears: <full | partial | none>
```

Example: `iron-law: L2, detectors: 5/19, ears: partial` = the tool enforces up to Level 2 with 5 detectors implemented and accepts EARS in part of its surface. See `iron-law.md` `[CONFORMANCE]` for level criteria and `detectors.schema.json` for detector IDs.

## [GLOSSARY]

| term | definition | industry alias |
|---|---|---|
| Trinity of Integrity | SSoT · Iron Law · Drift Detection (the 3 enforcement mechanisms) | — (Ironclad-native) |
| Iron Law | the 4-tier cumulative quality verification | graded gate (SLSA-style) |
| Drift | measurable mismatch between two of {Spec, Code, Tests} | inconsistency · divergence |
| SSoT | Single Source of Truth — the authoritative spec artifact (typically `spec.yaml`) | source of truth · canonical record |
| AC | Acceptance Criterion — one testable requirement (see `ears.md` BNF) | requirement · acceptance test |
| EARS | constrained natural-language grammar for ACs | requirement syntax (Mavin 2009) |
| HITL | Human-in-the-Loop — Level 4 of Iron Law (mandatory human review) | manual gate · acceptance review |
| Conformance Level | a tool's declared Iron Law level (L1-L4); cumulative | WCAG conformance level · SLSA level |
| Fixture | one test case (pass case + fail case) defined in `conformance/level-N.md` | test case · scenario |
| Pass Signal | tool output proving a fixture's pass case succeeded | green check · exit 0 |
| Fail Signal | tool output proving a fixture's fail case correctly fails | red flag · non-zero exit |
| Falsification | counter-example: a case that passes Level N yet has a Spec/Code/Tests mismatch | (Popper); see `falsifications/README.md` |
| Self-certification cycle | failure mode: same LLM authors code AND its verifying test → same misconception encoded twice | — (Ironclad-native; see `iron-law.md [AI_ERA_RATIONALE]`) |
| LLM-free verification budget | structural guarantee that detector catalog requires zero LLM calls (18/19 deterministic, 1 semantic) | — (Ironclad-native; see `iron-law.md [LLM_COST_PROFILE]`) |

## [STRUCTURE_RATIONALE]

8 sections total, structured as 5 + 2 + 1:
- **5 core** (load-bearing): identity (CLAIM), scope (DIMENSIONS), inventory (ARTIFACTS), evolution (VERSIONING), honesty (LIMITATIONS).
- **2 entry** (external adoption): GLOSSARY (terminology lookup) + CONFORMANCE (declaration form).
- **1 self** (meta): STRUCTURE_RATIONALE — this section, explaining its own structure.

Anything beyond belongs to the artifact files (`iron-law.md`, `detectors.schema.json`, `ears.md`, `GOVERNANCE.md`, `conformance/`) — README is the index, not the spec body.

**Translations** (non-normative, comprehension aid only): see `i18n/<lang>/` for translated mirrors. The English root is the SSoT; on any conflict, the English text wins.

## [LIMITATIONS] v0.0.20

- conformance test suite covers all 4 levels, but refinement based on first external tool declaration pending
- no formal RFC track (proposals/ created on first RFC)
- no external adoption — single reference implementation only (`harness-boot`); see `GOVERNANCE.md` `[DRAFT_GRADUATION]` for v1.0 graduation criteria
- BDFL-only governance (qwerfunch)
- `falsifications/` registry just opened with v0.0.18 — zero accepted entries yet (honest baseline, not validation)
