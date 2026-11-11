# ADR-0004: Regional data strategy

- **Date:** 11 November 2026
- **State:** One regional data plane accepted; Ireland hosting remains a candidate, not production approval
- **Owner:** Platform/operations and country legal owners not named; assign in MN-PLAN-008
- **Evidence:** `AGENTS.md`; `READINESS-DECISIONS.md`; `TECH-STACK.md`; `TECHNICAL-SPECIFICATION.md` §§3, 14.8/14.11; `DECISION-REGISTER.md` MN-DEC-012/018/019

## Context

V1 serves Kenya, Uganda, and Tanzania. The approved architecture posture is one regional data plane with country-by-country transfer safeguards and legal launch gates. Convex Europe (Ireland) and Vercel Dublin (`dub1`) are the leading managed-region candidates, but candidate status does not establish latency, contract, capacity, cost, legal transfer, or production residency.

## Decision

- Plan for one regional data plane across the three launch countries, with separate country readiness evidence before production personal-data processing is enabled in each country.
- Treat Convex Europe (Ireland) and Vercel Dublin (`dub1`) as candidates only. No production region or residency/compliance claim is approved by this ADR.
- Before a production decision, validate measured latency from all three countries, current provider plan/capacity and failover behavior, service contracts, total cost, data-transfer routes/safeguards, and local legal requirements.
- Record country/category retention schedules and legal holds separately from physical hosting location. Local counsel must validate applicable periods and transfer requirements.

## Alternatives considered

- **One region in Ireland:** current leading candidate, subject to evidence and approval.
- **Separate in-country deployments from launch:** not the selected v1 posture; retain as an alternative if legal or measured-service requirements make the shared region unsuitable.
- **Multiple active regions:** not selected without a data-consistency, recovery, residency, and operational design.

## Security and operational tradeoffs

A regional deployment simplifies cross-border operation but creates transfer and latency obligations and may concentrate outage impact. Country-specific legal approval cannot be inferred from provider region selection. Multi-region operation may reduce some failure modes but creates replication, consistency, recovery, and cost risks.

## Interfaces and sequences

Region, country, service, and tenant scope must be available as controlled deployment metadata in `IC-DEPLOY-01` and `IC-OBS-01`; region selection does not replace authorization or data classification. See [`INTERFACE-AND-SEQUENCE-CONTRACTS.md`](INTERFACE-AND-SEQUENCE-CONTRACTS.md).

## Open items and revisit trigger

Obtain country counsel review, latency/capacity/cost evidence, plan/contract and failover details, backup/recovery evidence, and a named data/operations owner. Revisit when any country law, provider region capability, measured workload, or continuity requirement changes.
