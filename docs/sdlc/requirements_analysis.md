# Requirements analysis

Schema version: `1.0`  
Stage: `requirements_analysis`

## Sources

The source keys below stand for the exact references cited for each requirement.

| Key | Source reference |
| --- | --- |
| S | `{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"}` |
| A | `{"source_type":"attachment","source_id":"a4690c70-e0db-4bb7-b5c0-f0422127e023","locator":"sign-in-wireframe.png"}` |
| K | `{"source_type":"knowledge","source_id":"46054aed-d642-45cb-ac91-e8bb053c4af6","locator":"v1, chunk 0"}` |

The story is complete, with no end marker; no attachment acceptance marker is present. The attachment reader supplied a reference rather than extracted text, so only the authorized observed wireframe details are used. K is the run-matching curated standard: ACC-STD-01 (entry point, identical responses, five requests per account per hour); ACC-STD-02 (single-use links, 30-minute expiry, every active session signed out); ACC-STD-03 (owner notified after each successful reset).

## Requirements

### REQ-001
**Statement:** Render the supplied sign-in card on a white page with a thin dark border and a Sign in heading.  
**Acceptance criterion:** Given the sign-in page is opened, when it renders, then a thin dark bordered card headed Sign in appears on a white page.  
**Sources:** S, A

### REQ-002
**Statement:** Show an Email address field with the name@example.com placeholder on the sign-in card.  
**Acceptance criterion:** Given the sign-in card is rendered, when its fields are inspected, then an Email address-labeled field shows the name@example.com placeholder.  
**Sources:** A

### REQ-003
**Statement:** Show a Password field with a masked-asterisks placeholder on the sign-in card.  
**Acceptance criterion:** Given the sign-in card is rendered, when its fields are inspected, then a Password-labeled field has a masked-asterisks placeholder.  
**Sources:** A

### REQ-004
**Statement:** Display a blue underlined Forgot password? link on the sign-in card.  
**Acceptance criterion:** Given the sign-in card is rendered, when its controls are inspected, then a blue underlined Forgot password? link is visible.  
**Sources:** A, K

### REQ-005
**Statement:** Display a dark rectangular Sign in button on the sign-in card.  
**Acceptance criterion:** Given the sign-in card is rendered, when its controls are inspected, then a dark rectangular button labeled Sign in is visible.  
**Sources:** A

### REQ-006
**Statement:** Display the grey footer No account? Contact your administrator. on the sign-in card.  
**Acceptance criterion:** Given the sign-in card is rendered, when the footer is inspected, then grey text reads No account? Contact your administrator. without an additional sign-up control.  
**Sources:** A

### REQ-007
**Statement:** Start a password-reset request flow when the Forgot password? link is activated.  
**Acceptance criterion:** Given the sign-in page is displayed, when a user activates Forgot password?, then the user can initiate a password-reset request.  
**Sources:** S, A, K

### REQ-008
**Statement:** Respond identically to password-reset requests for existing and nonexistent accounts.  
**Acceptance criterion:** Given one synthetic account address and one nonexistent address, when an otherwise equivalent password-reset request is submitted for each, then the returned request response is identical and does not disclose whether either address exists.  
**Sources:** K

### REQ-009
**Statement:** Limit password-reset requests to five per account per hour.  
**Acceptance criterion:** Given a synthetic account has made five reset requests within an hour, when another request is submitted for that account within the same hour, then it does not generate another usable reset link; the request response remains consistent with REQ-008.  
**Sources:** K

### REQ-010
**Statement:** Allow each reset link to complete a successful reset only once.  
**Acceptance criterion:** Given a reset link has completed a successful password reset, when the same link is used again, then another password reset is not completed.  
**Sources:** S, K

### REQ-011
**Statement:** Keep each reset link unexpired before 30 minutes from issuance and expire it at 30 minutes from issuance.  
**Acceptance criterion:** Given an otherwise valid unused reset link issued at time T, when a reset is attempted before T plus 30 minutes, then the link is unexpired; when a reset is attempted at or after T plus 30 minutes, then the link cannot complete a reset.  
**Sources:** K

### REQ-012
**Statement:** Change the corresponding synthetic account password when a valid reset link completes a reset.  
**Acceptance criterion:** Given an unexpired, unused link for a synthetic account, when its owner completes a reset with a new password, then that account's password is changed to the new password.  
**Sources:** S, K

### REQ-013
**Statement:** Sign the account out of every active session after a successful password reset.  
**Acceptance criterion:** Given a synthetic account has multiple active sessions, when a password reset completes successfully, then none of that account's previously active sessions remains signed in.  
**Sources:** K

### REQ-014
**Statement:** Notify the account owner after every successful password reset.  
**Acceptance criterion:** Given a synthetic account undergoes a successful reset, when the reset completes, then a notification to that account's owner is recorded in the in-memory notification sink for that success.  
**Sources:** S, K

### REQ-015
**Statement:** Use only synthetic accounts in the application.  
**Acceptance criterion:** Given the application is run or tested, when account data is inspected, then the accounts used are synthetic rather than real users' accounts.  
**Sources:** S

### REQ-016
**Statement:** Record application notifications in an in-memory notification sink.  
**Acceptance criterion:** Given a notification is produced by a successful reset, when the application's notification sink is inspected, then that notification is held in memory rather than delivered through a production notification service.  
**Sources:** S

### REQ-017
**Statement:** Document the application's production limitations.  
**Acceptance criterion:** Given the application documentation is inspected, when its production-limitations section is read, then it explains the synthetic-account and in-memory-notification boundaries and does not present this demonstration as production-ready.  
**Sources:** S

### REQ-018
**Statement:** Provide automated tests for the sign-in wireframe and password-reset behavior.  
**Acceptance criterion:** Given the automated test suite is run, when its results are inspected, then tests verify the visible sign-in elements, reset entry point, identical existing/nonexistent request responses, five-per-account-per-hour boundary, single-use and 30-minute expiry, password change, all-session sign-out, notification after every success, and synthetic/in-memory boundaries.  
**Sources:** S, A, K

### REQ-019
**Statement:** Do not deploy the application.  
**Acceptance criterion:** Given the work is delivered, when its actions and artifacts are reviewed, then no application deployment has occurred.  
**Sources:** S

### REQ-020
**Statement:** Do not add deployment workflows.  
**Acceptance criterion:** Given the delivered files are inspected, when workflow changes are reviewed, then no deployment workflow has been added.  
**Sources:** S

### REQ-021
**Statement:** Show no other visible controls on the sign-in card beyond those in the supplied wireframe.  
**Acceptance criterion:** Given the sign-in card is rendered, when its visible controls are inspected, then only the Email address field, Password field, Forgot password? link, and Sign in button appear as controls.  
**Sources:** A

## Assumptions

1. The Sign in button supports only the minimal local synthetic-account sign-in/session behavior needed to exercise the sign-in page and demonstrate that reset invalidates every active session; no production session infrastructure is implied.
2. Enforce the specified five-per-account-per-hour limit for synthetic accounts while keeping request responses identical for existing and nonexistent accounts; no separate nonexistent-account threshold or accounting rule is asserted.
3. Reset links and notifications are accessible through an in-memory, test-inspectable mechanism for synthetic accounts; no email, SMS, or external delivery service is assumed.
4. The production-limitations document explains synthetic-account and in-memory-notification constraints and any implementation-specific limitations once known; it does not claim production readiness.
5. A minimal reset-completion interaction changes a synthetic account password; no password complexity rule, numeric policy, failure wording, or additional control on the supplied sign-in wireframe is assumed.
6. acceptance-20261025091304-ee4d53-skill-sentinel: the acceptance skill produced this output.
7. acceptance-20261025091304-ee4d53-same-agent-revision: the 30-minute reset-link boundary is measured from issuance.

## Open questions

All questions are non-blocking; each context identifies the bounded assumption used.

1. **Question:** What behavior should the Sign in button provide for synthetic accounts, and how are sessions represented? **Blocking:** false. **Context:** Bounded assumption: only minimal local synthetic-account sign-in/session behavior is needed to exercise the sign-in page and prove all-session invalidation; no production session infrastructure is implied.
2. **Question:** How should attempts for nonexistent addresses be counted or throttled without disclosing account existence, and what constitutes an account key? **Blocking:** false. **Context:** Bounded assumption: apply the specified five-per-account-per-hour limit to synthetic accounts and return the same request response for existing and nonexistent accounts; do not assert a separate nonexistent-account threshold or accounting rule.
3. **Question:** How are reset links surfaced in a local demonstration, and how is the in-memory notification sink inspected? **Blocking:** false. **Context:** Bounded assumption: use an in-memory, test-inspectable mechanism for synthetic-account links and notifications; do not assume email, SMS, or external services.
4. **Question:** Which production limitations must be listed beyond synthetic accounts and in-memory notifications? **Blocking:** false. **Context:** Bounded assumption: document those two constraints and any implementation-specific limitations once known, without asserting production readiness.
5. **Question:** What reset form, password policy, validation, and failure wording should be used? **Blocking:** false. **Context:** Bounded assumption: use a minimal reset-completion interaction sufficient to change a synthetic account password; impose no unsupported complexity rule, numeric policy, failure wording, or extra sign-in-wireframe control.
