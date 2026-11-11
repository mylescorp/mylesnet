# ADR-0010: Deployment topology, environments, and rollback

- **Date:** 11 November 2026
- **State:** Service boundaries and isolation rules accepted; pipeline and rollout decisions remain open
- **Owner:** Release/platform operations owner not named; assign in MN-PLAN-008
- **Evidence:** `AGENTS.md`; `TECH-STACK.md`; `TECHNICAL-SPECIFICATION.md` §§3, 9, 14.8; `READINESS-DECISIONS.md`; `DECISION-REGISTER.md` MN-DEC-012/019

## Context

The product combines Next.js surfaces, Convex, identity, payment/messaging providers, RADIUS, and router workers with different protocols, credentials, and availability needs. The repository is in planning/foundation setup; no deployment or live service is authorized. Production region and provider plans are not approved.

## Decision

- Use Vercel as the Next.js hosting direction and Convex as the application backend direction; Vercel Dublin and Convex Ireland remain candidate regions, subject to ADR-0004 and country gates.
- Deploy RADIUS and router/network workers separately from browser-facing applications and scope their credentials to their responsibilities. Payment, messaging, domain/TLS, and long-running workers also use isolated credential/service boundaries.
- Maintain distinct local-synthetic, shared-development, integration/sandbox, staging/preproduction, and production environments. Separate service deployments, OAuth callbacks, provider credentials, domains, queues, and secrets. Local defaults must never point at production.
- Require versioned, reviewed deployment changes and migration plans that support compatible old/new application versions. Use expand/migrate/contract for schema/data migrations; every migration defines checkpoints, idempotency, latency/lock impact, observability, rollback/compensation, and ownership.
- Preserve append-only money and audit facts during rollback. Correct through compensating entries, not database rewinds that erase settled effects.
- No ADR in this set authorizes a deployment, environment provisioning, or production operation.

## Alternatives considered

- **One shared runtime for UI, RADIUS, and router control:** rejected due protocol, availability, and credential-boundary requirements.
- **Node-container or Cloudflare as the v1 web host:** superseded by the approved Vercel direction; may be reconsidered only through an approved architecture change.
- **Use production services for local development:** rejected; use synthetic data and isolated credentials.
- **Rollback by restoring an old database snapshot over current financial state:** rejected because it can erase immutable ledger/audit history.

## Security and operational tradeoffs

Separate services reduce credential blast radius but add deployment, monitoring, network policy, and compatibility work. Staged environments improve safety but need clear data-separation and promotion rules. Rollback is not always a binary code reversal: provider callbacks, jobs, domains, and additive data migrations need idempotent compensation and operator runbooks. Exact provider plan limits and region constraints can affect deployability.

## Interfaces and sequences

See `IC-DEPLOY-01`, `IC-MIGRATION-01`, `IC-RECOVERY-01`, and `SEQ-RESTORE-01` in [`INTERFACE-AND-SEQUENCE-CONTRACTS.md`](INTERFACE-AND-SEQUENCE-CONTRACTS.md).

## Open items and revisit trigger

Define deployment/promotion pipeline, required checks, environment inventory and owners, rollout/health gates, rollback authority, maintenance windows, provider limits, region/failover, migration observability, and dependency update policy before Phase 1 delivery. Revisit if the approved stack, provider limits, recovery evidence, or release risk changes.
