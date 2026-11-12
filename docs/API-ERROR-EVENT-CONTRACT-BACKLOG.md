# API, error, event, and idempotency contract backlog

**Task:** MN-PLAN-006 · **Plan phase:** Phase 0 — close planning and turn decisions into build contracts<br>
**Status:** Contract backlog; exact schemas and implementations remain open.<br>
**Authority:** `TECHNICAL-SPECIFICATION.md` §§8–9, 14.3–14.5; ADR-0003/0005/0006/0011; [`INTERFACE-AND-SEQUENCE-CONTRACTS.md`](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md).

## Boundary and contract shape

Convex functions are the primary typed application contract. Next.js Server Actions/Route Handlers, WorkOS callbacks, provider webhooks, RADIUS relays, router workers, storage, and messaging adapters are untrusted or separately trusted boundaries; they must not become alternate authorization systems. The list below establishes required fields and behaviors, not final TypeScript/JSON schemas. MN-PLAN-006 must keep unknown numeric bounds, error details, field names, and owners open until approved.

Every operation backlog item records:

1. Stable operation/event ID, producer, consumer, and contract version.
2. Identity source and server-resolved tenant/branch/resource context; client-supplied tenant IDs are never authority.
3. Actor, permission, lifecycle/entitlement, MFA step-up, support-grant and field-scope requirements.
4. Runtime input/output validation, nullable/optional semantics, payload/page/date bounds, and safe response minimization.
5. Idempotency key scope, concurrency/transaction rule, immutable side effects, duplicate behavior, and retryability.
6. Error family, safe user text, correlation ID, provider/internal detail redaction, and whether the caller may retry.
7. Durable event, audit fields, authorized projections, trace/metric source, retention category, and operational owner.
8. Positive, negative, cross-tenant/branch, boundary, duplicate/out-of-order, outage, recovery, and acceptance evidence.

## Standard request and response expectations

- Validate authentication/session, current tenant mapping, status, permission, branch/resource scope, entitlement, origin/CSRF, and idempotency at the operation boundary. Public endpoints have an explicit public capability and abuse limit.
- Use tenant-first indexed access and bounded pagination, report windows, upload size, batch size, concurrency, and job output. Numeric maxima and rate thresholds remain workload decisions.
- Use opaque cursors tied to tenant, filter/sort, and version. Allowlist filters/sorts. Do not disclose whether another tenant owns a requested identifier.
- A mutation states its expected current status, transaction/concurrency rule, idempotency boundary, append-only/audit effect, and compensation path.
- Return a stable machine-readable error family, safe user message, retryable flag, field validation details only when safe, and correlation ID. Never return secrets, provider raw responses, stack traces, or unnecessary PII.
- Async jobs expose accepted/pending/running/succeeded/failed/quarantined/expired states as appropriate, progress timestamps and correlation IDs; the server rechecks authorization and entitlement when the job executes and when output is read.
- Event delivery updates only authorized subscriptions. Show last-updated/source time; stale data is explicitly stale.

## Error family contract

The following families are already proposed by the technical specification. Keep their semantics stable and add new codes through a reviewed contract change.

| Code | Meaning | Retry guidance | Disclosure and audit rule |
|---|---|---|---|
| `AUTH_REQUIRED` | Missing, invalid, or expired authenticated context | Reauthenticate; no automatic mutation retry | Do not reveal whether another identity exists. Record security-relevant denial without credentials. |
| `PERMISSION_DENIED` | Current actor lacks the named action | No retry until permission/context changes | Do not return hidden field/resource data; sensitive denials may be audited. |
| `TENANT_UNAVAILABLE` | Tenant state does not permit this operation | Retry only after documented recovery/reactivation | Return safe state guidance; do not expose restricted tenant records. |
| `TENANT_SCOPE_MISMATCH` | Supplied resource is outside verified tenant/branch scope | Do not retry with guessed IDs | Use a non-enumerating response; log safe scope mismatch metadata. |
| `ENTITLEMENT_REQUIRED` | Server-resolved capability is unavailable | Retry only after approved plan/exception change | UI is informational only; recheck at execution time. |
| `QUOTA_EXCEEDED` | New addition would exceed server-owned quota | No retry until capacity changes | Preserve existing services; include safe allowed alternative. |
| `VALIDATION_FAILED` | Input schema or domain invariant failed | Correct input; no blind retry | Return field-level safe details, no stack/provider internals. |
| `CONFLICT` | Current record state prevents requested transition | Refresh/read current authorized state | Include stable conflict code and correlation ID. |
| `RATE_LIMITED` | User/tenant/provider/workload rate limit reached | Retry only with bounded guidance/backoff | Expose safe retry hint; do not silently drop queued work. |
| `IDEMPOTENCY_CONFLICT` | Same key was reused for a materially different request | Do not retry with same changed body | Preserve original operation link; audit conflict. |
| `PROVIDER_UNAVAILABLE` | External provider cannot currently complete operation | Safe idempotent retry or visible pending/exception state | No false paid/activated status; redact provider response/secrets. |
| `RESOURCE_STALE` | Source version/timestamp is stale or changed concurrently | Refresh before retry | Show freshness; do not overwrite authoritative state. |
| `INTERNAL_ERROR` | Unexpected server failure | Retry only when operation contract says safe | Generic text and correlation ID; no stack, query, secret, or PII disclosure. |

The error envelope's exact field names and HTTP/Convex status mapping are open until one schema is reviewed across all adapters. Do not create endpoint-specific synonyms for these conditions.

## Event envelope and producer authority

Use the technical specification's representative envelope as the starting contract. `tenantRef` is a server-resolved association, never trusted from an unsigned payload. Payload schemas remain event-specific and versioned.

| Event family | Authoritative producer/evidence | Application responsibility | Idempotency and reconciliation boundary |
|---|---|---|---|
| Staff identity/MFA/org membership | WorkOS | Resolve verified identity/org to internal user/tenant; enforce current application state and permissions. | Provider event reference plus current-state read; revocation must not wait for stale client claims. |
| Signup/provisioning | Verified Owner identity plus internal provisioning workflow | Idempotently create/resume WorkOS organization, tenant, Owner membership, defaults, trial and progress. | Signup/provisioning key per verified Owner and request; recover partial cross-system creation without duplicate tenant/Owner. |
| Payment settled/failed/reversed | Signed provider callback plus reconciliation | Durably store raw event; resolve merchant/tenant/intent; validate amount/currency; append at most one financial effect and activation. | Provider account + immutable event/transaction reference; quarantine unknown/mismatched, late, duplicate, and out-of-order events. |
| Tenant ledger adjustment/refund | Provider evidence or authorized manual workflow | Append linked reversal/credit/debit with reason, actor, approver and source reference. | One idempotency key per requested adjustment; refund retry is a new request when the original failed record is immutable. |
| RADIUS auth/accounting | FreeRADIUS and dedicated PostgreSQL AAA store; NAS mapping trusted server-side | Persist packet/event and outbox; project idempotently to Convex; keep RADIUS off Convex request path. | Tenant/NAS/session/event identity; account for duplicate/interim/stop-before-start/out-of-order cases and reconcile without erasing packet history. |
| Router job/health | Isolated worker or approved router report | Verify allowlist, scope, approval and credential reference; record request/result/timeouts; never expose router secret. | Job ID plus device/command version; no blind retry of non-idempotent command. Exact command catalog open. |
| Message delivery/consent | Messaging provider callback plus tenant consent source | Store consent purpose/source/time and normalized delivery state; preserve STOP/suppression. | Provider message ID plus tenant; dedupe callbacks and distinguish promotional from required service notices. |
| Subscription, entitlement, usage | Platform billing provider and application subscription workflow | Keep platform ledger separate; compute entitlement server-side; schedule effective transition and preserve existing records. | Invoice/plan version/effective boundary; reconcile provider payment before upgrade effect. |
| Domain/TLS/provisioning status | Verified DNS and TLS/provider evidence | Activate only verified unique tenant host; retain migration state and old callback host as approved. | Host + verification attempt/certificate state; stale/ambiguous mapping stays inactive. |
| Support grant and role/access change | Approved tenant Owner or distinct emergency approvers; application audit service | Enforce current scope/expiry/MFA and immediately close subscriptions when authority changes. | Grant/event ID and approval set; revocation and every access record remain auditable. |

## Idempotency, ordering, retry, and reconciliation policy

- Create an idempotency key at each external callback and user-command boundary that can be retried. Scope keys by the trusted source/account and operation; do not let one tenant collide with another. Exact format, retention, and conflict behavior need a schema decision.
- Persist the source reference, normalized event ID, key, schema version, received/occurred times, processing state, attempt history, result, and correlation/causation references. Keep sensitive raw fields in a protected inbox only when necessary; ordinary logs use a redacted form.
- Acknowledge an external callback only after durable acceptance. Work exceeding callback limits proceeds asynchronously. The raw callback record and any financial/domain effect are separate; invalid signature is rejected, unknown mapping/amount/currency mismatch is quarantined with no financial effect.
- Retry only retryable failures, within a bounded per-provider/job budget. Use the approved bounded exponential-retry direction where specified; base delay, maximum attempts, total age, jitter, and service-specific queue capacity remain open until load/provider evidence is available. Permanent validation, authorization, signature, or schema errors do not retry automatically.
- Quarantine/dead-letter exhausted, unknown-version, ambiguous-tenant, or conflicting events with reason, owner, timestamp, source reference, and safe replay controls. Replay retains original source ID and records actor/reason; it cannot bypass current permission or idempotency.
- Consumers tolerate duplicate and out-of-order delivery where the source allows it. Define ordering keys only for domains that require order. Preserve immutable event history; materialized projections can be recomputed from valid authoritative events.
- Reconciliation compares provider/router source state with stored state and records a discrepancy plus resolution; it does not silently overwrite ledger or audit facts. Establish scheduled intervals, exception ownership, thresholds, and customer/operator paths per provider/country.
- Numeric latency targets in `READINESS-DECISIONS.md` are planning targets. Source timestamps, numerator/denominator, exclusions, measurement windows, alert thresholds, and acceptance evidence remain open under ADR-0011.

## Contract backlog

| ID | Required contract artifact | Primary requirement/flow | Current state / owner |
|---|---|---|---|
| `API-001` | Shared operation envelope: identity, tenant/branch scope, permissions, entitlement, validation, bounds, error, audit and correlation | All `REQ-*`; ADR-0003 | Requirements captured; schema and application owner open. |
| `API-002` | Stable error catalog and adapter mapping | All authenticated/public boundaries | Families listed above; exact envelope/HTTP mapping and owner open. |
| `EVT-001` | Provider callback inbox, signature/replay, tenant mapping, versioning, durable acknowledgement and quarantine | `J-CAP-01`, `J-SUB-02`, `REQ-PLAT-03`; ADR-0006 | Rules captured; per-provider contract, secret rotation and payment owner open. |
| `EVT-002` | RADIUS accounting/outbox, NAS mapping, dedupe, retry, recovery drain and projection | `J-NET-01`; ADR-0005 | Rules captured; PostgreSQL/queue limits and network owner open. |
| `EVT-003` | Signup/provisioning and membership lifecycle idempotency | `REQ-SIGNUP-01`, `J-IAM-01/02`; ADR-0002 | Sequence captured; WorkOS event and recovery details open. |
| `EVT-004` | Router job and worker result/retry/rotation contract | `J-NET-02`; ADR-0005 | Boundary captured; command allowlist, transport and lab evidence open. |
| `EVT-005` | Messaging consent, delivery callback, suppression and campaign cost boundary | `REQ-TEN-08`, `REQ-SUB-03` | Provider open; schema/retention/country owner open. [Public provider evidence](SMS-PROVIDER-PUBLIC-EVIDENCE.md) shows that accepted/send statuses differ from handset delivery and that callback retry/recovery behavior needs provider-specific contracts. |
| `EVT-006` | Internal audit/causation/correlation envelope | Sensitive actions, `REQ-PLAT-07`, `REQ-TEN-13` | Append-only requirements captured; field schema, class, retention and security owner open. |
| `JOB-001` | Background job identity, tenant scope, actor/authority recheck, checkpoint, retry and cancellation | Imports, reports, router/domain, exports, billing | Rule captured; concurrency, quotas, queue service and operations owner open. |
| `RECON-001` | Payment provider ↔ payment intent ↔ immutable ledger reconciliation | `REC-009`–`REC-011` | Required; provider-specific interval, exception queue and finance owner open. |
| `RECON-002` | RADIUS source/outbox ↔ Convex session projection reconciliation | `REC-007`–`REC-008` | Required; event freshness, queue drain and network owner open. |
| `RECON-003` | WorkOS identity/org ↔ internal membership/tenant status reconciliation | `REC-001`–`REC-003` | Required; provider event strategy and identity owner open. |
| `FX-001` | Immutable rate observation and invoice/report conversion provenance | `REC-020`; `MN-DEC-015/022`; [`FX-CONVERSION-PROVENANCE-CONTRACT.md`](FX-CONVERSION-PROVENANCE-CONTRACT.md) | Logical provenance is captured; source/quote semantics, freshness/fallback, precision/rounding, snapshot persistence, retention and Finance owner remain open. |
| `EXPORT-001` | Scoped async export and reauthorized expiring download | `AC-X-01/02/03`, `IC-EXPORT-01` | Owner approval/MFA/audit boundary captured; storage, manifest and retention details open. |

For the tenant CSV import lifecycle under `JOB-001`, see the [guided CSV migration contract](CSV-MIGRATION-CONTRACT.md). It details authority rechecks, preview/commit sequencing, idempotent row processing, rollback boundaries, and unresolved job policy choices.

## Related artifacts

- [Data ownership and classification](DATA-OWNERSHIP-AND-CLASSIFICATION.md)
- [Threat model](THREAT-MODEL.md)
- [Synthetic-data rules](SYNTHETIC-DATA-AND-TEST-ENVIRONMENTS.md)
- [Requirement/data/event crosswalk](REQUIREMENT-ROUTE-TRACEABILITY.md#cross-cutting-data-event-and-threat-contracts)
- [ADR/interface/sequence contracts](adr/README.md)
