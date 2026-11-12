# Guided CSV Tenant Migration Contract

**Task:** MN-PLAN-011 · **Plan phase:** Phase 0 — close planning and turn decisions into build contracts<br>
**Status:** Planning contract draft; open choices below require owner decisions before implementation.<br>
**Authority:** [Project charter](../AGENTS.md), [MN-DEC-024](DECISION-REGISTER.md), [guided CSV flow](CORE-FLOWS-AND-DATA.md#guided-tenant-csv-import-v1), [API/error/event backlog](API-ERROR-EVENT-CONTRACT-BACKLOG.md), and [Phase 0 plan](IMPLEMENTATION-PLAN.md#phase-0--close-planning-and-turn-decisions-into-build-contracts).

This contract turns the approved v1 guided-CSV direction into a reviewable import lifecycle. It does not choose entity schemas, retention periods, numeric limits, or failure policy where the project records those as open. It authorizes no product implementation or import against live tenant data.

## 1. Scope and entity coverage

V1 migration uses guided CSV imports with server-side validation, duplicate checks, a dry-run preview, explicit commit, an audit trail, and safe batch-scoped rollback. Named system-specific connectors are deferred beyond v1.

The current canonical requirements identify subscribers/customers and leads as import entities. The route inventory also includes import/export paths for tickets and payments; those paths do not, by themselves, approve those data types for migration. Their inclusion must be confirmed before templates or write behavior are designed. Financial imports need a separate reconciliation and immutable-history decision.

| Entity group | Current contract status | Required closure |
|---|---|---|
| Subscribers/customers | Import is specified in the tenant-console requirements. | Approve field schema, identity matching, branch assignment, and treatment of credentials, plan references, expiry, and existing service state. |
| Leads | Import is specified in the tenant-console requirements. | Approve field schema, duplicate matching, assignment, and conversion/history behavior. |
| Tickets and payments | Import/export routes appear in the route inventory; migration scope is not settled here. | Product and data owners confirm inclusion. Payment imports additionally require finance approval, source reconciliation, and reversal/history rules. |
| Other operational entities | Not approved by this contract. | Add only through an explicit scope decision and entity-specific mapping, authorization, and rollback review. |

Every accepted import is bound to one server-verified tenant and an explicit tenant-wide or branch scope permitted for the actor. A row cannot choose or override its authoritative tenant or branch by supplying an identifier in the file.

## 2. Approved behavior and proposed contract

### Approved by existing project decisions

- Validate the uploaded file, headers, required fields, formats, and tenant/branch references on the server.
- Check duplicates within the file and against the selected tenant scope. Never silently overwrite or merge existing records.
- Present row-level ready, warning, error, and duplicate outcomes before any operational record is written.
- Require an explicit commit. Process the job idempotently in verified tenant/branch scope and show progress and per-row results.
- Record who uploaded, validated, committed, or rolled back; selected entity and scope; mapping/template version; source-file digest; timestamps; and result counts. Do not put row contents, credentials, or other secrets in logs.
- Permit only safe batch-scoped rollback. Never erase audit or immutable financial history, overwrite later edits, or delete records that other work depends on. Preserve unsafe rows and provide an audited exception/reconciliation path.
- Apply authorization, bounded work, secure temporary-file handling, retention controls, and queue/backpressure policy.

### Proposed implementation contract pending owner review

- Treat upload/validation/preview as a job whose metadata and audit events may persist, while the dry run creates or changes **no tenant operational records**.
- On commit, recheck the actor's current membership, tenant state, permission, branch scope, and relevant record versions. A row that became invalid or duplicated after preview returns a safe conflict outcome; the job must not overwrite it.
- Give each job and source row a stable identity. A retry of the same job/row must not create a second effect. Final key format, retention, and transaction boundaries remain open.
- Preserve successful row outcomes across retries and make each retried failure visible. Whether a job commits atomically or allows row-level partial commit is an owner decision, not assumed here.
- Permit rollback only for rows created by the identified job and only when their current state and dependencies still meet the approved safe-rollback rule. Rollback itself is an audited operation with per-row outcomes; unsafe rows remain intact for reconciliation.

## 3. Lifecycle and sequence

The application should expose the standard asynchronous-job lifecycle from `API-ERROR-EVENT-CONTRACT-BACKLOG.md` (`accepted`, `pending`, `running`, `succeeded`, `failed`, `quarantined`, `expired`) and associate import-specific stage and row outcomes with it. Exact persisted states, transitions, and cancellation behavior need schema approval.

```mermaid
sequenceDiagram
  actor Staff
  participant Web as Tenant console
  participant App as Authorized import service
  participant Stage as Restricted temporary storage
  participant Job as Bounded job worker
  participant Data as Tenant records and audit

  Staff->>Web: Select entity and permitted scope
  Web->>App: Upload CSV and mapping version
  App->>App: Recheck identity, tenant, permission, and branch scope
  App->>Stage: Store protected source and digest
  App-->>Web: Accepted job ID and correlation ID
  Job->>Stage: Read staged source
  Job->>Data: Read scoped records for validation only
  Job-->>Web: Preview rows, warnings, errors, and duplicates
  Staff->>Web: Confirm explicit commit
  Web->>App: Commit job ID and preview version
  App->>App: Recheck current authority and preview freshness
  App->>Job: Enqueue idempotent commit
  Job->>Data: Apply approved row outcomes and audit events
  Job-->>Web: Progress and row-level result summary
  Staff->>Web: Request safe batch rollback
  Web->>App: Roll back job ID with reason
  App->>App: Recheck authority and rollback eligibility
  App->>Job: Enqueue idempotent rollback
  Job->>Data: Reverse only eligible job-created records; audit exceptions
  Job-->>Web: Rollback result and rows requiring reconciliation
```

This is the planning sequence. It does not specify a storage provider, queue implementation, API schema, or a numeric file/job limit.

## 4. Template, validation, and duplicate handling

Each entity needs a versioned template and mapping specification that states source header, destination field, type/format, requiredness, allowed transformation, and safe preview treatment. The service must reject unsupported or ambiguous mappings rather than guess. A preview identifies rows by a stable source-row reference and returns actionable, non-secret reasons.

Validation must cover file structure, field syntax and domain rules, cross-field constraints, referenced tenant/branch records, duplicates inside the file, and conflicts with current records in the selected scope. Exact duplicate keys, normalization rules, “skip versus resolve” behavior, and field-level update policy are entity-specific open decisions. No entity may silently merge into or overwrite an existing record.

Before commit, the actor resolves or explicitly skips rows that the approved entity policy marks as blocking. If source records change after preview, revalidate at commit and return a conflict or refreshed preview rather than applying a stale overwrite. Use the shared safe error families, including `VALIDATION_FAILED`, `CONFLICT`, `PERMISSION_DENIED`, `TENANT_SCOPE_MISMATCH`, `RATE_LIMITED`, and `INTERNAL_ERROR`; do not return stack traces or internal/provider details.

File extension/MIME handling, encoding, delimiter, header normalization, date/phone formats, field transformation rules, maximum file size, maximum rows, and per-tenant concurrency remain open. These values must be agreed and enforced server-side before implementation.

## 5. Commit, partial failure, retry, and reconciliation

Commit is a separate user-confirmed transition from preview. The server records the commit request, caller, scope, mapping version, preview version, and correlation/idempotency reference before processing. The worker rechecks authorization when it executes and whenever protected output is read.

The following choices require a product/data and engineering decision:

- **Atomicity:** all-or-nothing job, or row-level partial commit with eligible rows committed while failed rows remain retryable.
- **Stale preview:** reject the entire commit for refresh, or reject only rows whose source state changed.
- **Retry unit:** whole job with per-row deduplication, or explicit retry of failed rows only.
- **Cancellation:** allowed stages and outcome when a job is cancelled during validation or commit.
- **Concurrency:** whether a tenant may run multiple jobs concurrently and how interactive work is protected.

Whatever policy is approved, a retry must not repeat a completed row's mutation; a changed request cannot reuse an idempotency key; and the UI must distinguish queued/running, completed, failed, quarantined, and expired work. Partial outcomes must be reconciled visibly and must not be represented as full success.

## 6. Rollback and immutable history

Rollback is compensation for eligible records created by one import job; it is not deletion of the job, audit trail, or source history. It must use the existing tenant authorization and audit contract, require an explicit reason, record the initiating actor and outcome, and remain idempotent.

The service must refuse automatic reversal when a record has been subsequently edited, referenced by dependent operational work, activated into service, or used to create financial, security, or legal history, unless an approved entity-specific compensation rule exists. It preserves that row and presents the reason plus a reconciliation action. Financial and audit records remain append-only or use explicit reversals under their canonical policies.

Exact rollback eligibility by entity, dependency detection, archive versus compensating mutation, cutoff window, approver/MFA requirements, and treatment of partial rollback remain open. Do not ship a broad “undo import” that hard-deletes records without these decisions.

## 7. Data protection and operations

- Use restricted temporary storage for uploaded source files and generated preview artifacts. Enforce tenant and actor scope on staging reads and result access.
- Store the approved source digest and necessary job metadata; keep raw row contents, credentials, payment secrets, and full extracts out of ordinary logs, telemetry, and audit payloads.
- Keep job metadata under `RET-OPERATIONAL`. Apply `RET-EXPORT` to source files, preview artifacts, row-error files, and result exports, including expiring/revocable access and applicable legal holds. Exact periods are open; this contract does not invent a duration.
- Bound upload, parsing, batch, concurrency, retry, and result sizes after workload review. Apply queue backpressure so bulk imports cannot starve interactive tenant operations, payment integrity, or network work.
- Expose progress, timestamps, safe row-level outcomes, and a correlation ID. Redact personal values from diagnostics; show sensitive values only to actors currently authorized to see them.
- Recheck the actor's permission when uploading, viewing the preview/result, committing, retrying, and rolling back. Client-supplied tenant/branch IDs never grant access.

## 8. Acceptance cases to implement after authorization

| ID | Acceptance case |
|---|---|
| CSV-AC-01 | Reject an unauthenticated actor, removed membership, unavailable tenant, missing import permission, or out-of-scope branch at each protected operation. |
| CSV-AC-02 | Reject malformed files, invalid headers/fields/references, and over-limit work with stable safe errors and no operational-record writes. |
| CSV-AC-03 | Preview duplicates found within the file and in the selected tenant scope; prove no existing record is silently overwritten or merged. |
| CSV-AC-04 | Show a non-mutating preview with stable row references, ready/warning/error/duplicate dispositions, safe reasons, scope, mapping version, and job correlation ID. |
| CSV-AC-05 | Require explicit commit, recheck permission/scope and stale conflicts, and create no records before commit. |
| CSV-AC-06 | Exercise duplicate commit requests, worker retries, interrupted jobs, and partial failures without repeating completed row effects; display reconciled row outcomes. |
| CSV-AC-07 | Roll back only eligible records created by the named job; preserve later edits, dependent records, audit history, and financial/security/legal history; expose exceptions. |
| CSV-AC-08 | Enforce source/preview/result retention and tenant isolation; confirm logs and diagnostics contain no raw rows, credentials, or unnecessary PII. |
| CSV-AC-09 | Demonstrate queue limits/backpressure keep interactive work responsive under the approved workload profile. |

These are future acceptance cases, not tests run by this planning task. Numeric limits, exact schemas, job-state persistence, permissions, atomicity, and rollback policy must be approved before the cases can become executable specifications.

## 9. Owner decisions required before implementation

| Open ID | Decision needed | Accountable role to appoint | Evidence/approval required |
|---|---|---|---|
| CSV-OPEN-01 | Confirm entity coverage, including ticket/payment import routes and whether imports are limited initially to subscribers/customers and leads. | Product owner; Finance owner for financial entities | Approved scope and per-entity acceptance mapping. |
| CSV-OPEN-02 | Approve schemas, downloadable templates, mapping versioning, transformations, and required/optional fields per entity. | Product/data owner | Versioned template and mapping catalog with migration examples. |
| CSV-OPEN-03 | Define duplicate keys, normalization, candidate presentation, conflict handling, and whether any entity permits field updates. | Product/data owner; security/privacy owner for sensitive fields | Entity-specific duplicate rules and negative/positive cases. |
| CSV-OPEN-04 | Set file/row/result limits, concurrency, backpressure, and rate limits. | Engineering/operations owner | Workload model and measured capacity envelope. |
| CSV-OPEN-05 | Approve source-file, preview, error, and result retention; redaction and legal-hold behavior. | Data/privacy owner and country counsel | Retention schedule and data-transfer/legal review. |
| CSV-OPEN-06 | Choose atomic versus partial commit, stale-preview behavior, retry/cancellation semantics, and reconciliation ownership. | Product/data and engineering owners | Approved job state model and failure/recovery acceptance matrix. |
| CSV-OPEN-07 | Define safe rollback eligibility, dependency rules, cutoffs, archival/compensation behavior, and approval/MFA requirements. | Product/data owner; Finance/Security owners where applicable | Entity-specific rollback matrix preserving immutable history. |
| CSV-OPEN-08 | Map permissions and sensitive-data visibility for upload, preview, commit, retry, export, and rollback. | Security owner and product owner | Approved role/branch access matrix and step-up policy. |

No individual is assigned by this document. The project owner must appoint the listed owners; they must accept the decisions and provide the evidence before the contract is treated as approved for implementation.

## Related artifacts

- [Phase 0 implementation plan](IMPLEMENTATION-PLAN.md#phase-0--close-planning-and-turn-decisions-into-build-contracts)
- [Guided tenant CSV flow](CORE-FLOWS-AND-DATA.md#guided-tenant-csv-import-v1)
- [API/error/event contract backlog](API-ERROR-EVENT-CONTRACT-BACKLOG.md#contract-backlog), especially `JOB-001`
- [Data ownership map](DATA-OWNERSHIP-AND-CLASSIFICATION.md#critical-record-and-event-ownership-map), especially `REC-016`
- [MN-DEC-024](DECISION-REGISTER.md)
