# Support Coverage and Incident Operations Worksheet

- **Task:** MN-PLAN-032 — prepare owner decisions for country support coverage and incident operations
- **Phase:** 0 — Close planning and turn decisions into build contracts
- **Status:** Planning worksheet; no staffing plan, support commitment, on-call rota, maintenance window, or response target is approved here.
- **Related requirements:** `READINESS-DECISIONS.md` §Platform operations and design; `COUNTRY-AND-OPERATIONS-PACKS.md` `CP-SUPPORT-01`; `TECHNICAL-SPECIFICATION.md` §§10–11; `IMPLEMENTATION-PLAN.md` Phase 0 items 5, 6, and 8.

## Purpose and boundary

The country pack records a proposed weekday support window and 24/7 major-incident route, but explicitly says these are planning targets, not staffed service commitments. Owners still need to confirm country calendars, languages, channels, staffing, on-call coverage, escalation responsibilities, tenant communications, and maintenance operations.

This worksheet organizes those choices for Kenya, Uganda, and Tanzania. It does not select schedules, assign named people, set service-level objectives, commit to support availability, create runbooks, or authorize live operations. Keep all proposed values open until owners provide feasibility evidence and sign-off.

Scope boundaries:

- MN-PLAN-019, owned separately, covers service objectives and quantitative operational evidence. Use service-specific severity, acknowledgement/update targets, alert thresholds, and measurement plan only after that work is approved; this worksheet sets none of those values and does not modify that task or PR.
- MN-PLAN-029 covers backup, restore, continuity, and RPO/RTO decisions. This worksheet records who coordinates incident communications and recovery handoff only; it does not set recovery targets.
- MN-PLAN-020 and country counsel own legal/privacy obligations, including breach notification deadlines. This worksheet does not interpret laws or set statutory timelines.
- No provider account, support inbox, status page, pager, customer contact, or incident channel is configured or contacted during planning.

## Existing baselines to preserve

- Launch planning covers Kenya, Uganda, and Tanzania; country approval remains separate for each.
- The country support pack proposes weekday 08:00–17:00 EAT coverage excluding public holidays and a 24/7 on-call route for approved major incidents. These are unstaffed planning targets, not a commitment or an accepted schedule.
- English/Kiswahili product language support is approved; country copy, legal text, and support-staff language coverage remain open in `CP-LOCALE-01`.
- A support lead and incident commander are required owner roles, but individuals remain unassigned in the [Phase 0 owner register](PHASE-0-OWNER-APPOINTMENT-REGISTER.md) and [story map](PRIORITIZED-V1-STORY-MAP.md).
- Keep tenant communications scoped: do not disclose another tenant's information or details of a restricted security investigation. Log incident and maintenance communication actions without copying unnecessary subscriber data.

See the [country and operations packs](COUNTRY-AND-OPERATIONS-PACKS.md), [service-objective ADR](adr/ADR-0011-observability-and-slo-measurement.md), and [implementation status](IMPLEMENTATION-STATUS.md) for adjacent evidence and ownership.

## Country support decision matrix

Complete one record for each country. Copy the table when more than one service or customer class needs different coverage.

| Decision field | Kenya | Uganda | Tanzania |
|---|---|---|---|
| Accountable support owner and deputy | Open | Open | Open |
| Staffed service hours, time zone, weekends, and public-holiday calendar | Open | Open | Open |
| Supported languages and escalation translation coverage | Open | Open | Open |
| Customer-facing channels and verified contact path | Open | Open | Open |
| Ticket intake, severity handoff, and ticket owner coverage | Open | Open | Open |
| Out-of-hours incident route, primary/backup rota, and handoff evidence | Open | Open | Open |
| Country-specific provider, regulator, counsel, and ISP escalation contact roles | Open | Open | Open |
| Tenant status/incident update channel and accessibility requirements | Open | Open | Open |
| Support system access, tenant verification, privacy, and retention review | Open | Open | Open |
| Staffing capacity, coverage gaps, cost assumptions, and approval evidence | Open | Open | Open |

Do not fill these cells with example hours as accepted policy. Record a proposed value, the staffed capacity that supports it, its evidence source, decision owner, reviewer, and approval date in a separate completed record.

## Cross-service incident and maintenance ownership

For each service class in the technical specification, record accountable roles and handoffs. A role can be shared only when independent approval or security separation is not required.

| Service or event class | Service owner | Primary responder and backup | Incident commander / decision authority | External escalation owner | Tenant communication owner | Recovery/release handoff |
|---|---|---|---|---|---|---|
| Web application, identity, and tenant access | Open | Open | Open | Open | Open | Open |
| Convex data, realtime projections, and background jobs | Open | Open | Open | Open | Open | Open |
| Payment callbacks, ledgers, reconciliation, and messaging | Open | Open | Open | Open | Open | Open |
| FreeRADIUS, PostgreSQL AAA, and subscriber access | Open | Open | Open | Open | Open | Open |
| Router worker, tunnels, and portal delivery | Open | Open | Open | Open | Open | Open |
| Security, privacy, or suspected tenant-boundary incident | Open | Open | Open | Open | Open | Open |

For every row, decide the incident declaration authority, backup when that person is unavailable, tenant-impact review, provider/ISP/country-counsel escalation path, service-disable or rollback authority, evidence custodian, resolution note, and post-incident review owner. Quantitative severity, alert, and notification thresholds remain in the separate service-objective work.

## Maintenance and customer communications

Record a decision for each maintenance class without inventing a calendar or notice period:

| Decision | Owner record |
|---|---|
| Who proposes, risk-assesses, approves, and executes planned maintenance | Open |
| How affected countries, tenants, services, and dependencies are identified | Open |
| Which maintenance needs advance notice, and the owner of approved notice timing | Open |
| How emergency work is authorized, communicated, monitored, and reviewed | Open |
| How tenant impact, degraded modes, disabled features, and workarounds are described | Open |
| Rollback/stop authority and handoff to backup, payment, RADIUS, or provider recovery | Open |
| Where notices, approvals, changes, and completion evidence are retained | Open |
| How delivery failures or tenants without an eligible contact are handled | Open |

Public status-page publication, customer notices, or external escalations require a later approved operating process and designated accounts. No communication is sent by completing this worksheet.

## Sign-off evidence and readiness checks

The [Phase 0 owner appointment register](PHASE-0-OWNER-APPOINTMENT-REGISTER.md) tracks role appointments. For each country and service class, the appointed support lead, incident commander, release/operations owner, security/privacy owner, and relevant country owner must record:

- proposed coverage and staffing capacity, including a deputy and leave/holiday handover;
- approved support channels, identity/tenant verification, language, accessibility, and data-handling controls;
- on-call rota ownership and tested escalation route for the approved major-incident classes;
- tenant impact assessment and communication workflow, including who may approve public or tenant-facing notices;
- planned and emergency maintenance approval, monitoring, stop/rollback, and recovery handoff;
- evidence source, limitations, residual coverage gap, accountable owner, reviewer, approval date, and revisit trigger.

Before country pilot readiness, also require an approved service-objective and alert contract from MN-PLAN-019; recovery/restore coordination from MN-PLAN-029; country legal/privacy validation from MN-PLAN-020; and a synthetic or sandbox exercise of the staffed handoffs. Exercise evidence does not itself approve production support commitments.

Until those records exist, `CP-SUPPORT-01`, support staffing, incident command, tenant communication, and maintenance readiness remain open. This worksheet does not close Phase 0 or authorize implementation, production operations, or country launch.
