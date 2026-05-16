---
spec: ironclad/conformance
version: 0.0.1
status: draft
parent: ironclad
language: ko (translation of README.md — for human comprehension only)
references:
  - ../iron-law.md
  - ../detectors.schema.json
  - ../GOVERNANCE.md
---

# Conformance (한국어 대조본)

> 본 문서는 [`README.md`](./README.md) (영어 SSoT) 의 한국어 번역 — *이해 보조용*. 충돌 시 **영어 본이 우선**.

## [CLAIM] 핵심 정의

Ironclad Level N 호환성 선언을 위한 test fixture.

## [STRUCTURE] 구성

| level | 파일 | 범위 | 상태 |
|---|---|---|---|
| L1 | `level-1.md` | 6 static stage (Type · Lint · Drift · Commit · Arch · Secret) | v0.0.1 drafted |
| L2 | `level-2.md` | 2 mocked logic stage (Unit · Cov) | TBD |
| L3 | `level-3.md` | 3 empirical stage (Smoke · Perf · Visual) | TBD |
| L4 | `level-4.md` | 2 HITL stage (Audit · UAT) | TBD |

## [DECLARATION_FORMAT] 선언 형식

외부 도구는 자기 README/docs 에 다음 형식으로 호환성 선언:

```
ironclad-spec: <spec version>
iron-law:      L<N>
detectors:     <subset, 예: "5/19" 또는 "[UNTESTED_AC, STATUS_DRIFT, ...]">
ears:          <full | partial | none>
```

## [TEST_PROCEDURE] 테스트 절차

```
1. 도구가 target Level N 의 stage 들을 구현.
2. 도구가 level-<N>.md 의 fixture 들을 실행.
3. 모든 hard-stage fixture 통과 + soft-stage fixture 보고 → 도구는 iron-law: L<N> 선언 가능.
4. Phase B+ 외부 auditor 가 독립 검증 위해 재실행 가능.
```

누적 규칙 적용: `iron-law: L3` 선언은 L1 + L2 + L3 fixture 모두 통과 필수.

## [FIXTURE_FORMAT] Fixture 형식

각 stage fixture 는 균일 구조:

```
### [STAGE_<N>.<M>] <Name>
- Pass case:    <stage 통과해야 하는 최소 artifact>
- Fail case:    <stage 실패해야 하는 최소 artifact>
- Pass signal:  <도구 출력 — exit 0 또는 detector severity != error>
- Fail signal:  <도구 출력 — fail 표시>
```

## [LIMITATIONS] v0.0.1 한계

- Level 1 fixture 만 제공. L2-L4 TBD.
- 자동 test runner 아직 없음 — 도구가 self-verify. Phase B+ 에서 audit tooling 추가 가능.
- Fixture 는 시나리오 서술. 언어/스택 무관 — 각 도구가 자기 stack 에 매핑.

