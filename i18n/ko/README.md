---
spec: ironclad
version: 0.0.23
status: draft
language: ko (translation of README.md — for human comprehension only)
license: MIT
reference_implementation: https://github.com/qwerfunch/harness-boot
inspired_by:
  - "SLSA (graded supply-chain integrity)"
  - "WCAG (graded accessibility conformance)"
  - "OpenAPI (machine-readable spec + reference impl)"
---

# Ironclad 표준 (한국어 대조본)

> 본 문서는 [`README.md`](../../README.md) (영어 SSoT) 의 한국어 번역 — *이해 보조용*. 사양 충돌 시 **영어 본이 우선**.

## [CLAIM] 핵심 주장

> Spec · Code · Tests 사이의 정합성을 등급으로 **반증 가능하게** 정의하는 표준.

반증 가능: 셋 중 하나라도 어긋난 사례가 Level N 을 통과한 채 발견되면 표준이 부정됨. 반례 등록부는 `falsifications/` 참조.

LLM 보조 저작 시대 특정 정당화 (자가검증 순환 차단 · LLM 비의존 검증 예산): `iron-law.md` `[AI_ERA_RATIONALE]` 참조.

## [DIMENSIONS] 차원

| dim | pieces | 역할 |
|---|---|---|
| measured | Spec · Code · Tests | 정합성 측정의 대상 |
| mechanism | SSoT · Iron Law · Drift Detection | 강제 도구 (Trinity of Integrity) |

## [ARTIFACTS] 명세 구성

| 파일 | 내용 | 버전 |
|---|---|---|
| `iron-law.md` | Level 1-4 · 통과 기준 · conformance | v0.0.6 |
| `detectors.schema.json` | 19 detector contract (JSON Schema + 카탈로그) | v0.0.3 |
| `ears.md` | EARS AC 문법 (5 패턴 + BNF) | v0.0.2 |
| `GOVERNANCE.md` | RFC 절차 · BDFL · 외부 채택 | v0.3 |
| `conformance/` | Level test suite (L1-L4) | v0.0.1 |
| `falsifications/` | 반례 등록부 (반증 가능성 운영화) | v0.0.2 |

## [VERSIONING] 버전 정책

- major: breaking change (Level 정의 · detector contract · BNF 변경)
- minor: additive (새 detector · 새 stage 추가)
- patch: 문서 · 예시
- harness-boot 버전과 **독립**

## [CONFORMANCE] 호환성

외부 도구는 다음 형식으로 호환성 선언:

```
iron-law: L<N>           # Level 1, 2, 3, 또는 4
detectors: <subset>      # 예: "5/19" 또는 detector ID 목록
ears: <full | partial | none>
```

예: `iron-law: L2, detectors: 5/19, ears: partial` = 도구가 Level 2 까지 강제, 5 개 detector 구현, EARS 를 일부 surface 에 수용. Level 기준은 `iron-law.md [CONFORMANCE]`, detector ID 는 `detectors.schema.json` 참조.

## [GLOSSARY] 용어집

| 용어 | 정의 | 산업 대응 |
|---|---|---|
| Trinity of Integrity | SSoT · Iron Law · Drift Detection (3 강제 메커니즘) | — (Ironclad 고유) |
| Iron Law | 4 단계 누적 품질 검증 | graded gate (SLSA 식) |
| Drift | {Spec, Code, Tests} 중 두 piece 사이의 측정 가능한 불일치 | inconsistency · divergence |
| SSoT | Single Source of Truth — 권위 있는 spec 산출물 (보통 `spec.yaml`) | source of truth · canonical record |
| AC | Acceptance Criterion — 검증 가능한 단일 요구사항 (see `ears.md` BNF) | requirement · acceptance test |
| EARS | AC 의 제약된 자연어 문법 | requirement syntax (Mavin 2009) |
| HITL | Human-in-the-Loop — Iron Law 의 Level 4 (사람 검토 필수) | manual gate · acceptance review |
| Conformance Level | 도구가 선언한 Iron Law level (L1-L4); 누적 | WCAG conformance level · SLSA level |
| Fixture | `conformance/level-N.md` 에 정의된 test case 하나 (pass case + fail case) | test case · scenario |
| Pass Signal | fixture 의 pass case 가 성공함을 증명하는 도구 출력 | green check · exit 0 |
| Fail Signal | fixture 의 fail case 가 올바르게 실패함을 증명하는 도구 출력 | red flag · non-zero exit |
| Falsification | 반례: Level N 을 통과한 상태에서 Spec/Code/Tests 의 불일치가 발견된 case | (Popper); `falsifications/README.md` 참조 |
| Self-certification cycle | 실패 모드: 같은 LLM 이 코드와 그 검증 테스트를 모두 저작 → 같은 오해가 두 번 인코딩됨 | — (Ironclad 고유; `iron-law.md [AI_ERA_RATIONALE]` 참조) |
| LLM-free verification budget | detector 카탈로그가 LLM 호출 0 회를 보장하는 구조 (18/19 deterministic, 1 semantic) | — (Ironclad 고유; `iron-law.md [LLM_COST_PROFILE]` 참조) |

## [STRUCTURE_RATIONALE] 8 절의 정당화

8 절 총 = 5 + 2 + 1 구조:
- **5 core** (load-bearing): 정체성 (CLAIM), 범위 (DIMENSIONS), 인벤토리 (ARTIFACTS), 진화 (VERSIONING), 정직 (LIMITATIONS).
- **2 entry** (외부 채택): GLOSSARY (terminology lookup) + CONFORMANCE (선언 형식).
- **1 self** (meta): STRUCTURE_RATIONALE — 본 절, 자기 구조 설명.

그 이상은 artifact 파일 (`iron-law.md`, `detectors.schema.json`, `ears.md`, `GOVERNANCE.md`, `conformance/`) 에 속함 — README 는 *index* 이지 *spec body* 아님.

**번역본 (비규범, 이해 보조용)**: `i18n/<lang>/` 디렉터리에 번역 미러. 영어 root 가 SSoT — 충돌 시 영어 본 우선.

## [LIMITATIONS] 현재 한계 v0.0.20

- conformance test suite 가 4 level 모두 cover, 단 첫 외부 도구 호환 선언 기반 정밀화 보류
- 정식 RFC 트랙 없음 (첫 RFC 시 proposals/ 생성)
- 외부 채택 사례 없음 — reference 구현은 `harness-boot` 단독. v1.0 졸업 조건은 `GOVERNANCE.md` `[DRAFT_GRADUATION]` 참조
- BDFL-only governance (qwerfunch 단독)
- `falsifications/` 등록부는 v0.0.18 에서 막 개설 — accepted 항목 0 (정직한 baseline, 검증 아님)
