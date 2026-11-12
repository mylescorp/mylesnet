# Service Objectives and Operational Evidence Plan

**Task:** MN-PLAN-019
**Plan phase:** Phase 0 — close planning and turn decisions into build contracts
**Status:** Evidence contract prepared; no SLO, region, RPO/RTO, staffing commitment, or production readiness is newly approved.
**Related decision:** [MN-DEC-019](DECISION-REGISTER.md)
**Related ADR:** [ADR-0011: Observability, SLO measurement, and incident communication](adr/ADR-0011-observability-and-slo-measurement.md)

## Purpose and boundary

This plan turns the open reliability and operations questions in MN-DEC-019 into a reviewable evidence contract. It organizes existing planning targets, identifies the measurements and proof still needed, and lists accountable roles the project owner must appoint.

It does not set new target values, choose a final service region, name people, validate a provider or country, or establish that an SLO or recovery process is operating. Approved target values remain planning goals until their definitions, instrumentation, ownership, and measured evidence are accepted. Product implementation, production monitoring, failover, restore drills, and customer-facing service commitments remain behind their existing approval gates.

## Existing planning targets

The values below come from `AGENTS.md` and `READINESS-DECISIONS.md`. They are copied here as a baseline; this packet does not approve them as customer commitments or claim that they have been measured.

| Outcome | Existing target | Current qualification | Evidence still required |
|---|---|---|---|
| Active paying ISP tenants | 1,000 by 12 months after public launch | Planning outcome; the definition of the paying state and launch cohort must be fixed for reporting. | Source-of-truth tenant/subscription states, month-end counting rule, exclusions, and monthly scorecard owner. |
| Active subscriber accounts | 100,000 by 12 months after public launch | Count distinct, non-test accounts with a valid service entitlement at month-end; exclude expired, suspended, duplicate, and test accounts. The 100-per-tenant average is a scenario assumption, not a market forecast. | Entitlement source, deduplication rule, snapshot boundary, test-account labeling, and scorecard owner. |
| Eligible subscriber login success | At least 99.5% monthly | Credential, account-state, and policy denials are separated from infrastructure failures. | Approved eligible-attempt definition, event source, retry/duplicate handling, country and NAS attribution, and denominator audit. |
| Managed RADIUS availability | 99.95% monthly | Applies to the MylesNet managed RADIUS endpoint; existing sessions stay up and new logins fail closed during an outage. | Probe locations, success criteria, maintenance treatment, network/provider attribution, failover coverage, and authoritative timestamps. |
| Hosted captive portal availability | 99.95% monthly | Separate from payment provider settlement and customer-side network faults. | Route/state probe, expected response, dependency attribution, cache behavior, regional/country coverage, and maintenance treatment. |
| Payment confirmation to service activation | At least 99% within 30 seconds | Clock starts when MylesNet receives a valid confirmation; provider delay before receipt is reported separately. | Exact confirmation and activation events, clock source, callback retries, what constitutes usable service, and how entitlement/network propagation is proven. |
| Payment recording and reconciliation | At least 99% within one minute | Applies to provider-confirmed payments; unmatched cases enter a visible exception queue immediately while safe retries continue. | Confirmation authority, ledger and reconciliation terminal states, event identity/deduplication, exact timer boundary, and exception-queue evidence. |
| Support and major incidents | Weekdays 08:00–17:00 EAT excluding public holidays; 24/7 on-call for a MylesNet-managed tenant-wide or multi-tenant access/captive-portal outage, a broad payment-processing outage, or a major security incident. Customer-side router, power, and uplink faults are excluded. P1 triage within 15 minutes, updates every 30 minutes, and a 60-minute safe-workaround objective. | These are planning targets. The workaround objective is subject to feasibility validation before any customer-facing promise. | Country holiday calendars, staffed rota, severity rules, alert routing, incident commander, communications coverage, and evidence from exercises. |

Other service classes, including the public web UI, Convex application backend, payment callbacks, router worker, and messaging, still need separate approved availability/latency/freshness budgets. Do not infer targets from the rows above.

## Draft measurement contract

The following measurement approaches are proposals for the accountable owners to approve. They preserve the existing target wording while making the missing decisions visible.

| Measure | Candidate numerator / end event | Candidate denominator / start event | Required separation and open definition |
|---|---|---|---|
| Eligible login success | Eligible attempts accepted by the authoritative authentication policy and counted as successful under the approved login sequence. | Eligible authentication attempts received by the managed service. | Keep credential, account-state, and policy denials outside the infrastructure-failure numerator; decide treatment of timeouts, malformed requests, retries, duplicate NAS events, maintenance, and test traffic. Confirm whether each attempt or a deduplicated user action is counted. |
| RADIUS availability | Measurement intervals in which the approved managed endpoint meets the success criteria. | All eligible measurement intervals in the monthly window. | Define independent probes and coverage, transport/authentication criteria, maintenance windows, regional attribution, and how provider, customer uplink, and MylesNet failures are distinguished. Do not replace endpoint availability with subscriber login success. |
| Captive portal availability | Measurement intervals in which the approved public portal route returns its expected usable state. | All eligible measurement intervals in the monthly window. | Define routes and synthetic states, redirect handling, CDN/cache behavior, and whether identity, payment, tenant configuration, and subscriber uplink dependencies are reported separately. A portal page response does not prove payment or network activation. |
| Confirmation-to-activation time | Valid confirmations for which the approved service-activation terminal event occurs within the target interval. | Valid provider confirmations first received by MylesNet. | Record server-side receive and activation timestamps with one correlation identity. Decide whether terminal activation requires only an active entitlement or also verified policy propagation to the access plane. Exclude time before MylesNet receives confirmation and report it separately. |
| Confirmation-to-reconciliation time | Provider-confirmed payments with an immutable financial record and an approved reconciled state inside the target interval. | Valid provider-confirmed payments received by MylesNet. | Define the evidence that closes reconciliation, duplicate/replay handling, correction/reversal treatment, and the immediate visibility requirement for unmatched items. A client redirect is not confirmation evidence. |
| Tenant/subscriber growth | Distinct active records under the approved month-end state definition. | Not a reliability denominator; report as a point-in-time count. | Define paying subscription state, valid entitlement state, tenant/account deduplication, test-data exclusions, and the exact month boundary. Keep the 100-per-tenant planning assumption labeled as an assumption. |
| Support response | Incidents acknowledged, triaged, updated, or mitigated against the approved severity clock. | Incidents classified under the approved coverage and severity rules. | Define severity assignment, clock start/stop, reclassification, customer-side exclusions, public-holiday coverage, after-hours incident entry, language coverage, and safe-workaround verification. The 60-minute target is not a customer promise. |

For each metric, the owner must approve the calendar/timezone boundary, numerator, denominator, exclusions, source-of-truth event, clock source, aggregation and correction policy, dashboard freshness, data-retention period, and whether the measure is suitable for a service commitment. These values are open where the canonical documents do not decide them.

## Recovery objective and proof register

No numeric RPO or RTO is approved. Assigning a target requires workload, retention/legal, provider, architecture, and cost evidence. The following register defines the minimum proof to collect after implementation is authorized.

| Recovery domain | Authority and failure boundary | Evidence required before a recovery claim | Accountable role to appoint |
|---|---|---|---|
| SaaS application state and tenant configuration | Convex is the SaaS application-state authority; the candidate managed region remains subject to contract and regional review. | Approved backup/export scope, encryption and access control, retention/expiry, restore order, integrity checks, tenant-isolation proof, and an isolated restore drill with measured data-loss and recovery intervals. | Platform/data operations owner and security owner. |
| Subscriber AAA and active access policy | FreeRADIUS plus its dedicated PostgreSQL AAA store own time-critical authentication/accounting behavior; Convex is not on the RADIUS request path. | Independent AAA backup/recovery plan; replication/failover evidence; accounting replay/deduplication; policy freshness; approved fail-closed behavior; measured effects on existing sessions and new logins. | Network/RADIUS operations owner. |
| Payment callbacks and financial records | Provider evidence is the confirmation source; MylesNet's append-only ledger owns accepted financial effects. Retries and recovery cannot create duplicate effects or erase posted history. | Durable intake/outbox or equivalent recovery proof, replay/idempotency evidence, exception visibility, ledger integrity and reversal handling, and provider-to-ledger reconciliation after restoration. | Payments operations owner with finance and security. |
| Identity and service configuration | WorkOS supplies staff authentication; application authorization remains authoritative. Secrets belong in managed secret storage. | Recovery of identity integration/configuration without stale membership grants, secret rotation evidence, callback/domain recovery, and audit of privileged restoration. | Identity/security and platform operations owners. |
| Portal assets, domains, and certificates | Public and tenant-facing assets must remain separated from tenant-private data; DNS/TLS and old-domain migration are independent dependencies. | Versioned asset/config restoration, certificate/DNS failure and recovery evidence, tenant-host isolation, callback migration checks, and safe degraded messaging. | Platform/domain operations owner. |
| Audit, telemetry, and incident evidence | Audit and financial history is append-only; operational telemetry is minimized, redacted, access-controlled, and retained under an approved schedule. | Recovery of required records, redaction/access evidence, retention/legal-hold behavior, correlation without raw secrets/PII, and investigation access review. | Security/data governance and service operations owners. |

Every recovery exercise must use an isolated target and synthetic or approved non-production data. Record the failure injected, dependencies affected, start/end events, data loss, restored state, tenant boundary checks, financial/network invariants, observed duration, failures, and remediation owner. A backup configuration or provider feature description alone is not a successful restore.

## Operational owner appointments

The project owner must appoint named individuals to the following accountable roles before operational readiness can be accepted. Role labels below do not assign a person.

| Role to appoint | Accountable area | Required evidence of readiness |
|---|---|---|
| Platform/service operations owner | Web, Convex, deployment region, capacity, monitoring and service dashboards. | Service map, escalation route, freshness/latency budgets, capacity/cost review, maintenance policy and rollback authority. |
| Network/RADIUS operations owner | FreeRADIUS, PostgreSQL AAA, NAS policy path, network dependencies and access recovery. | Lab matrix, health probes, failover/degraded-mode policy, accounting backlog recovery and staffed escalation. |
| Payments/finance operations owner | Provider callback intake, ledger/reconciliation, settlement exceptions and country capability state. | Provider evidence, queue/replay operations, reconciliation ownership, financial incident runbook and exact-once evidence. |
| Security/incident owner | Threat response, sensitive telemetry, security alerts, incident command and breach escalation. | Severity/notification matrix, responder coverage, access controls, incident exercises and post-incident review process. |
| Data/recovery owner | Backups, restore, retention, exports/deletion, legal holds and recovery evidence. | Approved recovery values, backup inventory, retention alignment, isolated drill cadence and restore sign-off. |
| Support lead and incident commander | Tenant communication, ticket routing, support-hour coverage and cross-team incident coordination. | Named rota, country holiday/language coverage, escalation contacts, update channels and exercise evidence. |

Country counsel, privacy owners, country operations, and finance/tax owners remain necessary for their respective launch gates in [`COUNTRY-AND-OPERATIONS-PACKS.md`](COUNTRY-AND-OPERATIONS-PACKS.md). An operational owner cannot approve legal or provider requirements outside their authority.

## Evidence sequence and acceptance

1. **Planning:** appoint owners; approve metric definitions and source boundaries; keep numeric RPO/RTO, service-region, capacity, cost, and unstaffed support values explicitly open.
2. **Local synthetic development:** validate calculation logic, correlation IDs, safe/redacted telemetry, retry/replay semantics, and dashboard stale/offline states. This proves implementation behavior only, not provider or production availability.
3. **Provider sandbox and isolated network lab:** correlate signed sandbox confirmations through ledger/activation, and test RADIUS/AAA failure and recovery with synthetic tenants and lab NAS devices. Provider settlement and network service remain independently evidenced.
4. **Staging and recovery drills:** use representative traffic and failure scenarios to measure the approved objectives; perform isolated restore drills; prove tenant boundaries, financial immutability, retries, alerts, runbooks, and responder routing.
5. **Country launch gate:** accept legal, provider, region, cost, staffing and measured reliability evidence per enabled country. Production evidence or customer-facing commitments require the project's separate launch and deployment approvals.

Before an SLO or recovery objective is called accepted, the evidence record must identify the exact requirement, owner, environment, service boundary, measurement window, data source, observed results, coverage and exclusions, failure scenarios, remediation, reviewer/approver, and revisit trigger. Use [`ADR-0011`](adr/ADR-0011-observability-and-slo-measurement.md), the [country/operations evidence register](COUNTRY-AND-OPERATIONS-PACKS.md), and the [data recovery obligations](DATA-OWNERSHIP-AND-CLASSIFICATION.md#failure-recovery-and-evidence-obligations) together; none alone demonstrates production readiness.

## Decisions left open

- Final Vercel/Convex and network service regions, provider contracts, country transfer review, measured latency, availability, failover and cost.
- Exact numerator/denominator, event sources, eligibility, error attribution, maintenance, time boundary, freshness, retention, aggregation and alert thresholds for each SLI.
- Numeric service latency/freshness/error-budget objectives beyond the existing approved planning targets.
- Numeric per-domain RPO/RTO, backup frequency/retention, recovery ordering, tenant-selective recovery, and acceptable recovery tests.
- Service-size forecasts, peak/burst profile, queue and fairness budgets, and what capacity/latency evidence is sufficient.
- Named service, network, finance, security, data/recovery, support and incident-command owners; staffed country coverage and feasibility of the current support objectives.
- Customer-facing SLA wording, status-page disclosures, maintenance windows, and any exceptions approved after evidence review.

MN-DEC-019 remains open until these decisions have accountable owners and the required evidence. This packet is a plan for collecting that evidence, not a substitute for it.
