---
spec: ironclad/falsifications
version: 0.0.2
status: draft
parent: ironclad
references:
  - ../iron-law.md
  - ../detectors.schema.json
  - ../README.md
inspired_by:
  - "WCAG Errata (W3C)"
  - "CWE Submission Process (MITRE)"
---

# Falsifications Registry

## [CLAIM]

Operational channel for the falsifiability clause in [`README.md`](../README.md) `[CLAIM]`:

> falsifiable: the standard is negated if any one of the three pieces is found to mismatch yet pass Level N.

## [SUBMISSION_PROCEDURE]

```
1. Open a GitHub issue tagged [falsification] with a 1-line summary.
2. Raise a PR adding falsifications/NNN-<slug>.md (use template.md).
3. Maintainer (Phase A: BDFL) verifies reproduction within 14 days.
4. Decision:
   - Reproduced + valid: merge as status: accepted. Triggers [ACCUMULATION_POLICY].
   - Reproduced + out-of-scope: merge as status: closed-out-of-scope with reason.
   - Cannot reproduce: close PR with reason; submitter may re-raise with more detail.
5. Accepted falsifications are immutable once merged (append-only history).
```

## [ACCUMULATION_POLICY]

| trigger | response |
|---|---|
| 3 accepted falsifications cite the same missed detector | RFC to revise that detector's `pass_criteria` or `description` |
| 3 accepted falsifications cite the same Level passing despite mismatch | RFC to add a new detector OR raise a stage from soft to hard within that Level |
| 1 accepted falsification at L4 (HITL bypass) | immediate RFC — L4 anchors reviewer ≠ author; regressions critical |

## [SCOPE]

| in | out |
|---|---|
| `iron-law: L<N>` declaration was issued AND a mismatch was *subsequently* discovered | implementation bugs that did not result in a Level declaration |
| Spec, Code, Tests visibly disagree on the same observable behavior | subjective disagreements about a Level's definition (use RFC via [`GOVERNANCE.md`](../GOVERNANCE.md) instead) |
| | performance, ergonomics, naming bikeshedding |

## [LIMITATIONS] v0.0.2

- No external submissions yet — registry opens with zero entries (honest baseline, not validation).
- Threshold (3) is provisional for Phase A; revisable via GOVERNANCE RFC in Phase B+.
- Phase A reproduction window (14 days) may need extension once submissions arrive concurrently.
- Cross-registry deduplication (same falsification in two tools' trackers) not yet automated.
