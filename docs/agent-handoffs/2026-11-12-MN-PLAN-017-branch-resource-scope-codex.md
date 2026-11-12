# Task Handoff: Branch resource scope decision packet

- **Task ID:** `MN-PLAN-017`
- **Date:** `2026-11-12`
- **Agent/owner:** `Coordinating agent`
- **Plan phase:** `Phase 0 — Close planning and turn decisions into build contracts`
- **Status:** `In progress`
- **Related requirements:** `IMPLEMENTATION-PLAN.md` Phase 0 work items 3–4; `MN-DEC-016`; `TECHNICAL-SPECIFICATION.md` §§6, 14.2–14.3; ADR-0003; tenant branch requirements in `REQUIREMENT-ROUTE-TRACEABILITY.md`
- **Dependencies:** Existing approved multi-tenant/branch role boundaries and shared-tenant architecture. Product, architecture, and security owners are not named; the exact branch/resource decision remains open.

## Objective

Prepare a reviewable Phase 0 decision packet for resource-to-branch assignment, shared resources, tenant-wide and unassigned semantics, scope inheritance, indexes, migration, and acceptance evidence. Do not choose the policy, freeze a data schema, or implement product workflows.

## Work completed

- Recorded approved constraints separately from open resource-level choices.
- Compared single-branch ownership, explicit multi-branch assignment, and parent-derived scope patterns without selecting a model.
- Added a resource-family review matrix for identity, subscribers, service plans, network, finance, support/messaging, and derived jobs/reports/exports.
- Listed authorization questions for shared visibility, cross-branch operations, unassigned records, resource movement, and live revocation.
- Added future Phase 0 acceptance evidence, including tenant/branch isolation, all derived access paths, migration, and resource-scope changes.
- Propagated the packet through the roadmap and canonical authorization/data/API/route/story records while keeping `MN-DEC-016` open and product implementation unauthorized.

## Implementation or documentation details

- **Files changed:**
  - `docs/BRANCH-RESOURCE-SCOPE-DECISION-PACKET.md`
  - `docs/IMPLEMENTATION-PLAN.md`
  - `docs/TECHNICAL-SPECIFICATION.md`
  - `docs/adr/ADR-0003-convex-tenant-authorization.md`
  - `docs/DECISION-REGISTER.md`
  - `docs/DATA-OWNERSHIP-AND-CLASSIFICATION.md`
  - `docs/API-ERROR-EVENT-CONTRACT-BACKLOG.md`
  - `docs/REQUIREMENT-ROUTE-TRACEABILITY.md`
  - `docs/PRIORITIZED-V1-STORY-MAP.md`
  - `docs/IMPLEMENTATION-STATUS.md`
  - `docs/agent-handoffs/2026-11-12-MN-PLAN-017-branch-resource-scope-codex.md`
- **Approach:** Separate approved invariants from alternatives and owner questions; classify resource families by decision dimensions without imposing one tenant/branch schema.
- **Decisions made:** None. Existing server-side authorization, role scope, tenant ownership, and explicit tenant-wide rules were preserved.
- **Decisions left open:** Resource-family ownership/cardinality, shared visibility, unassigned workflows, scope inheritance, cross-branch actions, historical attribution, query/index design, migration, and named accountable owners.

## Verification and evidence

- **Checks run:** `git diff --check` passed; all added local Markdown link targets exist; conflict-marker scan passed. Gitleaks, Vercel, and Vercel Preview Comments passed on commits `2839e730eec025c6a1ed75712278da918c41080f` and `17e2becdbfccae7d6e014465719acda5d7660ddf`. This final handoff update triggers a fresh CI run; its result will be reported against the resulting head.
- **Checks not run:** Tests/builds were not run or requested for this documentation-only task. REA was not used; existing planning requirements are the evidence source.
- **Evidence:** `AGENTS.md`; `TECHNICAL-SPECIFICATION.md` §§6, 14.2–14.3; ADR-0003; `DATA-OWNERSHIP-AND-CLASSIFICATION.md`; `REQUIREMENT-ROUTE-TRACEABILITY.md`; `DECISION-REGISTER.md` MN-DEC-016.
- **REA evidence:** Not used.
- **Failures:** None.

## Remaining work

### Done

- Options and future acceptance questions for `MN-DEC-016` are documented and cross-referenced.

### Pending

- Verify refreshed non-review CI checks after this handoff-only commit; report the exact latest-head result.
- Keep the PR open/unmerged during the user's review deferral.
- The appointed owners must select or explicitly gate resource-specific semantics and propagate the approved decision to the ADR, requirements, APIs, and migration contracts.

### Blocked

- Phase 0 exit and product implementation remain gated by unassigned owners, independent sign-off, unresolved external evidence, and explicit user authorization. This task does not satisfy those gates.

## Risks and handoff instructions

- **Risks/limitations:** This packet does not supply a final authorization predicate, database schema, query/index plan, or migration. The proposal cannot be used as implementation approval.
- **Next action:** Finish document validation, create and push the focused PR, then monitor ordinary required CI. Request code review only after the user says review is restored.
- **User approval required:** Appointed product/architecture/security owners must approve `MN-DEC-016` before entity/API contracts are frozen; the user must explicitly authorize moving beyond Phase 0 before product implementation begins.

## Delivery state

- **Commit/branch:** `17e2becdbfccae7d6e014465719acda5d7660ddf` / `codex/mn-plan-017-branch-resource-scope`; this final evidence update will produce the next head.
- **PR/review:** [PR #23](https://github.com/mylescorp/mylesnet/pull/23) is open and draft against `main`; code review is deferred at the user's direction and was not requested.
- **Merge:** Not merged.
- **Deployment:** Not deployed; no deployment was authorized.
