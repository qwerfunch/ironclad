---
spec: ironclad
file: CHANGELOG
format: keep-a-changelog 1.1.0
language: ko (translation of CHANGELOG.md — for human comprehension only)
---

# 변경 이력 (한국어 대조본)

> 본 문서는 [`CHANGELOG.md`](./CHANGELOG.md) (영어 SSoT) 의 한국어 번역 — *이해 보조용*. 충돌 시 **영어 본이 우선**.

Ironclad 표준의 모든 의미 있는 변경 사항.

표준 버전은 `harness-boot` (reference implementation) 버전과 **독립**.

## v0.0.14 - 2026-05-16

### Added
- `conformance/` 디렉터리 + `conformance/README.md` v0.0.1: 구성 (L1-L4) · 선언 형식 (`iron-law: L<N>, detectors: <subset>, ears: <full|partial|none>`) · 테스트 절차 · fixture 형식.
- `conformance/level-1.md` v0.0.1: Level 1 (Base Static) stage 의 6 fixture — 각 fixture: pass case · fail case · pass signal · fail signal. Stage-agnostic (언어/스택 무관).
- `conformance/level-1.ko.md` + `conformance/README.ko.md` 한국어 짝.
- `[CONFORMANCE_DECLARATION]` 절차: 4 단계 (구현 · fixture 실행 · signal 확인 · 기록).

### Changed
- README `[ARTIFACTS]` 표: `conformance/` 상태 TBD → `v0.0.1 (L1 only)`.
- README `[LIMITATIONS]`: "conformance test suite 없음" → "Level 1 만 (L2-L4 TBD)".

### Note
- L2/L3/L4 fixture 는 첫 외부 도구의 L2+ 호환 선언 시까지 보류.
- Fixture 는 시나리오 서술 — 언어-특화 코드 아님. 각 도구가 자기 stack 에 매핑.

## v0.0.13 - 2026-05-16

### Added
- README `[GLOSSARY]`: 7 brand 어휘 (Trinity of Integrity · Iron Law · Drift · SSoT · AC · EARS · HITL) + 산업 대응.
- README `[CONFORMANCE]`: 외부 도구 호환성 선언 형식 (`iron-law: L<N>, detectors: <subset>, ears: <full|partial>`).
- README `[STRUCTURE_RATIONALE]`: 5 절 정당화 + 진입용 2 절 (Glossary + Conformance).
- README frontmatter `inspired_by`: SLSA · WCAG · OpenAPI (GOVERNANCE.md 의 inspired_by 패턴과 정합).

### Note
- 기존 5 절 (CLAIM · DIMENSIONS · ARTIFACTS · VERSIONING · LIMITATIONS) 변경 X.
- README 가 이제 단일 진입점: terminology lookup 위한 glossary, 채택 form 을 위한 conformance, self-explanation 을 위한 rationale.

## v0.0.12 - 2026-05-16

### Changed
- `GOVERNANCE.md` → v0.2. `[PHASES]` 및 `[ROLES]` 표에 `industry alias` 칼럼 추가 (Solo Maintainer · TSC · Steering Council · Committer · Author).
- frontmatter 에 `inspired_by` 추가 (Python PEP 1, CNCF Governance, TC39 RFC stages).
- proposal template 에 `[SECURITY_IMPLICATIONS]` 절 필수로 추가.

### Added
- `[PHASE_COUNT_RATIONALE]` — 3 phase load-bearing (BDFL 시작 · working group 성장 · federation 성숙); 부재 phase 위치 (Foundation = 법적 래퍼 phase 아님 · Bus-factor 1→2 는 A→B 내 · Sunset 은 DEPRECATION_POLICY · Fork 는 범위 밖).
- `[QUORUM]` — phase 별 유효 의사결정 최소 maintainer 수 (A:1, B:floor(N/2)+1, C:3+1 steering).
- `[DEPRECATION_POLICY]` — spec 요소 폐기 절차 (5 단계), phase 별 폐기 기간, prior deprecation 없는 제거에 `--emergency-removal-reason`.
- `[LIMITATIONS]`: Code of Conduct Phase B trigger 까지 보류.

### Note
- Phase 수 변경 X (3).
- Role 수 변경 X (3).
- RFC 절차 변경 X (6 단계).

## v0.0.11 - 2026-05-16

### Changed
- `ears.md` → v0.0.2. `[PATTERNS]` 표에 `산업 대응` 칼럼 추가 (RFC 2119 · Gherkin · feature flag · exception path).
- BNF 정밀화: `ear_compound` 가 *최소 1 절* 필수, 각 절 최대 1 회, 순서 고정 (When → While → Where → If). 절들이 named production (`when_clause` 등) 으로 분리.
- RESPONSE 문법 노트: quantifier 포함 가능 (`within Xms`, `at most N times`).
- frontmatter 에 `modal_authority: RFC 2119` 추가 (shall/must/should 구별).

### Added
- `[NEGATION]` 절: 금지 표현 `shall not` — *없는 동작* 과 구별. 금지 동작을 명시적·검증 가능하게.
- `[PATTERN_COUNT_RATIONALE]` 절: 5 패턴 load-bearing (각 patern 이 다른 temporal/logical 의미); 부재 패턴 위치 (Quantified → RESPONSE · User Story → 범위 밖 · BDD Given-When-Then → compound EARS · Use Case → 범위 밖 · Prohibition → NEGATION).

### Note
- 패턴 수 변경 X (5 + compound).
- 패턴 이름 변경 X (Mavin 2009 원본).
- Retroactive 정합 정정: `iron-law.md` frontmatter version 0.0.1 → 0.0.2 으로 올림 (v0.0.9 patch 에서 누락됨).

## v0.0.10 - 2026-05-16

### Changed
- `detectors.schema.json` → v0.0.2. Detector 스키마에 `industry_alias` 와 `notes` optional 필드 추가.
- 13/19 detector 에 `industry_alias` 박음 (Orphan Artifact, Missing Code, Requirement Drift, Stack Violation, Architecture Linting, Semantic Lint, Test Gap, Test Lag, Coverage Regression, Secret Scanning, Performance Regression, Dead Spec, Reference Validation). 나머지 6 개는 Ironclad 고유 (산업 대응 어휘 없음).
- 7 detector 에 disambiguation `notes` (예: MISSING_TESTS vs UNTESTED_AC granularity, EVIDENCE_MISMATCH vs STALE_EVIDENCE dimension, HARDCODED_SECRET 의 iron-law stage_1.6 double-net).
- 3 모호 description 정밀화: UNMAPPED_ARTIFACT ("tracked source file ... not declared"), CONVENTION_DRIFT ("beyond syntactic lint"), EVIDENCE_MISMATCH ("hash differs ... stale at runtime").

### Added
- `detectors.schema.json` root `rationale` 객체: 수의 정당화, 의도된 중복, 부재 detector 의 위치 (Dependency Vulnerability · API Contract Breakage · Documentation Drift · Type Safety Erosion · License Compliance).

### Note
- Detector 수 변경 X (19).
- Detector ID 변경 X (버전 안정성).

## v0.0.9 - 2026-05-16

### Changed
- `iron-law.md`: `[LEVELS]` 표에 `산업 대응` 칼럼 추가 (SAST · ISTQB · acceptance).
- `iron-law.md`: `[STAGES]` 표에 `alias (산업)` 칼럼 모든 stage 추가; 모호한 이름 (`Commit`, `Arch`, `Smoke`, `Audit`) 명료화.
- `iron-law.md`: stage_3.1 Smoke 는 integration + system + basic e2e 흡수 (ISTQB 위계 AC 단위 통합) 명시.
- `iron-law.md`: stage_4.1 Audit 2 중 (evidence + code review, 같은 사람이 둘 다 sign — AI 자가검증 차단) 명시.

### Added
- `iron-law.md`: `[STAGE_COUNT_RATIONALE]` 절 — 13 stage 가 load-bearing 인 이유 (레벨별 정당화 + 부재 stage 위치).
- 한국어 번역 `iron-law.ko.md` 동기 갱신.

### Note
- Stage 이름 자체 변경 X (버전 안정성); alias 와 description 만 보강.
- Stage 수 변경 X (13).

## v0.0.8 - 2026-05-16

### Added
- `GOVERNANCE.md` v0.1: 3 거버넌스 phase (BDFL → working group → federation), RFC 절차, 역할 정의 (User · Contributor · Maintainer), phase 별 의사결정 모델, 전환 trigger, 분쟁 해소, 보안 신고 정책.
- `GOVERNANCE.ko.md` 한국어 대조본.
- 현재 phase: A (BDFL, maintainer = qwerfunch).

### Status
- 핵심 spec 4 종 (iron-law · detectors · ears · GOVERNANCE) + LICENSE + CHANGELOG 모두 초안 완료.
- 남음: `conformance/` test suite (post-MVP).

## v0.0.7 - 2026-05-16

### Added
- `ears.md` v0.0.1: 5 EARS 패턴 (ubiquitous · event-driven · state-driven · optional-feature · unwanted-behavior) + 복합 형태. 형식 BNF, 유효성 예시, AC ↔ 테스트 추적성 규칙.
- `ears.ko.md` 한국어 대조본.
- Reference 구현 `spec.yaml` acceptance_criteria 와의 통합 (문자열 + 구조화 형태).

## v0.0.6 - 2026-05-16

### Added
- `detectors.schema.json` v0.0.1: 19 drift detector 카탈로그 + self-describing 스키마. Severity `error|warn`, axes `spec_vs_code|code_vs_test|spec_vs_test|environment`, status `implemented|renaming|planned`. 각 detector: id (UPPER_SNAKE_CASE) · pass_criteria · remediation · legacy_kind (harness-boot 매핑).
- Detector 분포: spec_vs_code(6) · code_vs_test(6) · spec_vs_test(4) · environment(3).
- `DriftFinding` schema (detector 출력 형태).

## v0.0.5 - 2026-05-16

### Added
- `iron-law.md` v0.0.1: 4 단계 (L1-L4), 13 stage + 통과 기준, 누적 규칙, evidence 규칙 (모드 기반), 호환성 매핑 (모드 = 등급), LLM 비용 프로파일 (피처당 O(1)), 역사 (7 gates → 4 levels 정직한 명명).
- `iron-law.ko.md` 한국어 대조본.
- `LICENSE` (MIT) 및 `CHANGELOG.md` (+ .ko) 본 버전 cycle 에서 추가.

## v0.0.4 - 2026-05-16

### Changed
- README 를 영어로 재작성 (SSoT · 토큰 효율).
- README.ko.md 추가 — 한국어 번역 (이해 보조용. 충돌 시 영어 우선).

## v0.0.3 - 2026-05-16

### Changed
- README 에 LLM-first 스타일 적용: frontmatter YAML · 표 위주 · narrative 제거 · 고유 ID (`[CLAIM]`, `[DIMENSIONS]`, `[ARTIFACTS]`, `[VERSIONING]`, `[LIMITATIONS]`).
- 분량: 94 → 47 줄, 정보 손실 없음.

## v0.0.2 - 2026-05-15

### Changed
- `[CLAIM]` 핵심 주장 한 줄 현재 형태로 확정: "A standard defining graded, provable consistency among Spec, Code, and Tests."
- Trinity of Integrity brand 충돌 해소: *메커니즘 차원* (SSoT · Iron Law · Drift Detection) 과 *비교 대상 차원* (Spec · Code · Tests) 을 `[DIMENSIONS]` 에서 분리.

## v0.0.1 - 2026-05-15

### Added
- 초기 seed: README.md MVP placeholder.
- Artifacts 계획: `iron-law.md` · `detectors.schema.json` · `ears.md` · `GOVERNANCE.md` · `conformance/` (모두 TBD).
- 버전 정책: harness-boot 와 독립. semver (major: breaking · minor: additive · patch: doc).

