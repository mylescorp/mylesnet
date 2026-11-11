# ADR-0011: Observability, SLO measurement, and incident communication

- **Date:** 11 November 2026
- **State:** Privacy and measurement requirements accepted; SLO evidence and ownership remain open
- **Owner:** Service operations/security owners not named; assign in MN-PLAN-008
- **Evidence:** `AGENTS.md`; `READINESS-DECISIONS.md`; `TECHNICAL-SPECIFICATION.md` §§10, 14.7; `DECISION-REGISTER.md` MN-DEC-010/019

## Context

Approved readiness targets cover RADIUS and captive portal availability, eligible subscriber login success, payment activation, payment recording/reconciliation, and growth. The targets are planning goals, not demonstrated SLOs. Measurement requires explicit sources, exclusions, service boundaries, windows, and owners. Telemetry spans tenant data and sensitive payment, subscriber, and network events.

## Decision

- Correlate browser request, Next.js boundary, Convex operation, provider callback/job, and RADIUS/router worker with a trace/correlation identifier. Record service, operation, region, result, and internal tenant context only in access-controlled telemetry.
- Default telemetry excludes tenant names, credentials, raw subscriber identifiers, payment secrets, message bodies, and unnecessary PII. Redaction and access controls apply to logs, traces, metrics labels, incident artifacts, and support views.
- Separate MylesNet service health from tenant configuration, provider, and customer-network failures. Record event freshness and queue lag; do not present stale data as current.
- Provide platform service dashboards and permission-gated tenant health views; tenant views cannot reveal other tenants or platform-only security investigations.
- Establish alert ownership for saturation, callback quarantine/drop, retry exhaustion, RADIUS timeouts/accounting lag, router command failures, cross-tenant authorization denials, and backup/restore failures. Incident procedures include severity, incident commander, customer impact, notification, update cadence, resolution and post-incident review.
- Treat the following readiness targets as targets only: 99.95% monthly availability for managed RADIUS and hosted captive portal; at least 99.5% monthly eligible-login success; 99% of valid payment confirmations activated within 30 seconds; 99% of provider-confirmed payments recorded/reconciled within one minute; 1,000 paying tenants and 100,000 subscriber accounts 12 months after launch. Definitions and sources are in `READINESS-DECISIONS.md`.
- Review measures monthly and reassess assumptions quarterly. Do not claim an SLO, production availability, compliance, or provider approval without operational evidence.

## Alternatives considered

- **One blended uptime metric for the whole platform:** rejected; each service class and dependency has distinct failure modes.
- **Treat target values as current SLO evidence:** rejected; values are not instrumented or demonstrated.
- **Put tenant names or raw identifiers in metric labels for convenience:** rejected due data exposure and high cardinality.

## Security and operational tradeoffs

Cross-service correlation improves diagnosis but creates a path to join identity, payment, and network activity; restrict access, minimize identifiers, define retention, and audit investigations. More granular metrics can improve fairness and tenant health but risk high-cardinality cost and PII leakage. Alerting without named responders creates noise rather than response capability.

## Interfaces and sequences

See `IC-OBS-01`, `IC-EVENT-01`, and `IC-INCIDENT-01` in [`INTERFACE-AND-SEQUENCE-CONTRACTS.md`](INTERFACE-AND-SEQUENCE-CONTRACTS.md).

## Open items and revisit trigger

Define numerator/denominator and exclusions per SLI; authoritative event sources; event freshness thresholds; metric retention; cardinality/tenant fairness budgets; alert thresholds; service/error budgets; dashboard access; P1 on-call feasibility; and named service owners. Revisit targets only with measured evidence and approved product/operations changes.
