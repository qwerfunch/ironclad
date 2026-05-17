---
falsification: NNN
title: <한 줄 요약>
submitter: <github handle>
status: draft | accepted | closed-out-of-scope | cannot-reproduce
created: YYYY-MM-DD
level_declared: L1 | L2 | L3 | L4
detector_missed: <DETECTOR_ID 또는 "none — level 정의의 공백">
language: ko (translation of falsifications/template.md — for human comprehension only)
references:
  - ../../../falsifications/template.md
---

# 반증 NNN — <제목>

> 본 문서는 [`falsifications/template.md`](../../../falsifications/template.md) (영어 SSoT) 의 한국어 번역 — *이해 보조용*. 충돌 시 **영어 본이 우선**.

## [SUMMARY] 요약

<무엇이 선언되었고 · 무엇이 발견되었으며 · 왜 표준을 부정하는지 — 한 단락>

## [LEVEL_PASSED] 통과한 Level

```
tool:        <도구명 + 버전 + 선언한 ironclad-spec 버전>
declaration: iron-law: L<N>, detectors: <subset>, ears: <full|partial>
evidence:    <release notes · PR · commit 링크>
```

## [MISMATCH_FOUND] 발견된 불일치

| piece A | piece B | 관찰 차이 |
|---|---|---|
| Spec | Code | <예: AC-3 은 X 라 하나 코드는 Y 반환> |

최소 재현 (각 piece 발췌):

```
spec:  <발췌>
code:  <발췌>
test:  <발췌 또는 결과>
```

## [DETECTOR_GAP] detector 공백

- `<DETECTOR_ID>`: <detectors.schema.json 의 pass_criteria> — 왜 본 산출물에서 발화하지 않았는지.
- 또는: "<category> 에 대한 detector 없음. 제안 신규 detector: <스케치>."

## [REPRODUCTION] 재현

```
1. ...
2. ...
3. 관찰: 도구가 iron-law: L<N> pass 보고, 하지만 <관찰된 mismatch>.
```

## [PROPOSED_REMEDY] 제안 처방

다음 중 하나:
- **detector 개정**: `<DETECTOR_ID>` `pass_criteria` 를 `<새 문구>` 로 개정.
- **detector 신설**: 신규 detector `<PROPOSED_ID>`, axis `<axis>`, severity `<severity>`, pass_criteria `<text>`.
- **stage 엄격도 승급**: `<stage_N.M>` 을 `<level>` 내에서 soft → hard 로 변경.
- **level 정의 개정**: `<level>` 에 `<추가 검사>` 요구 명확화.
