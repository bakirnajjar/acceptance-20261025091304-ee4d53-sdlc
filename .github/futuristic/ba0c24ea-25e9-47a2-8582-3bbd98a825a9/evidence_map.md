# Requirements evidence map

## Authorized sources

| Key | Exact source reference | Evidence used |
| --- | --- | --- |
| S | `{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"}` | Complete pinned story: “Build a small password-reset application with the supplied sign-in wireframe. Use the curated recovery standard. Use synthetic accounts and an in-memory notification sink. Document production limitations. Provide automated tests. Do not deploy anything or add deployment workflows.” Story reader returned `next_offset: null`; no end marker exists. |
| A | `{"source_type":"attachment","source_id":"a4690c70-e0db-4bb7-b5c0-f0422127e023","locator":"sign-in-wireframe.png"}` | Supplied visible-image observation: white page; thin dark border around card; `Sign in` heading; labeled `Email address` field with `name@example.com` placeholder; labeled `Password` field with masked asterisks placeholder; blue underlined `Forgot password?` link; dark rectangular `Sign in` button; grey `No account? Contact your administrator.` footer; no other visible controls. Attachment reader returned a reference, not extracted image text. No attachment acceptance marker exists. |
| K | `{"source_type":"knowledge","source_id":"46054aed-d642-45cb-ac91-e8bb053c4af6","locator":"v1, chunk 0"}` | Run-matching curated account recovery standard, v1: ACC-STD-01 (forgot-password entry point, identical existing/nonexistent request responses, five requests per account per hour); ACC-STD-02 (single-use links, 30-minute expiration, all active sessions signed out on completion); ACC-STD-03 (owner notified after every successful reset). The other search hits repeat this text for other runs and are not used. |

The keys in the rightmost column below refer **only** to the exact source-reference objects above. Proposed IDs are stable handoff identifiers, not claims of extra evidence. Each theme should become a separately testable Given/When/Then criterion in the later contract.

## Atomic themes and coverage

| Proposed ID | Observable theme / acceptance boundary | Source keys and standard identifiers |
| --- | --- | --- |
| REQ-001 | Render the sign-in page as a white page with a thin dark bordered card headed `Sign in`. | S, A |
| REQ-002 | Show an `Email address`-labeled entry field with `name@example.com` placeholder. | A |
| REQ-003 | Show a `Password`-labeled entry field with a masked-asterisks placeholder. Do not infer a password policy from its appearance. | A |
| REQ-004 | Display a blue underlined `Forgot password?` link on the sign-in card. | A, K (ACC-STD-01) |
| REQ-005 | Display a dark rectangular `Sign in` button; the exact authentication behavior is unresolved. | A |
| REQ-006 | Display grey footer text `No account? Contact your administrator.` with no invented signup control. | A |
| REQ-007 | Activating `Forgot password?` starts a password-reset request flow. | S, A, K (ACC-STD-01) |
| REQ-008 | Give an identical response to a reset request for an existing account and a nonexistent account. Avoid a visible existence signal in that response. | K (ACC-STD-01) |
| REQ-009 | Limit reset requests to five per account per hour; do not invent a separate numerical limit for nonexistent addresses. | K (ACC-STD-01) |
| REQ-010 | Permit a valid reset link to be used for a successful reset only once, rejecting reuse. | S, K (ACC-STD-02) |
| REQ-011 | Expire reset links 30 minutes after issuance; do not accept an expired link. | K (ACC-STD-02) |
| REQ-012 | Let a valid reset link complete a password change for the corresponding synthetic account; how the new password is entered is not specified by the wireframe. | S, K (ACC-STD-02) |
| REQ-013 | On successful reset, sign that account out of every active session, not merely the initiating session. | K (ACC-STD-02) |
| REQ-014 | Notify the account owner after **every** successful password reset. No external delivery mechanism is specified. | S, K (ACC-STD-03) |
| REQ-015 | Use synthetic accounts, not real account data, in this application. | S |
| REQ-016 | Use an in-memory notification sink, not production notification delivery. | S |
| REQ-017 | Document production limitations of this synthetic/in-memory application without claiming production readiness. | S |
| REQ-018 | Provide automated tests for the supported flow and recovery-standard behaviors, including both account-existence cases, rate boundary, link reuse/expiry, session invalidation and notification after each success. | S, K (ACC-STD-01, ACC-STD-02, ACC-STD-03) |
| REQ-019 | Do not deploy the application or add deployment workflows. | S |

Coverage check: every supplied source appears above. All clauses of ACC-STD-01 map to REQ-007–009 (and the visible entry point to REQ-004); ACC-STD-02 maps to REQ-010–013; ACC-STD-03 maps to REQ-014. The complete wireframe observation maps to REQ-001–006; the story's synthetic accounts, in-memory sink, documentation, tests and deployment prohibitions map to REQ-015–019. No unseen image controls, password rules, storage guarantees, notification transport, or extra limits are asserted.

## Non-blocking questions and bounded assumptions for the later contract

1. **Question (non-blocking):** What behavior should the `Sign in` button provide for synthetic accounts, and how are sessions represented? **Assumption:** implement only the minimal local synthetic-account sign-in/session behavior needed to exercise the visible entry point and prove that reset invalidates every active session; no production session infrastructure is implied.
2. **Question (non-blocking):** How should attempts for nonexistent addresses be counted or throttled without disclosing account existence, and what constitutes an account key? **Assumption:** enforce the specified five-per-account-per-hour limit for synthetic accounts and keep the visible request response identical for existing and nonexistent addresses; no undocumented threshold or nonexistent-account accounting rule is claimed.
3. **Question (non-blocking):** How are reset links surfaced in a local demonstration, and how is the in-memory notification sink inspected? **Assumption:** use an in-memory, test-inspectable mechanism for synthetic-account notifications and link access; do not infer email/SMS delivery or external services.
4. **Question (non-blocking):** Which production limitations must be listed beyond synthetic accounts and in-memory notifications? **Assumption:** explicitly explain those limitations and any implementation-specific non-production constraints once known; do not claim production-ready persistence, delivery, security guarantees or deployment.
5. **Question (non-blocking):** What reset form, password policy, validation and failure wording should be used? **Assumption:** use a minimal reset-completion interaction sufficient to change a synthetic account password and test link validity; do not impose unsupported complexity, numeric policies, or UI controls on the supplied sign-in wireframe.

The final contract's last assumption must begin `acceptance-20261025091304-ee4d53-skill-sentinel` and state briefly that the acceptance skill produced the output. There is no story end marker or attachment acceptance marker to copy.
