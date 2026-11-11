# Task Handoff: Agent handoff and implementation tracking rules

- **Task ID:** `MN-GOV-001`
- **Date:** `2026-11-11`
- **Agent/owner:** Coordinating agent
- **Plan phase:** Phase 0 — close planning and turn decisions into build contracts
- **Status:** `In progress`
- **Related requirements:** `AGENTS.md` mandatory implementation-plan and agent handoff protocol; `docs/IMPLEMENTATION-PLAN.md` §11; `docs/IMPLEMENTATION-STATUS.md`
- **Dependencies:** Existing implementation roadmap and repository charter; no product implementation dependencies.

## Objective

Make the end-to-end implementation plan mandatory context for repository work and require every agent to leave a durable handoff that identifies completed, pending, blocked, and verified work.

## Work completed

- Added mandatory pre-task planning and source-document review rules to root `AGENTS.md`.
- Required each task agent to write an independent handoff and the coordinator to reconcile delegated handoffs into a central phase/status ledger.
- Added a reusable handoff template and handoff-folder workflow guide.
- Added a phase tracker and task ledger with the current planning status and unresolved gates.
- Linked the roadmap, status ledger, and handoff process from the project README.

## Implementation or documentation details

- **Files changed:** `AGENTS.md`, `README.md`, `docs/IMPLEMENTATION-PLAN.md`, `docs/IMPLEMENTATION-STATUS.md`, `docs/agent-handoffs/README.md`, `docs/agent-handoffs/TEMPLATE.md`, `docs/agent-handoffs/2026-11-11-MN-GOV-001-coordinator.md`.
- **Approach:** Keep the detailed roadmap as the phase/acceptance authority, use `IMPLEMENTATION-STATUS.md` as the concise current-state index, and store one non-overwriting file per task/agent in `docs/agent-handoffs/`.
- **Decisions made:** Status vocabulary is `Not started`, `In progress`, `Blocked`, and `Accepted` for phases; task/handoff vocabulary is `Done`, `In progress`, and `Blocked`. Phase acceptance requires exit evidence and coordinator status update.
- **Decisions left open:** None for this documentation mechanism. Future tasks must follow project-specific authorization and release gates.

## Verification and evidence

- **Checks run:** Python read-only scan checked relative Markdown links in `docs/IMPLEMENTATION-PLAN.md` and verified required scope markers; the command is available in session history.
- **Checks not run:** No product tests, dependency installs, builds, or runtime checks; this is a documentation-only change and the project charter remains planning-only.
- **Evidence:** Repository status and diff review remain pending at handoff creation; current branch is `docs/agent-handoff-governance`.
- **Failures:** None observed.

## Remaining work

### Done

- The project instructions, plan, template, status ledger, README links, and this handoff are written.

### Pending

- Review the final diff and run documentation link/format checks.
- Commit/push the focused branch, open the PR, and complete required CI/Greptile review workflow as available.
- Update `docs/IMPLEMENTATION-STATUS.md` with final PR/review/merge state before closing the task.

### Blocked

- **Current blocker:** none for local documentation work. Remote delivery remains pending PR/review gates; no claim of merge or deployment is made.

## Risks and handoff instructions

- **Risks/limitations:** The ledger is a manual status artifact; coordinator must reconcile every worker handoff and keep phase states evidence-based.
- **Next action:** Inspect diff, validate all links in changed Markdown files, then follow repository PR policy.
- **User approval required:** Product implementation and production deployment still require explicit authorization under `AGENTS.md`.

## Delivery state

- **Commit/branch:** not committed; branch `docs/agent-handoff-governance`
- **PR/review:** not created
- **Merge:** not merged
- **Deployment:** not deployed
