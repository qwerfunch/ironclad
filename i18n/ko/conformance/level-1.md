---
spec: ironclad/conformance/level-1
version: 0.0.1
status: draft
parent: ironclad/conformance
language: ko (translation of level-1.md — for human comprehension only)
references:
  - ../../../iron-law.md
  - ../../../detectors.schema.json
---

# Level 1 (Base Static) — Conformance Fixtures (한국어 대조본)

> 본 문서는 [`level-1.md`](../../../conformance/level-1.md) (영어 SSoT) 의 한국어 번역 — *이해 보조용*. 충돌 시 **영어 본이 우선**.

## [CLAIM] 핵심 정의

외부 도구의 Level 1 (Base Static) stage 구현 검증을 위한 6 fixture.

## [SCOPE] 범위

Level 1 = 6 static stage. 모두 `hard` strictness, 모두 `fast` speed. 런타임 불필요.

| stage | name | iron-law.md 참조 |
|---|---|---|
| stage_1.1 | Type | type checker exit 0 |
| stage_1.2 | Lint | linter exit 0 |
| stage_1.3 | Drift | 모든 detector severity != error |
| stage_1.4 | Commit | clean working tree |
| stage_1.5 | Arch | layer 위반 없음 |
| stage_1.6 | Secret | credential 패턴 없음 |

## [FIXTURES]

### [STAGE_1.1] Type

- **Pass case**: 프로젝트 타입 시스템에 맞는 유효 타입 주석을 가진 source 파일.
- **Fail case**: 동일 파일에 `string` 을 `number` 타입 변수에 할당하는 코드 1 개 추가.
- **Pass signal**: 타입 체커 exit 0, error 진단 0 개.
- **Fail signal**: 타입 체커 exit non-zero OR error 진단 ≥ 1.

### [STAGE_1.2] Lint

- **Pass case**: 프로젝트 lint 설정 준수하는 source 파일.
- **Fail case**: 동일 파일에 lint 규칙 위반 1 개 (예: 미사용 변수, 강제된 세미콜론 누락).
- **Pass signal**: linter exit 0, error-level finding 0 개.
- **Fail signal**: linter 가 error-level finding ≥ 1 보고.

### [STAGE_1.3] Drift

- **Pass case**: spec.yaml 와 source 트리 일관 — 모든 `features[].modules[]` entry 가 파일과 대응, 모든 tracked source 파일이 feature 와 매핑.
- **Fail case**: 매핑되지 않은 source 파일 1 개 추가 (`UNMAPPED_ARTIFACT` detector trigger).
- **Pass signal**: 19 detector 모두 severity != error 반환 (`detectors.schema.json` 참조).
- **Fail signal**: detector ≥ 1 개가 severity `error` 반환.

### [STAGE_1.4] Commit

- **Pass case**: working tree clean (`git diff --quiet && git diff --cached --quiet`).
- **Fail case**: working tree 에 unstaged modification 1 개.
- **Pass signal**: 두 diff 검사 모두 exit 0.
- **Fail signal**: 둘 중 하나라도 non-zero.

### [STAGE_1.5] Arch

- **Pass case**: import graph 가 선언된 layer 순서 준수 (예: UI → service → data, 역방향 edge 없음).
- **Fail case**: layer 순서를 역방향으로 만드는 import 1 개 추가 (예: data layer 가 UI 에서 import).
- **Pass signal**: arch lint 위반 0 개.
- **Fail signal**: arch lint 위반 ≥ 1.

### [STAGE_1.6] Secret

- **Pass case**: tracked 파일 어디에도 API key / token / password 패턴 없음.
- **Fail case**: `API_KEY = "sk_live_..."` 같은 line 을 가진 파일 1 개 추가.
- **Pass signal**: secret scanner match 0 개.
- **Fail signal**: secret scanner match ≥ 1.

## [CONFORMANCE_DECLARATION] 호환성 선언

도구가 `iron-law: L1` 호환성 선언 위한 조건:

1. 6 stage 모두 구현 (또는 무관 stage 는 사유와 함께 N/A 표시).
2. 6 fixture 실행 (각각 pass case AND fail case).
3. Pass case 에 pass signal 모두 발화 AND fail case 에 fail signal 모두 발화 확인.
4. 결과를 도구의 CHANGELOG 또는 audit log 에 기록.

## [LIMITATIONS] v0.0.1 한계

- Fixture 는 언어/스택 무관 서술. 각 도구가 자기 구현에 매핑.
- 아직 machine-readable harness 없음 — 도구가 6 fixture self-run.
- N/A 허용: stage_1.5 Arch 는 프로젝트가 < 2 layer 일 때만 N/A; stage_1.4 Commit 은 VCS 미사용 시 N/A.
- Phase B+ 에서 더 강한 요구사항 추가 가능 (예: detector subset 별 severity 임계값).

