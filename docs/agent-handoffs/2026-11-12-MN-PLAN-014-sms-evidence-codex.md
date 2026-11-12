# Task Handoff: SMS provider public evidence

- **Task ID:** `MN-PLAN-014`
- **Date:** `2026-11-12`
- **Agent/owner:** `Codex`
- **Plan phase:** `Phase 0 — close planning and turn decisions into build contracts`
- **Status:** `In progress`
- **Related requirements:** `IMPLEMENTATION-PLAN.md` Phase 0 work items 5–7; `CP-COMMS-01`; `MN-DEC-014`; `EVT-005`; technical-specification messaging consent and delivery-report requirements.
- **Dependencies:** MN-PLAN-007 country and operations packs and MN-PLAN-006 event/data contracts; accountable communications/privacy and country-counsel owners remain unassigned.

## Objective

Record official public SMS API, safe-test, delivery-report, and country/sender-registration evidence for Africa's Talking and Infobip. Keep provider selection, country enablement, and live messaging approval open. This is a planning-only task; no accounts, credentials, test endpoints, or SMS destinations may be used without separate authorization.

## Work completed

- Added a dated public-source comparison, safe evaluation sequence, and explicit unknowns in `docs/SMS-PROVIDER-PUBLIC-EVIDENCE.md`.
- Recorded that Africa's Talking documents a simulator that does not reach a handset; Infobip trial tutorials send to verified phone recipients and show differing example test sender names.
- Captured the distinction between accepted/queued and handset-delivered statuses, Infobip's documented callback retry/event-loss behavior, and provider-declared country/sender rules with appropriate age and account-scope caveats.
- Linked the evidence from the Phase 0 implementation plan, country pack, decision register, readiness decisions, and EVT-005.
- No vendor was selected, no external account or endpoint was configured, and no message was sent.

## Implementation or documentation details

- **Files changed:** `docs/SMS-PROVIDER-PUBLIC-EVIDENCE.md`; `docs/COUNTRY-AND-OPERATIONS-PACKS.md`; `docs/DECISION-REGISTER.md`; `docs/API-ERROR-EVENT-CONTRACT-BACKLOG.md`; `docs/IMPLEMENTATION-PLAN.md`; `docs/READINESS-DECISIONS.md`; `docs/IMPLEMENTATION-STATUS.md`; `docs/agent-handoffs/2026-11-12-MN-PLAN-014-sms-evidence-codex.md`.
- **Approach:** Used official vendor documentation only. Separated vendor-published interface/testing descriptions from account-specific coverage, handset delivery, commercial terms, security guarantees, and legal approval. Preserved Twilio as the separate price benchmark; did not refresh prices.
- **Decisions made:** None. Africa's Talking and Infobip remain comparison candidates; no provider is selected.
- **Decisions left open:** Current provider API/callback contracts, callback authentication and replay protections, country/MNO routes and delivery quality, quotes, sender approval/timelines, data location/retention, support terms, local legal review, and test-pilot authorization.

## Verification and evidence

- **Checks run:** Official-source review on 12 November 2026. Africa's Talking sandbox guide states sandbox interactions occur in its simulator rather than on a handset; its country list claims SMS availability in Kenya, Uganda, and Tanzania. Infobip docs distinguish `PENDING_ACCEPTED` from final handset delivery, define the `DELIVERED_TO_HANDSET` status, and document a 20-retry/41-hour-30-minute webhook window before events may be lost. Country sender-registration statements are attributed to Infobip documentation and marked for current provider/counsel confirmation.
- **Checks not run:** No automated tests, build, application integration, account setup, endpoint setup, or SMS transmission; outside this documentation task and not authorized.
- **Evidence:** Official links and dates/limitations are recorded in `docs/SMS-PROVIDER-PUBLIC-EVIDENCE.md`. Africa's Talking developer SMS pages returned HTTP 403 to the review tool; this is recorded as an inspection limitation only.
- **REA evidence:** Not used; normal documentation review was sufficient.
- **Failures:** No project checks have been run yet. The HTTP 403 from the official Africa's Talking developer-doc site limited inspection; provider behavior is not inferred from that access failure.

## Remaining work

### Done

- Documented public provider API/test/callback/country evidence and linked it to applicable Phase 0 contracts.
- Maintained the no-selection, no-live-message planning boundary.

### Pending

- Run documentation integrity checks, commit and push this focused branch, create this task's own draft PR, and verify current-head secret-scan/preview checks.
- Obtain comparable country/MNO quotes, sender approvals, privacy/security terms, and authorized handset evidence before any selection. Appoint the accountable communications/privacy and country-counsel owners.

### Blocked

- None for this documentation deliverable. Provider selection and live pilot remain gated on named owners, country/provider evidence, legal/privacy review, and explicit authorization.

## Risks and handoff instructions

- **Risks/limitations:** Provider pages may change; published availability and sender rules do not prove account eligibility, legal compliance, route quality, delivery latency, handset delivery, or an SLA. Infobip trial sender names differ across public guides. Africa's Talking API docs were inaccessible to this review tool. No SMS was sent.
- **Next action:** Finish documentation checks, push the task branch, open its own draft PR, and keep it open/unmerged. The user has deferred code review until they say it is restored; do not request review while the deferral remains active.
- **User approval required:** Explicit user approval and privacy/legal/provider safeguards before any handset delivery pilot; explicit user instruction when code review is restored.

## Delivery state

- **Commit/branch:** Pending; branch `codex/mn-plan-014-sms-provider-evidence`.
- **PR/review:** Pending own draft PR; code review deferred at the user's direction.
- **Merge:** Not merged.
- **Deployment:** Not deployed; no product service was implemented or enabled.
