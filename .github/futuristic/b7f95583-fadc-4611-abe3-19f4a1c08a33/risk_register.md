# T001 planning notes: password-reset delivery risks

This is a **planning artifact**, not the final JSON contract or readable deliverable. The entries below are *proposed delivery risks*, not observed defects, test results, or assertions about an implemented or production architecture. The only predecessor authority is the approved `requirements_analysis` output, whose identifiers are `REQ-001` through `REQ-021`. Its stage-output reader supplies no source-reference object, so the risk entries cite the original authorized evidence below, not an invented predecessor locator. The earlier draft evidence map does not establish requirement numbering.

## Evidence key

| Key | Exact authorized source reference | Relevant bounds |
| --- | --- | --- |
| S1 | `{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"}` | Small password-reset app; synthetic accounts; in-memory notification sink; production limitations; automated tests; no deployment or deployment workflows. |
| S2 | `{"source_type":"attachment","source_id":"a4690c70-e0db-4bb7-b5c0-f0422127e023","locator":"sign-in-wireframe.png"}` | Supplied direct observations only: white page, thin dark bordered card, bold Sign in heading, Email address field with `name@example.com`, Password field with masked-asterisks placeholder, blue underlined Forgot password? link, dark rectangular Sign in button, grey `No account? Contact your administrator.` footer, and no other visible controls. The attachment reader provides a reference, not image content. |
| S3 | `{"source_type":"knowledge","source_id":"46054aed-d642-45cb-ac91-e8bb053c4af6","locator":"v1, chunk 0"}` | Run-matched standard: ACC-STD-01 reset entry point, identical existing/nonexistent request responses and five requests per account per hour; ACC-STD-02 single-use reset links, expiry 30 minutes after issuance, sign out all active sessions on reset; ACC-STD-03 notify owner after every successful reset. No other search hits are used. |

**Assessment scale:** Priority is a proposed order for verification, not a measured probability or actual severity. `High` means a missed approved security or delivery boundary could materially defeat acceptance; `Medium` means a visible, testability or documentation mismatch could undermine acceptance. No numerical likelihood is asserted without implementation evidence.

## Proposed risk register

### RA4A9E-001 — Supplied sign-in appearance or control inventory drifts

- **Requirement links:** REQ-001, REQ-002, REQ-003, REQ-004, REQ-005, REQ-006, REQ-021.
- **Evidence:** S1, S2. **Priority:** Medium.
- **Risk and consequence:** A sign-in card could omit or alter an observed label, placeholder, color/shape distinction or footer, or add a visible sign-up or other control. The supplied-wireframe acceptance criteria would then fail, and an extra control could imply an unsupported user journey.
- **Proposed mitigation:** Keep the sign-in card restricted to the observed email and password fields, Forgot password? link, Sign in button, and non-control footer; compare the intended rendering against the supplied observations. Do not extrapolate unspecified reset-page design from the image.
- **Proposed verification:** Inspect/render the white page, thin border, heading, both labels/placeholders, blue underlined link, dark rectangular button, grey footer text, and *absence of other visible controls on the card*; add automated assertions for the visible elements where feasible.

### RA4A9E-002 — Visible recovery link does not start a request

- **Requirement links:** REQ-004, REQ-007, REQ-018.
- **Evidence:** S1, S2, S3 (ACC-STD-01). **Priority:** High.
- **Risk and consequence:** A visually correct Forgot password? link could be inert or lead nowhere; users could not initiate the mandated recovery request despite a compliant-looking card.
- **Proposed mitigation:** Connect the observed link to a minimal reset-request interaction, without adding a control to the sign-in card that was not observed.
- **Proposed verification:** Activate Forgot password? from the sign-in page in an automated interaction test and confirm a reset request can be initiated.

### RA4A9E-003 — Reset requests disclose account existence

- **Requirement links:** REQ-008, REQ-009, REQ-018.
- **Evidence:** S3 (ACC-STD-01). **Priority:** High.
- **Risk and consequence:** Distinct returned request responses for a synthetic account and a nonexistent address, including at a rate boundary, could reveal whether an account exists. The exact shape of nonexistent-address counting is not established by the standard.
- **Proposed mitigation:** Design the returned request response to be identical for otherwise equivalent existing/nonexistent submissions; preserve that response when an existing account's issuance limit is reached. Do not assert a separate numerical limit for nonexistent addresses.
- **Proposed verification:** Compare returned responses for otherwise equivalent existing/nonexistent submissions and test the existing-account sixth request within the hour: no additional usable link, with the response still consistent with REQ-008. Do not infer that response parity proves all possible side channels are addressed.

### RA4A9E-004 — Per-account issuance exceeds the approved hourly limit

- **Requirement links:** REQ-009, REQ-018.
- **Evidence:** S3 (ACC-STD-01). **Priority:** High.
- **Risk and consequence:** Miscounting the five requests allowed per synthetic account per hour could generate a sixth usable link in the same hour, defeating the approved request limit.
- **Proposed mitigation:** Track issuance attempts for each synthetic account against the five-per-account-per-hour boundary, with the time window explicitly testable. Do not introduce a purported standard for nonexistent-address keys or thresholds.
- **Proposed verification:** Within one hour, exercise requests one through five and a sixth for the same account; confirm the sixth yields no additional usable reset link, while retaining REQ-008's response equivalence. Check that the applicable window is one hour rather than an arbitrary shorter period.

### RA4A9E-005 — Reset link reuse or issuance-time expiry is mishandled

- **Requirement links:** REQ-010, REQ-011, REQ-012, REQ-018.
- **Evidence:** S1, S3 (ACC-STD-02). **Priority:** High.
- **Risk and consequence:** A reused link might change a password again, or a link may remain usable at/after 30 minutes from issuance; conversely, premature expiry would prevent an otherwise valid reset. A reset flow that accepts a valid link but fails to change the corresponding synthetic account password would also defeat recovery.
- **Proposed mitigation:** Bind a link to its synthetic account and issuance time; allow at most one successful completion and apply the 30-minute cutoff from *issuance*, not from access or a later interaction. Keep the completion interaction minimal rather than inventing a password-complexity policy.
- **Proposed verification:** With an otherwise valid unused link, exercise just before T+30 minutes and at T+30 minutes from issuance; confirm only the former is unexpired. Complete a valid reset, verify the corresponding account password changes, then retry the same link and verify it cannot complete another reset.

### RA4A9E-006 — A successful reset leaves active sessions signed in

- **Requirement links:** REQ-013, REQ-018.
- **Evidence:** S3 (ACC-STD-02). **Priority:** High.
- **Risk and consequence:** If even one previously active session survives a successful password reset, a prior signed-in client could remain active contrary to the approved all-session sign-out boundary.
- **Proposed mitigation:** Tie the minimal synthetic sign-in/session representation to the account and invalidate *every* active session on successful reset. This proposes local demonstration behavior, not production session infrastructure.
- **Proposed verification:** Establish multiple active sessions for one synthetic account, successfully reset its password, and assert none of those previously active sessions remains signed in.

### RA4A9E-007 — Owner notification is missing after a successful reset

- **Requirement links:** REQ-014, REQ-016, REQ-018.
- **Evidence:** S1, S3 (ACC-STD-03). **Priority:** High.
- **Risk and consequence:** A successful reset could fail to record a notification to the account owner, or only the first success could be recorded. Introducing real external delivery would also exceed the story's in-memory boundary.
- **Proposed mitigation:** Record one owner-directed notification for each successful reset in the in-memory sink; use a test-inspectable local mechanism, without claiming an email/SMS service.
- **Proposed verification:** Perform separate successful resets using distinct links and inspect the in-memory sink after each; check that each success has an owner-directed record and that no production notification service is required.

### RA4A9E-008 — Demonstration boundaries are hidden or overstated

- **Requirement links:** REQ-015, REQ-016, REQ-017, REQ-018.
- **Evidence:** S1. **Priority:** Medium.
- **Risk and consequence:** Real-user account data, an external notification dependency, or missing production-limitations documentation could make the demonstration violate its scope or be mistaken for a production-ready application.
- **Proposed mitigation:** Use only synthetic accounts and in-memory notifications; explicitly document both limits and any additional implementation-specific limitations once known, without implying persistence, actual delivery, or production security guarantees.
- **Proposed verification:** Inspect account fixtures and the notification mechanism; check the production-limitations section names the synthetic-account and in-memory-delivery constraints and avoids a production-readiness claim. Test the synthetic/in-memory boundaries.

### RA4A9E-009 — Automated tests omit approved acceptance boundaries

- **Requirement links:** REQ-001, REQ-002, REQ-003, REQ-004, REQ-005, REQ-006, REQ-007, REQ-008, REQ-009, REQ-010, REQ-011, REQ-012, REQ-013, REQ-014, REQ-015, REQ-016, REQ-018, REQ-021.
- **Evidence:** S1, S2, S3. **Priority:** High.
- **Risk and consequence:** A passing suite that only checks a happy-path reset could miss sign-in control drift, response parity, rate boundary, token reuse/expiry, password update, all-session sign-out, owner notification or demonstration boundaries; acceptance would be unverified.
- **Proposed mitigation:** Plan automated tests against the approved REQ-018 criterion and the linked requirement IDs, including negative/boundary cases. Visual element/control assertions should rely only on the supplied observations, not imagined pixel specifications.
- **Proposed verification:** Review and run the test suite for each enumerated behavior: sign-in elements and no extra controls, recovery entry, existing/nonexistent response parity, five-per-account-per-hour boundary, single-use, T+30-minute expiry, correct password change, all-session invalidation, notification after every success, and synthetic/in-memory behavior. Passing tests would be evidence to gather later, not a current claim.

### RA4A9E-010 — Delivery breaches the no-deployment boundary

- **Requirement links:** REQ-019, REQ-020.
- **Evidence:** S1. **Priority:** High.
- **Risk and consequence:** Deploying the application or introducing a deployment workflow would directly violate the two distinct approved prohibitions, even if the local demonstration otherwise works.
- **Proposed mitigation:** Keep the delivery local; exclude deployment actions and deployment-workflow additions from the work.
- **Proposed verification:** Review delivered actions/artifacts for absence of deployment and inspect workflow changes for absence of newly added deployment workflows; do not run any deployment as a verification step.

## Approved requirement coverage matrix

Each ID below is from the authorized `requirements_analysis` predecessor; the risk IDs are proposed stable identifiers for the subsequent contract. A row's mapping identifies at least one substantive risk with a proposed consequence, mitigation and verification above. This matrix does not assert implementation or test completion.

| Approved requirement | Proposed risk coverage |
| --- | --- |
| REQ-001 | RA4A9E-001, RA4A9E-009 |
| REQ-002 | RA4A9E-001, RA4A9E-009 |
| REQ-003 | RA4A9E-001, RA4A9E-009 |
| REQ-004 | RA4A9E-001, RA4A9E-002, RA4A9E-009 |
| REQ-005 | RA4A9E-001, RA4A9E-009 |
| REQ-006 | RA4A9E-001, RA4A9E-009 |
| REQ-007 | RA4A9E-002, RA4A9E-009 |
| REQ-008 | RA4A9E-003, RA4A9E-009 |
| REQ-009 | RA4A9E-003, RA4A9E-004, RA4A9E-009 |
| REQ-010 | RA4A9E-005, RA4A9E-009 |
| REQ-011 | RA4A9E-005, RA4A9E-009 |
| REQ-012 | RA4A9E-005, RA4A9E-009 |
| REQ-013 | RA4A9E-006, RA4A9E-009 |
| REQ-014 | RA4A9E-007, RA4A9E-009 |
| REQ-015 | RA4A9E-008, RA4A9E-009 |
| REQ-016 | RA4A9E-007, RA4A9E-008, RA4A9E-009 |
| REQ-017 | RA4A9E-008 |
| REQ-018 | RA4A9E-002, RA4A9E-003, RA4A9E-004, RA4A9E-005, RA4A9E-006, RA4A9E-007, RA4A9E-008, RA4A9E-009 |
| REQ-019 | RA4A9E-010 |
| REQ-020 | RA4A9E-010 |
| REQ-021 | RA4A9E-001, RA4A9E-009 |

## Bounded non-blocking questions

These preserve the predecessor's unresolved decisions; none blocks documenting the proposed risks, and no answer here establishes a new product policy.

1. What minimal Sign in behavior and synthetic-account session representation will allow all-session invalidation to be demonstrated? Assume only local behavior needed for the sign-in page and reset verification; do not infer production session infrastructure.
2. How will nonexistent-address attempts be accounted for, and what identifies an account key, without disclosing existence? Enforce the specified five-per-account-per-hour limit for synthetic accounts and response parity; do not invent a separate threshold for nonexistent addresses.
3. How will a synthetic account's reset link be accessible locally and the in-memory sink be inspected in tests? Assume only a local, test-inspectable mechanism, not email, SMS or external delivery.
4. Which further production limitations will become clear from the eventual implementation? At minimum document synthetic accounts and in-memory notification constraints; do not assert unspecified operational characteristics or production readiness.
5. What minimal reset-completion form, validation and failure wording will be selected? The supplied image specifies only the sign-in card; do not invent a password-complexity rule, numeric policy, failure text or extra sign-in-card control.

**Next-task boundary:** T001 writes only these planning notes. The pinned final JSON and Markdown deliverables are not created or modified here.
