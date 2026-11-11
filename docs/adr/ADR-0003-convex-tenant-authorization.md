# ADR-0003: Convex tenant/branch authorization and realtime revocation

- **Date:** 11 November 2026
- **State:** Accepted shared-deployment and server-authorization boundaries; detailed contracts remain open
- **Owner:** Application security/Convex owner not named; assign in MN-PLAN-008
- **Evidence:** `AGENTS.md`; `TECH-STACK.md`; `TECHNICAL-SPECIFICATION.md` §§3–4, 8–9, 14.2–14.3; `DECISION-REGISTER.md` MN-DEC-004/016

## Context

Convex provides typed application operations and live subscriptions. V1 includes multi-tenant, multi-branch access with a shared deployment. A correct check must cover more than a database query: mutations, actions, jobs, files, exports, caches, search, and subscriptions can all disclose or change tenant data.

## Decision

- Use one shared Convex deployment for v1 with strict logical tenant and branch isolation. Convex owns SaaS application state and realtime projections; it is not on the RADIUS authentication/accounting request path.
- Every tenant-owned record, event, file/object, export, search document, cache key, and job payload carries a server-resolved tenant scope. Tenant ownership is immutable except through a separately audited migration.
- Derive active tenant, membership, branch scope, role, permission, tenant state, entitlement, and resource assignment from trusted server-side records. Caller-supplied tenant/branch values are input hints at most and never authorization.
- Recheck current authority for every query, mutation, action, export, job, and live subscription. Apply permission and resource-scope checks to subscription updates, not only initial subscription creation.
- On membership removal, revoke sessions/subscriptions and clear disallowed client data immediately. On role/scope changes, recompute authority and stop feeds that exceed the new scope.
- Tenant-wide versus branch-specific resource assignment must be explicit. `null` cannot ambiguously mean tenant-wide, unassigned, or invalid.
- Consider dedicated deployment/sharding only after a documented workload, legal, isolation, and cost trigger plus a tested migration path; it is not a v1 tenant choice.

## Alternatives considered

- **Dedicated Convex deployment per tenant in v1:** rejected by the approved shared-deployment direction.
- **Trust client tenant/branch identifiers or long-lived role claims:** rejected because membership and permission changes must take effect immediately.
- **Use the public hostname as an authorization scope:** rejected; hostname resolution and identity authorization are separate.

## Security and operational tradeoffs

Shared deployment reduces operational duplication but makes a missed predicate a cross-tenant risk. Authorization helpers and query indexes must be designed together; every path requires positive, negative, cross-tenant, cross-branch, lifecycle, and revocation evidence. Live subscriptions and caches increase revocation complexity. Dedicated isolation may later require migrations, cost, and data-residency changes.

## Interfaces and sequences

See `IC-DATA-01`, `IC-IAM-02`, `IC-APP-02`, and `SEQ-IAM-01` in [`INTERFACE-AND-SEQUENCE-CONTRACTS.md`](INTERFACE-AND-SEQUENCE-CONTRACTS.md).

## Open items and revisit trigger

MN-DEC-016 remains open for resource-to-branch assignment, shared resources, unassigned semantics, indexes, and migration. Define query/index budgets, authorization wrapper contracts, live-query revocation mechanics, and a measurable future isolation threshold. Revisit the shared model only with workload, legal, security, and migration evidence.
