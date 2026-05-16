---
spec: ironclad
file: CHANGELOG
format: keep-a-changelog 1.1.0
---

# Changelog

All notable changes to the Ironclad standard.

Spec version is independent from `harness-boot` (reference implementation) version.

## v0.0.16 - 2026-05-17

### Added
- README `[GLOSSARY]`: 4 conformance terms (Conformance Level · Fixture · Pass Signal · Fail Signal) added (7 → 11).
- `iron-law.md` `[CONFORMANCE]`: cross-link to `conformance/level-{1,2,3,4}.md` test fixtures.

### Changed
- README `[STRUCTURE_RATIONALE]`: self-counting bug fixed. Was "5 + 2 = 7" (omitted self). Now "5 + 2 entry + 1 self = 8 sections" — honest about its own meta-layer.

### Note
- All 20 files frontmatter version-sync verified.
- All references field cross-links verified.
- detectors.schema.json: 19 detectors · 4 axes · 2 severities · 3 statuses · 13/19 alias · 7/19 notes · unique IDs all valid.

## v0.0.15 - 2026-05-17

### Added
- `conformance/level-2.md` v0.0.1: 2 fixtures for Level 2 (Mocked Logic) — Unit (hard) + Cov (soft).
- `conformance/level-3.md` v0.0.1: 3 fixtures for Level 3 (Full Empirical) — Smoke (hard, subsumes integration/system/e2e) + Perf (soft) + Visual (hard, VRT).
- `conformance/level-4.md` v0.0.1: 2 fixtures for Level 4 (HITL) — Audit + UAT as **checklist procedures** (manual review infrastructure), plus 4 `[TOOL_REQUIREMENTS]` (identity tracking, per-AC sign-off, audit trail, anti-self-certification guard).
- All 3 levels paired with `.ko.md` Korean translations.

### Changed
- `conformance/README.md` `[STRUCTURE]` table: L2/L3/L4 status TBD → `v0.0.1 drafted`.
- `conformance/README.md` `[LIMITATIONS]`: "Only Level 1 fixtures" → "All 4 levels drafted, refinement pending first external declaration".
- README `[ARTIFACTS]`: `conformance/` status → `v0.0.1 drafted` (L1-L4).
- README `[LIMITATIONS]`: rewrote conformance line to reflect full coverage.

### Note
- L4 fixtures intentionally checklist-form (manual review infrastructure), not pass/fail cases — L4 verifies human judgment infrastructure, not tool behavior.
- This commit completes the *real MVP*: spec's 4-level promise is now 100% verifiable via conformance fixtures. Previous v0.0.14 was a seed (L1 only).

## v0.0.14 - 2026-05-16

### Added
- `conformance/` directory + `conformance/README.md` v0.0.1: structure (L1-L4) · declaration format (`iron-law: L<N>, detectors: <subset>, ears: <full|partial|none>`) · test procedure · fixture format.
- `conformance/level-1.md` v0.0.1: 6 fixtures for Level 1 (Base Static) stages — each fixture: pass case · fail case · pass signal · fail signal. Stage-agnostic (language/stack-neutral).
- `conformance/level-1.ko.md` + `conformance/README.ko.md` Korean pairs.
- `[CONFORMANCE_DECLARATION]` procedure: 4-step (implement · run fixtures · confirm signals · record).

### Changed
- README `[ARTIFACTS]` table: `conformance/` status TBD → `v0.0.1 (L1 only)`.
- README `[LIMITATIONS]`: "no conformance test suite" → "covers Level 1 only (L2-L4 TBD)".

### Note
- L2/L3/L4 fixtures deferred until first external tool declares L2+ conformance.
- Fixtures describe scenarios, not language-specific code (each tool maps to its own stack).

## v0.0.13 - 2026-05-16

### Added
- README `[GLOSSARY]`: 7 brand terms (Trinity of Integrity · Iron Law · Drift · SSoT · AC · EARS · HITL) with industry aliases.
- README `[CONFORMANCE]`: external tool conformance declaration form (`iron-law: L<N>, detectors: <subset>, ears: <full|partial>`).
- README `[STRUCTURE_RATIONALE]`: 5-section justification + entry-point sections (Glossary + Conformance).
- README frontmatter `inspired_by`: SLSA · WCAG · OpenAPI (matches inspired_by pattern in GOVERNANCE.md).

### Note
- Existing 5 sections (CLAIM · DIMENSIONS · ARTIFACTS · VERSIONING · LIMITATIONS) unchanged.
- README now serves as the single entry point: glossary for terminology, conformance for adoption form, rationale for self-explanation.

## v0.0.12 - 2026-05-16

### Changed
- `GOVERNANCE.md` → v0.2. `[PHASES]` and `[ROLES]` tables add `industry alias` column (Solo Maintainer · TSC · Steering Council · Committer · Author).
- frontmatter adds `inspired_by` (Python PEP 1, CNCF Governance, TC39 RFC stages).
- proposal template adds mandatory `[SECURITY_IMPLICATIONS]` section.

### Added
- `[PHASE_COUNT_RATIONALE]` — 3 phases load-bearing (BDFL starting · working group growth · federation maturity); absent-phase placement (Foundation = legal wrapper not phase · Bus-factor 1→2 in A→B · Sunset in DEPRECATION_POLICY · Fork out of scope).
- `[QUORUM]` — minimum maintainer count for valid decisions per phase (A:1, B:floor(N/2)+1, C:3+1 steering).
- `[DEPRECATION_POLICY]` — spec element retirement procedure (5 steps), deprecation period per phase, `--emergency-removal-reason` for prior-deprecation bypass.
- `[LIMITATIONS]`: Code of Conduct deferred to Phase B trigger.

### Note
- Phase count unchanged (3).
- Role count unchanged (3).
- RFC procedure unchanged (6 steps).

## v0.0.11 - 2026-05-16

### Changed
- `ears.md` → v0.0.2. `[PATTERNS]` table adds `industry equivalent` column (RFC 2119 · Gherkin · feature flag · exception path).
- BNF refined: `ear_compound` now requires *at least one* clause, each at most once, fixed order (When → While → Where → If). Clauses extracted as named productions (`when_clause`, etc.).
- RESPONSE grammar note: may include quantifier (`within Xms`, `at most N times`).
- frontmatter adds `modal_authority: RFC 2119` (shall/must/should differentiation).

### Added
- `[NEGATION]` section: `shall not` for prohibitions — distinct from absent positive requirement, makes prohibited behavior explicit and verifiable.
- `[PATTERN_COUNT_RATIONALE]` section: 5 patterns load-bearing (each carries distinct temporal/logical semantics); absent-pattern placement (Quantified → RESPONSE · User Story → out of scope · BDD Given-When-Then → compound EARS · Use Case → out of scope · Prohibition → NEGATION).

### Note
- Pattern count unchanged (5 + compound).
- Pattern names unchanged (Mavin 2009 originals).
- Retroactive consistency fix: `iron-law.md` frontmatter version bumped 0.0.1 → 0.0.2 (was omitted in v0.0.9 patch despite content changes).

## v0.0.10 - 2026-05-16

### Changed
- `detectors.schema.json` → v0.0.2. Schema gains `industry_alias` and `notes` optional fields on Detector.
- 13/19 detectors annotated with `industry_alias` (Orphan Artifact, Missing Code, Requirement Drift, Stack Violation, Architecture Linting, Semantic Lint, Test Gap, Test Lag, Coverage Regression, Secret Scanning, Performance Regression, Dead Spec, Reference Validation). Remaining 6 are Ironclad-native (no industry equivalent).
- 7 detectors gain disambiguation `notes` (e.g., MISSING_TESTS vs UNTESTED_AC granularity, EVIDENCE_MISMATCH vs STALE_EVIDENCE dimension, HARDCODED_SECRET double-net with iron-law stage_1.6).
- 3 ambiguous descriptions sharpened: UNMAPPED_ARTIFACT (now "tracked source file ... not declared"), CONVENTION_DRIFT (now "beyond syntactic lint"), EVIDENCE_MISMATCH (now "hash differs ... stale at runtime").

### Added
- `detectors.schema.json` root `rationale` object: count justification, intentional duplicates, absent-detector placement (Dependency Vulnerability · API Contract Breakage · Documentation Drift · Type Safety Erosion · License Compliance).

### Note
- Detector count unchanged (19).
- Detector IDs unchanged (version stability).

## v0.0.9 - 2026-05-16

### Changed
- `iron-law.md`: `[LEVELS]` table adds `industry analog` column (SAST · ISTQB · acceptance).
- `iron-law.md`: `[STAGES]` table adds `alias (industry)` column for every stage; ambiguous names (`Commit`, `Arch`, `Smoke`, `Audit`) clarified.
- `iron-law.md`: stage_3.1 Smoke noted as subsuming integration + system + basic e2e (ISTQB hierarchy unified at AC granularity).
- `iron-law.md`: stage_4.1 Audit two-fold (evidence + code review, same human signs both — anti AI-self-certification).

### Added
- `iron-law.md`: `[STAGE_COUNT_RATIONALE]` section — justifies why 13 stages is load-bearing (per-level rationale + absent-stage placement).
- Korean translation updated in `iron-law.ko.md`.

### Note
- Stage names themselves unchanged (version stability); only alias and description boosted.
- Stage count unchanged (13).

## v0.0.8 - 2026-05-16

### Added
- `GOVERNANCE.md` v0.1: 3 governance phases (BDFL → working group → federation), RFC procedure, role definitions (User · Contributor · Maintainer), decision model per phase, transition triggers, dispute resolution, security disclosure policy.
- `GOVERNANCE.ko.md` as Korean translation.
- Current phase: A (BDFL, maintainer = qwerfunch).

### Status
- All 4 core spec artifacts drafted (iron-law · detectors · ears · GOVERNANCE) + LICENSE + CHANGELOG.
- Remaining: `conformance/` test suite (post-MVP).

## v0.0.7 - 2026-05-16

### Added
- `ears.md` v0.0.1: 5 EARS patterns (ubiquitous · event-driven · state-driven · optional-feature · unwanted-behavior) + compound form. Formal BNF, validation examples, AC ↔ test traceability rule.
- `ears.ko.md` as Korean translation.
- Integration with reference impl `spec.yaml` acceptance_criteria (string + structured forms).

## v0.0.6 - 2026-05-16

### Added
- `detectors.schema.json` v0.0.1: 19 drift detector catalog with self-describing schema. Severity `error|warn`, axes `spec_vs_code|code_vs_test|spec_vs_test|environment`, status `implemented|renaming|planned`. Each detector has id (UPPER_SNAKE_CASE), pass_criteria, remediation, legacy_kind (harness-boot mapping).
- Detector distribution: spec_vs_code(6) · code_vs_test(6) · spec_vs_test(4) · environment(3).
- `DriftFinding` schema for detector output.

## v0.0.5 - 2026-05-16

### Added
- `iron-law.md` v0.0.1: 4-tier Levels (L1-L4), 13 stages with pass criteria, cumulative rule, evidence rule (mode-based), conformance mapping (mode = level), LLM cost profile (O(1) per feature), history (7 gates → 4 levels honest naming).
- `iron-law.ko.md` as Korean translation.
- `LICENSE` (MIT) and `CHANGELOG.md` (+ .ko) added in this version cycle.

## v0.0.4 - 2026-05-16

### Changed
- README rewritten in English (SSoT, token efficiency).
- README.ko.md added as Korean translation (for human comprehension only — English is authoritative on conflict).

## v0.0.3 - 2026-05-16

### Changed
- README adopted LLM-first style: frontmatter YAML · table-driven · narrative removed · unique IDs (`[CLAIM]`, `[DIMENSIONS]`, `[ARTIFACTS]`, `[VERSIONING]`, `[LIMITATIONS]`).
- Format: 94 → 47 lines, no information loss.

## v0.0.2 - 2026-05-15

### Changed
- `[CLAIM]` revised to current form: "A standard defining graded, provable consistency among Spec, Code, and Tests."
- Resolved Trinity of Integrity brand collision: mechanism dimension (SSoT · Iron Law · Drift Detection) separated from measured dimension (Spec · Code · Tests) in `[DIMENSIONS]`.

## v0.0.1 - 2026-05-15

### Added
- Initial seed: README.md as MVP placeholder.
- Artifacts plan: `iron-law.md` · `detectors.schema.json` · `ears.md` · `GOVERNANCE.md` · `conformance/` (all TBD).
- Versioning policy: independent from harness-boot, semver (major: breaking · minor: additive · patch: doc).

