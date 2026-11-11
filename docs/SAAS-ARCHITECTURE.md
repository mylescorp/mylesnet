# SaaS architecture plan and boundaries

**Status: planning and documentation only.** The sections below describe a proposed future system. They are not evidence that the panels, tenant isolation, integrations, or deployment exist today. Do not start implementation until the open product, security, integration, and operations decisions are documented and agreed.

## Panels

### Platform super-admin

- Platform health and tenant counts
- Tenant onboarding, domain status, subscription status, limits, and suspension
- Platform operator accounts, roles, MFA status, and audit events
- Support access requests with reason, expiry, and immutable audit records
- Provider/service health views without exposing tenant secrets

### ISP tenant console

- Tenant overview, subscribers, plans, sessions, routers, invoices, payments, vouchers, support, analytics, staff, and settings
- Tenant-specific branding, timezone, currency, domain, portal template, and provider configuration
- Role-scoped access for tenant owner/admin, finance, support, sales, and network staff

### Subscriber surfaces

- Customer account/renewal portal
- Router-hosted captive portal template
- No operator controls exposed to subscriber identities

## Build sequence

1. Establish the Next.js, Convex, and WorkOS development foundation with synthetic identities and tenant data.
2. Decide tenant data isolation; implement and verify organization-to-tenant mapping and Convex authorization boundaries.
3. Add platform super-admin authentication/roles and tenant console authentication/roles.
4. Build reference-matched shells, dashboards, settings, and seeded synthetic data with live Convex updates.
5. Add core ISP records, append-only audit/ledger semantics, and payment webhook idempotency.
6. Prove the RADIUS data plane, policy synchronization, accounting-event durability, and active-session projections on a test network.
7. Add RouterOS provisioning jobs, captive portal asset sync, notifications, and provider adapters.
8. Add observability, backup/restore drills, production hardening, and release automation.

## Security rules

- Every tenant query and queued job must be scoped to an authenticated tenant context.
- Never accept a tenant ID from the browser as authorization. Derive active organization from the verified WorkOS token, map it server-side to a tenant, and enforce tenant membership and permissions in each Convex function.
- Super-admin access to tenant operational data is off by default and audited when enabled.
- Never put router passwords, provider secrets, session keys, or payment secrets in source control, logs, or screenshots.
- Keep payment receipts and accounting events immutable; use reversals for corrections.
- Validate RADIUS NAS identity and shared secret before resolving a tenant; rate-limit malformed requests and keep accounting/auth paths observable.
- Keep captive portal templates as a separate deployable artifact that can be rolled back per router.

## Current stage

The repository contains a setup-only Next.js/Turborepo shell and planning documents. Convex is the selected reactive application backend because live updates are required. No product panel, tenant provisioning, real service integration, or production deployment is in progress. The remaining launch gates and provider/operations validations are tracked in `READINESS-DECISIONS.md` and the technical specification. Do not connect local setup to production services.
