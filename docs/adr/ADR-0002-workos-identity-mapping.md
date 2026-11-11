# ADR-0002: WorkOS identity and organization mapping

- **Date:** 11 November 2026
- **State:** Accepted identity and authority boundary; compatibility details remain open
- **Owner:** Identity/security owner not named; assign in MN-PLAN-008
- **Evidence:** `AGENTS.md`; `READINESS-DECISIONS.md`; `TECH-STACK.md`; `TECHNICAL-SPECIFICATION.md` §§3–4, 6, 9, 14.2/14.6; `DECISION-REGISTER.md` MN-DEC-005

## Context

Staff authentication and tenant authorization are separate concerns. WorkOS organizations represent tenant workspaces, while branch membership, built-in/custom roles, permissions, and current tenant status are application data. Staff identities may join multiple tenants, but one verified identity may own at most one tenant. Subscriber identity remains separate.

## Decision

- Use WorkOS AuthKit for staff authentication, invitations, MFA capabilities, and organization identity. Map one WorkOS organization to one internal `tenantId`.
- The application remains authoritative for tenant lifecycle, active membership, branch scope, roles, permission ceilings, and resource access. WorkOS organization membership alone never authorizes an operation.
- Resolve a verified staff identity and organization on the server for each operation. Re-evaluate current application membership, tenant state, role, permission, and branch scope rather than relying on stale session claims.
- Tenant switching is explicit and clears data/subscriptions from the prior tenant context.
- Removing a member immediately invalidates active sessions and live subscriptions; role/scope changes immediately recompute access and remove data no longer permitted.
- Keep subscriber credentials and identities separate from staff identity by default. Enterprise SSO and SCIM are deferred beyond v1.
- Require MFA for platform administrators and step-up MFA for designated tenant high-risk actions. The exact WorkOS enrollment/challenge flow must pass the documented pre-build compatibility spike; recent authentication time alone is not proof of an MFA challenge.

## Alternatives considered

- **Use WorkOS organization roles as the complete authorization model:** rejected because it cannot express the approved branch/resource and application-specific permission rules.
- **Map a WorkOS organization to each branch:** rejected; the approved mapping is one organization per tenant, with branches owned by the application.
- **Build a separate staff identity provider or merge staff/subscriber identities:** not selected; neither is approved for v1.

## Security and operational tradeoffs

External identity simplifies authentication and organization membership but creates a synchronization boundary. A delayed provider event must not preserve application access: server checks use current application state, and revocation is applied immediately to sessions and subscriptions. Recovery, MFA API compatibility, provider outage behavior, and account-linking threats need explicit acceptance evidence.

## Interfaces and sequences

See `IC-IAM-01`, `IC-IAM-02`, `SEQ-IAM-01`, `SEQ-SIGNUP-01`, and `SEQ-IAM-02` in [`INTERFACE-AND-SEQUENCE-CONTRACTS.md`](INTERFACE-AND-SEQUENCE-CONTRACTS.md).

## Open items and revisit trigger

Run the WorkOS MFA compatibility spike for tenant/platform separation, optional staff enrollment, independently verified step-up, recovery, and failure behavior. Define provider event/reconciliation handling and session topology. Revisit only if WorkOS cannot meet an approved identity/security requirement or the v1 SSO/SCIM scope changes.
