# High-Risk Permission and Approval Worksheet

- **Task:** MN-PLAN-031 — prepare owner decisions for high-risk permissions and approval rules
- **Phase:** 0 — Close planning and turn decisions into build contracts
- **Status:** Planning aid; no permission thresholds or new approval policies are approved here.
- **Related requirements:** `READINESS-DECISIONS.md` §Tenant lifecycle, users, and data boundaries; `ROUTES-AND-ACCESS.md` §Authorization rules to decide and enforce; `TECHNICAL-SPECIFICATION.md` §6; route/action traceability catalog.

## Purpose and boundary

The canonical documents define role names, server-side authorization principles, support-access limits, and several high-risk controls. They still leave the complete action-level permission matrix, approval thresholds, and named owners open. This worksheet groups those decisions so accountable owners can complete and sign them before Phase 3 implementation.

This is a non-decisional worksheet. It does not grant any role a permission, select a threshold, appoint an owner, approve an exception, or authorize implementation, provider access, support access, export, payment, or router activity. Preserve all values below as open until an accountable owner records evidence and the decision is approved in the project decision process.

## Approved baselines to preserve

These controls are already recorded in canonical project documents and are not reopened by this worksheet:

- Authorization is based on explicit permissions and trusted server-side tenant and branch scope, not role-name checks or client-supplied IDs.
- Tenant Owner control of high-risk grants, the Admin permission ceiling, and the prohibition on self-escalation are approved. Tenant-wide and branch-scoped assignments remain distinct.
- Platform support has no routine tenant-data access. A standard support grant is Owner-approved, read-only, scoped, visible, audited, and expires within one hour. Emergency access requires two distinct platform approvers, is read-only, expires, and notifies the tenant; no approver may approve their own request.
- Sensitive actions require the documented MFA step-up where specified. Stored secret values are never returned or revealed; credential operations use controlled secret-manager flows.
- Financial and audit history is append-only; corrections use linked, reasoned reversal or adjustment events.

See [ROUTES-AND-ACCESS.md](ROUTES-AND-ACCESS.md), [TECHNICAL-SPECIFICATION.md](TECHNICAL-SPECIFICATION.md) §§6 and 9, [ADR-0003](adr/ADR-0003-convex-tenant-authorization.md), and the [support access sequence](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md#sequence-registry) for the authoritative details.

## Action families requiring owner decisions

For each row, record the exact permission action(s), requesting/granting/approving/executing roles, tenant/branch/platform scope, independent-approval rule, applicable threshold, MFA requirement, audit evidence, expiry/revocation behavior, and accountable owner. The existing baseline column summarizes only already documented controls; it does not fill unresolved policy fields.

| Action family | Existing baseline | Decisions still required before implementation |
|---|---|---|
| Refunds, manual credits, and payment adjustments | Finance permission; Owner-approved grant for refund/credit authority; immutable linked correction and reason. | Amount/count/time-window thresholds; whether each transaction needs approval; requester/approver/executor separation; treatment of reversals, failed or duplicate requests; branch/entity scope. |
| Withdrawals and payout-destination changes | Owner-approved, branch-scoped withdrawal permission; each withdrawal uses the documented Owner verification; only the Owner approves destination additions/changes. | Amount and velocity thresholds; whether a second approver is needed; destination cooling-off and change-notification evidence; emergency and recovery handling; exact executor roles. |
| Sensitive exports | Owner authorization and explicit tenant `export` access; scope, purpose, MFA, safe expiry, and audit; support grants cannot authorize tenant-data exports. | Sensitive-data classes and field masks; record/time/size thresholds; approval per export versus standing grant; expiry/download limits; treatment of scheduled, API, and offboarding exports. |
| Credential and secret operations | No secret read-back; controlled rotation/write flow; Owner-approved permission, MFA step-up, and audit. | Which credential operations are allowed to which role; dual approval for provider or network credentials; rotation/revocation timing; failure recovery and evidence without exposing values. |
| Role assignment, permission changes, and Owner recovery | Admin operates within their own ceiling; Owner controls high-risk grants; no self-escalation; Owner recovery has independent security approval. | Which actions require per-change Owner approval; delegation depth; grant expiry/review; approver independence for each recovery path; notification, cooldown, and fallback evidence. |
| Bulk messaging and subscriber compensation | Owner-approved bulk-messaging permission; preview, audience/cost checks, consent, approval, limits, delivery status, and cancellation where possible. | Recipient/count/cost thresholds; campaign versus compensation permissions; approval frequency; rate limits; pause/cancel cutoff and partial-delivery handling. |
| Router and network commands | Network permission; Owner-approved grants for destructive/bulk actions; target/impact preview, reason, tenant/NAS verification, queued audit/result. | Command allowlist by role; per-router versus multi-router thresholds; second approval for network-wide impact; expiry/replay protection; timeout, rollback, and safe failure behavior. |
| Tenant suspension, reactivation, and offboarding | Platform role, reason, impact preview, approval, export/retention steps, and recoverable grace policy. | Separate requester/approver/executor roles; trigger and scope thresholds; emergency suspension path; recovery window and notification evidence; exact lifecycle permissions. |
| Standard and emergency support access | The approved one-hour, read-only standard grant and two-person emergency boundary above. | Exact tenant/resource selectors, permitted fields and internal notes, case/reason requirements, approver roster, extension/revocation handling, alert and review evidence, and break-glass audit ownership. |

## Decision record to complete for each action

Duplicate this record per permission action or coherent action family. Use separate records where scope, data sensitivity, or approval threshold differs.

| Field | Owner decision / evidence |
|---|---|
| Stable requirement and permission action IDs | Open |
| Routes, jobs, APIs, exports, and callbacks covered | Open |
| Requester, grantor, approver, and executor roles | Open |
| Tenant, branch, legal-entity, or platform scope | Open |
| Lifecycle and entitlement conditions | Open |
| Approval rule and independent-approver requirement | Open |
| Numeric/impact threshold and its source or rationale | Open |
| MFA or reauthentication requirement | Open |
| Required reason, case, provider, or target evidence | Open |
| Audit fields, retention reference, and alert owner | Open |
| Approval expiry, revocation, retries, and duplicate handling | Open |
| Denied, partial-failure, recovery, and safe-stop behavior | Open |
| Accountable owner and independent security/finance/network review | Open |
| Evidence link, decision date, and approval status | Open |

Do not treat an empty field as permission to choose a default. Record “not applicable” with a reason where a control does not apply. If the policy is intentionally deferred, name the phase, owner, and enforcement-safe disabled behavior.

## Sign-off and implementation acceptance

The [Phase 0 owner appointment register](PHASE-0-OWNER-APPOINTMENT-REGISTER.md) tracks who must be appointed; this worksheet does not assign people. Required reviewers depend on the action and should include the accountable product/platform owner plus the relevant security, finance, network, support, and privacy/country owner. Record actual names and independent sign-offs only after appointment.

Before Phase 3 begins, confirm that:

- Every in-scope route, action, job, export, and callback maps to a stable permission ID, actor, scope, lifecycle/entitlement state, data class, audit event, and acceptance case in [REQUIREMENT-ROUTE-TRACEABILITY.md](REQUIREMENT-ROUTE-TRACEABILITY.md).
- Every high-risk action above has a completed decision record, an accountable owner, evidence for any numeric threshold, and the required independent approvals.
- Tenant and platform permission matrices agree with these records and with the approved support boundary. No role can exceed its permission ceiling or elevate itself.
- Denial, stale membership, changed scope, approval expiry, replay/duplicate, partial failure, and revocation cases have server-side acceptance coverage across direct API/job/export paths as well as the UI.
- The approved decisions are entered in [READINESS-DECISIONS.md](READINESS-DECISIONS.md) and [DECISION-REGISTER.md](DECISION-REGISTER.md) where applicable, then propagated to the route catalog, technical specification, ADR/contracts, and implementation plan.

Until those conditions are evidenced, this permission/approval gate remains open. Phase 0 completion and explicit user authorization remain necessary before product implementation.
