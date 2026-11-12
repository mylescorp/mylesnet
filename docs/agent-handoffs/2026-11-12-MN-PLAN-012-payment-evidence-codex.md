# Task Handoff: Public payment-provider documentation evidence

- **Task ID:** `MN-PLAN-012`
- **Date:** `2026-11-12`
- **Agent/owner:** `Coordinator`
- **Plan phase:** `Phase 0 — close planning and turn decisions into build contracts`
- **Status:** `In progress`
- **Related requirements:** `IMPLEMENTATION-PLAN.md` Phase 0 work items 5–6; `DECISION-REGISTER.md` MN-DEC-013; `COUNTRY-AND-OPERATIONS-PACKS.md` CP-PAYMENT-01; `API-ERROR-EVENT-CONTRACT-BACKLOG.md` EVT-001 and RECON-001
- **Dependencies:** MN-PLAN-007 country and operations packs; public-document discovery supplements, but does not satisfy, required payment-provider, legal, commercial, security, sandbox, or country approvals.

## Objective

Record public provider documentation and regulator-published listing evidence for the existing payment candidates in Kenya, Uganda, and Tanzania. Separate observations from inference and unresolved approvals. No provider selection, sandbox test, API call, account creation, payment, or product implementation is in scope.

## Work completed

- Added a dated evidence record covering Safaricom Daraja, ioTec Pay, ClickPesa, and Selcom, with source links and explicit limitations.
- Recorded the key safety finding from ClickPesa's current official Quickstart: it says no sandbox exists, API Explorer requests use production and may move real funds, and API access is gated on KYC. A safe non-financial test arrangement remains required before API testing.
- Recorded Daraja's provider-described sandbox path while noting it does not prove every required collection flow is sandboxed.
- Recorded ioTec's documented collection/disbursement/status/callback interfaces and sample/test values while leaving sandbox-vs-live separation unverified.
- Recorded Selcom's documented collection/status/callback interfaces and business-team credential process; absence of a sandbox reference in the reviewed page is not treated as proof no sandbox exists.
- Added regulator-published listing observations: CBK's May 2026 directory lists Safaricom PLC and merchant payment processing services; BoT's 2025 report lists DLT CP Tanzania Company Limited t/a Clickpesa and Selcom Paytech Limited; a BoU September 2023 publication lists IOTEC Limited with specified payment-system categories.
- Kept the regulator evidence explicitly bounded: the Uganda listing is historical, and none of the listings resolves current candidate permission scope, product/API approval, or MylesNet merchant eligibility.
- Linked the evidence to CP-PAYMENT-01 and MN-DEC-013; updated the Phase 0 roadmap, kickoff next action, and repository status.

## Implementation or documentation details

- **Files changed:** `docs/PAYMENT-PROVIDER-PUBLIC-EVIDENCE.md`; `docs/COUNTRY-AND-OPERATIONS-PACKS.md`; `docs/DECISION-REGISTER.md`; `docs/IMPLEMENTATION-PLAN.md`; `docs/EXECUTION-KICKOFF-PLAN.md`; `docs/IMPLEMENTATION-STATUS.md`; `docs/agent-handoffs/2026-11-12-MN-PLAN-012-payment-evidence-codex.md`.
- **Approach:** Read official public provider developer/product pages and regulator publications. Recorded date, legal-entity names, listing descriptions, and limitations; did not infer product approval or MylesNet eligibility from an entity listing.
- **Decisions made:** None. MN-DEC-013 remains `Candidate`; all country collection/payout rails remain disabled until written evidence and approvals close the existing gates.
- **Decisions left open:** Current license status and permission scope, merchant eligibility, country/operator coverage, contract and fees, settlement, refunds/reversals, payout, disputes, callback authentication/replay, data-transfer terms, security review, sandbox/test account access, service/support commitments, and counsel approval. The Uganda regulator record is a June 2023 snapshot; ClickPesa's safe test arrangement is specifically unresolved.

## Verification and evidence

- **Checks run:** Prior pushed PR head `0539dce` passed Gitleaks, Vercel deployment, and Vercel Preview Comments. On the current documentation update, `git diff --check`, local Markdown links (6 changed files), and conflict-marker scan passed. The PR checks will rerun after push; this handoff does not claim results for the new head.
- **Checks not run:** No tests or builds were run for this documentation-only task. No provider endpoint, API Explorer, account, or sandbox was used. No REA runtime inspection was needed.
- **Evidence:** Provider and regulator source URLs, publication/access dates, and limits are recorded in `docs/PAYMENT-PROVIDER-PUBLIC-EVIDENCE.md`, including the CBK directory (15 May 2026 snapshot), BoU September 2023 newsletter (June 2023 listing snapshot), and BoT National Payment Systems Annual Report 2025.
- **REA evidence:** Not used; this task reviewed public documentation, not a product runtime.
- **Failures:** None from the documentation checks. No product/runtime validation was attempted.

## Remaining work

### Done

- Capture public provider documentation and identify what it does not prove.
- Preserve the planning-only and no-real-payment boundary in the country pack and decision register.

### Pending

- Have appointed payments/finance, security, and country-counsel owners verify current license scope and request/assess written eligibility, sandbox, commercial, callback-security, data-transfer, settlement, refund, and support evidence.
- Keep PR #18 open and unmerged during the user's Greptile deferral. Request review only after the user says Greptile is restored.

### Blocked

- Provider selection and country approval require named owners and evidence the agent cannot create from public docs. This does not block completion of the public-document research record.

## Risks and handoff instructions

- **Risks/limitations:** Public docs and regulator listings can change and may omit account-specific terms or conditions. The BoU ioTec record is dated June 2023. Entity listings do not establish current product/API permission scope or MylesNet merchant eligibility. ClickPesa documentation explicitly warns that API requests are production requests; do not call its API or API Explorer as a test. Do not treat preview endpoints as a sandbox without written provider confirmation.
- **Next action:** Continue to keep all payment rails disabled pending owner evidence and approvals. When Greptile is restored, request review on PR #18's then-current head.
- **User approval required:** No product/provider integration or real payment action is authorized. Phase 1 remains behind the Phase 0 exit and explicit user authorization. Code review remains deferred until the user says it is restored.

## Delivery state

- **Commit/branch:** Initial evidence commit `12b08df`; additional regulator-evidence changes are on `codex/mn-plan-012-payment-provider-evidence` and will be recorded in the updated PR #18 head.
- **PR/review:** [PR #18](https://github.com/mylescorp/mylesnet/pull/18), draft/open; user-directed code review deferral remains active; no review requested.
- **Merge:** Not merged.
- **Deployment:** Not deployed; no deployment authorized.
