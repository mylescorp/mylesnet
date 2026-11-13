# ADR-0005: RADIUS, PostgreSQL, event outbox, and router boundary

- **Date:** 11 November 2026
- **State:** Approved network-plane and outage boundaries; synchronization and recovery details remain open
- **Owner:** Network/platform operations owner not named; assign in MN-PLAN-008
- **Evidence:** `AGENTS.md`; `TECH-STACK.md`; `CORE-FLOWS-AND-DATA.md`; `TECHNICAL-SPECIFICATION.md` §§3, 5.7, 8–9, 14.9; `DECISION-REGISTER.md` MN-DEC-006/017

## Context

Authentication/accounting packets have strict latency and availability needs and must not depend synchronously on Convex. Application entitlements and tenant configuration still need to reach network policy safely, and accounting events must be durable and project into authorized live application views. Router management uses a separately credentialed network worker.

## Decision

- Use FreeRADIUS 3.2.x with a dedicated PostgreSQL AAA store, operated as a separate MylesNet-managed regional service. Do not put Convex, WorkOS, browser authorization, payment callbacks, or platform-admin credentials on the RADIUS request path.
- Treat Convex/application records as the owner of SaaS tenant, plan, entitlement, and policy intent. PostgreSQL is the runtime AAA policy/accounting store used by FreeRADIUS. Define an explicit, observable projection/synchronization contract; do not introduce untracked dual writes.
- Persist accounting events and an outbox durably near the RADIUS/AAA service before attempting delivery to Convex. Relay with stable event identity, idempotent acceptance, bounded retries/backpressure, and a recoverable delivery result. Convex outage must not lose acknowledged accounting events.
- On RADIUS service loss, preserve already-active sessions at the NAS where possible, reject new logins until recovery, and never fail open. Recovery and stale policy handling must not weaken that rule.
- Use a separate least-privilege RouterOS 7 worker/provisioning path. The WireGuard tunnel permits RADIUS and CoA/Disconnect only; it must block WinBox, SSH, and RouterOS API. Provisioning credentials/configuration are single-use, expiring, auditable, and revocable.
- Do not connect to real routers during Phase 0. Validate only with an isolated lab after implementation is authorized.

## Alternatives considered

- **Convex lookup for every Access-Request or accounting packet:** rejected; it couples network access to application/WAN availability.
- **Redis or Convex as the RADIUS policy/accounting store:** superseded for v1 by the dedicated PostgreSQL AAA store.
- **Router-admin tunnel/API exposure to the web app:** rejected by least-privilege boundary.

## Security and operational tradeoffs

The independent data plane improves failure isolation but requires reliable projection, local durability, queue capacity, replay protection, reconciliation, and operational ownership. Stale policy may permit or deny access incorrectly; define maximum staleness and revocation propagation before launch. Router commands remain high risk even over a tunnel and require allowlists, authorization, audit, timeouts, and result capture.

## Interfaces and sequences

See `IC-RADIUS-01`, `IC-ROUTER-01`, `SEQ-RADIUS-01`, and `SEQ-ROUTER-01` in [`INTERFACE-AND-SEQUENCE-CONTRACTS.md`](INTERFACE-AND-SEQUENCE-CONTRACTS.md).

## Open items and revisit trigger

Define projection ownership, policy versioning/cache invalidation, NAS mapping, PostgreSQL HA, local outbox capacity, ordering/deduplication key, retry/drain behavior, backup/restore, credential rotation, supported RouterOS lab devices, CGNAT/firewall behavior, and RADIUS operational targets. The [MN-PLAN-030 router lab and acceptance worksheet](../ROUTER-LAB-AND-ACCEPTANCE-WORKSHEET.md) records the lab choices and evidence gate without changing approved boundaries. Revisit only with lab, load, outage, and recovery evidence.
