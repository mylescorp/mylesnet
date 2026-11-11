# MylesNet Architecture Decision Records

**Task:** MN-PLAN-005 · **Plan phase:** Phase 0 — close planning and turn decisions into build contracts
**Date:** 11 November 2026
**Status:** Planning records; implementation is not authorized by these documents.

This set records the approved architectural boundaries and identifies decisions that still need product, security, legal, provider, cost, or operational evidence. An accepted ADR records a decision already present in the project charter or approved readiness documents; it does not claim that the system is built or production-ready. A candidate is not a production approval. An open question must remain open until its listed evidence and owner are supplied.

## ADR index

| ADR | Topic | Decision state | Main open items |
|---|---|---|---|
| [ADR-0001](ADR-0001-app-session-topology.md) | Application split, sessions, and host routing | App boundaries accepted; session/domain details open | Cookie/session sharing, callback allowlists, subscriber host and route ownership |
| [ADR-0002](ADR-0002-workos-identity-mapping.md) | WorkOS identity and organization mapping | Identity boundary accepted | MFA compatibility spike, recovery operations and provider behavior |
| [ADR-0003](ADR-0003-convex-tenant-authorization.md) | Convex tenant/branch authorization and revocation | Shared deployment and server authorization accepted | Resource/branch assignments, indexes, revocation implementation and future isolation trigger |
| [ADR-0004](ADR-0004-data-region.md) | Data-region strategy | One regional data plane accepted; Ireland is a candidate | Legal transfer, latency, contract, capacity, cost, failover and final region approval |
| [ADR-0005](ADR-0005-radius-and-router-boundary.md) | RADIUS, PostgreSQL, event outbox and router boundary | FreeRADIUS/PostgreSQL separation and outage policy accepted | Synchronization, HA, queue limits, recovery, lab inventory and operating ownership |
| [ADR-0006](ADR-0006-payment-ledger-and-country-gates.md) | Payment ledger, merchant models and country gates | Ledger separation and country gates accepted | Provider contracts, sandbox evidence, fees, settlement ownership and legal approval |
| [ADR-0007](ADR-0007-domains-dns-tls-and-callbacks.md) | Domains, DNS, TLS and callback migration | Tenant domain lifecycle accepted | Registrar, DNS/TLS automation, callback inventory and provider-specific migration |
| [ADR-0008](ADR-0008-theme-compiler-and-schema.md) | Theme source, compiler and schema | Token-source and brand-boundary rules accepted | Runtime compiler/schema, source audit gaps, attribution and approved assets |
| [ADR-0009](ADR-0009-storage-exports-and-backups.md) | File storage, export and backup/recovery | Security and export invariants accepted; storage/recovery choice open | Storage provider, retention, backup scope, RPO/RTO, restore process and owner |
| [ADR-0010](ADR-0010-deployment-and-rollback.md) | Deployment topology and rollback | Service boundaries and environment separation accepted | Exact environments, pipeline, rollout gates, provider plans, rollback authority and region |
| [ADR-0011](ADR-0011-observability-and-slo-measurement.md) | Observability, SLO measurement and incident communication | Measurement/privacy guardrails accepted; targets are not demonstrated | Metric sources, exclusions, alert thresholds, service owners and on-call capacity |

## Cross-cutting contracts

[`INTERFACE-AND-SEQUENCE-CONTRACTS.md`](INTERFACE-AND-SEQUENCE-CONTRACTS.md) defines planning-level interfaces and sequence outlines. The [requirement traceability catalog](../REQUIREMENT-ROUTE-TRACEABILITY.md#cross-cutting-architecture-contracts) maps requirement and journey IDs to those contracts. These contracts state authority, scope, trust boundaries, retry/recovery expectations, and acceptance evidence; wire schemas, provider credentials, deployments, and product workflows remain future work.

## Decision-record requirements

Each ADR records its status, context, decision or explicit open question, alternatives, security and operational tradeoffs, accountable owner state, evidence, and revisit trigger. Owners remain explicitly unassigned where the approved sources do not name one. MN-PLAN-008 must assign accountable roles before the Phase 0 exit review.

## Source precedence

Use the project charter in [`../../AGENTS.md`](../../AGENTS.md), approved decisions in [`../READINESS-DECISIONS.md`](../READINESS-DECISIONS.md), and reconciled state in [`../DECISION-REGISTER.md`](../DECISION-REGISTER.md) as authority. The [`end-to-end implementation plan`](../IMPLEMENTATION-PLAN.md) governs sequencing and gates. Older proposals are context only when marked superseded or candidate. No ADR authorizes product code, live provider/router activity, tenant provisioning, production residency claims, or deployment.
