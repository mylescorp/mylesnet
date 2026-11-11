# ADR-0001: Application split, sessions, and host routing

- **Date:** 11 November 2026
- **State:** Accepted application boundaries; session and host details remain open
- **Owner:** Application architecture owner not named; assign in MN-PLAN-008
- **Evidence:** `AGENTS.md`; `TECH-STACK.md`; `TECHNICAL-SPECIFICATION.md` §§3–4, 14.2; `DECISION-REGISTER.md` MN-DEC-003/005/020

## Context

MylesNet has public/signup, subscriber, authenticated platform/tenant, and design-documentation surfaces. Staff may belong to multiple tenant organizations. Tenant subdomains and verified custom domains identify a surface, while authenticated identity and current membership determine access. The exact subscriber host and shared-session behavior across app origins are not approved.

## Decision

- Use `apps/web` for public marketing, signup, subscriber account, and captive-facing web surfaces; use `apps/admin` for authenticated platform and ISP operator surfaces; use `apps/storybook` for design documentation and visual review.
- Keep application route ownership and server/client boundaries explicit. Storybook is not a product portal.
- Treat a hostname as tenant-surface routing context only. Resolve it to a verified tenant/domain record; never use a host or browser-supplied tenant ID as authorization.
- Authenticate staff with WorkOS AuthKit and resolve current tenant/branch authority on the server as specified in ADR-0002 and ADR-0003.
- On tenant switching, discard the previous tenant's client state and live subscriptions before presenting data for the newly verified organization.
- Unknown or suspended tenant hosts fail closed. Public pages remain independently cacheable only when their content is not tenant-private.

The decision does not choose cookie domain, cross-app SSO mechanics, OAuth callback allowlists, subscriber portal hostname, or exact route inventory ownership where the canonical requirements leave them open.

## Alternatives considered

- **One Next.js app for all surfaces:** rejected in favor of the user-directed three-app boundary.
- **A separate app per audience or tenant:** not selected; it would expand deployment and session coordination without an approved need.
- **Use hostname or organization membership as authorization:** rejected; neither proves current role, tenant state, permission, or branch scope.

## Security and operational tradeoffs

Separate apps clarify deployment and client/server boundaries but make session continuity, callback registration, and cache invalidation across origins explicit design work. Tenant host resolution must be isolated from authorization to prevent host-header, stale-domain, or cache-key confusion. Public CDN caching must not include authenticated or tenant-private responses.

## Interfaces and sequences

See `IC-APP-01`, `IC-IAM-01`, `IC-IAM-02`, `SEQ-IAM-01`, and `SEQ-SIGNUP-01` in [`INTERFACE-AND-SEQUENCE-CONTRACTS.md`](INTERFACE-AND-SEQUENCE-CONTRACTS.md).

## Open items and revisit trigger

The application owner must specify route ownership, allowed origins/callbacks, cookie/session topology, tenant switching behavior across app origins, and subscriber portal host before implementation. Revisit this ADR only if the approved product surface or app layout changes.
