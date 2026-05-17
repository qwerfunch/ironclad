---
spec: ironclad/iron-law
version: 0.0.6
status: draft
parent: ironclad
language: ko (translation of iron-law.md — for human comprehension only)
references:
  - ../../detectors.schema.json
  - ../../ears.md
  - ../../GOVERNANCE.md
---

# Iron Law (한국어 대조본)

> 본 문서는 [`iron-law.md`](../../iron-law.md) (영어 SSoT) 의 한국어 번역 — *이해 보조용*. 충돌 시 **영어 본이 우선**.

## [CLAIM] 핵심 정의

4 단계 누적 품질 검증 — 각 상위 단계는 하위 단계 통과 + 더 강한 증거를 요구.

## [LEVELS] 등급

| level | name | 환경 | 산업 대응 | LLM 비용 / 피처 |
|---|---|---|---|---|
| L1 | Base Static | 도구만 (런타임 없음) | SAST + linter + secret scanner | 0 |
| L2 | Mocked Logic | 런타임, mocked externals | unit + coverage (ISTQB) | 0 |
| L3 | Full Empirical | 런타임, 실제 환경 | integration + system + e2e | 0-1 (결과 해석) |
| L4 | HITL | 사람 검토 | acceptance + audit | 0 (LLM), 1 (사람) |

## [STAGES] 13 단계

| stage | level | name | alias (산업) | 엄격도 | 속도 | 통과 기준 |
|---|---|---|---|---|---|---|
| stage_1.1 | L1 | Type | Type Check / SAST | hard | fast | 타입 체커 exit 0, 오류 없음 |
| stage_1.2 | L1 | Lint | Static Analysis | hard | fast | 린터 exit 0, 오류 없음 |
| stage_1.3 | L1 | Drift | Consistency Check (Ironclad 고유) | hard | fast | 모든 detector (see `detectors.schema.json`) severity != error |
| stage_1.4 | L1 | Commit | VCS Hygiene · Clean Tree | hard | fast | `git diff --quiet && git diff --cached --quiet` |
| stage_1.5 | L1 | Arch | Architectural Conformance | hard | fast | 의존성 방향 유효, 레이어 위반 없음 |
| stage_1.6 | L1 | Secret | Secret Scanning | hard | fast | tracked 파일에 API key / token / credential 패턴 없음 |
| stage_2.1 | L2 | Unit | Unit Test (ISTQB) | hard | mid | 테스트 러너 exit 0, 모든 unit 테스트 통과 (mocked externals) |
| stage_2.2 | L2 | Cov | Code Coverage | soft | mid | 커버리지 >= 임계값 (프로젝트 정의) |
| stage_3.1 | L3 | Smoke | System Integration Probe | hard | heavy | per-AC smoke 증명: 실제 환경, 관찰 가능한 결과 |
| stage_3.2 | L3 | Perf | Performance Test | soft | heavy | `performance_budget` 내 (프로젝트 정의) |
| stage_3.3 | L3 | Visual | Visual Regression Test (VRT) | hard | heavy | per-AC 시각 증명 (VRT 일치 또는 사람 승인 baseline) |
| stage_4.1 | L4 | Audit | Evidence + Code Review | hard | manual | reviewer 가 AC 별 evidence 충분성 AND 코드/테스트 변경 둘 다 sign |
| stage_4.2 | L4 | UAT | User Acceptance Test | hard | manual | AC 단위 의도 부합 사람 확인 |

Notes:
- `stage_3.1 Smoke` 는 ISTQB 위계의 integration + system + basic e2e 모두 흡수 — Ironclad 는 AC 단위로 통합 (per-AC real-env proof).
- `stage_3.3 Visual` 은 UI 가 없는 피처에서 N/A (auto-pass).
- `stage_4.1 Audit` 은 AC 별 2 중: (a) evidence 충분성, (b) 코드/테스트 검토. 같은 사람이 둘 다 sign — AI 자가검증에 대한 구조적 장벽 (암호학적 보장 아님; `conformance/level-4.md [LIMITATIONS]` 참조).
- `stage_1.3 Drift`: 19 detector 중 18 은 `determinism: deterministic` (LLM 비의존; hash/diff/regex/graph), 1 은 `semantic` (`CONVENTION_DRIFT`, 여전히 LLM 비의존). LLM 요구 detector 없음 — 도구는 deterministic 만 부분 호환 선언 가능.

## [STAGE_COUNT_RATIONALE] 13 의 정당화

13 stage 수는 *load-bearing* — padded / minimal 아님.

| level | stages | 정당화 |
|---|---|---|
| L1 | 6 | 정적 검사는 별개 도구 surface 로 분해 — 합치면 명료성 손실 |
| L2 | 2 | mocked logic = unit + coverage (격리 테스트의 ISTQB primitive) |
| L3 | 3 | functional + non-functional + visual (실증 증명의 3 차원) |
| L4 | 2 | mechanical (evidence audit) + qualitative (intent alignment) |

**부재 stage 와 그 위치:**

| absent | 위치 |
|---|---|
| Integration / System / E2E | stage_3.1 Smoke (의도된 통합) |
| Format / Style | stage_1.2 Lint |
| Regression | stage_2.1 Unit + stage_3.1 Smoke 재실행 |
| Mutation Testing | 표준 범위 밖 (프로젝트 결정) |
| Fuzz Testing | 표준 범위 밖 (프로젝트 결정) |

14 번째 stage 는 기존 stage 의 surface 중복 OR marginal gain 위한 의미 분할. 12-stage 버전은 Type/Lint, Smoke/Perf, Audit/UAT 통합 — 각 분리가 *load-bearing*.

## [CUMULATIVE_RULE] 누적 규칙

```
L(N) 은 L(N-1) 통과 후에만 시도 가능.
한 레벨 내에서 모든 hard stage 통과 필수, soft stage 는 결과 보고 필수 (pass | fail | n/a).
```

## [EVIDENCE_RULE] 증거 규칙

완료에 필요한 *declared evidence* (kind != `gate_run` / `gate_auto_run`) — 모드별:

| 모드 | 최소 declared evidence | 강제 최대 level |
|---|---|---|
| tutorial | 1 | L1 |
| prototype | 1 | L2 |
| product | 3 | L3 |
| regulated | 3 | L4 |

`--hotfix-reason` 사용 시 product / regulated 도 1 declared evidence 로 완료 가능. 사유는 `kind=hotfix` evidence 로 자동 기록 (audit trail 보존).

## [CONFORMANCE] 호환성

모드 ↔ 레벨 매핑은 *대각선* (매트릭스 아님):

```
tutorial   conforms to L1
prototype  conforms to L2
product    conforms to L3
regulated  conforms to L4
```

외부 도구는 `iron-law: L<N>` 형태로 호환성 선언 가능. 각 level 검증을 위한 test fixture 는 `conformance/level-{1,2,3,4}.md` 참조.

`risk-aware override`: `entities[].sensitive` 플래그 또는 security / payment area 피처는 자동으로 한 단계 상향 (예: product 모드의 sensitive=true 피처 → L4 강제).

## [LLM_COST_PROFILE] LLM 비용 프로파일

| level | 도구 | LLM 호출 / 피처 |
|---|---|---|
| L1 | type checker, linter, drift detector, git, arch lint, secret scan | 0 |
| L2 | test runner, coverage tool | 0 |
| L3 | smoke harness, perf tool, visual diff | 0-1 (해석만; 결정론적 pass/fail 이면 0) |
| L4 | 사람 reviewer | 0 (LLM 비용), 1 사람 검토 |

4 레벨 모두 통과해도 최대 LLM 호출 1 회 (L3 결과 해석; 결정론적 pass/fail 도달 시 skip). 피처당 LLM 비용은 상수에 bound; N 피처에 대한 총 비용은 O(N).

## [HISTORY] 역사

4 단계 구조는 harness-boot 의 pre-Ironclad 7 평면 게이트 (`gate_0..gate_5, gate_perf`) 에서 파생:

| pre-Ironclad gate | ironclad stage | 변화 |
|---|---|---|
| gate_0 (tests) | stage_2.1 Unit | 재분류 |
| gate_1 (typecheck) | stage_1.1 Type | 재분류 |
| gate_2 (lint) | stage_1.2 Lint | 재분류 |
| gate_3 (coverage) | stage_2.2 Cov | 재분류 |
| gate_4 (commit) | stage_1.4 Commit | 재분류 |
| gate_5 (smoke) | stage_3.1 Smoke | 재분류 |
| gate_perf | stage_3.2 Perf | 재분류 |
| (없음) | stage_1.3 Drift | `harness check` 에서 승급 |
| (없음) | stage_1.5 Arch | 신설 |
| (없음) | stage_1.6 Secret | 신설 |
| (없음) | stage_3.3 Visual | 신설 |
| (없음) | **L4 (HITL)** | **재프레이밍 — reviewer ≠ LLM author** |

정직한 명명: L1-L3 = 재분류 + 신설 3 종. L4 = 재프레이밍된 HITL. 4 단계 모양은 산업 등급 표준 (SLSA: 4 등급, PCI DSS: 4 등급, CMM: 5 등급) 과 정합하여 *부분 호환성* 가능.

## [L4_DEFECT_CATEGORIES] L4 가 잡는 결함 카테고리

L4 (HITL) 가 존재하는 이유: 다음 카테고리는 *구조적으로 비자동화* — 판단 자체가 알고리즘적이지 않음.

| # | 카테고리 | L1-L3 가 못 잡는 이유 | L4 surface |
|---|---|---|---|
| 1 | **Intent misalignment (의도 불일치)** | AC 는 literally 통과하나 구현이 저자의 *의도한 의미* 와 어긋남. | `stage_4.2 UAT` — AC 별 의도 확인 |
| 2 | **Self-certification cycle (자가검증 순환)** | 같은 LLM 이 구현과 검증 테스트를 모두 작성 — 같은 오해가 두 번 인코딩됨. | `stage_4.1 Audit` (c) — reviewer ≠ LLM author |
| 3 | **Hidden side effects / surprise (숨겨진 부작용)** | AC 는 만족하나 어떤 AC 도 명시 금지하지 않은 동작 도입 (negative space 비열거). | `stage_4.2 UAT` — "surprise 없음" sign |
| 4 | **Trade-off acceptability (트레이드오프 수용성)** | 기술적으로 *옳지만 사용자가 아직 인가하지 않은* 선택. | `stage_4.2 UAT` — trade-off 수용 sign |
| 5 | **Security context conflict (보안 맥락 충돌)** | 어떤 정적/실증 검사도 인코딩하지 않은 방식으로 위협 모델과 충돌. | `stage_4.1 Audit` (b) — security context 교차확인 |

Load-bearing: 5 는 최소 — 6 번째는 위 5 에 포섭되지 않음을 입증해야. L4 는 L1-L3 결함 (perf · type · missing tests) 의 catch-all 아님 — 그건 `[CUMULATIVE_RULE]` 의 몫.

Tie-break: 한 defect 가 여러 카테고리에 해당할 때, *근본 원인* 으로 분류 (증상 아님) — self-certification cycle (#2) 가 intent misalignment (#1) 증상을 만들면 #2 에 속함 (stage_4.1 (c)). Security vs trade-off 충돌 (#4 ∩ #5) 은 위협 모델 규칙이 관련되면 #5, 그렇지 않으면 #4.

## [AI_ERA_RATIONALE] AI 시대 정당화

4 단계 모양은 산업 등급 표준 (`[HISTORY]`) 과 정합. Iron Law 를 LLM 보조 저작 시대의 표준으로 만드는 것은 두 구조적 앵커:

| 앵커 | AI 시대 특정성 | 위치 |
|---|---|---|
| 1. 자가검증 순환 차단 | 같은 모델이 코드와 그 검증 테스트를 동시 저작 → 같은 오해가 두 번 인코딩 → 기존 자동화가 통과시킴. | stage_4.1 (c) reviewer ≠ LLM author · level-4.md `[TOOL_REQUIREMENTS]` #4 |
| 2. LLM 비의존 검증 예산 | LLM 출력을 LLM 으로 검증하는 것은 turtles-all-the-way-down; detector 카탈로그가 구조적으로 LLM 비의존 (18/19 deterministic, 1 semantic, 0 llm_assisted). 피처당 LLM 비용 O(1). | `[LLM_COST_PROFILE]` · detectors.schema.json `rationale.determinism_guarantee` |

산업 표준 SQA stage 의 산업 기원은 `[STAGES]` alias 칼럼 참조 — 본 표준은 이를 재분류로 흡수. `[HISTORY]` 의 "(없음)" 행 너머의 Ironclad 고유: 표준 메타 채널로서의 falsification 등록부.

v0.0.18 비평 #3 (SCR — `author: llm` 자기 태깅) 는 *self-reporting is gameable* 이유로 거부; determinism + reviewer ≠ author 가 게이밍 surface 없는 구조적 동등물.

## [LIMITATIONS] v0.0.5 한계

- 자가 치유 자동화 (3 회 재시도, auto-rollback) 는 *지향점* — 본 spec 에 아직 포함 X.
- Conformance test suite (`conformance/level-{N}/`) 아직 제공 X.
- `stage_3.3 Visual` baseline 승인 절차는 도구 정의 — 표준화 X.

