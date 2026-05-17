---
falsification: NNN
title: <one-line summary>
submitter: <github handle>
status: draft | accepted | closed-out-of-scope | cannot-reproduce
created: YYYY-MM-DD
level_declared: L1 | L2 | L3 | L4
detector_missed: <DETECTOR_ID or "none — gap in level definition">
references:
  - ../README.md
  - ../iron-law.md
---

# Falsification NNN — <title>

## [SUMMARY]

<what was declared · what was found · why it falsifies the standard — 1 paragraph>

## [LEVEL_PASSED]

```
tool:        <name + version + claimed ironclad-spec version>
declaration: iron-law: L<N>, detectors: <subset>, ears: <full|partial>
evidence:    <link to release notes, PR, or commit>
```

## [MISMATCH_FOUND]

| piece A | piece B | observable difference |
|---|---|---|
| Spec | Code | <e.g., AC-3 says X, code returns Y> |

Minimal reproducer (excerpt from each piece):

```
spec:  <excerpt>
code:  <excerpt>
test:  <excerpt or result>
```

## [DETECTOR_GAP]

- `<DETECTOR_ID>`: <pass_criteria from detectors.schema.json> — why it did not fire on this artifact.
- OR: "No detector exists for <category>. Proposed new detector: <sketch>."

## [REPRODUCTION]

```
1. ...
2. ...
3. Observe: tool reports iron-law: L<N> pass, yet <observed mismatch>.
```

## [PROPOSED_REMEDY]

One of:
- **Amend detector**: revise `<DETECTOR_ID>` `pass_criteria` to `<new wording>`.
- **Add detector**: new detector `<PROPOSED_ID>` with axis `<axis>`, severity `<severity>`, pass_criteria `<text>`.
- **Raise stage strictness**: change `<stage_N.M>` from soft to hard within `<level>`.
- **Revise level definition**: clarify `<level>` to require `<additional check>`.
