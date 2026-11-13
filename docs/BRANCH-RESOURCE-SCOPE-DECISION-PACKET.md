# Branch resource scope decision packet

- **Task:** MN-PLAN-017 · **Plan phase:** Phase 0 — close planning and turn decisions into build contracts
- **Status:** Analysis and acceptance packet only. `MN-DEC-016` remains open; no resource-to-branch policy or database schema is approved here.
- **Authority:** `AGENTS.md`; `DECISION-REGISTER.md` MN-DEC-016; `TECHNICAL-SPECIFICATION.md` §§6, 14.2–14.3; ADR-0003; `DATA-OWNERSHIP-AND-CLASSIFICATION.md`; `REQUIREMENT-ROUTE-TRACEABILITY.md`.

## Purpose and boundary

V1 supports tenants with multiple branches, and every operation must enforce server-resolved tenant and branch authority. The remaining decision is how each resource family belongs to branches, including shared resources, records awaiting assignment, inherited scope, indexing, and safe migration. This packet organizes those decisions for the appointed product, architecture, and security owners; it does not choose them.

### Constraints already approved

- Every tenant-owned record, event, file/object, export, search document, cache key, and job payload has a server-resolved `tenantId`. Tenant ownership is immutable except through an audited migration.
- Client-supplied tenant or branch identifiers never authorize access. Resolve current tenant membership, role, permission, branch grants, tenant state, resource state, and resource ownership on each operation and live delivery.
- The Tenant Owner is tenant-wide. Other tenant roles can be tenant-wide or assigned to selected branches. A member may combine role assignments by scope; tenant-wide grants apply throughout the tenant and branch grants apply within their selected branches. Admin delegation stays within the Admin's own permission ceiling.
- Scope changes recompute effective access and revoke no-longer-authorized subscriptions/data immediately.
- A tenant-wide record is explicit. `null` cannot ambiguously mean tenant-wide, unassigned, or invalid.
- Shared service-plan templates may have branch-specific local price, currency, and availability. This does not approve a physical model for those relationships.

These constraints come from the charter, technical specification, ADR-0003, and the approved shared-template direction. They do not determine which records are branch-owned, tenant-wide, shared, inherited, or temporarily unassigned.

## Decision dimensions

For each resource family, the owner decision must answer each applicable dimension. The labels below are analysis vocabulary, not frozen enum values or a final schema.

| Dimension | Questions to resolve |
|---|---|
| Ownership | Is the record tenant-wide, owned by exactly one branch, or intentionally shared across a set of branches? Can that answer vary by subtype or lifecycle state? |
| Assignment cardinality | Does a record have one owning branch, several authorized branches, or none while awaiting assignment? Is sharing direct, through a template/parent, or via an explicit relationship? |
| Scope inheritance | Does a child inherit scope from its parent? Can a child have a narrower or broader scope? What happens if the parent moves, is archived, or is reassigned? |
| Unassigned state | Which workflows may create an unassigned record? Which roles may find, claim, assign, or clear it? Must each assignment transition be reasoned and audited? Scope must be explicit; a null or missing branch value cannot imply branch visibility. |
| Historical scope | When a subscriber, staff member, network device, job, or other mutable resource moves, do past financial, session, audit, and service records retain the original branch context? How is effective time represented? |
| Cross-branch action | Can one operation affect records in multiple branches? Does the caller need a tenant-wide grant, access to every affected branch, or a separately approved aggregate permission? How are partial outcomes handled? |
| Query and index | What tenant-first indexes, relationship indexes, pagination limits, cache keys, search filters, job partitions, and report windows are required for each cardinality? |
| Lifecycle and migration | What validation, backfill, conflict queue, dry-run, audit, rollback, and compatibility window are needed to convert any existing implicit or nullable scope? |

## Model alternatives for owner review

These patterns are not mutually exclusive. An approved contract may use different patterns for different resource families.

| Pattern | Shape | Benefits | Costs and questions |
|---|---|---|---|
| **A. Single owning branch or explicit tenant-wide** | A branch-local resource has one branch owner; a tenant-wide resource is marked explicitly. A shared item needs a separate tenant-wide template or duplicated branch instance. | Direct authorization predicate and simpler tenant-plus-branch indexes; fewer ambiguous joins. | Does not represent one operational resource shared by branches without a separate template/instance design. Duplication can split history or create synchronization and reconciliation work. |
| **B. Explicit multi-branch assignment relation** | Resource ownership remains tenant-scoped; one or more explicit branch-assignment links grant branch-context access. Tenant-wide scope is represented separately. | Supports a real shared resource while keeping each authorized branch visible; assignment changes can be recorded as relations. | More join/index work for reads, caches, search, reports, live queries, exports, and jobs; every use must define whether overlap with the caller's grants is sufficient and what fields/actions are visible. |
| **C. Parent-derived scope** | A child obtains scope from a validated parent relationship instead of carrying an independent branch assignment. | Can reduce duplicate scope fields and keep detail records tied to an aggregate or service context. | Every query, mutation, export, and event must resolve and validate the complete parent chain; parent moves and cross-branch references can silently widen scope if semantics are not frozen. |

An encoding that separates `tenant-wide`, `branch-owned`, `multi-branch`, and `unassigned` states is one candidate for review. Exact fields, tables, enums, nullability, cardinality, and indexing remain open. In particular, a generic nullable `branchId` is not sufficient unless each resource contract gives it exactly one unambiguous meaning.

## Resource-family review matrix

The matrix records current requirements and questions for the decision owners. It does not assign final scopes.

| Resource family | Existing requirement direction | Decision still required |
|---|---|---|
| Tenant configuration, branding, domains, and platform billing | Tenant-wide settings exist; country, currency, timezone, tax, and invoicing are associated with branches/legal entities. | Which settings are tenant-wide vs branch/legal-entity scoped; whether one legal entity serves several branches; which settings are inherited or overridden. |
| Staff membership, roles, and support grants | Membership is tenant-bound; Owner is tenant-wide; other role assignments may be tenant-wide or selected-branch; support grant has explicit tenant/resource/scope. | Whether a staff record itself has a home branch; how member assignment interacts with role grants; how shared resources and scoped support approvals intersect with selected branches. |
| Subscribers, leads, service accounts, and entitlements | Tenant owns subscriber/service data; branch scope applies where relevant; subscriber identity is separate from staff identity. | Single or multiple branch assignment; household/account movement; entitlement and session scope; preserve historical branch attribution; handling of cross-branch service. |
| Plans, templates, and offers | Templates may be shared, with branch-specific local price/currency/availability. | Template versus branch offer ownership; whether an offer can be shared by several branches; price/history versioning and local override rules. |
| Routers, NAS, access points, sessions, and accounting | Network devices and sessions are tenant-scoped with branch context where determined; router/NAS identity is the trusted source for network event mapping. | One or multiple serving branches per device; authoritative assignment source; session/accounting branch attribution; safe behavior when mapping is missing or changes. |
| Subscriber payments, invoices, refunds, payouts, and ledger entries | Financial record is tenant/legal-entity/currency scoped, with a branch where applicable; settled facts and audit evidence are immutable. | Sale branch versus payment branch, multi-branch invoices, legal-entity relation, attribution of adjustments/refunds, and which branch totals are visible in reports. |
| Tickets, vouchers, agents, campaigns, and communications | Tenant/branch and audience scope must be explicit; opt-out and audience constraints apply. | Whether work items or batches can span branches; assignment/claim behavior; redemption or message attribution; branch visibility after transfer. |
| Imports, jobs, files, search, exports, caches, subscriptions, and reports | Each carries explicit tenant context and is bounded, permission-checked, and reauthorized at execution and retrieval. | Whether a job/output can span several branches; how assignment filters propagate to every derived object; exact cache/search keys, indexes, expiry, and revocation behavior. |

The source requirement map continues to carry `Tenant + assigned branch` as an authorization boundary. The exact entity-specific assignments in this matrix must be reflected in that map only after owner approval.

## Authorization questions that need explicit answers

1. If a resource is assigned to branches A and B, can a user authorized only for A read and edit the full resource, read only an A-specific projection, or perform only explicitly permitted actions?
2. For a request spanning several branches, must the actor have tenant-wide permission, permission for every requested branch, or a separate cross-branch grant? A filter must not broaden server-resolved scope.
3. How does the role-composition rule interact with a resource shared across branches, an unassigned resource, a branch transition, or a tenant-wide parent with branch-local children?
4. Which assignments are versioned/effective-dated, and how do historical finance, audit, session, and export records preserve the branch context that applied when created?
5. Which owner-approved transfer/deactivation process can change resource scope, and how are related children, live sessions, queued jobs, cache entries, search documents, and active subscriptions revalidated?

The implementation must fail closed where it cannot resolve an unambiguous tenant/resource scope. The owner must still specify permitted unassigned-resource workflows and whether a shared resource can be partially visible; this packet does not choose those policies.

## Phase 0 acceptance and future evidence

Before any tenant-scoped schema or authorization contract is frozen:

- Appoint product, architecture, and security owners for `MN-DEC-016`; record the approved decision in `READINESS-DECISIONS.md` and this register when it is made.
- Complete the resource-family matrix with an explicit ownership pattern, cardinality, unassigned behavior, parent/inheritance rule, movement/history rule, permitted actions, and accountable record owner for every `REC-*` family and route/action requirement.
- Reconcile the matrix with `TECHNICAL-SPECIFICATION.md` §14.2, ADR-0003, `DATA-OWNERSHIP-AND-CLASSIFICATION.md`, API/query contracts, route/action acceptance cases, and migration plans. Remove wording that implies a final assignment where none is approved.
- Approve tenant-first indexes and scope predicates together; define query limits and the branch relationship needed by each list, detail, search, cache, job, export, live subscription, and report.
- Define positive and negative future acceptance cases for tenant-wide, single-branch, shared multi-branch, unassigned, reassigned, cross-tenant, parent-derived, stale job, and live-revocation paths. Demonstrate that no client-supplied branch ID can widen access.
- Define a dry-run/backfill and audit strategy for migrating any records that have missing or implicit branch scope; preserve finance, security, and legal history.

No runtime capture was needed to analyze these source contracts. REA, tests, source schema, migration, and product implementation are future evidence or work after the appropriate authorization and dependency gates.

## Related requirements

- [`IMPLEMENTATION-PLAN.md`](IMPLEMENTATION-PLAN.md) Phase 0 work items 3–4 and Phase 1/3 dependencies
- [`TECHNICAL-SPECIFICATION.md`](TECHNICAL-SPECIFICATION.md) §§6, 14.2–14.3
- [`ADR-0003`](adr/ADR-0003-convex-tenant-authorization.md)
- [`DATA-OWNERSHIP-AND-CLASSIFICATION.md`](DATA-OWNERSHIP-AND-CLASSIFICATION.md)
- [`API-ERROR-EVENT-CONTRACT-BACKLOG.md`](API-ERROR-EVENT-CONTRACT-BACKLOG.md)
- [`REQUIREMENT-ROUTE-TRACEABILITY.md`](REQUIREMENT-ROUTE-TRACEABILITY.md)
- [`PRIORITIZED-V1-STORY-MAP.md`](PRIORITIZED-V1-STORY-MAP.md)
