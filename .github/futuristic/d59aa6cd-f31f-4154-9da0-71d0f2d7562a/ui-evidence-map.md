# UI evidence map — approved REQ-001–REQ-021

This is a planning map for T001, **not** a claim that screens, behavior, tests, or deployment exist. Classification distinguishes what the supplied static image depicts from UI behavior proposed to satisfy the approved requirements and from non-UI obligations. The attachment reader returned a source reference but no inspectable pixels here; the **Observed** inventory below uses the image observation supplied for this task, without inferring additional visual details.

## Authorized evidence

| Key | Exact source reference | Use |
| --- | --- | --- |
| [A] | `source_type: attachment; source_id: a4690c70-e0db-4bb7-b5c0-f0422127e023; locator: sign-in-wireframe.png` | Supplied observation of the static sign-in wireframe only. |
| [S] | `source_type: user_story; source_id: user_story; locator: characters 0-16000` | Small reset application, synthetic accounts, in-memory notification sink, documentation, tests, no deployment or new deployment workflows. |
| [K] | `source_type: knowledge; source_id: 46054aed-d642-45cb-ac91-e8bb053c4af6; locator: v1, chunk 0` | Run-matched recovery standard: reset entry, identical request responses, five requests/account/hour, single use, expiry at 30 minutes from issuance, all-session sign-out, notification after each successful reset. |

REQ identifiers below come from the approved requirements output. Its stage-output reader supplied **no source-reference locator**; the identifiers are retained without inventing a `stage_output` citation. Only [A] supports an **Observed** visual statement; [S] and [K] support requirements and proposed behavior, not an image interpretation.

## Observed screen

**Observed — Sign in ([A]; REQ-001–REQ-006, REQ-021):** A white page contains a thin dark bordered card with a bold **Sign in** heading. Inside are an **Email address** label/input showing `name@example.com`, a **Password** label/input illustrated with masked asterisks, a blue underlined **Forgot password?** link, a dark rectangular **Sign in** button, and the grey footer **No account? Contact your administrator.** No other controls are visible **on the card**. The static depiction does not establish how the password field masks entered text, whether the asterisks are a functional placeholder, what the controls do, or whether the footer is interactive. It also does not show any validation, keyboard/focus state, loading state, result, recovery page, responsive behavior, or exact design tokens.

## Complete requirement classification

Exactly one primary classification is assigned to each approved ID. “Directly observed visual UI” describes appearance/presence only, not functioning implementation. Proposed UI implications of non-UI rules are noted separately, without reclassifying those rules as pictured.

| ID | Primary classification | Evidence-bounded coverage |
| --- | --- | --- |
| REQ-001 | Directly observed visual UI | White page; thin dark bordered card; bold **Sign in** heading. [A] |
| REQ-002 | Directly observed visual UI | **Email address** label/input and `name@example.com` placeholder. [A] |
| REQ-003 | Directly observed visual UI | **Password** label/input illustrated with masked asterisks; functional masking is not proven. [A] |
| REQ-004 | Directly observed visual UI | Blue underlined **Forgot password?** link is visible; activation is not shown. [A] |
| REQ-005 | Directly observed visual UI | Dark rectangular **Sign in** button is visible; submission behavior is not shown. [A] |
| REQ-006 | Directly observed visual UI | Grey **No account? Contact your administrator.** footer; no sign-up control is pictured. [A] |
| REQ-007 | UI behavior/proposed but not pictured | **Proposed:** link starts reset request; a static link cannot show navigation/activation. [A][K][S] |
| REQ-008 | UI behavior/proposed but not pictured | **Proposed:** the request's returned response/acknowledgment is identical for existing and nonexistent addresses; no response screen is pictured. [K] |
| REQ-009 | Non-UI | Account-level five-request/hour enforcement, not a visible control; **proposed UI implication:** preserve the neutral request response even at the limit. [K] |
| REQ-010 | UI behavior/proposed but not pictured | **Proposed:** a used link cannot complete another reset; invalid/used-link presentation is unspecified. No completion screen is pictured. [K][S] |
| REQ-011 | UI behavior/proposed but not pictured | **Proposed:** an otherwise valid unused link works before 30 minutes from issuance, but cannot reset at or after that boundary; expiry presentation is not pictured. [K] |
| REQ-012 | UI behavior/proposed but not pictured | **Proposed:** minimal reset-completion interaction changes the corresponding synthetic account password with a valid link. No form or password policy is supplied visually. [K][S] |
| REQ-013 | Non-UI | Invalidate **every** active session after a successful reset; a subsequent sign-in may expose the result, but the wireframe proves no session behavior. [K] |
| REQ-014 | Non-UI | Notify the owner **after each successful** reset; not an on-screen notice to the requester. [K][S] |
| REQ-015 | Non-UI | Use only synthetic accounts; no real-account content or account source is established by the image. [S] |
| REQ-016 | Non-UI | In-memory notification sink, not an email/SMS screen or delivery claim. [S] |
| REQ-017 | Non-UI | Production-limitations documentation; no image evidence of documentation. [S] |
| REQ-018 | Non-UI | Automated sign-in and reset tests; tests can check visible UI and flow, but no tests are pictured or claimed complete. [S][A][K] |
| REQ-019 | Non-UI | No deployment; outside the screen journey. [S] |
| REQ-020 | Non-UI | No new deployment workflows; outside the screen journey. [S] |
| REQ-021 | Directly observed visual UI | Only the email input, password input, forgot link, and sign-in button are visible **as card controls**; no extra card control is evidenced or proposed. [A] |

Coverage count: **7 directly observed visual UI** (001–006, 021); **5 UI behavior/proposed but not pictured** (007, 008, 010–012); **9 non-UI** (009, 013–020). Every REQ-001 through REQ-021 occurs once in the table.

## Proposed journey and screen states

All items in this section are **Proposed (inferred, not observed)**, unless explicitly marked **Observed [A]**. Screens beyond Sign in have no supplied layout or copy. The flow is a design outline, not a built UI or an assertion of how links are delivered.

1. **Sign in → reset entry (REQ-001–REQ-007, REQ-021).** **Observed [A]:** the sign-in card and link presence described above. **Proposed [K]:** activating **Forgot password?** opens a reset-request interaction. Keep the visible sign-in card control inventory unchanged; place recovery interactions on separate proposed screens. Minimal local synthetic-account sign-in/session behavior may be needed to demonstrate REQ-013; destination and session representation are not decided. [S][K]
2. **Reset request → neutral acknowledgment (REQ-007–REQ-009).** **Proposed [K]:** a separate request screen accepts an account identifier and submits the request. Show the same returned response for an existing or nonexistent account, with no account-existence clue in its content or account-specific error. Enforce five requests per synthetic account per hour without generating another usable link beyond the limit; keep the request response consistent with REQ-008. Do not claim a rule for counting nonexistent accounts or specify exact message, timing, or transport.
3. **Valid link → reset completion → result/sign-in return (REQ-010–REQ-014).** **Proposed [K][S]:** for a synthetic account, an unused link before 30 minutes from **issuance** permits a minimal new-password interaction and changes its password on success. At or after 30 minutes, or once used, it cannot complete another reset. After each successful reset, invalidate every active session and record an owner notification in the in-memory sink; those are application effects, not pictured UI. A proposed success state can offer a return to Sign in without suggesting that any previous session remains authenticated. Link exposure in this local demonstration is unresolved; do not imply production email/SMS delivery. [S]

| Screen/state | Validation and empty state (proposed) | Error state (proposed) | Loading/pending and success (proposed) |
| --- | --- | --- | --- |
| **Sign in** — visually observed card only [A]; behavior proposed [S] | Empty email/password: identify the missing field without treating placeholders as labels; malformed email: accessible field-specific guidance if email format is validated. No numeric password policy is inferred. | Invalid local synthetic credentials: accessible, non-sensitive feedback; actual wording and sign-in failure rules are open. Do not add a permanent visible card control. | Pending submission: communicate progress and prevent accidental repeat submit without asserting pictured spinner/disabled styling. Success: minimal local session outcome/destination remains undecided. |
| **Reset request** — entirely proposed [K] | Empty identifier or locally invalid format: accessible field-specific prompt; do not imply an account lookup. Initial empty form is distinct from a submitted request. | Service failure: accessible generic retry guidance, not a response that distinguishes existing and nonexistent accounts. The required ordinary request response, including rate-limited requests, remains neutral. | Pending request: announce progress without revealing account status. Completed request: identical returned acknowledgment for existing/nonexistent accounts and a limit-preserving response; exact copy/layout unspecified. |
| **Neutral acknowledgment** — entirely proposed [K] | No input/required-field validation; an initial empty result is not a second outcome. | If acknowledgment cannot load, show generic failure, not account-specific status; error behavior must not undermine REQ-008. | A prior request's pending state belongs to the request screen. Success here means the **request response** was returned, not proof an account or link exists; content identical for existing/nonexistent requests. |
| **Reset completion/result** — entirely proposed [K][S] | New-password entry required; empty value gets an accessible prompt. Whether there is a confirmation field, further policy, or validation wording remains open; do not invent one. | Used, expired (at/after issuance + 30 minutes), or otherwise invalid link cannot change a password; give actionable, accessible generic recovery guidance without claiming a depicted screen. | Pending completion: communicate progress, avoid duplicate submission. Success: show password changed/return-to-sign-in guidance; on the application side, invalidate all sessions and record an owner notification for **each** success. No notification delivery UI is assumed. |

## Accessibility: proposed checks, not observed behavior

**Observed [A]:** the sign-in depiction includes visible **Email address** and **Password** labels and an underlined blue **Forgot password?** link. These observations do **not** establish programmatic labels, keyboard operability, focus indication, screen-reader names, or sufficient contrast.

**Proposed [S][K]:** associate field labels and instructions with inputs; provide discernible screen-reader names for the link and each button; permit keyboard traversal/activation with logical focus order; place visible focus on the active element after navigation and after validation failures; give field-linked, textual errors and announced request/progress/result states without disclosing account existence. Check text and control contrast, specifically the blue link, grey footer, dark button, and error/focus states; do not infer color values or compliance from the image. Apply these checks separately to sign-in, reset request, neutral acknowledgment, and completion/result, and keep keyboard users able to recover from invalid/used/expired links. Avoid using placeholder or color alone as a label/error indicator. No accessibility conformance is asserted from a static wireframe.

## Unresolved design decisions and boundaries

- Proposed recovery-page layout, exact neutral copy, and whether the request identifier is typed anew or carried from sign-in are not pictured or specified. A carried-forward address must not alter the identical response rule. [A][K]
- Reset link presentation in a synthetic, in-memory local demo; password policy, optional confirmation entry, and expired/used-link wording are open. Do not assume external delivery or an additional sign-in-card control. [S][K][A]
- The sign-in success destination, representation of multiple synthetic sessions, responsive layout, precise tokens, focus/announcement behavior, and failure copy need design/validation; they are not observed. [A][S][K]
- Non-UI deliverables remain binding: synthetic accounts (REQ-015), in-memory sink (REQ-016), honest production-limitations documentation (REQ-017), automated tests (REQ-018), no deployment (REQ-019), and no new deployment workflow (REQ-020). Nothing in this map claims implementation or verification. [S]
