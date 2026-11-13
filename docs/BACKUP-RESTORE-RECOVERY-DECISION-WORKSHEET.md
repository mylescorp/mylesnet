# Backup, restore, and recovery decision worksheet

**Task:** MN-PLAN-029

**Plan phase:** Phase 0 — close planning and turn decisions into build contracts

**Status:** Owner worksheet; provider, recovery targets, and operational choices remain open.

## Purpose and boundary

Use this worksheet with [ADR-0009](adr/ADR-0009-storage-exports-and-backups.md) and the [`IC-RECOVERY-01` / `SEQ-RESTORE-01` contracts](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md) to record accountable choices for backups and recovery. ADR-0009 remains the source for accepted access and integrity requirements. This worksheet records the missing scope, targets, evidence, and approvals.

It selects no storage provider, region, backup schedule, retention period, RPO/RTO, restore mode, staffing plan, or production process. Provider documentation is not a successful restore drill. No live account, production service, or tenant data was used; this packet does not authorize a real backup, restore, or promotion.

## Approved boundaries to preserve

- Keep storage-provider selection open until workload, region, lifecycle, cost, recovery, and ownership evidence is available.
- Scope access server-side, isolate tenant objects, keep exports permission-checked and expiring, and preserve append-only financial/audit history.
- Apply category retention and legal holds to backup expiry and restoration. Do not invent retention values or restore deleted/held data without an approved rule.
- Use isolated restore exercises. Record measured results and tenant-boundary/integrity evidence. Production recovery or promotion requires its own authorization.
- Keep secrets in managed secret stores; explicitly decide whether and how secret references are part of disaster recovery without copying secret values into this record.

These controls come from [ADR-0009](adr/ADR-0009-storage-exports-and-backups.md), [MN-DEC-018/019](DECISION-REGISTER.md), and the [Phase 0 restore sequence](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md).

## Data scope and recovery targets

Record each system and data class separately. State whether the data is authoritative, a rebuildable projection, externally owned evidence, or a derived artifact. Do not assume every external provider's records are backed up by MylesNet.

| System or data domain | In scope? / source of truth | Backup/export mechanism and isolation | Recovery point objective | Recovery time objective | Restore order/dependencies | Evidence, accountable owner, approval |
|---|---|---|---|---|---|---|
| Convex tenant/platform application records | Open | Open | Open | Open | Open | Open |
| Tenant assets, generated files, and export artifacts | Open by category | Open | Open | Open | Open | Open |
| FreeRADIUS/PostgreSQL AAA and accounting records | Open; distinguish policy state, current sessions, and durable accounting | Open | Open | Open | Open | Open |
| MylesNet-managed payment/event and financial evidence | Open; distinguish immutable ledgers from provider-held records | Open | Open | Open | Open | Open |
| Configuration, deployment metadata, and secret references | Open; never record secret values | Open | Open | Open | Open | Open |
| Derived analytics, caches, and rebuildable projections | Open | Open | Open | Open | Open | Open |

For every recovery target, state its measurement boundary, start/stop event, allowed data-loss window, service availability scope, and evidence source. Keep numeric targets blank until the service owner, data owner, and operations owner agree to a measurable value.

## Backup, retention, and isolation decisions

| Decision | Required owner record | Evidence before approval | Status |
|---|---|---|---|
| Storage provider and regions | Candidate comparison, data residency/transfer impact, access boundaries, lifecycle and migration plan | Current terms/capabilities, workload profile, cost and legal review | Open |
| Backup scope and consistency | Systems, data classes, asset inclusion, point-in-time consistency, dependencies, exclusions, and source-of-truth treatment | Documented data flow and restore-order review | Open |
| Encryption and recovery access | Key ownership, rotation, separation of duties, break-glass access, and secret-reference recovery | Security review and access/audit design; no secret values in the worksheet | Open |
| Backup frequency and retention | Schedule and period per data class; backup expiry and legal-hold interaction | Owner-approved recovery targets plus country/data retention approval | Open |
| Tenant boundary and selective recovery | Tenant/branch selection, blast radius, cross-tenant proof, restore into isolated environment, export/restore relationship | Negative isolation cases and owner-approved recovery procedure | Open |
| Deletion and restore interaction | Treatment of offboarded/deleted, corrected, and legally held records in snapshots and restored state | Retention/legal-hold policy and repeatable restore evidence | Open |
| Operational ownership | Backup monitoring, failure alert, restore initiator/approver, incident lead, support and recovery roles | Named appointments and accepted runbooks | Open |

## Restore drill and recovery evidence

For each service/data-domain row above, define a repeatable isolated exercise before claiming readiness.

| Exercise | Procedure and acceptance evidence | Owner / reviewer | Status |
|---|---|---|---|
| Isolated full-service restore | Restore selected encrypted snapshots/assets in a disposable environment; record versions, dependencies, elapsed time, recovered point, and integrity checks | Open | Open |
| Tenant/branch selective restore | Prove requested scope is restored without exposing or mutating another tenant; document records that cannot safely be restored | Open | Open |
| Financial, audit, and AAA integrity | Reconcile counts, immutable history, event ordering/idempotency, and source provenance against an approved recovery baseline | Open | Open |
| Retention, deletion, and legal holds | Prove restore/expiry does not improperly resurrect deleted data or remove held data; document remaining backup-copy expiry | Open | Open |
| Failure and interruption | Exercise incomplete backup, corrupt/unavailable snapshot, failed restore, and safe retry/escalation; retain auditable evidence | Open | Open |
| RPO/RTO measurement | Measure agreed start/end boundaries and recovered data point; compare with approved service-specific targets | Open | Open |
| Restore authorization and promotion boundary | Prove isolated test credentials and approvals; production promotion is a separate change with explicit authorization | Open | Open |

Set drill cadence, environment, synthetic-data method, evidence retention, independent review, and remediation owner before calling a drill complete. This worksheet records no executed drill.

## Related decisions and closure

Complete a dated record for each system/data domain, recovery target, retention category, storage/provider choice, access role, and exercise. Name an accountable service/data owner and an independent reviewer through the [Phase 0 owner appointment register](PHASE-0-OWNER-APPOINTMENT-REGISTER.md). Attach redacted evidence; do not include keys, sensitive exports, or live customer data.

Before backup/recovery requirements can be accepted for implementation, reconcile this worksheet with ADR-0009, `IC-RECOVERY-01`, `SEQ-RESTORE-01`, the data classification/retention map, country transfer requirements, and the operations runbook. Record provider-supported capabilities separately from MylesNet measurements. Leave unsupported or unmeasured targets explicitly open.

## Related artifacts

- [ADR-0009 — file storage, exports, backups, and recovery](adr/ADR-0009-storage-exports-and-backups.md)
- [Interface and sequence contracts](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md)
- [Data ownership and classification](DATA-OWNERSHIP-AND-CLASSIFICATION.md)
- [Phase 0 owner appointment register](PHASE-0-OWNER-APPOINTMENT-REGISTER.md)
- [Readiness decisions](READINESS-DECISIONS.md)
- [Implementation plan](IMPLEMENTATION-PLAN.md)
