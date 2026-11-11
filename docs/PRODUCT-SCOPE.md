# Product scope proposal

**Status: planning only.** This document turns the saved reference platform review into an initial workflow/product outline for an independent ISP operations SaaS, using the user-approved MylesNet Design System v1.0 package as the product design and token source. It is not a claim that these features exist in this project. “Observed” means visible in the reviewed product UI or captured public site; “proposed” means a candidate requirement for the new platform.

## Product goal

Give internet service providers one tenant workspace to manage subscribers, internet plans, network access, payments, billing, vouchers, communications, support, and reporting. Give the SaaS operator a separate control plane to onboard and govern tenants without sharing tenant operational data by default.

## Product surfaces

| Surface | Intended users | Scope |
|---|---|---|
| Public website | ISP prospects and customers | Marketing pages, feature/pricing information, docs, signup and recovery. Reference coverage includes the 308 public sitemap URLs in the saved route manifest. |
| Platform control plane | SaaS owner and platform operators | Tenant lifecycle, SaaS plans/subscriptions, platform health, provider status, support access, platform audit. This panel was not observed in reference platform; it is a new requirement. |
| Tenant console | ISP owner and staff | Subscriber, network, finance, voucher, support, outreach, analytics, and tenant settings. The route inventory is derived from the signed-in reference review. |
| Subscriber portal | ISP customers | Account status, plan purchase/renewal, invoices or receipts, payment status, support entry points, and service instructions. |
| Captive portal | Guests and hotspot subscribers | Tenant-branded plan selection, voucher/login, payment handoff, terms/privacy, and post-payment connection result. |
| Router/RADIUS data plane | MikroTik NAS devices and subscribers | Authenticate and account for PPPoE/Hotspot sessions; enforce plan attributes and expiry; report session state to the tenant console. |

## Tenant-console capability groups

- **Overview and customer operations:** dashboard, subscribers, leads, tickets, imports/exports, customer compensation/outage credits.
- **Network:** active sessions, internet packages, NAS/router inventory, network map, CPE and firmware, equipment, fiber map, PPPoE and Hotspot settings.
- **Finance:** payments, invoices, expenses, loyalty points, payment gateways, accounting integration, reconciliation, and finance exports.
- **Vouchers and sales:** voucher batches/templates, voucher sales, agents, commissions, package assignments, and redemption history.
- **Communications and growth:** SMS, WhatsApp, email, message templates, campaigns, notification settings, and referral program.
- **Insights and governance:** analytics, audits, staff and roles, subscription, changelog, and feedback board.
- **Tenant configuration:** the v1.0 tenant theme schema permits tenant name, logo/mark/favicon, primary color, approved UI/display font, shape preset, default Comfortable/Compact density, login image, and constrained email header/footer content. Status/focus colors, layout, type sizes, touch sizing and custom CSS remain locked. Custom domains are configured separately and available on every plan using the approved A-record verification flow. See [DESIGN-SYSTEM-HANDOFF.md](DESIGN-SYSTEM-HANDOFF.md) and its [archive audit](MYLESNET-DESIGN-SYSTEM-V1.0-AUDIT.md); secondary-accent and shape-preset runtime behavior remain explicit source gaps.

## Platform-control-plane capability groups

- Tenant directory, onboarding state, tenant domain verification, plan/limits, subscription state, suspension/reactivation, and provisioning status.
- Platform plans and SaaS billing, kept separate from each ISP tenant’s subscriber payments and accounting ledger.
- Platform staff identities, role grants, MFA state, platform audit events, job/queue health, and integration health.
- Support-access grants with a reason, approval policy, narrow scope, expiry, visible session banner, and immutable audit trail.
- Tenant usage and operational health summaries that do not reveal subscriber secrets by default.

## Suggested delivery stages after planning is approved

1. **Decision and design baseline:** approve geography, roles, isolation model, service protocols, payment providers, hosting, retention, and UX reference scope.
2. **SaaS foundation:** sign-in/MFA, tenant provisioning lifecycle, domain resolution, plan/limits, platform panel, tenant shell, and audit trail using synthetic data.
3. **ISP core:** subscribers, plans, staff, routers/NAS registry, tenant settings, import, and basic operational reporting.
4. **Access and money movement:** RADIUS authorization/accounting, session management, captive portal flow, payment adapters, verified webhook flow, invoices, and reconciliation.
5. **Operations breadth:** vouchers/agents, support, campaigns, inventory, analytics, custom domains, accounting integrations, and mobile-sized views.
6. **Production readiness:** security review, isolation/recovery drills, provider certification, monitoring, incident response, migration, and release procedures.

The stages above are retained as an early product-scope proposal. They are superseded as the execution sequence by [`IMPLEMENTATION-PLAN.md`](IMPLEMENTATION-PLAN.md), whose phases, dependencies, and exit gates are authoritative. Provider sandboxes and network deployment evidence remain inputs to the relevant plan phases, not reasons to use this older sequence.

## Product principles

- Tenant isolation and authorization are enforced server-side on every page, action, file, job, broadcast, and integration callback.
- A subscriber’s access state changes only after authoritative payment/access events; retries are idempotent and corrections are auditable reversals.
- Tenant branding follows the approved v1.0 input schema and is isolated to verified tenant-facing surfaces. The platform derives safe theme values from the DTCG source; status/focus meaning, layout, type sizes and touch sizing remain platform-owned. See the handoff for the unresolved secondary-accent and preset implementation gaps.
- Platform subscription billing and tenant ISP billing are separate products, ledgers, and reporting scopes.
- Start with the network and payment providers that can be tested end to end; keep unsupported integrations behind explicit capability flags.
- Reference screenshots guide layout and visual language; they do not define source behavior, legal terms, or a guarantee of feature parity.
