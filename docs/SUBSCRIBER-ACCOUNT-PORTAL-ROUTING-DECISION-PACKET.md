# Subscriber Account Portal Routing Decision Packet

**Task:** MN-PLAN-022 · **Plan phase:** Phase 0 — close planning and turn decisions into build contracts
**Status:** Decision support prepared; host topology, route names, and navigation remain open
**Related decisions:** MN-DEC-003, MN-DEC-007, MN-DEC-020, MN-DEC-023

## Purpose and boundary

This packet turns the subscriber account portal host and navigation question into a reviewable Phase 0 decision. It separates that question from public-site editorial and CMS scope in [MN-PLAN-018's packet](PUBLIC-CONTENT-EDITORIAL-DECISION-PACKET.md).

It records approved routing, identity, domain, and branding constraints; compares viable host topologies; and lists the owner decisions and later acceptance evidence needed before Phase 9. It does not select a hostname, URL path, cross-tenant identity model, domain entitlement, design, CMS, or implementation. This work changes no product code, tenant configuration, DNS, TLS, authentication, payment callback, or production service.

## Evidence baseline

| Status | Evidence and implication |
|---|---|
| Approved | apps/web owns public and subscriber surfaces. Tenant host and theme are routing/presentation inputs; neither is authorization. Resolve tenant hostnames from a unique, verified server-side mapping and fail closed for unknown, unverified, duplicate, or suspended hosts. See [IC-APP-01/02](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md#interface-contract-registry) and [ADR-0007](adr/ADR-0007-domains-dns-tls-and-callbacks.md). |
| Approved | Subscriber identity is separate from staff AuthKit and roles. Subscriber reads and writes are limited to the verified identity's own records within a resolved tenant; recovery must avoid account enumeration. See [REQ-SUB-01–03](REQUIREMENT-ROUTE-TRACEABILITY.md#subscriber-captive-and-network-journeys). |
| Approved | Subscriber account and captive surfaces use the approved tenant brand inputs, with no staff controls. Account tasks include service status, eligible renewal, payment result, receipts, support, and permitted contact preferences. |
| Approved | Tenant subdomains are provided by default, and tenant custom portal domains are available across subscription plans. The DNS A-record, explicit Owner Verify DNS, automatic HTTPS, and old-host migration lifecycle are documented in readiness decisions and ADR-0007. Whether the account portal shares a tenant custom hostname or needs a separate mapping remains open. |
| Approved | A captive payment redirect is not payment evidence. Only provider confirmation or reconciliation can create the financial effect or activate access. The captive flow must retain pending, failure, expiry, and retry handling. |
| Open | The default host and route for account access; whether account and captive portals share a host; whether tenant custom domains cover the account portal, captive portal, or both; pre-login tenant discovery; cross-tenant subscriber identity behavior; and the account navigation hierarchy. |
| Open | Exact account-to-captive links, recovery and notification URLs, canonical paths, locale behavior, subscriber session/cookie boundaries, and domain migration effects on saved links. These are build-contract decisions, not reasons to change the approved security boundaries. |

The [public editorial packet](PUBLIC-CONTENT-EDITORIAL-DECISION-PACKET.md) keeps all 308 public marketing URLs static or structured unless a justified editing need is approved. This packet concerns authenticated or captive subscriber-facing routes and does not change that public URL inventory.

## Routing topologies for owner review

The examples below are conceptual labels, not reserved domains or proposed production URLs.

| Option | Shape | Benefits | Costs and questions |
|---|---|---|---|
| A. Tenant-scoped MylesNet host | Resolve a tenant-specific MylesNet hostname, then serve account and captive areas at distinct paths on that host. | Tenant context is apparent before sign-in; uses the approved default subdomain model; can make captive-to-account links direct. | Sharing an origin may couple cookies, caching, captive-browser behavior, and release controls. Decide whether the captive surface can safely share the account origin and whether account routes need a separate app boundary. |
| B. Shared account host with tenant discovery | Keep tenant captive portals on their resolved tenant hosts; use one MylesNet-owned account host and ask the subscriber to identify/select the ISP before sign-in. | One stable account entry point; account portal need not inherit the captive domain configuration. | Tenant discovery must not reveal whether an account exists. A tenant slug or selected ISP is context, not authentication. Decide how saved links and subscribers associated with more than one tenant work. |
| C. Separate tenant-scoped hosts | Use a tenant-scoped hostname for the account portal and a distinct tenant-scoped hostname for captive access. | Separates captive-browser behavior and customer account sessions; keeps tenant resolution explicit for each surface. | Requires multiple host mappings, DNS/TLS and migration states if custom domains are extended to both. Callback, allowlist, support, and communication links must be maintained per host. |

### Custom-domain scope is a separate choice

The project charter makes tenant custom portal domains available across plans, and readiness decisions specify the captive portal DNS workflow. The account-to-captive hostname relationship is not settled. The owner should select one of these scopes and confirm its DNS/TLS and migration consequences:

1. The tenant's MylesNet subdomain serves the account portal; an optional tenant-owned domain remains captive-only.
2. One verified tenant-owned custom domain serves both account and captive paths.
3. Account and captive surfaces each use separately verified tenant-owned hostnames.

Any selected custom-domain scope must use ADR-0007's unique verified host mapping and migration controls. A custom hostname, path, tenant slug, referrer, or redirect parameter must never grant access or change the server-authorized subscriber identity.

## Decisions and acceptance evidence required

Before Phase 9 implementation, the project owner should appoint a product decision owner and obtain architecture/security and domain-operations review. Record the selected value, rationale, evidence, accountable owner, and revisit trigger for each item:

| Decision | Required answer |
|---|---|
| Canonical account entry | Tenant-specific host or shared account host; define what a subscriber sees before authentication. |
| Account/captive relationship | Shared host with separate paths, distinct hosts, or another documented topology; define safe links from captive completion and return-login states. |
| Custom-domain scope | Captive only, account and captive together, or separately verified hosts; align the choice with DNS/TLS, callbacks, redirects, migration, and support. |
| Navigation and routes | Name the account tasks and route hierarchy for service summary, renewal, receipts, support, profile/contact choices, sign-in, and recovery. Keep proposed labels distinct from final URLs until approved. |
| Subscriber identity context | Decide behavior when a subscriber uses multiple tenant services, and how account discovery, sign-in, and recovery avoid cross-tenant disclosure. Do not reuse staff tenant-switch or WorkOS session assumptions. |
| Session and return behavior | Define host-only cookie scope, login/logout/recovery return destinations, pending-payment resume behavior, and which same-site/cross-site transitions are needed. Never treat a browser return as payment confirmation. |
| Localization and links | Define tenant default language, customer language switching, and canonical host/route used by SMS, email, receipts, support content, and bookmarks. |
| Private-page delivery | Define noindex, cache, redirect, and referrer behavior so authenticated subscriber data cannot enter public search or shared caches. Keep public marketing routes separate. |

## Later acceptance cases

These are evidence requirements for the eventual product decision and implementation; no tests or runtime captures were run for this planning packet.

- A known verified tenant host resolves one tenant; unknown, unverified, duplicate, and suspended host mappings fail closed.
- Changing a path, hostname, tenant label, or redirect input cannot expose another subscriber or tenant's records. Server-side subscriber identity and tenant scope are checked on every operation.
- Sign-in and recovery do not reveal whether a phone, email, or subscriber account exists. Staff authentication and staff navigation are absent from subscriber surfaces.
- The chosen route topology supports first-time captive purchase or voucher redemption, verified activation, subsequent account access, renewal, receipts, support, and safe pending/failure/retry returns.
- If both surfaces use one custom domain, verified DNS/TLS and the migration sequence cover both. If they use separate hosts, each mapping and each saved/external link has an explicit owner and retirement procedure.
- Tenant branding and English/Kiswahili behavior follow the approved design and localization contracts. Portal paths remain private and tenant data is not shared through caches, cookies, previews, or redirects.
- Direct links, bookmarks, service notices, and recovery return to the correct tenant context after hostname or route migration, without embedding credentials or treating URL state as authorization.

Browser and captive-browser validation will be needed once a disposable seeded product environment exists. The current documentation is sufficient to frame the decision; no REA capture or live subscriber inspection was needed.

## Open items retained

- No topology, public hostname, route name, account/captive host relationship, or custom-domain scope is selected.
- The project's accountable subscriber-product, architecture/security, and domain-operations owners are not named.
- Subscriber identity uniqueness across tenants and tenant discovery/recovery behavior need a separate explicit product/security decision if the owner chooses a shared account entry.
- Phase 0 remains in progress. This packet does not close MN-DEC-020, accept MN-PLAN-018, authorize Phase 9, or approve product implementation.
