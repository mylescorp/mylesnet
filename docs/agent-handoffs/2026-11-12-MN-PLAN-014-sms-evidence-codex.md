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

- **Checks run:** `git diff --check` passed. Local Markdown link scan passed for all changed-document targets except one missing private reference image already present in base `HEAD` (`docs/READINESS-DECISIONS.md` → `reference-images/reference-domain-settings.png`); this is not introduced by this task. Conflict-marker scan passed. Gitleaks, Vercel deployment, and Vercel Preview Comments passed on PR #20 head `fc7600326a33c84ad2653b7b7d407cb93660b0f3`. Official-source review on 12 November 2026: Africa's Talking sandbox guide states sandbox interactions occur in its simulator rather than on a handset; its country list claims SMS availability in Kenya, Uganda, and Tanzania. Infobip docs distinguish `PENDING_ACCEPTED` from final handset delivery, define `DELIVERED_TO_HANDSET`, and document up to 20 retries after the initial webhook attempt, with the final retry 41 hours 30 minutes later. Country sender-registration statements are attributed to Infobip documentation and marked for current provider/counsel confirmation.
- **Checks not run:** No automated tests, build, application integration, account setup, endpoint setup, or SMS transmission; outside this documentation task and not authorized.
- **Evidence:** Official links and dates/limitations are recorded in `docs/SMS-PROVIDER-PUBLIC-EVIDENCE.md`. Africa's Talking developer SMS pages returned HTTP 403 to the review tool; this is recorded as an inspection limitation only.
- **REA evidence:** Not used; normal documentation review was sufficient.
- **Failures:** No failing project checks. The HTTP 403 from the official Africa's Talking developer-doc site limited inspection; provider behavior is not inferred from that access failure. The pre-existing private reference-image link remains unresolved and unchanged.

## Remaining work

### Done

- Documented public provider API/test/callback/country evidence and linked it to applicable Phase 0 contracts.
- Maintained the no-selection, no-live-message planning boundary.

### Pending

- Run documentation integrity checks and current-head secret-scan/preview checks after this rebase; obtain a fresh Greptile review and merge only if the current head has an explicit 5/5 score, zero unresolved issues, and passing required checks. The user resumed review and merge authorization for the open draft PR batch #16–#26 on 2026-11-12.
- Obtain comparable country/MNO quotes, sender approvals, privacy/security terms, and authorized handset evidence before any selection. Appoint the accountable communications/privacy and country-counsel owners.

### Blocked

- None for this documentation deliverable. Provider selection and live pilot remain gated on named owners, country/provider evidence, legal/privacy review, and explicit authorization.

## Risks and handoff instructions

- **Risks/limitations:** Provider pages may change; published availability and sender rules do not prove account eligibility, legal compliance, route quality, delivery latency, handset delivery, or an SLA. Infobip trial sender names differ across public guides. Africa's Talking API docs were inaccessible to this review tool. No SMS was sent.
- **Next action:** PR #20 is in the user-authorized resumed review batch #16–#26. Keep it open until the rebased current head receives explicit Greptile 5/5, zero unresolved findings, and all required checks pass; then merge, update the status ledger, and clean only its dedicated merged worktree/branch.
- **User approval required:** Privacy/legal/provider safeguards and separate explicit authorization before any handset delivery pilot.

## Delivery state

- **Commit/branch:** Rebased task commits `7c80f71`–`7ff265c` on `codex/mn-plan-014-sms-provider-evidence`, with `main` at `3842391` as the base; the final handoff refresh commit is being added now.
- **PR/review:** [PR #20](https://github.com/mylescorp/mylesnet/pull/20), ready/open; Greptile identified an outdated pending-work list and a missing review-restoration note. Both are corrected on this same branch. Fresh review and checks are pending.
- **Merge:** Not merged.
- **Deployment:** Not deployed; no product service was implemented or enabled.
