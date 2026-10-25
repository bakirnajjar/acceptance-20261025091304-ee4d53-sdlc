# UI analysis — password-reset demonstration

**Applicability:** applicable. This is an evidence-bounded screen and flow proposal, not a claim that an application, interactions, tests, or deployment exist. Approved requirement IDs are taken from `docs/sdlc/requirements_analysis.json`; the prior-stage reader returned no citation locator, so no `stage_output` reference is invented.

## Sources and interpretation

| Key | Source type | Source ID | Locator | Scope |
| --- | --- | --- | --- | --- |
| [A] | attachment | `a4690c70-e0db-4bb7-b5c0-f0422127e023` | `sign-in-wireframe.png` | Only the supplied static sign-in observations. |
| [S] | user_story | `user_story` | `characters 0-16000` | Small reset application, synthetic accounts, in-memory notification sink, documentation, tests, no deployment/workflow additions. |
| [K] | knowledge | `46054aed-d642-45cb-ac91-e8bb053c4af6` | `v1, chunk 0` | Run-matched account recovery standard: neutral response, five requests/account/hour, single use, 30 minutes from issuance, all-session sign-out, notify after each success. |

**Observed** statements below are limited to the supplied sign-in wireframe. Other similarly titled standards are not cited. All recovery screens, behavior, states and accessibility recommendations are **Proposed (inferred, not observed)**. No source establishes a link-delivery channel, password complexity policy, pixel-exact tokens or responsive design.

## Screen and control inventory

**Observed — Sign in [A] (REQ-001–REQ-006, REQ-021):** A white page has a thin dark bordered card headed in bold **Sign in**. The card contains the **Email address** label/input with `name@example.com` placeholder, the **Password** label/input illustrated with masked asterisks, a blue underlined **Forgot password?** link, a dark rectangular **Sign in** button, and grey **No account? Contact your administrator.** footer text. The only visible *card controls* are those two inputs, link and button; no other card controls are shown. Asterisks do not prove functional masking, and footer text does not prove an interaction. The static image does not show activation, validation, errors, loading, focus, outcomes, other screens, responsive layout, or design tokens.

**Proposed — Reset request [S][K] (REQ-007–REQ-009):** A separate interaction receives an account identifier and a reset request, without adding a permanent control to the observed sign-in card. Exact layout and whether the sign-in address is carried forward are open.

**Proposed — Neutral acknowledgment [K] (REQ-008–REQ-009):** A no-input result for a completed request. Its returned response is identical for existing and nonexistent addresses; it does not confirm that an account, link, or delivery exists. Exact copy is open.

**Proposed — Reset completion and result [S][K] (REQ-010–REQ-014):** A minimal new-password interaction for a valid, unused link, with an accessible failure/recovery path for expired, used or invalid links and success guidance to return to sign-in. No form layout, confirmation field, password policy or link transport is evidenced. All-session sign-out and owner notification are application effects, not pictured screens.

## Step-by-step flows

1. **Sign in → reset request (REQ-004, REQ-007, REQ-021).** **Observed [A]:** the link is present on the sign-in card. **Proposed [K][S]:** activating it opens the separate reset-request interaction. No link activation is proven by the image, and no extra permanent card control is introduced.
2. **Request → neutral acknowledgment (REQ-007–REQ-009).** **Proposed [K]:** submit an account identifier; return an identical request response for an existing and nonexistent address. For a synthetic account, after five requests in an hour another request in that hour creates no additional usable link, while its returned response remains consistent with the neutral rule. The standard does not specify how nonexistent identifiers are counted.
3. **Link → completion → result/sign-in return (REQ-010–REQ-014).** **Proposed [S][K]:** an unused link for a synthetic account permits a password change before 30 minutes from *issuance*, but not at or after that instant; a used link cannot complete another reset. On **each** successful reset, invalidate **every** active session and record an owner notification in memory. The result may guide a new sign-in, not preserve authentication on an old session. Link exposure in the local demonstration is unresolved; this does not assert email or SMS delivery.
4. **Ordinary sign-in demonstration (REQ-005, REQ-013, REQ-015).** **Proposed [S][K]:** only minimal local synthetic-account sign-in/session behavior is contemplated to exercise the card and verify all-session invalidation. Success destination, multiple-session representation and failure copy are open, not observed.

## Complete requirement mapping

“Direct visual” means presence or appearance in [A], **not** functioning implementation. Each ID has one primary classification. **No direct visual evidence** exists for REQ-007–REQ-020, including any tests or operational behavior.

| ID | Primary classification | Evidence-bounded UI implication or nonvisual obligation |
| --- | --- | --- |
| REQ-001 | Direct visual — Observed [A] | White page, thin dark bordered card, bold Sign in heading. |
| REQ-002 | Direct visual — Observed [A] | Email address label/input and `name@example.com` placeholder. |
| REQ-003 | Direct visual — Observed [A] | Password label/input with masked asterisks pictured; actual masking is not proven. |
| REQ-004 | Direct visual — Observed [A] | Blue underlined Forgot password? link; its behavior is not pictured. |
| REQ-005 | Direct visual — Observed [A] | Dark rectangular Sign in button; no submission outcome shown. |
| REQ-006 | Direct visual — Observed [A] | Grey No account? Contact your administrator. footer; no sign-up control pictured. |
| REQ-007 | UI behavior — Proposed [A][K][S] | Activation starts reset request; only the link's presence is observed. |
| REQ-008 | UI behavior — Proposed [K] | Identical returned request response for existing/nonexistent accounts; neutral acknowledgment. |
| REQ-009 | Nonvisual rule — Proposed UI implication [K] | Five requests per account per hour; sixth within hour generates no usable link; response remains neutral. |
| REQ-010 | UI behavior — Proposed [S][K] | Used link cannot complete a second reset; offer accessible recovery guidance. |
| REQ-011 | UI behavior — Proposed [K] | Unexpired before, expired at or after 30 minutes **from issuance**; show recovery guidance. |
| REQ-012 | UI behavior — Proposed [S][K] | Valid unused link enables a synthetic-account password change; form/policy not specified. |
| REQ-013 | Nonvisual effect — Proposed UI implication [K] | Invalidate every active session on success; result must not promise continued sign-in. |
| REQ-014 | Nonvisual effect — Proposed UI implication [S][K] | Notify owner after **every** successful reset; not a requester-facing or delivered-message screen. |
| REQ-015 | Nonvisual boundary [S] | Synthetic accounts only; do not infer a real-account source or dataset from the image. |
| REQ-016 | Nonvisual boundary [S] | In-memory notification sink; do not claim production delivery. |
| REQ-017 | Nonvisual deliverable [S] | Document synthetic/in-memory and implementation-specific production limitations without readiness claim. |
| REQ-018 | Nonvisual verification [S][A][K] | Automated tests must cover observed card inventory, entry and recovery rules; no test is pictured or claimed complete. |
| REQ-019 | Nonvisual delivery constraint [S] | Do not deploy; no screen evidence. |
| REQ-020 | Nonvisual delivery constraint [S] | Do not add deployment workflows; no screen evidence. |
| REQ-021 | Direct visual — Observed [A] | Only the two inputs, forgot link and sign-in button are visible **as card controls**; no extra card control proposed. |

Coverage: 7 directly observed visual IDs (REQ-001–REQ-006, REQ-021), 5 proposed unpictured UI-behavior IDs (REQ-007, REQ-008, REQ-010–REQ-012), 9 primarily nonvisual IDs (REQ-009, REQ-013–REQ-020). A nonvisual rule may still constrain the proposed UI.

## Screen states — all Proposed [S][K], not visible in [A]

| Screen | Initial/empty and locally invalid | Error, expired or used | Loading and success |
| --- | --- | --- | --- |
| Sign in (REQ-001–REQ-005, REQ-021) | Initial form has no entered credentials; submitting an empty field gives field-linked text. If identifier format is checked, give locally invalid guidance; placeholders are not labels. | Invalid local synthetic credentials give accessible, nonsensitive feedback; no password complexity or exact failure wording is asserted. Expired/used link is not a sign-in-card state. | Announce pending sign-in and guard against accidental repeat submit. A minimal local success/session outcome is open. Do not introduce an extra permanent visible card control. |
| Reset request (REQ-007–REQ-009) | Initial empty identifier is not an account lookup; empty or locally invalid submission prompts accessible field guidance without asserting whether an account exists. | Generic service failure/retry guidance must not disclose account status. Ordinary returned responses, including at the per-account limit, remain consistent with the existing/nonexistent identical-response rule. Expired/used link is not this screen's state. | Announce pending request generically. Completed request reaches the same neutral returned acknowledgment for existing and nonexistent accounts; no account-specific success indicator. |
| Neutral acknowledgment (REQ-008–REQ-009) | No editable input, so no required-field validation; an uninitialized result is not an alternate account outcome. | If the result cannot be presented, provide generic failure guidance without account-specific status. Expired/used link is not this screen's state. | Pending belongs to the request transition; acknowledgment means only that the request response returned, not that an account or link exists. |
| Reset completion/result (REQ-010–REQ-014) | An empty new password prompts field-linked guidance; additional policy or confirmation fields are open rather than invented. | Invalid, already used, or expired **at or after issuance + 30 minutes** cannot complete a reset; give accessible recovery guidance. Exact wording and whether these cases share one message are open. | Announce pending completion and avoid duplicate submission. A valid unused link before the boundary leads to a password-changed result and sign-in guidance; behind the UI, sign out all active sessions and notify owner for each success. |

## Accessibility — Proposed [A][S][K], not verified

The **Observed [A]** card shows visible Email address and Password labels and an underlined link, but proves no programmatic association, keyboard operation, focus styling, contrast ratio or screen-reader output. **Proposed:** use label/input associations independent of placeholders; discernible screen-reader names for link and buttons; logical keyboard order and activation on every screen; visible focus after navigation and on validation errors; field-linked textual errors rather than color alone; announced generic pending and result states that do not disclose account existence. Test contrast of the pictured blue link, grey footer and dark button, plus proposed errors and focus indicators, without inferring color tokens or conformance from the static image. Provide keyboard-reachable recovery guidance for invalid, used and expired links. Neutral acknowledgment has no fields but its result and any error must still be perceivable and announced.

## Assumptions and open decisions

- **Assumption:** Only the supplied sign-in wireframe supports **Observed [A]** statements. Source references for approved predecessor IDs were not given a stage-output locator.
- **Assumption:** Minimal local synthetic sign-in/session behavior, test-inspectable in-memory links and notifications, and a minimal password-change interaction suffice to outline the journey; none implies production infrastructure or external delivery. The standard's link boundary is measured from issuance.
- **Open:** Recovery-page layout and exact neutral copy; identifier re-entry versus carried-forward value. A carried value must not change the identical returned response.
- **Open:** Local link exposure, in-memory notification inspection, and nonexistent-identifier throttling/account key without an existence signal.
- **Open:** New-password fields/policy, error and expired/used-link wording, and whether any confirmation field exists; do not silently add complexity requirements.
- **Open:** Sign-in success destination, multiple-session representation, responsive layout, contrast tokens, focus placement and announcements. These are design/validation decisions, not image observations.

All open decisions above are nonblocking at UI-analysis stage; implementation must resolve them without weakening REQ-001–REQ-021 or claiming the demonstration is production-ready. No deployment or deployment-workflow addition is proposed.
