# Task Handoff: Transactional email provider public evidence

- **Task ID:** `MN-PLAN-013`
- **Date:** `2026-11-12`
- **Agent/owner:** `Coordinator`
- **Plan phase:** `Phase 0 — close planning and turn decisions into build contracts`
- **Status:** `In progress`
- **Related requirements:** `IMPLEMENTATION-PLAN.md` Phase 0 work items 5–6; `DECISION-REGISTER.md` MN-DEC-014; `COUNTRY-AND-OPERATIONS-PACKS.md` CP-COMMS-01; `TECHNICAL-SPECIFICATION.md` §§3, 9, 12
- **Dependencies:** MN-PLAN-007 country and operations packs. This task records public documentation, but does not satisfy legal, contractual, country, delivery, or production approval.

## Objective

Record public technical evidence for the approved separation between WorkOS identity email and MylesNet application email, and for the conditional Resend and SES Ireland candidates. Keep the provider choice and all launch gates open. No provider account, integration, configuration, message send, or production change is in scope.

## Work completed

- Added a dated provider evidence report covering WorkOS default sender behavior, Resend data location/retention/deletion/pricing, and Amazon SES Ireland regional resources/events/pricing.
- Distinguished provider-published facts from MylesNet implications, unknowns, and the country-specific review and delivery evidence still needed.
- Updated the technical specification, technology stack, readiness decisions, decision register, country operations pack, Phase 0 plan, kickoff action, and repository task ledger with links to the evidence report.
- Preserved the approved direction: WorkOS default sender for identity flows; Resend as a conditional application-email candidate; SES Ireland as application-email fallback if review rejects Resend. No provider was selected or approved.

## Implementation or documentation details

- **Files changed:** `docs/EMAIL-PROVIDER-PUBLIC-EVIDENCE.md`; `docs/TECHNICAL-SPECIFICATION.md`; `docs/TECH-STACK.md`; `docs/READINESS-DECISIONS.md`; `docs/DECISION-REGISTER.md`; `docs/COUNTRY-AND-OPERATIONS-PACKS.md`; `docs/IMPLEMENTATION-PLAN.md`; `docs/EXECUTION-KICKOFF-PLAN.md`; `docs/IMPLEMENTATION-STATUS.md`; `docs/agent-handoffs/2026-11-12-MN-PLAN-013-email-provider-evidence-codex.md`.
- **Approach:** Reviewed official WorkOS, Resend, and AWS documentation and public pricing accessed 12 November 2026. The report labels provider claims and avoids treating a selected send region or a vendor compliance statement as a country approval.
- **Decisions made:** None. Existing identity/application email separation and provider candidate direction remain unchanged.
- **Decisions left open:** Country-specific transfer/contract approval; WorkOS data location, retention, and terms for identity mail; representative inbox delivery; exact quotes and total cost; provider account readiness; sender authentication; suppression, webhook, retry, and retention contracts; named communications/privacy owners.

## Verification and evidence

- **Checks run:** `git diff --cached --check` passed. Local Markdown-link and conflict-marker validation passed for all 10 documentation files, excluding one pre-existing private image target that is absent in this checkout. On PR #19 head `198a7c9`, Gitleaks, Vercel, and Vercel Preview Comments all passed. This handoff-only follow-up changes documentation on the same PR, so hosted checks will rerun on its new head.
- **Checks not run:** No tests or builds were run for this documentation-only task. No provider account, endpoint, configuration, or message send was used.
- **Evidence:** Official direct sources and access date are in `docs/EMAIL-PROVIDER-PUBLIC-EVIDENCE.md`.
- **REA evidence:** Not used. This task used public provider documentation and did not need runtime evidence.
- **Failures:** One pre-existing link target, `docs/reference-images/reference-domain-settings.png`, is absent in this checkout. It is outside this task and was not changed.

## Remaining work

### Done

- Record the public provider evidence and link it from the canonical architecture, readiness, country, and task-tracking documents.

### Pending

- Commit and push the dedicated task branch; open draft PR #19 stacked on PR #18. This is complete; verify the hosted checks again after this handoff update.
- Complete CP-COMMS-01 through appointed owners: obtain country-specific legal/contract review and written quotes; test representative identity/application messages in a controlled delivery pilot; validate security, suppression, webhook, retention, and sender controls.
- Keep the PR open and unmerged while the user's Greptile review deferral is active. Request review only after the user says Greptile is restored.

### Blocked

- Provider selection and country approvals require named owners and external evidence. Those gaps do not prevent recording public documentation.

## Risks and handoff instructions

- **Risks/limitations:** Public docs and list prices can change and do not establish local inbox delivery, legal compliance, provider eligibility, account state, or contractual controls. Resend documents US storage even when an Ireland send region is chosen. SES Ireland is only a regional endpoint/resource option, not proof that every related data path is region-contained.
- **Next action:** Verify the CI rerun after this handoff update, keep PR #19 open and unmerged during the Greptile deferral, and continue to independent planning tasks only after this task's required PR state is clear.
- **User approval required:** Product implementation, live provider configuration, production email, and production deployment remain unauthorized. Phase 1 remains behind Phase 0 exit and explicit authorization.

## Delivery state

- **Commit/branch:** Evidence commit `4e782ff` and PR/status follow-up `198a7c9` on `codex/mn-plan-013-email-provider-evidence`, based on PR #18's then-current head.
- **PR/review:** [PR #19](https://github.com/mylescorp/mylesnet/pull/19), open as a draft and stacked on PR #18. The last verified CI run passed on head `198a7c9`; current-head checks must be verified after this handoff update. Greptile review remains deferred by the user and is not requested.
- **Merge:** Not merged.
- **Deployment:** Not deployed; no deployment authorized.
