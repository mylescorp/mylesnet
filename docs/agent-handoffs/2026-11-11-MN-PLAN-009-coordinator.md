# Task Handoff: Project-specific REA validation plan

- **Task ID:** `MN-PLAN-009`
- **Date:** `2026-11-11`
- **Agent/owner:** Coordinating agent
- **Plan phase:** Phase 0 — close planning and turn decisions into build contracts
- **Status:** `In progress`
- **Related requirements:** `docs/IMPLEMENTATION-PLAN.md` §§6–7; `AGENTS.md` REA usage rule; `docs/REA-VALIDATION-PLAN.md`
- **Dependencies:** Existing route/screenshot inventories, design handoff, implementation plan, and agent handoff protocol.

## Objective

Define how REA will be used during MylesNet planning, implementation, and launch verification, including evidence handling, data safeguards, phase-specific uses, and limitations.

## Work completed

- Added a project-specific REA validation plan covering Phase 0 through country pilots/GA.
- Distinguished reference, MylesNet implementation, and external provider/network evidence.
- Required reuse of captured references, bounded investigations, observation/inference/unknown labeling, privacy safeguards, and separate disposable targets for mutating capture.
- Defined repository placement, evidence-ledger fields, investigation priorities, usage boundaries, and an end-of-task REA checklist.
- Linked the plan from `AGENTS.md`, the implementation roadmap, README, and handoff template.
- Added this task to the implementation status ledger.
- Strengthened root `AGENTS.md` so every agent must read the REA validation plan at task start, even when REA is not needed; clarified that this is a consultation requirement, not a reason to invoke REA unnecessarily.

## Implementation or documentation details

- **Files changed:** `AGENTS.md`, `README.md`, `docs/IMPLEMENTATION-PLAN.md`, `docs/IMPLEMENTATION-STATUS.md`, `docs/REA-VALIDATION-PLAN.md`, `docs/agent-handoffs/TEMPLATE.md`, `docs/agent-handoffs/2026-11-11-MN-PLAN-009-coordinator.md`.
- **Approach:** REA is supplemental evidence for reference research and MylesNet UI observation. Source tools remain primary for source questions; provider sandboxes, network labs, legal review, tests, and monitoring remain authoritative for their respective domains.
- **Decisions made:** Reuse existing captures first. Keep raw private reference data out of commits. State-changing recordings require a disposable/seeded target. An AgentEnv may be used only as a bounded synthetic fixture and not as a substitute for tests or production services.
- **Decisions left open:** No additional product requirements were decided by this document.

## Verification and evidence

- **Checks run:** Inline Python relative-link scan across seven changed Markdown files found no broken links; `git diff --cached --check` passed after whitespace cleanup. Previous PR head `4f6c0f9` had passing Vercel preview and Gitleaks checks; the root-instruction follow-up requires checks to rerun.
- **Checks not run:** No browser capture, live reference interaction, product tests, builds, or provider/network experiments. This task documents the future workflow only.
- **Evidence:** Existing references and route inventories are referenced by path; no new REA capture was made. PR #14 Vercel preview deployed successfully and Gitleaks passed. Greptile responded “Your trial has ended,” so no confidence score or findings were supplied for the current head.
- **Failures:** None observed.

## Remaining work

### Done

- The project-specific usage stages, safety rules, evidence fields, target boundaries, and limitations are documented.

### Pending

- Greptile review is blocked until its service trial is reactivated; obtain an explicit current-head score and findings, remediate as needed, and request a fresh review after any head change.
- Merge only when the explicit current-head 5/5 and zero-unresolved-issues gate and required checks pass.
- Update the status ledger with final review, merge, and deployment state.

### Blocked

- Greptile review for PR #14 is unavailable because its configured trial ended. Any state-changing future REA scenario remains blocked until it targets an explicitly disposable/seeded environment.

## Risks and handoff instructions

- **Risks/limitations:** AgentEnv is capture-bounded; do not treat its unobserved operations as supported. A UI/browser outcome is not payment, network, legal, security, or production evidence.
- **Next action:** Reactivate the configured Greptile service, then review PR #14's latest head and finish the required score/findings gate.
- **User approval required:** Product implementation, production use, or any real state-changing provider/router scenario still needs its applicable explicit approval.

## Delivery state

- **Commit/branch:** Branch `codex/mn-rea-usage-plan`; follow-up is included in PR #14
- **PR/review:** [PR #14](https://github.com/mylescorp/mylesnet/pull/14); Vercel preview passed; Gitleaks passed; Greptile returned a trial-ended notice without a score
- **Merge:** not merged
- **Deployment:** not deployed
