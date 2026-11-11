# ADR-0007: Tenant domains, DNS, TLS, and callback migration

- **Date:** 11 November 2026
- **State:** Tenant domain lifecycle accepted; registrar and automation details remain open
- **Owner:** Platform/domain operations owner not named; assign in MN-PLAN-008
- **Evidence:** `AGENTS.md`; `CORE-FLOWS-AND-DATA.md` tenant domain section; `TECHNICAL-SPECIFICATION.md` §§3, 14.1/14.2; `DESIGN-SYSTEM-HANDOFF.md` §1; `DECISION-REGISTER.md` MN-DEC-007/020

## Context

Tenant subdomains are provisioned by default. Tenant-owned custom portal domains are available on every plan. A hostname change affects portal access, TLS, verified tenant routing, and payment callbacks that may be configured externally.

## Decision

- Offer the MylesNet tenant subdomain by default and allow a verified tenant-owned custom domain on every plan; domain availability is not an entitlement.
- For a tenant-owned hostname, provide a domain-specific DNS A-record instruction, wait for detection, and require an explicit Owner-initiated Verify DNS check. Provision HTTPS automatically only after domain ownership verification succeeds.
- Resolve a hostname only through a verified, unique tenant-domain mapping. Unknown, unverified, duplicate, or suspended-host mappings fail closed. The host is routing context and never authorization.
- During domain migration, continue serving previous verified addresses until the Tenant Owner confirms dependent integrations have moved and explicitly stops serving the old address. Keep the retired hostname reserved to that tenant; audit the change.
- Surface callback dependencies and migration status. Provider dashboards or provider support may require separate callback URL updates; MylesNet cannot imply that changing its DNS updates those records.
- Treat the design-system package's conflicting CNAME/paid-tier copy as superseded by MylesNet product decisions.

## Alternatives considered

- **Custom domains on paid tiers only:** rejected by the approved all-tier decision.
- **CNAME-only setup copied from the design package:** rejected by the product A-record decision.
- **Immediately stop serving the old host after changing DNS:** rejected because callbacks and subscribers may still use it.
- **Buy or select a registrar during planning:** left open; no registrar is approved.

## Security and operational tradeoffs

Domain verification prevents unauthorized tenant binding; DNS propagation and certificate issuance are asynchronous and need clear state/expiry. Serving prior addresses supports migration but increases attack surface and certificate/tenant-mapping lifecycle complexity. DNS rebinding, hostname normalization, duplicate ownership, certificate renewal/revocation, and callback abuse require threat review.

## Interfaces and sequences

See `IC-DOMAIN-01`, `SEQ-DOMAIN-01`, and [`REQ-TEN-11`/`AC-TEN-11`](../REQUIREMENT-ROUTE-TRACEABILITY.md#tenant-console-requirements) in [`INTERFACE-AND-SEQUENCE-CONTRACTS.md`](INTERFACE-AND-SEQUENCE-CONTRACTS.md).

## Open items and revisit trigger

Select a registrar only after country/availability/cost review. Define DNS polling/TTL, ownership token or proof, hostname normalization, TLS provider and renewal/revocation runbook, migration inventory/status, callback allowlists, and safe rollback. Revisit when provider callback rules, DNS/TLS capabilities, or all-tier product decision changes.
