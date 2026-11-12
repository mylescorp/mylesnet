# Task Handoff: FX conversion provenance contract

- **Task ID:** `MN-PLAN-016`
- **Date:** `2026-11-12`
- **Agent/owner:** `Coordinating agent`
- **Plan phase:** `Phase 0 — Close planning and turn decisions into build contracts`
- **Status:** `In progress`
- **Related requirements:** `IMPLEMENTATION-PLAN.md` Phase 0 item 5, Phase 8 finance item 4, Phase 10 analytics; `MN-DEC-015`, `MN-DEC-022`; `TECHNICAL-SPECIFICATION.md` §14.5
- **Dependencies:** Existing approved billing/reporting requirements. The independent public-source evidence task PR #21 is still open during review deferral; source selection remains open and is not assumed by this contract.

## Objective

Document the minimum immutable provenance needed to explain the official FX observation and calculation used for platform invoice issuance or consolidated reporting. Keep this in planning documentation; do not select a source/policy or implement a provider client, financial workflow, or production integration.

## Work completed

- Added a logical contract that separates source-rate observations from invoice/report conversion snapshots.
- Distinguished the already-approved invoice-issuance timing from report view-time conversion.
- Specified source/pair/as-of/retrieval/calculation provenance, correction lineage, preservation and display requirements, and future acceptance scenarios.
- Kept source series, quote direction, freshness/fallback, precision/rounding, report persistence, retention, licensing, accounting, and named owners explicitly open.
- Linked the contract from the roadmap, technical specification, core flows, data ownership map, API backlog, decision/readiness records, and status ledger.
- Kept Phase 0 open and product implementation unauthorized.

## Implementation or documentation details

- **Files changed:**
  - `docs/FX-CONVERSION-PROVENANCE-CONTRACT.md`
  - `docs/IMPLEMENTATION-PLAN.md`
  - `docs/TECHNICAL-SPECIFICATION.md`
  - `docs/CORE-FLOWS-AND-DATA.md`
  - `docs/DATA-OWNERSHIP-AND-CLASSIFICATION.md`
  - `docs/API-ERROR-EVENT-CONTRACT-BACKLOG.md`
  - `docs/DECISION-REGISTER.md`
  - `docs/READINESS-DECISIONS.md`
  - `docs/IMPLEMENTATION-STATUS.md`
  - `docs/agent-handoffs/2026-11-12-MN-PLAN-016-fx-conversion-contract-codex.md`
- **Approach:** Record required logical semantics and evidence links without freezing implementation-level schemas or source policy. Keep original currencies/amounts authoritative and conversion metadata non-authoritative for settlement or wallet movement.
- **Decisions made:** None. Applied only previously approved invoice/report timing and display requirements from MN-DEC-022 and the canonical specification.
- **Decisions left open:** Exact institution/series/pairs; quote direction/rate meaning; source timing/cutoff and corrections; retrieval/cache/freshness/fallback; precision/rounding; persistence/restatement/retention; licensing/access terms; tax/accounting and named owners.

## Verification and evidence

- **Checks run:** `git diff --check` passed; focused validation found every added local Markdown link target; conflict-marker scan passed.
- **Checks not run:** Tests/builds were not run or requested for this documentation-only task. No REA capture was needed; this is ordinary planning-source analysis.
- **Evidence:** Canonical invoice and report requirements in `TECHNICAL-SPECIFICATION.md` §14.5 and `IMPLEMENTATION-PLAN.md` Phase 8/10; open policy items in `DECISION-REGISTER.md` MN-DEC-015 and MN-DEC-022.
- **REA evidence:** Not used.
- **Failures:** None.

## Remaining work

### Done

- Logical rate-observation/conversion-snapshot contract documented and propagated to the referenced planning records.

### Pending

- Commit, push, open this task's own PR, and record current-head non-review checks.
- Keep the PR open/unmerged during the user's code-review deferral.
- Resolve source and calculation policy only with the accountable Finance, engineering, data/privacy, and country owners and their evidence.

### Blocked

- Phase 0 exit and product implementation remain gated by missing named owners, independent sign-off, external evidence, and explicit user authorization. This task does not satisfy those gates.

## Risks and handoff instructions

- **Risks/limitations:** A logical contract does not validate the live fields or access terms of any central-bank series and does not approve accounting treatment. No source value or external rate data was fetched or integrated in this task.
- **Next action:** Finish document validation, create and push the focused PR, then monitor ordinary required CI. Request code review only after the user says review is restored.
- **User approval required:** Finance/accounting owners must approve conversion policy before implementation; the user must explicitly authorize moving beyond Phase 0 before product code begins.

## Delivery state

- **Commit/branch:** `codex/mn-plan-016-fx-conversion-contract` / documentation changes not committed yet.
- **PR/review:** Not created yet; code review deferred at the user's direction.
- **Merge:** Not merged.
- **Deployment:** Not deployed; no deployment was authorized.
