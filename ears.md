---
spec: ironclad/ears
version: 0.0.2
status: draft
parent: ironclad
references:
  - iron-law.md
  - detectors.schema.json
based_on: "Mavin, A. et al. (2009) Easy Approach to Requirements Syntax"
modal_authority: "RFC 2119 (shall / must / should differentiation)"
---

# EARS — Acceptance Criterion Grammar

## [CLAIM]

A constrained natural-language grammar for acceptance criteria that bounds LLM reasoning to formal logical patterns and enables AC ↔ test traceability.

## [PATTERNS]

5 patterns + 1 compound form (Mavin 2009 originals).

| pattern | template | industry equivalent | use case |
|---|---|---|---|
| ubiquitous | `The <system> shall <response>.` | RFC 2119 absolute requirement | always-active behavior |
| event-driven | `When <trigger>, the <system> shall <response>.` | Gherkin When | response to an event |
| state-driven | `While <state>, the <system> shall <response>.` | Gherkin Given (state) | behavior during a state |
| optional-feature | `Where <feature>, the <system> shall <response>.` | feature flag / capability gate | gated by a feature being present |
| unwanted-behavior | `If <trigger>, then the <system> shall <response>.` | exception path / error case | error or edge case |
| compound | combines `When` · `While` · `Where` · `If` then `the <system> shall <response>.` | Gherkin scenario | combined conditions |

## [BNF]

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
                      (* at least one clause must be present;
                         each clause appears at most once;
                         order is fixed: When → While → Where → If *)

when_clause       ::= "When"  TRIGGER ","
while_clause      ::= "While" STATE   ","
where_clause      ::= "Where" FEATURE ","
if_clause         ::= "If"    TRIGGER "," "then"

SYSTEM            ::= noun_phrase
RESPONSE          ::= verb_phrase (object | adverbial)*
                      (* may include a quantifier: "within Xms", "at most N times" *)
TRIGGER           ::= clause
STATE             ::= clause
FEATURE           ::= noun_phrase
```

The `shall` modal is mandatory; `should`, `must`, `will` are non-conformant.

## [NEGATION]

EARS supports `shall not` for prohibitions. The grammar is identical except `shall` is replaced with `shall not`:

| pattern | example |
|---|---|
| ubiquitous (negation) | "The system shall not store user passwords in plaintext." |
| event-driven (negation) | "When a session expires, the system shall not auto-renew without consent." |

`shall not` is a *prohibition*, distinct from a *missing positive* requirement — making absent behavior explicit and verifiable.

## [EXAMPLES]

| pattern | example |
|---|---|
| ubiquitous | "The login form shall display two input fields and a submit button." |
| event-driven | "When the user submits invalid credentials, the system shall display an error message within 1 second." |
| state-driven | "While the user is offline, the system shall queue actions locally." |
| optional-feature | "Where dark-mode is enabled, the dashboard shall render with the dark color palette." |
| unwanted-behavior | "If the API returns 500, then the client shall retry up to 3 times with exponential backoff." |
| compound | "When the cart has items, while the user is authenticated, the checkout button shall be enabled." |

## [VALIDATION]

| status | example | reason |
|---|---|---|
| ✓ valid | "When X, the system shall Y." | event-driven, full form |
| ✗ invalid | "When X, the system Y." | missing `shall` |
| ✗ invalid | "The system should Y." | wrong modal (`should` not allowed) |
| ✗ invalid | "Y when X happens." | non-EARS word order |
| ✓ valid | "The system shall Y." | ubiquitous |
| ✓ valid (compound) | "When X, while Z, the system shall Y." | compound conditions |

## [INTEGRATION]

Reference implementation (`harness-boot`) accepts AC in two forms via `spec.yaml`:

```yaml
acceptance_criteria:
  - "When the user clicks save, the system shall persist the form."   # EARS string
  - id: AC-2                                                          # structured
    condition: "the network is offline"
    action: "the user submits the form"
    response: "the system shall queue the submission locally"
```

EARS-string form is validated by this grammar. Structured form is mapped to compound EARS at consumption time:
`When <action>, while <condition>, the system shall <response>.`

## [TRACEABILITY]

Each AC must have a unique ID (`AC-N` or `<feature-id>.AC-N`). The `UNTESTED_AC` detector (see `detectors.schema.json`) verifies every AC ID is referenced by at least one test.

## [PATTERN_COUNT_RATIONALE]

5 patterns + compound is load-bearing — not padded, not minimal.

| pattern | role |
|---|---|
| ubiquitous | the only pattern without a precondition (covers absolute requirements) |
| event-driven | distinct trigger semantics (one-time event) |
| state-driven | distinct state semantics (persistent condition) |
| optional-feature | distinct presence semantics (capability gate) |
| unwanted-behavior | distinct exception semantics (error path) |

Reducing to 4 would collapse two of these — each carries a different temporal/logical meaning.
Expanding to 6+ would split semantics already covered (e.g., "periodic" is event-driven with a recurring trigger).

**Absent patterns and where they live:**

| absent | location |
|---|---|
| Quantified Requirement | inside RESPONSE (e.g., "within 200ms", "at most 3 retries") |
| User Story ("As a user, I want...") | out of AC scope — belongs to feature metadata |
| Given-When-Then (BDD) | expressible as compound EARS: While `<Given>`, when `<When>`, the system shall `<Then>`. |
| Use Case | out of AC scope — higher-level than individual AC |
| Prohibition (shall not) | `[NEGATION]` (grammar variant, not new pattern) |

## [LIMITATIONS] v0.0.2

- No automated EARS validator yet (planned alongside `detectors.schema.json` runtime).
- Parsing accepts case-insensitive keywords but examples normalize to lowercase keywords with capital sentence start.
- `<system>`, `<response>`, etc. are natural-language phrases; semantic validation is out of scope for grammar.

