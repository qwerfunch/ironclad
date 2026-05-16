---
spec: ironclad/conformance/level-2
version: 0.0.1
status: draft
parent: ironclad/conformance
language: ko (translation of level-2.md — for human comprehension only)
references:
  - ../iron-law.md
---

# Level 2 (Mocked Logic) — Conformance Fixtures (한국어 대조본)

> 본 문서는 [`level-2.md`](./level-2.md) (영어 SSoT) 의 한국어 번역 — *이해 보조용*. 충돌 시 **영어 본이 우선**.

## [CLAIM] 핵심 정의

외부 도구의 Level 2 (Mocked Logic) stage 구현 검증을 위한 2 fixture.

## [SCOPE] 범위

Level 2 = 2 stage. 런타임 + mocked externals.

| stage | name | strictness | iron-law.md 참조 |
|---|---|---|---|
| stage_2.1 | Unit | hard | test runner exit 0, mocked externals |
| stage_2.2 | Cov | soft | 커버리지 >= 프로젝트 임계값 |

**선결 조건**: Level 1 (6 stage 모두) 통과 후 Level 2 시도.

## [FIXTURES]

### [STAGE_2.1] Unit

- **Pass case**: unit test ≥ 1 개의 모듈; 모든 test 통과; external I/O (network, disk, DB) mocked.
- **Fail case**: 동일 모듈에 의도적으로 test 1 개 실패 (예: assertion fail).
- **Pass signal**: test runner exit 0; pass count = total test count.
- **Fail signal**: test runner exit non-zero OR fail count ≥ 1.

### [STAGE_2.2] Cov

- **Pass case**: source + tests 로 line coverage ≥ 프로젝트 임계값 (예: 80%).
- **Fail case**: test 제거하여 coverage 가 임계값 미만으로.
- **Pass signal**: coverage tool 측정값 ≥ 임계값.
- **Fail signal**: coverage tool 측정값 < 임계값.

`stage_2.2 Cov` 는 `soft` (iron-law.md) — coverage drop 은 보고하나 `kind=cov_below` evidence + 사유 기록 시 완료 차단 X.

## [CONFORMANCE_DECLARATION] 호환성 선언

도구가 `iron-law: L2` 선언 위한 조건:

1. Level 1 fixture 모두 통과 (누적 규칙).
2. Level 2 두 stage 구현.
3. 두 fixture 실행 (pass case AND fail case).
4. Pass case 에 pass signal 발화 AND fail case 에 fail signal 발화 확인.
5. 도구의 CHANGELOG 또는 audit log 에 결과 기록.

## [LIMITATIONS] v0.0.1 한계

- Cov 임계값은 프로젝트 정의 — spec 이 고정 수치 X.
- "Mocked externals" 경계 (무엇이 external 인지) 는 프로젝트 정의.
- Property-based · mutation testing 은 범위 밖 (프로젝트 결정).
