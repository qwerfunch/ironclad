---
spec: ironclad/governance
version: 0.3
status: draft
parent: ironclad
references:
  - iron-law.md
  - CHANGELOG.md
inspired_by:
  - "Python PEP 1 (BDFL → Steering Council)"
  - "CNCF Governance (TSC + Maintainer)"
  - "TC39 RFC stages (Champion + Stage 0-4)"
---

# Governance

## [CLAIM]

The model for proposing, reviewing, and accepting changes to the Ironclad standard — starting from BDFL and scaling to working-group consensus as external contributors accumulate.

## [PHASES]

| phase | condition | industry alias | decision model | current |
|---|---|---|---|---|
| A: BDFL | sole maintainer | Solo Maintainer · Benevolent Dictator (Python, Linux pre-2022) | maintainer decides | ✓ |
| B: working group | 3+ active maintainers from 2+ orgs | TSC (CNCF) · Core Team (Rust) | lazy consensus + veto | TBD |
| C: federation | 5+ maintainers + 1+ external production adoption | Steering Council (Python) · Foundation TSC (CNCF) | formal working group + steering committee | TBD |

Transition is automatic on trigger condition; this document is patched to record the date of transition.

## [PHASE_COUNT_RATIONALE]

3 phases is load-bearing — neither padded nor minimal.

| phase | role |
|---|---|
| A (BDFL) | starting state — required for any new spec (sole author) |
| B (working group) | growth state — first external maintainer triggers institutional layer |
| C (federation) | mature state — multi-org governance + production-grade decisions |

**Absent phases and where they live:**

| absent | location |
|---|---|
| Foundation (Apache, Eclipse) | Phase C is sufficient — Foundation is a legal wrapper, not a distinct governance phase |
| Bus-factor-1 → 2 transition | within Phase A → B (2nd maintainer = B trigger) |
| Sunset / Archive | `[DEPRECATION_POLICY]` (spec-level) — not a governance phase |
| Fork governance | out of scope — fork uses its own governance |

## [CURRENT_PHASE]

Phase A (BDFL). Maintainer: `qwerfunch`.

## [ROLES]

| role | industry alias | responsibility | how to become |
|---|---|---|---|
| User | Adopter · Implementer | reads spec, optionally adopts in own tool | open access |
| Contributor | Author · Submitter (RFC) | submits RFC PR, files issues | submit any accepted PR |
| Maintainer | Committer (Apache) · Core Team (Rust) | reviews RFCs, merges, releases versions | invited by current maintainers (Phase A: BDFL invites) |

Sub-roles (Editor · Reviewer · Champion · Security Officer) are absorbed by Maintainer until Phase B+; explicit separation deferred.

## [RFC_PROCEDURE]

All proposed changes to spec content (not typos) go through this procedure, including Phase A.

```
1. Open a GitHub issue tagged [rfc] describing the change.
2. If maintainer endorses, raise a PR adding the proposal under proposals/<NNN>-<slug>.md.
3. Discussion period: 7 days minimum (Phase A) · 14 days (Phase B+).
4. Decision:
   - Phase A: BDFL accepts, defers, or rejects with reason.
   - Phase B+: lazy consensus (no maintainer veto in 7 days = accept).
5. If accepted: merge proposal, then merge spec-changing PR.
6. Update CHANGELOG.md (+ .ko.md) with semver-appropriate version.
```

Proposal file template (`proposals/NNN-slug.md`):

```yaml
---
proposal: NNN
title: <title>
author: <github handle>
status: draft | accepted | deferred | rejected | superseded
created: YYYY-MM-DD
---

## [MOTIVATION]
## [PROPOSAL]
## [IMPACT]
## [SECURITY_IMPLICATIONS]
## [ALTERNATIVES]
```

`[SECURITY_IMPLICATIONS]` is mandatory: state "none" if proposal does not touch security surface, else describe.

## [DECISION_MODEL]

| phase | rule |
|---|---|
| A (BDFL) | maintainer accepts, defers (asks for revision), or rejects with reason |
| B (working group) | lazy consensus: 7 days no veto from any maintainer = accept; explicit -1 from any maintainer = blocked, requires revision |
| C (federation) | formal vote: simple majority of maintainers, with steering committee tiebreak |

## [QUORUM]

Minimum maintainers required for valid decisions:

| phase | quorum | rule |
|---|---|---|
| A (BDFL) | 1 | sole maintainer's decision is sufficient |
| B (working group) | floor(N/2) + 1 of total maintainers | majority must be reachable within 7-day window |
| C (federation) | 3 maintainers + 1 steering committee member | quorum unmet → decision deferred |

A vote cast outside quorum is non-binding (advisory only).

## [VERSIONING]

See [`CHANGELOG.md`](./CHANGELOG.md) for the SemVer policy (major: breaking · minor: additive · patch: doc).

A version bump is a Maintainer responsibility, triggered by merging RFC-accepting PRs.

## [DRAFT_GRADUATION]

Spec `status` transitions from `draft` to stable `v1.0` only when **both** conditions hold:

1. **Two independent implementations**: at least two reference implementations from distinct authors / orgs pass the full L1-L4 conformance fixture suite (see [`conformance/level-{1..4}.md`](./conformance/)).
2. **Zero open critical falsifications**: no accepted L4 falsification is unresolved (see [`falsifications/README.md`](./falsifications/README.md) `[ACCUMULATION_POLICY]`).

v0.x = single-impl bootstrap (`harness-boot`); pre-1.0 by construction. Separate axis from the Phase A→B governance transition (`[PHASES]` above).

| condition | check | current status |
|---|---|---|
| 2+ impls pass L1-L4 fixtures | conformance/ declarations | 1/2 (harness-boot only) |
| 0 open L4 falsifications | falsifications/ status: accepted, unresolved | 0/0 (registry just opened) |

## [DEPRECATION_POLICY]

When a spec element (section, detector, EARS pattern, Level, etc.) must be retired:

```
1. RFC marks element as `deprecated` (status: accepted, change: deprecation).
2. Element stays in spec with `deprecated: true` flag and `deprecated_since: vX.Y.Z`.
3. Deprecation period:
   - Phase A:   1 minor version (≥ 60 days observable in releases)
   - Phase B:   2 minor versions
   - Phase C:   1 major version
4. Deprecated element is removed in a major bump after the period.
5. CHANGELOG records BREAKING for both deprecation announcement (heads-up) and removal.
```

Removal without prior deprecation requires `--emergency-removal-reason` (security or correctness issue making continued declaration harmful); recorded permanently in CHANGELOG.

## [TRANSITION_TRIGGERS]

| from → to | trigger | actions |
|---|---|---|
| A → B | 3+ maintainers from 2+ orgs | update `[CURRENT_PHASE]`, add maintainers list, extend RFC period to 14 days |
| B → C | 5+ maintainers + 1+ production adoption | establish steering committee, formal voting, working group sub-charters |

## [DISPUTES]

If two contributors disagree on a proposal:

| phase | resolution |
|---|---|
| A | BDFL's decision is final; dissenter may fork |
| B | escalate to all maintainers; if deadlocked, BDFL casts deciding vote |
| C | steering committee vote |

## [SECURITY]

Vulnerabilities in the spec (e.g., a detector definition that misses an attack class) may be reported privately. Phase A: email maintainer. Phase B+: dedicated security contact published in `SECURITY.md`.

The spec itself does not execute code; vulnerabilities here are about *what the spec leaves unprotected*, not about code execution. Reference-implementation vulnerabilities follow that implementation's policy (e.g., `harness-boot`'s).

## [LICENSE]

MIT — see [`LICENSE`](./LICENSE).

## [LIMITATIONS] v0.3

- No `proposals/` directory yet (created when first RFC raised).
- No `SECURITY.md` yet (added when transitioning to Phase B).
- BDFL succession protocol (what if maintainer disappears) not yet defined; deferred until Phase B has multiple maintainers.
- Code of Conduct not yet adopted (Phase B trigger).
- `[DRAFT_GRADUATION]` current status (1/2 impls, 0/0 falsifications) reflects v0.0.18 baseline — both conditions are zero-progress (no external impl, registry just opened).

