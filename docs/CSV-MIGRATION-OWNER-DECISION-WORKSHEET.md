# CSV migration owner decision worksheet

**Task:** MN-PLAN-028

**Plan phase:** Phase 0 — close planning and turn decisions into build contracts

**Status:** Owner sign-off worksheet; every policy value remains open.

## Purpose and boundary

Use this worksheet with the [guided CSV migration contract](CSV-MIGRATION-CONTRACT.md) to record the owner decisions that must be settled before implementation. The contract remains the source for lifecycle requirements, approved behavior, security boundaries, and acceptance cases; this companion provides fields for entity-by-entity approval and the evidence behind each decision.

This worksheet approves no entity, schema, duplicate rule, limit, retention period, partial-commit policy, rollback rule, permission, or customer instruction. Do not implement from blank or proposed fields. No live tenant data, uploads, provider services, or product code are part of this planning task.

## Approved behavior to preserve

These existing decisions remain unchanged:

- V1 uses guided CSV imports; named system-specific connectors are deferred.
- Validate on the server, check duplicates within the file and selected tenant scope, and show a non-mutating dry-run preview before any write.
- Require an explicit commit, maintain an audit trail, and permit only safe batch-scoped rollback.
- Never silently merge or overwrite records. Preserve financial, audit, security, and legal history.
- Enforce current server-verified tenant and permitted branch scope at every protected operation. File-supplied identifiers do not authorize access.

See [MN-DEC-024](DECISION-REGISTER.md), [Readiness decisions](READINESS-DECISIONS.md), and the [contract's approved behavior](CSV-MIGRATION-CONTRACT.md#2-approved-behavior-and-proposed-contract).

## Entity coverage and template approval

Complete one line per candidate entity. Mark the inclusion decision explicitly, including exclusions; a route in the inventory alone does not approve an import.

| Entity | Current documented scope | Include in V1? | Template/mapping version and evidence | Branch/scope rules | Sensitive fields and handling | Owner, date, approval evidence |
|---|---|---|---|---|---|---|
| Subscribers/customers | Specified candidate; field and state behavior open | Open | Open | Open | Open | Open |
| Leads | Specified candidate; field and history behavior open | Open | Open | Open | Open | Open |
| Tickets | Route exists; migration inclusion is not approved | Open | Open | Open | Open | Open |
| Payments | Route exists; financial migration needs separate reconciliation approval | Open | Open | Open | Open | Open |
| Other operational entities | Not approved by the current contract | Open; require explicit scope approval | Open | Open | Open | Open |

For each included entity, attach its versioned field map: source header, destination field, type/format, requiredness, permitted transformation, reference lookup, validation, preview redaction, and behavior on missing or invalid values. Reject ambiguous mappings rather than guessing.

## Decision and evidence register

Complete the owner, decision, date/version, evidence, and approval fields for each `CSV-OPEN` item in the [contract's open-decision register](CSV-MIGRATION-CONTRACT.md#9-owner-decisions-required-before-implementation).

| Contract ID | Decision record to complete | Accountable roles | Decision / evidence reference | Status |
|---|---|---|---|---|
| `CSV-OPEN-01` | Included and explicitly excluded entities; per-entity acceptance scope | Product owner; Finance for financial data | Open | Open |
| `CSV-OPEN-02` | Versioned templates, field mappings, transformations, required and optional fields | Product/data owner | Open | Open |
| `CSV-OPEN-03` | Duplicate keys, normalization, match presentation, resolution and update rules | Product/data owner; Security/Privacy for sensitive fields | Open | Open |
| `CSV-OPEN-04` | File, row, result, concurrency, rate, and queue limits supported by a workload model | Engineering/operations owner | Open | Open |
| `CSV-OPEN-05` | Source, preview, error, and result retention, redaction, legal holds, and transfer controls | Data/privacy owner; country counsel | Open | Open |
| `CSV-OPEN-06` | Atomicity/partial commit, stale preview, retry, cancellation, concurrency, and reconciliation owner | Product/data and engineering owners | Open | Open |
| `CSV-OPEN-07` | Rollback eligibility, dependencies, later edits, cutoff, compensation/archive, and approval/MFA | Product/data owner; Finance/Security where applicable | Open | Open |
| `CSV-OPEN-08` | Upload, preview, commit, retry, result/export, and rollback role/branch permissions | Product and security owners | Open | Open |

No individual is assigned by this worksheet. Record named decision-makers in the [Phase 0 owner appointment register](PHASE-0-OWNER-APPOINTMENT-REGISTER.md) before approval.

## Operational limits and retention record

Keep each value unset until the accountable owner supplies evidence. Include the unit, scope, environment, measurement period, and revisit trigger.

| Setting | Approved value and unit | Scope / enforcement point | Evidence and owner | Status |
|---|---|---|---|---|
| Maximum upload size | Open | Server-side | Open | Open |
| Maximum rows and field/result size | Open | Per job and entity | Open | Open |
| Accepted encoding, delimiter, headers, date and phone formats | Open | Versioned parser/template | Open | Open |
| Concurrent jobs, request rate, batch size, queue budget/backpressure | Open | Per tenant and shared service | Workload model and measured capacity | Open |
| Source-file and preview-artifact retention | Open | Restricted temporary storage | Country/data retention approval | Open |
| Row errors and result exports retention/access expiry | Open | Authorized actor and tenant scope | Country/data retention approval | Open |
| Legal holds and deletion behavior | Open | Applicable data classes | Counsel and data/privacy approval | Open |

## Failure, retry, and rollback approval

Record one approved behavior per case and tie it to the relevant acceptance cases in the contract. Keep the job and per-row result semantics consistent across preview, commit, retry, cancellation, and reconciliation.

| Case | Approved behavior | Owner and evidence | Status |
|---|---|---|---|
| Commit atomicity and partial outcomes | Open: choose and specify; no default assumed | Open | Open |
| Preview becomes stale before commit | Open: whole-job refresh or per-row conflict behavior | Open | Open |
| Retry unit and idempotency retention | Open: job/row retry boundary and key lifecycle | Open | Open |
| Cancellation during validation or commit | Open: allowed stages and resulting row/job state | Open | Open |
| Reconciliation of partial or quarantined rows | Open: accountable queue owner and completion evidence | Open | Open |
| Rollback after later edits, activation, dependencies, or immutable history | Open: per-entity eligibility and safe compensation | Open | Open |
| Rollback cutoff, approver/MFA, and partial rollback | Open | Open | Open |

Do not approve a general hard-delete or “undo” operation. Preserve rows that are unsafe to reverse and define their audited reconciliation path.

## Access, customer guidance, and acceptance sign-off

For each operation, record the permission, tenant/branch rule, sensitive-value visibility, step-up requirement, and acceptance evidence. Confirm that help text explains supported files, required preparation, duplicate outcomes, preview warnings, explicit commit, failure recovery, and rollback limits in the approved launch languages.

| Operation | Role/permission | Tenant and branch rule | Sensitive-value visibility / step-up | Acceptance evidence | Status |
|---|---|---|---|---|---|
| Upload and validate | Open | Open | Open | Open | Open |
| View preview and job result | Open | Open | Open | Open | Open |
| Commit or retry | Open | Open | Open | Open | Open |
| Export row errors/results | Open | Open | Open | Open | Open |
| Request rollback and view exceptions | Open | Open | Open | Open | Open |

Before this packet is approved for implementation, the owners should provide dated approval for each included entity, all eight `CSV-OPEN` items, workload/retention limits, failure and rollback behavior, access rules, and customer guidance. Update the contract, role/action traceability, data-retention record, and implementation acceptance cases to match those decisions. Until then, MN-DEC-024 remains approved only at the high-level workflow described above; details remain open.

## Related artifacts

- [Guided CSV migration contract](CSV-MIGRATION-CONTRACT.md)
- [Phase 0 implementation plan](IMPLEMENTATION-PLAN.md#phase-0--close-planning-and-turn-decisions-into-build-contracts)
- [MN-DEC-024](DECISION-REGISTER.md)
- [Readiness decisions and open requirements](READINESS-DECISIONS.md)
- [Phase 0 owner appointment register](PHASE-0-OWNER-APPOINTMENT-REGISTER.md)
- [Requirement, route, and action traceability](REQUIREMENT-ROUTE-TRACEABILITY.md)
