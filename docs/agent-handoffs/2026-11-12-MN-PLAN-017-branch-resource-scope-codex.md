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

- **Checks run:** `git diff --check` passed; all added local Markdown link targets exist; conflict-marker scan passed. Rebased onto current `main` at `150f180` with conflicts resolved. `git diff --check origin/main...HEAD` passed, the conflict-marker scan found no markers, and a focused Markdown link scan checked 310 local links with zero missing targets. Earlier Gitleaks/Vercel checks and the previous 5/5 review are stale after rebase; new-head checks and review are pending.
- **Checks not run:** Tests/builds were not run or requested for this documentation-only task. REA was not used; existing planning requirements are the evidence source.
- **Evidence:** `AGENTS.md`; `TECHNICAL-SPECIFICATION.md` §§6, 14.2–14.3; ADR-0003; `DATA-OWNERSHIP-AND-CLASSIFICATION.md`; `REQUIREMENT-ROUTE-TRACEABILITY.md`; `DECISION-REGISTER.md` MN-DEC-016.
- **REA evidence:** Not used.
- **Failures:** None.

## Remaining work

### Done

- Options and future acceptance questions for `MN-DEC-016` are documented and cross-referenced.

### Pending

- Verify refreshed non-review CI checks after this handoff-only commit; report the exact latest-head result.
- Obtain a fresh current-head Greptile 5/5 review with zero unresolved issues and passing required checks before merge.
- The appointed owners must select or explicitly gate resource-specific semantics and propagate the approved decision to the ADR, requirements, APIs, and migration contracts.

### Blocked

- Phase 0 exit and product implementation remain gated by unassigned owners, independent sign-off, unresolved external evidence, and explicit user authorization. This task does not satisfy those gates.

## Risks and handoff instructions

- **Risks/limitations:** This packet does not supply a final authorization predicate, database schema, query/index plan, or migration. The proposal cannot be used as implementation approval.
- **Next action:** Push the rebased branch, verify required checks, obtain fresh Greptile review on the current head, and fix actionable findings in PR #23 before merging. Product implementation remains unauthorized.
- **User approval required:** Appointed product/architecture/security owners must approve `MN-DEC-016` before entity/API contracts are frozen; the user must explicitly authorize moving beyond Phase 0 before product implementation begins.

## Delivery state

- **Commit/branch:** Rebased decision-packet commits are on `codex/mn-plan-017-branch-resource-scope`; final handoff correction remains to be committed.
- **PR/review:** [PR #23](https://github.com/mylescorp/mylesnet/pull/23) is open and ready against `main`. Its previous 5/5 review is tied to pre-rebase head `1200fd8`; fresh review and required checks are pending.
- **Merge:** Not merged.
- **Deployment:** Not deployed; no deployment was authorized.
