# ISP signup and tenant-provisioning recovery decision packet

**Task:** MN-PLAN-026

**Plan phase:** Phase 0 — close planning and turn decisions into build contracts

**Status:** Decision support only; signup recovery policy and implementation details remain open.

## Purpose and boundary

This packet organizes the remaining failure, retry, recovery, and data-handling decisions for public ISP signup and initial tenant provisioning. It complements the existing signup sequence and identity contracts by making the unclosed cases explicit for the appointed owners.

It does not select policy values, change approved signup behavior, define a provider contract, authorize product implementation, create a WorkOS organization, provision a tenant, or establish legal retention approval. No live account or service was used.

## Approved direction to preserve

The following decisions are already recorded in the project charter, readiness decisions, technical specification, and identity ADR:

- Verify the first Owner's email before automatic provisioning. Phone verification may happen later. The 14-day trial starts after email verification and does not require an upfront payment method.
- A verified identity may own at most one ISP tenant. Block a duplicate tenant subdomain but allow duplicate business names. Do not collect government-issued business registration IDs in v1.
- Create or resume the internal tenant and its WorkOS organization idempotently, then establish the first Owner membership, default roles/settings, and trial state. WorkOS identity or organization membership alone does not grant application access.
- Retry transient provisioning failures within a bounded policy, preserve durable progress, show a pending status, and provide a support path after the threshold. Do not show the workspace as ready until provisioning and Owner access succeed.
- Keep the application authoritative for tenant status, membership, Owner role, and permissions. Signup and provisioning errors must not disclose whether another person's identity or tenant exists.

See [`CORE-FLOWS-AND-DATA.md`](CORE-FLOWS-AND-DATA.md#isp-signup-and-tenant-provisioning), [`TECHNICAL-SPECIFICATION.md`](TECHNICAL-SPECIFICATION.md#52-isp-registration-and-account-recovery), [`ADR-0002`](adr/ADR-0002-workos-identity-mapping.md), and `SEQ-SIGNUP-01` in [`INTERFACE-AND-SEQUENCE-CONTRACTS.md`](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md).

## Open owner decisions

No values below are approved. Appoint the named roles through the [Phase 0 owner appointment and sign-off register](PHASE-0-OWNER-APPOINTMENT-REGISTER.md); the role labels here do not assign an individual.

| ID | Decision to close | Dimensions/options to compare | Accountable roles | Acceptance evidence |
|---|---|---|---|---|
| SIGNUP-OPEN-01 | Verification challenge and abuse controls | Challenge expiry; resend and attempt limits; cooldown and recovery behavior; whether support can assist without bypassing verification; enumeration-safe responses. | Product, identity/security, communications | Approved state/error matrix and abuse analysis; provider-compatible verification and resend behavior documented before implementation. |
| SIGNUP-OPEN-02 | Data and subdomain handling before verification | Whether an unverified request is persisted; minimum fields retained; when a requested subdomain is checked or reserved; expiry and cleanup for abandoned requests; lawful retention basis. | Product, platform/operations, data/privacy, country counsel | Data-flow and reservation lifecycle with retention/deletion evidence and conflict/reuse behavior. |
| SIGNUP-OPEN-03 | Duplicate Owner identity recovery | Safe response for an identity that already owns a tenant; how a legitimate returning Owner signs in or recovers access; support path and identity proof; protection against account/tenant enumeration. | Product, identity/security, support | Approved non-enumerating response and recovery flow with authorization, audit, and abuse cases. |
| SIGNUP-OPEN-04 | Provisioning idempotency and reconciliation | Durable signup/job identity; checkpoints across WorkOS and internal records; ordering and uniqueness constraints; handling of duplicate/concurrent requests; recovery when one system succeeds and the other fails. | Identity/engineering, platform/operations, security | State-transition and reconciliation contract showing one tenant, one Owner, one trial, and repeatable resume after failure at every boundary. |
| SIGNUP-OPEN-05 | Retry budget and operator retry | Transient versus permanent failure classes; delay/backoff, maximum attempts/age, jitter, alert threshold, queue capacity, and the authority/audit needed for manual retry or quarantine resolution. | Engineering, platform/release operations, security | Bounded retry and exhausted-job policy tied to workload/provider evidence, with audited operator actions and no blind replay of permanent failures. |
| SIGNUP-OPEN-06 | Pending progress and support handoff | User-visible stages and safe messages; status refresh/resume; correlation ID; which verified channel receives notices; support context, response ownership, and escalation after exhaustion. | Product, support, platform/operations, communications | State-to-message and support handoff matrix that avoids exposing provider internals, secrets, or another tenant's information. |
| SIGNUP-OPEN-07 | Subdomain conflict, reservation, and release | Conflict response without tenant disclosure; reservation point and duration; release after abandoned/failed signup; rules for never reusing an active or renamed tenant alias. | Product, platform/operations, security, legal/data privacy | Unique-host lifecycle and cleanup contract consistent with the approved permanent tenant-owned redirect-alias rule. |
| SIGNUP-OPEN-08 | Lifecycle boundary and completion criteria | Mapping of `pending_verification`, `provisioning`, `onboarding`, and `trial`; which owner actions are required before readiness; relationship between trial start, tenant status, and usable Owner access. | Product, identity/engineering, finance, support | State-transition table preserving the approved verification-based trial start and defining exactly when access can be described as ready. |

## Recovery scenarios required for acceptance

Before Phase 0 signup contracts are accepted for implementation, the owners should define expected outcomes and evidence for each scenario:

1. Verification email is delayed, expired, resent repeatedly, or submitted with an invalid/used challenge; the response is bounded and does not reveal account existence.
2. Two requests for the same verified Owner or subdomain race; at most one tenant and one Owner are created, with safe recovery guidance for the other request.
3. A retry follows each provisioning boundary: after WorkOS organization creation, after internal tenant creation, after membership/default setup, and before Owner access is confirmed. Resume must not duplicate organizations, tenants, memberships, trial periods, or reserved hosts; each retry attempt remains separately auditable.
4. WorkOS or the application store is unavailable or times out; the UI remains pending, durable checkpoints are reconciled, and no partial workspace is shown as ready.
5. The retry budget is exhausted; the user receives a safe support path with a correlation reference, and an authorized operator can inspect/retry or resolve the job with an audit trail.
6. Signup is abandoned before verification or remains partially provisioned; cleanup and retention follow the approved legal/data policy without prematurely releasing a host that could route to another tenant.
7. Provisioning completes but Owner authorization or session establishment fails; access is not reported as ready until the application can confirm current Owner membership and tenant state.
8. Duplicate business names are accepted when the verified Owner and subdomain rules allow them; no government registration identifier is requested.

These are planning acceptance scenarios, not tests run in this task. The final verification plan should add retry, concurrency, outage, authorization, privacy, and recovery evidence to the relevant signup/job contracts before implementation begins.

## Contract flow to reconcile

The existing `SEQ-SIGNUP-01` is the source sequence: verify email, request provisioning with an idempotency key, create or resume the WorkOS organization, create or resume the internal tenant/Owner/trial/defaults, persist retryable progress and support correlation on partial failure, and report ready only after Owner access succeeds.

The owners still need to specify durable state transitions, idempotency-key scope and retention, cross-system reconciliation, retry and cancellation behavior, safe errors, operator permissions, audit fields, and applicable retention. Do not infer atomicity across WorkOS and the application database or treat a browser success redirect as proof of provisioning.

## Completion criteria and next steps

- Named owners accept each decision in the appointment register and record evidence against `SIGNUP-OPEN-01`–`08`.
- The approved state model and recovery behavior are reconciled into the signup flow, `EVT-003`, the signup sequence, and the implementation runbook without contradicting the approved identity, billing, or tenant-lifecycle rules.
- Product and security owners approve safe duplicate handling, authorization, abuse controls, and user-facing recovery; data/privacy and country counsel approve pre-verification data and cleanup/retention handling.
- WorkOS compatibility, provider behavior, and failure-injection evidence are gathered only in an authorized non-production environment when implementation/testing is authorized.
- Update MN-DEC-025 and the Phase 0 exit review only when the appointed owners have supplied the required decisions and evidence. This packet itself closes no Phase 0 gate.

## Related artifacts

- [Phase 0 implementation plan](IMPLEMENTATION-PLAN.md#phase-0--close-planning-and-turn-decisions-into-build-contracts)
- [Implementation status](IMPLEMENTATION-STATUS.md)
- [Decision register](DECISION-REGISTER.md)
- [Readiness decisions](READINESS-DECISIONS.md)
- [Owner appointment and sign-off register](PHASE-0-OWNER-APPOINTMENT-REGISTER.md)
- [API/error/event contract backlog](API-ERROR-EVENT-CONTRACT-BACKLOG.md), especially `EVT-003` and `RECON-003`
- [WorkOS identity ADR](adr/ADR-0002-workos-identity-mapping.md)
- [Sequence and interface contracts](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md)
