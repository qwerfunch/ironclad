---
spec: ironclad/conformance/level-3
version: 0.0.1
status: draft
parent: ironclad/conformance
language: ko (translation of level-3.md — for human comprehension only)
references:
  - ../../../iron-law.md
---

# Level 3 (Full Empirical) — Conformance Fixtures (한국어 대조본)

> 본 문서는 [`level-3.md`](../../../conformance/level-3.md) (영어 SSoT) 의 한국어 번역 — *이해 보조용*. 충돌 시 **영어 본이 우선**.

## [CLAIM] 핵심 정의

외부 도구의 Level 3 (Full Empirical) stage 구현 검증을 위한 3 fixture.

## [SCOPE] 범위

Level 3 = 3 stage. 실제 런타임 환경.

| stage | name | strictness | iron-law.md 참조 |
|---|---|---|---|
| stage_3.1 | Smoke | hard | per-AC smoke 증명, 실제 환경 |
| stage_3.2 | Perf | soft | `performance_budget` 내 |
| stage_3.3 | Visual | hard | per-AC 시각 증명 (VRT 일치) |

**선결 조건**: Level 1 + Level 2 (8 stage 모두) 통과.

`stage_3.1 Smoke` 는 integration + system + basic e2e 흡수 (iron-law.md).
`stage_3.3 Visual` 은 UI 없는 피처에서 N/A (auto-pass).

## [FIXTURES]

### [STAGE_3.1] Smoke

- **Pass case**: 피처가 실제 환경 (또는 production 동등 test env) 배포; 각 AC 가 관찰 가능한 결과 산출 (log line · HTTP response · state change).
- **Fail case**: 한 AC 의 smoke check 가 예상치 못한 결과 (예: 500 대신 200, log line 부재).
- **Pass signal**: 피처의 모든 AC ID 에 대해 per-AC smoke check 가 expected result 반환.
- **Fail signal**: AC smoke check ≥ 1 개가 unexpected result OR 관찰 신호 없음.

### [STAGE_3.2] Perf

- **Pass case**: 측정값이 선언된 `performance_budget` 내 (예: LCP < 2.5s, p95 latency < 500ms, bundle < 200KB).
- **Fail case**: 측정값이 budget 차원 ≥ 1 개 초과.
- **Pass signal**: 모든 budget 차원이 임계값 내; 이전 baseline 대비 noise 넘는 regression 없음.
- **Fail signal**: budget 차원 ≥ 1 개 초과 OR 통계적으로 유의한 regression.

`stage_3.2 Perf` 는 `soft` — over-budget 보고하나 `kind=perf_regression` evidence 기록 시 완료 차단 X (`kind=perf_resolved` 로 clear).

### [STAGE_3.3] Visual

- **Pass case**: 렌더링된 UI 가 저장된 baseline image (per AC) 와 VRT tolerance 내 일치, OR 사람이 현재 렌더링을 새 baseline 으로 승인.
- **Fail case**: 렌더링된 UI 가 baseline 과 tolerance 넘게 차이 AND 새 baseline 사람 승인 없음.
- **Pass signal**: 모든 AC 의 visual proof 에 대해 VRT diff ≤ tolerance.
- **Fail signal**: VRT diff > tolerance AND `harness visual-approve` (또는 동등) 없음.

UI 없는 피처 (`ui_surface.present: false`) 에서 stage_3.3 은 N/A (auto-pass).

## [CONFORMANCE_DECLARATION] 호환성 선언

도구가 `iron-law: L3` 선언 위한 조건:

1. Level 1 + Level 2 fixture 모두 통과 (누적 규칙).
2. Level 3 의 3 stage 모두 구현.
3. 3 fixture 실행 (각각 pass case AND fail case).
4. Pass case 에 pass signal 발화 AND fail case 에 fail signal 발화 확인.
5. 도구의 CHANGELOG 또는 audit log 에 결과 기록.

## [LIMITATIONS] v0.0.1 한계

- `performance_budget` 차원과 임계값은 프로젝트 정의.
- VRT tolerance (px diff · ssim threshold) 는 프로젝트 정의.
- "실제 환경" 경계는 프로젝트 정의 (production 동등 staging vs ephemeral test env).
- Multi-platform visual (iOS/Android/desktop) 은 플랫폼당 1 fixture; sub-stage 로 분리 X.
