# Phase 1 assessment: risk notes

## Mandate and boundaries

The pinned `custom_document` stage `acceptance_20261025091304_ee4d53_risk` must document delivery risks, each linked to at least one approved requirement. The eventual contract uses schema version `1.0`, item IDs prefixed `RA4A9E-`, and the pinned JSON and Markdown paths under `docs/sdlc/`. This assessment is not a deliverable contract or a plan. No source files, application code, deployment workflows, or product outputs are to be changed during assessment.

## Authorized evidence

| Source | Exact reference | Assessment use |
| --- | --- | --- |
| Full pinned story | `{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"}` | Small password-reset application, supplied wireframe, curated standard, synthetic accounts, in-memory notification sink, production-limitations documentation, automated tests, and no deployment or deployment workflows. The story reader reported `next_offset: null`. |
| Supplied image | `{"source_type":"attachment","source_id":"a4690c70-e0db-4bb7-b5c0-f0422127e023","locator":"sign-in-wireframe.png"}` | Direct visual observation: a white page and thin dark bordered sign-in card, `Sign in` heading, labeled email field with `name@example.com` placeholder, labeled password field with asterisks placeholder, blue underlined `Forgot password?` link, dark `Sign in` button, grey `No account? Contact your administrator.` footer; no other visible controls. The attachment reader provided a reference rather than image text. |
| Run-matched curated standard | `{"source_type":"knowledge","source_id":"46054aed-d642-45cb-ac91-e8bb053c4af6","locator":"v1, chunk 0"}` | ACC-STD-01: reset entry point, identical responses for existing/nonexistent accounts, five requests per account per hour. ACC-STD-02: single-use links, expiry 30 minutes after issuance, all active sessions signed out on completion. ACC-STD-03: notify the owner after every successful reset. Other search results belong to different runs and are not used. |
| Approved predecessor contract | Stage key `requirements_analysis` (the prior-output tool does not supply a source-reference object) | Approved `REQ-001` through `REQ-021`, acceptance criteria, assumptions and non-blocking questions. The prior-output tool returned the complete contract (`next_offset: null`). Reference individual requirements by their exact approved IDs; do not fabricate a source-reference locator for the predecessor. |

The repository's earlier evidence map is useful context but is not the authoritative approved contract: it proposes only `REQ-001` through `REQ-019`, while the approved predecessor splits deployment into `REQ-019` and `REQ-020` and adds the no-extra-controls boundary as `REQ-021`. Use the approved predecessor identifiers, not draft numbering.

## Risk coverage to carry into planning

| Delivery risk candidate | Approved requirement links | Evidence and consequence to capture |
| --- | --- | --- |
| Sign-in card drifts from supplied wireframe, adds a signup/control, or loses reset entry point | `REQ-001`-`REQ-007`, `REQ-021` | Visual image and ACC-STD-01; layout/labels, available controls and functional forgot-password path need explicit coverage. |
| Account-existence disclosure through differing reset responses, including when throttled | `REQ-008`, `REQ-009`, `REQ-018` | ACC-STD-01 and predecessor criteria require equivalent responses and tests of both account cases and rate boundary. |
| Requests exceed five per synthetic account per hour or rate accounting is ambiguous | `REQ-009`, `REQ-018` | ACC-STD-01 gives the exact per-account limit; do not invent a separate nonexistent-account numerical limit. |
| Reset link reuse or wrong 30-minute issuance boundary allows unauthorized or stale changes | `REQ-010`, `REQ-011`, `REQ-012`, `REQ-018` | ACC-STD-02 and approved criteria: usable before 30 minutes, expired at or after 30 minutes; one successful use only. |
| Successful reset leaves a prior session active or fails to notify the owner on every success | `REQ-013`, `REQ-014`, `REQ-018` | ACC-STD-02/03; test multiple active sessions and repeated distinct successful resets. |
| Demo is mistaken for production-ready because synthetic data, in-memory delivery and limitations are undocumented | `REQ-015`, `REQ-016`, `REQ-017` | Story and approved limitations criterion; avoid implying persistence, actual email/SMS delivery, or production security guarantees. |
| Automated coverage misses image, recovery security boundaries, or synthetic/in-memory behavior | `REQ-018` and the requirements it verifies | Approved test criterion explicitly enumerates visual controls, existence parity, rate, single-use/expiry, password change, session invalidation, notification and demo boundaries. |
| Delivery performs a deployment or adds a deployment workflow | `REQ-019`, `REQ-020` | Explicit story prohibition and separately approved requirements. |

These are assessment candidates, not pre-approved risk-item IDs or implementation claims. In the eventual document, every risk item must have a nonempty `requirement_ids` array drawn from the approved predecessor, stable `RA4A9E-###` IDs, and traceable source references. Distinguish likelihood/impact and mitigations as proposed risk management, not facts about an application implementation; no application implementation is evidenced in the authorized outputs.

## Gaps and bounded assumptions

- The approved predecessor leaves synthetic sign-in/session representation, nonexistent-address accounting, local reset-link access, reset-form/password wording, and further production limitations unresolved. These are non-blocking questions, not permission to invent policies. Risk notes can identify their consequences and proposed mitigations without asserting an implementation.
- The image does not specify reset-request or reset-completion screens, transport, persistence, password complexity, or production operations. Do not infer those from the sign-in design.
- The curated standard is the run-matched v1 result above. No evidence establishes actual code, test results, deployment, or a production architecture.
- There is no consequential missing information preventing a risk assessment grounded in the approved requirements; preserve these uncertainties as open questions in the later contract.
