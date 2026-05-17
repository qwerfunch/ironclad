---
spec: ironclad/falsifications
version: 0.0.2
status: draft
parent: ironclad
language: ko (translation of falsifications/README.md — for human comprehension only)
references:
  - ../../../falsifications/README.md
  - ../iron-law.md
  - ../../../detectors.schema.json
---

# 반증 등록부 (한국어 대조본)

> 본 문서는 [`falsifications/README.md`](../../../falsifications/README.md) (영어 SSoT) 의 한국어 번역 — *이해 보조용*. 충돌 시 **영어 본이 우선**.

## [CLAIM] 핵심 주장

[`README.md`](../README.md) `[CLAIM]` 의 반증 가능성 조항에 대한 *운영 채널*:

> 반증 가능: 셋 중 하나라도 어긋난 사례가 Level N 을 통과한 채 발견되면 표준이 부정됨.

## [SUBMISSION_PROCEDURE] 제출 절차

```
1. [falsification] 태그로 GitHub issue 개설, 1 줄 요약.
2. falsifications/NNN-<slug>.md 추가 PR (template.md 사용).
3. Maintainer (Phase A: BDFL) 가 14 일 이내 재현 검증.
4. 결정:
   - 재현됨 + 유효: status: accepted 로 머지. [ACCUMULATION_POLICY] 트리거.
   - 재현됨 + 범위 외: status: closed-out-of-scope 사유와 함께 머지.
   - 재현 불가: 사유와 함께 PR close. 제출자는 추가 상세로 재제출 가능.
5. accepted 반증은 머지 후 *immutable* (append-only history).
```

## [ACCUMULATION_POLICY] 누적 정책

| 트리거 | 응답 |
|---|---|
| 같은 detector 누락 인용 3 건 | 해당 detector 의 `pass_criteria` 또는 `description` 개정 RFC |
| 같은 Level 이 mismatch 에도 통과 인용 3 건 | 해당 Level 에 새 detector 추가 또는 stage soft→hard 승급 RFC |
| L4 (HITL 우회) 반증 1 건 | 즉시 RFC — L4 는 reviewer ≠ author 의 앵커, 회귀가 critical |

## [SCOPE] 범위

| 포함 | 배제 |
|---|---|
| `iron-law: L<N>` 선언이 발행되었고 *이후* mismatch 발견된 반례 | Level 선언으로 이어지지 않은 구현 버그 |
| Spec · Code · Tests 가 동일 관찰 가능 동작에 명백히 어긋난 경우 | Level 정의에 대한 주관적 이견 (대신 [`GOVERNANCE.md`](../GOVERNANCE.md) RFC) |
| | 성능 · 에르고노믹스 · naming bikeshedding |

## [LIMITATIONS] v0.0.2 한계

- 외부 제출 0 — 등록부 0 건으로 개설 (정직한 baseline, 검증 아님).
- 임계값 (3) 은 Phase A 잠정; Phase B+ 에서 GOVERNANCE RFC 로 개정 가능.
- Phase A 재현 윈도우 (14 일) 는 다수 제출 동시 접수 시 연장 필요할 수 있음.
- 교차 등록부 dedup (동일 반증이 두 도구 issue tracker 에 보고) 미자동화.
