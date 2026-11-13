# ADR-0009: File storage, exports, backups, and recovery

- **Date:** 11 November 2026
- **State:** Access and integrity requirements accepted; storage and backup design remain open
- **Owner:** Data/platform operations and privacy owners not named; assign in MN-PLAN-008
- **Evidence:** `AGENTS.md`; `TECHNICAL-SPECIFICATION.md` §§8–9, 14.3/14.8; `CORE-FLOWS-AND-DATA.md`; `REQUIREMENT-ROUTE-TRACEABILITY.md` cross-cutting export/destructive contracts; `DECISION-REGISTER.md` MN-DEC-018/019

## Context

MylesNet stores tenant branding and generated documents, issues sensitive exports, and must preserve audit/financial facts while respecting retention and legal holds. The technical direction allows Convex file storage or private S3-compatible storage; no final provider is selected. Backup scope, RPO/RTO, restore process, and retention values remain open.

## Decision

- Keep the storage-provider decision open between Convex file storage and private S3-compatible object storage until size, access, region, lifecycle, cost, and recovery evidence is available.
- Every file, export, and object is tenant-scoped where applicable. Resolve access server-side; do not expose durable public object URLs. Generated export access is permission-checked, scoped, expiring, and revalidated at retrieval.
- Tenant-data export requires explicit tenant Owner authorization and `export` permission, branch/data scope, MFA step-up, purpose/reason, field minimization, expiry, and audit. Platform export is limited to platform-owned records and non-personal tenant metadata. A SupportGrant never authorizes tenant-data export.
- Keep financial and audit history append-only; use explicit reversal/adjustment records. Export, offboarding, deletion, analytics, caches, files, logs, and backup expiry must honor category retention, legal holds, and country law. Never invent retention periods.
- Backup design must define encrypted scope for application data and assets, frequency, retention, isolation, restore order, tenant boundary proof, tenant-selective export/restore needs, residual backup expiry, and an isolated restore drill. Secret values remain in managed secret stores; whether/how secret references are included in disaster recovery needs an explicit design.

## Alternatives considered

- **Commit to Convex file storage now:** not supported by the current unresolved size, recovery, and retention evidence.
- **Commit to private S3-compatible storage now:** also open until region, contract, lifecycle, cost, and operational ownership are validated.
- **Make exports public or rely on a long-lived signed URL without reauthorization:** rejected.
- **Delete or rewrite settled financial/audit rows during cleanup:** rejected; use explicit corrections and retention rules.

## Security and operational tradeoffs

Managed storage may simplify integration but constrain region, restore, and tenant-selective operations; private object storage gives lifecycle/control options but adds key, policy, replication, and incident responsibilities. Exports concentrate PII and financial data and need least privilege, short-lived access, audit, and deletion tracking. Backup restoration risks resurrecting deleted/held records or cross-tenant exposure unless retention and tenant-boundary behavior is tested.

## Interfaces and sequences

See `IC-STORAGE-01`, `IC-EXPORT-01`, `IC-RECOVERY-01`, `SEQ-EXPORT-01`, `SEQ-OFFBOARD-01`, and `SEQ-RESTORE-01` in [`INTERFACE-AND-SEQUENCE-CONTRACTS.md`](INTERFACE-AND-SEQUENCE-CONTRACTS.md).

## Open items and revisit trigger

Choose storage after workload/region/cost and recovery review. The [hosting cost and capacity evidence plan](../HOSTING-COST-AND-CAPACITY-EVIDENCE.md) covers Vercel and Convex public pricing only; object-storage selection and backup costs remain out of scope there. Set category retention with counsel; define backup/restore ownership, frequency, RPO/RTO, isolated drill cadence, deletion from backups, and selective restore safeguards. The [MN-PLAN-029 backup and restore worksheet](../BACKUP-RESTORE-RECOVERY-DECISION-WORKSHEET.md) organizes these records without selecting values or claiming a completed drill. Revisit when legal holds, storage provider capabilities, data scale, or recovery targets change.
