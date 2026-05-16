---
spec: ironclad/ears
version: 0.0.2
status: draft
parent: ironclad
language: ko (translation of ears.md — for human comprehension only)
references:
  - ../../iron-law.md
  - ../../detectors.schema.json
based_on: "Mavin, A. et al. (2009) Easy Approach to Requirements Syntax"
modal_authority: "RFC 2119 (shall / must / should differentiation)"
---

# EARS — 수용 기준 문법 (한국어 대조본)

> 본 문서는 [`ears.md`](../../ears.md) (영어 SSoT) 의 한국어 번역 — *이해 보조용*. 충돌 시 **영어 본이 우선**.

## [CLAIM] 핵심 정의

LLM 추론을 *형식 논리 패턴* 으로 제한하는 자연어 제약 문법. AC ↔ 테스트 추적성 (traceability) 가능.

## [PATTERNS] 5 패턴 + 복합

5 패턴 + 1 복합 형태 (Mavin 2009 원본).

| pattern | template | 산업 대응 | 용도 |
|---|---|---|---|
| ubiquitous | `The <system> shall <response>.` | RFC 2119 절대 요구사항 | 상시 작동 동작 |
| event-driven | `When <trigger>, the <system> shall <response>.` | Gherkin When | 이벤트 응답 |
| state-driven | `While <state>, the <system> shall <response>.` | Gherkin Given (상태) | 특정 상태 중 동작 |
| optional-feature | `Where <feature>, the <system> shall <response>.` | feature flag / capability gate | feature 존재 시 동작 |
| unwanted-behavior | `If <trigger>, then the <system> shall <response>.` | exception path / error case | 오류 또는 edge case |
| compound | `When` · `While` · `Where` · `If` 조합 → `the <system> shall <response>.` | Gherkin scenario | 복합 조건 |

## [BNF] 형식 문법

```bnf
acceptance_criterion ::= ear_basic | ear_compound

ear_basic ::= ubiquitous
            | event_driven
            | state_driven
            | optional_feature
            | unwanted_behavior

ubiquitous        ::= "The" SYSTEM "shall" RESPONSE "."
event_driven      ::= "When"  TRIGGER  "," "the" SYSTEM "shall" RESPONSE "."
state_driven      ::= "While" STATE    "," "the" SYSTEM "shall" RESPONSE "."
optional_feature  ::= "Where" FEATURE  "," "the" SYSTEM "shall" RESPONSE "."
unwanted_behavior ::= "If"    TRIGGER  "," "then" "the" SYSTEM "shall" RESPONSE "."

ear_compound      ::= compound_prefix "the" SYSTEM "shall" RESPONSE "."
compound_prefix   ::= when_clause? while_clause? where_clause? if_clause?
                      (* 최소 1 절 필수;
                         각 절은 최대 1 회;
                         순서 고정: When → While → Where → If *)

when_clause       ::= "When"  TRIGGER ","
while_clause      ::= "While" STATE   ","
where_clause      ::= "Where" FEATURE ","
if_clause         ::= "If"    TRIGGER "," "then"

SYSTEM            ::= noun_phrase
RESPONSE          ::= verb_phrase (object | adverbial)*
                      (* quantifier 포함 가능: "within Xms", "at most N times" *)
TRIGGER           ::= clause
STATE             ::= clause
FEATURE           ::= noun_phrase
```

`shall` 조동사 필수. `should`, `must`, `will` 은 비호환.

## [NEGATION] 부정형

EARS 는 *금지* 표현을 위해 `shall not` 지원. 문법은 동일하되 `shall` → `shall not` 으로 교체:

| pattern | 예시 |
|---|---|
| ubiquitous (negation) | "The system shall not store user passwords in plaintext." |
| event-driven (negation) | "When a session expires, the system shall not auto-renew without consent." |

`shall not` 은 *금지* — *없는 동작* 과 구별됨. 부재 동작을 명시적·검증 가능하게 표현.

## [EXAMPLES] 예시

| pattern | 예시 |
|---|---|
| ubiquitous | "The login form shall display two input fields and a submit button." |
| event-driven | "When the user submits invalid credentials, the system shall display an error message within 1 second." |
| state-driven | "While the user is offline, the system shall queue actions locally." |
| optional-feature | "Where dark-mode is enabled, the dashboard shall render with the dark color palette." |
| unwanted-behavior | "If the API returns 500, then the client shall retry up to 3 times with exponential backoff." |
| compound | "When the cart has items, while the user is authenticated, the checkout button shall be enabled." |

## [VALIDATION] 유효성

| 상태 | 예시 | 이유 |
|---|---|---|
| ✓ valid | "When X, the system shall Y." | event-driven, 완전형 |
| ✗ invalid | "When X, the system Y." | `shall` 누락 |
| ✗ invalid | "The system should Y." | 잘못된 조동사 (`should` 불허) |
| ✗ invalid | "Y when X happens." | 비-EARS 어순 |
| ✓ valid | "The system shall Y." | ubiquitous |
| ✓ valid (compound) | "When X, while Z, the system shall Y." | 복합 조건 |

## [INTEGRATION] 통합

Reference 구현 (`harness-boot`) 은 `spec.yaml` 의 AC 를 두 형태로 수용:

```yaml
acceptance_criteria:
  - "When the user clicks save, the system shall persist the form."   # EARS 문자열
  - id: AC-2                                                          # 구조화 형태
    condition: "the network is offline"
    action: "the user submits the form"
    response: "the system shall queue the submission locally"
```

EARS 문자열 형태는 본 문법으로 검증. 구조화 형태는 소비 시 복합 EARS 로 매핑:
`When <action>, while <condition>, the system shall <response>.`

## [TRACEABILITY] 추적성

모든 AC 는 고유 ID (`AC-N` 또는 `<feature-id>.AC-N`) 보유. `UNTESTED_AC` detector (see `detectors.schema.json`) 가 모든 AC ID 가 최소 1 개 테스트에 참조되는지 검증.

## [PATTERN_COUNT_RATIONALE] 5 의 정당화

5 패턴 + 복합 = *load-bearing*. padded / minimal 아님.

| pattern | 역할 |
|---|---|
| ubiquitous | 전제 없는 유일 패턴 (절대 요구사항) |
| event-driven | 트리거 의미 (일회성 이벤트) |
| state-driven | 상태 의미 (지속 조건) |
| optional-feature | 존재 의미 (capability 게이트) |
| unwanted-behavior | 예외 의미 (오류 경로) |

4 패턴으로 줄이면 두 개 통합 발생 — 각 패턴이 다른 temporal/logical 의미.
6+ 로 확장하면 이미 흡수된 의미 분할 (예: "periodic" 은 *반복 트리거의 event-driven*).

**부재 패턴 및 그 위치:**

| absent | 위치 |
|---|---|
| Quantified Requirement (정량) | RESPONSE 내부 (예: "within 200ms", "at most 3 retries") |
| User Story ("As a user...") | AC 범위 밖 — feature 메타데이터에 속함 |
| Given-When-Then (BDD) | compound EARS 로 표현 가능: While `<Given>`, when `<When>`, the system shall `<Then>`. |
| Use Case | AC 범위 밖 — 개별 AC 보다 상위 |
| Prohibition (shall not) | `[NEGATION]` (문법 변형, 새 패턴 아님) |

## [LIMITATIONS] v0.0.2 한계

- 자동 EARS validator 아직 없음 (`detectors.schema.json` 런타임과 함께 후속).
- 키워드 대소문자 무관 파싱 가능하나 예시는 *소문자 키워드 + 문장 시작 대문자* 로 정규화.
- `<system>`, `<response>` 등은 자연어 구문 — 의미 검증은 본 문법의 범위 밖.

