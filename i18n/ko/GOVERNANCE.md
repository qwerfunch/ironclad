---
spec: ironclad/governance
version: 0.3
status: draft
parent: ironclad
language: ko (translation of GOVERNANCE.md — for human comprehension only)
references:
  - ../../iron-law.md
  - ../../CHANGELOG.md
inspired_by:
  - "Python PEP 1 (BDFL → Steering Council)"
  - "CNCF Governance (TSC + Maintainer)"
  - "TC39 RFC stages (Champion + Stage 0-4)"
---

# 거버넌스 (한국어 대조본)

> 본 문서는 [`GOVERNANCE.md`](../../GOVERNANCE.md) (영어 SSoT) 의 한국어 번역 — *이해 보조용*. 충돌 시 **영어 본이 우선**.

## [CLAIM] 핵심 정의

Ironclad 표준의 *변경 제안 · 검토 · 승인* 모델. BDFL 단독에서 시작 → 외부 contributor 누적 시 working-group 합의로 확장.

## [PHASES] 단계

| phase | 조건 | 산업 alias | 의사결정 모델 | 현재 |
|---|---|---|---|---|
| A: BDFL | 단독 메인테이너 | Solo Maintainer · Benevolent Dictator (Python · Linux pre-2022) | maintainer 결정 | ✓ |
| B: working group | 2+ 조직의 active maintainer 3+ 명 | TSC (CNCF) · Core Team (Rust) | lazy consensus + veto | TBD |
| C: federation | maintainer 5+ 명 + 1+ 외부 production 채택 | Steering Council (Python) · Foundation TSC (CNCF) | 정식 working group + steering committee | TBD |

전환은 trigger 조건 충족 시 자동. 본 문서를 *전환 일자* 와 함께 patch 갱신.

## [PHASE_COUNT_RATIONALE] 3 의 정당화

3 phase = *load-bearing*. padded / minimal 아님.

| phase | 역할 |
|---|---|
| A (BDFL) | 시작 상태 — 모든 신생 spec 의 필수 (단독 저자) |
| B (working group) | 성장 상태 — 첫 외부 maintainer 진입 시 institutional 층 발생 |
| C (federation) | 성숙 상태 — multi-org 거버넌스 + production-grade 결정 |

**부재 phase 와 그 위치:**

| absent | 위치 |
|---|---|
| Foundation (Apache · Eclipse) | Phase C 로 충분 — Foundation 은 *법적 래퍼* 이지 별도 거버넌스 phase 아님 |
| Bus-factor 1 → 2 전환 | Phase A → B 안에 포함 (2 번째 maintainer = B trigger) |
| Sunset / Archive | `[DEPRECATION_POLICY]` (spec 차원) — 거버넌스 phase 아님 |
| Fork 거버넌스 | 범위 밖 — fork 는 자체 거버넌스 |

## [CURRENT_PHASE] 현재 단계

Phase A (BDFL). Maintainer: `qwerfunch`.

## [ROLES] 역할

| 역할 | 산업 alias | 책임 | 자격 |
|---|---|---|---|
| User | Adopter · Implementer | spec 읽기, 자기 도구에서 채택 (선택) | 누구나 |
| Contributor | Author · Submitter (RFC) | RFC PR 제출, issue 보고 | 채택된 PR 1 개라도 보유 |
| Maintainer | Committer (Apache) · Core Team (Rust) | RFC 검토, merge, 버전 릴리즈 | 현재 maintainer 초대 (Phase A: BDFL 초대) |

Sub-role (Editor · Reviewer · Champion · Security Officer) 은 Phase B+ 까지 Maintainer 안에 흡수. 명시 분리는 후속.

## [RFC_PROCEDURE] RFC 절차

spec 내용을 변경하는 모든 제안 (오타 제외) 은 본 절차를 따름. Phase A 도 포함.

```
1. [rfc] 태그로 GitHub issue 개설 — 변경 내용 서술.
2. Maintainer 가 endorse 하면, proposals/<NNN>-<slug>.md 추가 PR 제기.
3. 토론 기간: Phase A 7 일 / Phase B+ 14 일 최소.
4. 결정:
   - Phase A: BDFL accepts | defers | rejects (사유 명시).
   - Phase B+: lazy consensus (maintainer veto 7 일 내 없으면 accept).
5. Accept 시: proposal merge → spec 변경 PR merge.
6. CHANGELOG.md (+ .ko.md) 에 semver 적절히 갱신.
```

Proposal 파일 템플릿 (`proposals/NNN-slug.md`):

```yaml
---
proposal: NNN
title: <title>
author: <github handle>
status: draft | accepted | deferred | rejected | superseded
created: YYYY-MM-DD
---

## [MOTIVATION] 동기
## [PROPOSAL] 제안
## [IMPACT] 영향
## [SECURITY_IMPLICATIONS] 보안 영향
## [ALTERNATIVES] 대안
```

`[SECURITY_IMPLICATIONS]` 필수: 보안 surface 미접촉이면 "none" 기재, 아니면 서술.

## [DECISION_MODEL] 의사결정 모델

| phase | 규칙 |
|---|---|
| A (BDFL) | maintainer 가 accept / defer (수정 요청) / reject (사유) |
| B (working group) | lazy consensus: 7 일 내 maintainer veto 없으면 accept. 한 maintainer 의 명시적 -1 = blocked, 수정 필요 |
| C (federation) | 정식 vote: maintainer 단순 다수, steering committee tiebreak |

## [QUORUM] 정족수

유효한 의사결정에 필요한 최소 maintainer 수:

| phase | quorum | 규칙 |
|---|---|---|
| A (BDFL) | 1 | 단독 maintainer 결정 충분 |
| B (working group) | floor(N/2) + 1 of 전체 maintainer | 7 일 window 내 majority 도달 가능해야 |
| C (federation) | maintainer 3 명 + steering committee member 1 명 | quorum 미달 → 결정 보류 |

Quorum 미달 vote 는 *비구속* (advisory only).

## [VERSIONING] 버전 정책

SemVer 정책은 [`CHANGELOG.md`](../../CHANGELOG.md) 참조 (major: breaking · minor: additive · patch: doc).

버전 bump 는 Maintainer 책임. RFC accept PR merge 가 트리거.

## [DRAFT_GRADUATION] draft → v1.0 졸업

spec `status` 가 `draft` → 안정 `v1.0` 으로 전환되려면 **두 조건이 모두** 충족되어야 한다:

1. **독립 구현체 2 개 이상**: 서로 다른 저자 / 조직의 reference 구현 2 개 이상이 L1-L4 conformance fixture suite 전체 통과 (see [`conformance/level-{1..4}.md`](../../conformance/)).
2. **미해결 critical 반증 0 건**: accepted L4 반증이 미해결로 남아있지 않음 (see [`falsifications/README.md`](../../falsifications/README.md) `[ACCUMULATION_POLICY]`).

v0.x = 단일 구현 (`harness-boot`) 부트스트랩 — 구성상 pre-1.0. 위 `[PHASES]` 의 Phase A→B 거버넌스 전환과 *별개 축*.

| 조건 | 점검 | 현재 |
|---|---|---|
| L1-L4 fixture 통과 구현체 2+ | conformance/ 선언 | 1/2 (harness-boot 단독) |
| 미해결 L4 반증 0 | falsifications/ status: accepted, unresolved | 0/0 (등록부 막 개설) |

## [DEPRECATION_POLICY] 폐기 정책

spec 요소 (section · detector · EARS 패턴 · Level 등) 를 폐기해야 할 때:

```
1. RFC 가 요소를 `deprecated` 표시 (status: accepted, change: deprecation).
2. 요소는 spec 안에 `deprecated: true` 플래그 + `deprecated_since: vX.Y.Z` 와 함께 잔존.
3. 폐기 기간:
   - Phase A: 1 minor 버전 (≥ 60 일 release 에서 관찰 가능)
   - Phase B: 2 minor 버전
   - Phase C: 1 major 버전
4. 폐기 기간 후 major bump 에서 제거.
5. CHANGELOG 에 BREAKING 두 번 기록: 폐기 발표 (heads-up) + 제거 시점.
```

prior deprecation 없는 제거는 `--emergency-removal-reason` 필요 (보안 또는 정확성 이슈로 declaration 유지가 해로운 경우). CHANGELOG 에 영구 기록.

## [TRANSITION_TRIGGERS] 단계 전환

| from → to | trigger | 작업 |
|---|---|---|
| A → B | 2+ 조직의 maintainer 3+ 명 | `[CURRENT_PHASE]` 갱신, maintainer 목록 추가, RFC 기간 14 일 연장 |
| B → C | maintainer 5+ 명 + 1+ production 채택 | steering committee 구성, 정식 voting, working group sub-charter |

## [DISPUTES] 분쟁

두 contributor 가 제안에 동의 못 할 시:

| phase | 해소 |
|---|---|
| A | BDFL 결정이 최종. 반대자는 fork 가능 |
| B | 전체 maintainer 에 escalate. 교착 시 BDFL 이 결정 vote |
| C | steering committee vote |

## [SECURITY] 보안

spec 자체의 취약점 (예: detector 정의가 공격 클래스 누락) 은 *비공개* 신고 가능. Phase A: maintainer 에게 email. Phase B+: 전용 보안 연락처 `SECURITY.md` 에 공개.

spec 자체는 코드 실행 X — 본 취약점은 *spec 이 보호 못 하는 영역* 에 관한 것이지 *코드 실행* 이 아님. Reference 구현의 취약점은 해당 구현의 정책 따름 (예: `harness-boot`).

## [LICENSE] 라이선스

MIT — [`LICENSE`](./LICENSE) 참조.

## [LIMITATIONS] v0.3 한계

- `proposals/` 디렉터리 아직 없음 (첫 RFC 제기 시 생성).
- `SECURITY.md` 아직 없음 (Phase B 진입 시 추가).
- BDFL 승계 프로토콜 (maintainer 부재 시 어떻게) 아직 미정 — Phase B (다수 maintainer) 도달 시까지 보류.
- Code of Conduct 아직 채택 X (Phase B trigger).
- `[DRAFT_GRADUATION]` 현재 상태 (1/2 구현체, 0/0 반증) 는 v0.0.18 baseline 반영 — 두 조건 모두 zero-progress (외부 구현 없음, 등록부 막 개설).

