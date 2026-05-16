---
spec: ironclad/conformance/level-4
version: 0.0.1
status: draft
parent: ironclad/conformance
language: ko (translation of level-4.md — for human comprehension only)
references:
  - ../../../iron-law.md
---

# Level 4 (HITL) — Conformance Fixtures (한국어 대조본)

> 본 문서는 [`level-4.md`](../../../conformance/level-4.md) (영어 SSoT) 의 한국어 번역 — *이해 보조용*. 충돌 시 **영어 본이 우선**.

## [CLAIM] 핵심 정의

Level 4 (Human-in-the-Loop) stage 구현 검증을 위한 2 fixture. L1-L3 과 달리 본 fixture 는 **checklist 절차** — 자동 pass/fail 아님. 이유: L4 는 *사람 판단* 을 검증하는 것이지 *도구 동작* 을 검증하는 게 아니기 때문.

## [SCOPE] 범위

Level 4 = 2 stage, manual 사람 검토.

| stage | name | strictness | iron-law.md 참조 |
|---|---|---|---|
| stage_4.1 | Audit | hard | reviewer 가 AC 별 evidence + code review sign |
| stage_4.2 | UAT | hard | 사람이 AC 별 의도 부합 확인 |

**선결 조건**: Level 1 + Level 2 + Level 3 (자동화 가능한 11 stage 모두) 통과.

L4 호환성은 도구가 *사람 검토를 위한 절차적 infrastructure 제공* 검증 — 사람 판단의 *옳음* 검증 아님.

## [FIXTURES]

### [STAGE_4.1] Audit — Reviewer Checklist

피처의 각 AC 에 대해 reviewer 가 검증 + sign 해야:

```
[ ] (a) Evidence 충분성
    - 본 AC 에 대해 declared evidence (kind != gate_run / gate_auto_run) ≥ 1 존재.
    - Evidence kind 가 AC 성격과 정합 (행위 → test, UX → manual_check 등).
    - Evidence timestamp 가 최근 (피처 작업 window 내).

[ ] (b) Code/test review
    - Code 변경이 AC 의 *기록된 내용* 을 구현 (인접 아님).
    - Test 가 AC 의 condition/response (ears.md 문법) 를 assert.
    - 명백한 self-certification 없음 (예: LLM 생성 test 가 LLM 생성 code 를 *독립 검증 없이* 통과).

[ ] (c) Anti-cheating
    - LLM 작성 evidence 는 tag (`author: llm` 또는 동등).
    - Reviewer 의 sign-off (본 checklist) 가 LLM 작성자와 *구별*.
```

**Pass signal**: 도구가 reviewer identity + timestamp + per-AC 체크마크 (3 항목 × 모든 AC) 기록.
**Fail signal**: AC 중 ≥ 1 항목 미체크 OR reviewer identity 부재 OR reviewer == LLM 작성자.

### [STAGE_4.2] UAT — Intent Alignment Checklist

피처 전체에 대해 user (또는 product owner) 가 검증 + sign 해야:

```
[ ] (a) 동작이 의도와 부합
    - 피처를 의도된 시나리오로 사용 시 결과가 원래 목표와 정합 (AC 자구 그대로가 아님).

[ ] (b) 예상 외 동작 없음
    - 피처가 user 가 예상하는 그대로; hidden side effect 없음, 무관 변경 없음.

[ ] (c) 수용 가능한 trade-off
    - 구현이 선택한 옵션 (예: 지연 vs 비용, 단순성 vs 유연성) 을 user 가 수용.
```

**Pass signal**: 도구가 user identity + timestamp + 3 항목 체크마크 기록.
**Fail signal**: 항목 미체크 OR user identity 부재 OR 피처가 수정 필요 표시.

## [TOOL_REQUIREMENTS] 도구 요구사항

`iron-law: L4` 선언 위해 도구가 제공해야:

1. **Identity tracking**: evidence/sign-off 마다 사람 reviewer 와 LLM 작성자 식별자 *분리*.
2. **Per-AC sign-off surface**: reviewer 가 AC 별 checklist 항목 mark 가능한 UI 또는 CLI.
3. **Audit trail**: 모든 checklist 활동의 timestamped 기록 (immutable 또는 append-only).
4. **Anti-self-certification guard**: reviewer identity == LLM 작성자 시 도구가 sign-off 거부.

이들은 **infrastructure 요구사항** — 동작 fixture 아님.

## [CONFORMANCE_DECLARATION] 호환성 선언

도구가 `iron-law: L4` 선언 위한 조건:

1. Level 1 + Level 2 + Level 3 fixture 모두 통과 (누적).
2. Audit (stage_4.1) 와 UAT (stage_4.2) checklist infrastructure 둘 다 구현.
3. 실제 피처에서 *real human signer ≠ LLM author* 로 두 checklist 의 end-to-end 실행 시연.
4. 4 [TOOL_REQUIREMENTS] 충족 문서화.
5. 도구의 CHANGELOG 에 기록.

## [LIMITATIONS] v0.0.1 한계

- L4 는 **infrastructure** 검증 — **판단 품질** 아님. Signer 의 quality 는 표준 범위 밖.
- "독립 검증" ([STAGE_4.1] (b) 항) 은 프로젝트 정의; 권고: reviewer 가 code/test 생성 LLM prompt 작성자가 아님.
- Checklist 항목이 *고무도장* 인지 자동 검증 불가 — signer 무결성 + audit trail review 에 의존.
- Multi-reviewer workflow (signer 2+, quorum) 은 MVP 범위 밖; 단일 signer 로 L4 선언 충분.
