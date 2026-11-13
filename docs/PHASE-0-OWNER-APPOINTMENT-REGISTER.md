# Phase 0 owner appointment and sign-off register

**Plan phase:** Phase 0 — close planning and turn decisions into build contracts

**Status:** Blank planning worksheet; no individual appointments or approvals are recorded here.
**Authority:** [`IMPLEMENTATION-PLAN.md`](IMPLEMENTATION-PLAN.md), the project charter in [`AGENTS.md`](../AGENTS.md), and the role baseline in [`PRIORITIZED-V1-STORY-MAP.md`](PRIORITIZED-V1-STORY-MAP.md).

## Purpose and boundaries

Use this register to record who the project owner appoints for the Phase 0 workstreams, the scope each person accepts, and the evidence for independent review and sign-off. It turns the existing role baseline into a recordable checklist; it does not appoint anyone, change decision authority, or approve any unresolved product, legal, financial, security, or operational value.

The project owner must fill the named-person, scope, evidence, and date fields. A blank field or a `Not recorded` status means the appointment or approval is still open. Do not infer an appointment from a task assignment, PR authorship, document authorship, or meeting attendance. Where a role names several people or country-specific approvers, record each person separately.

This worksheet does not satisfy the Phase 0 exit gate by itself. Product implementation remains unauthorized until all required Phase 0 evidence and owner approvals are complete and the user explicitly authorizes the transition.

## How to complete the register

1. The project owner names each accountable person and records the decision scope and any country or service boundary they accept.
2. Each appointee confirms acceptance in a durable project record; link that evidence and record the date.
3. Update the `Owner` column in [`DECISION-REGISTER.md`](DECISION-REGISTER.md) to point to the accepted person or role record. Keep unresolved decisions `Open` until their evidence and approval criteria are met.
4. Record independent reviewers separately from the authors of the artifact under review. Note any overlap, recusal, condition, or limitation; do not imply independence if the reviewer confirms a conflict.
5. Link decisions, sign-offs, and conditions to their source artifacts. Country-specific work must be recorded separately for Kenya, Uganda, and Tanzania where the evidence differs.

Suggested status labels are `Not appointed`, `Proposed`, `Appointed — acceptance pending`, `Accepted`, and `Replacement needed` for appointments, and `Not recorded`, `In review`, `Accepted`, `Accepted with conditions`, and `Not accepted` for sign-offs. These labels help describe progress; they do not grant approval or replace the existing project's decision authority.

## Appointment log

Every row is currently unassigned, based on the reviewed Phase 0 documents. Add one entry per named person when the project owner confirms the appointment.

| Workstream | Accountable role(s) | Named appointee(s) | Scope accepted (including country/service limits) | Acceptance evidence and date | Status |
|---|---|---|---|---|---|
| Project and product scope | Project owner; product owner | Not recorded | Not recorded | Not recorded | Not appointed |
| Architecture and application engineering | Engineering/architecture lead | Not recorded | Not recorded | Not recorded | Not appointed |
| Identity, application security, and threat review | Security lead | Not recorded | Not recorded | Not recorded | Not appointed |
| Design system and accessibility | Design-system/accessibility lead | Not recorded | Not recorded | Not recorded | Not appointed |
| Data governance and privacy | Data/privacy owner; country counsel | Not recorded | Not recorded; record each country counsel separately | Not recorded | Not appointed |
| Release and service operations | Release/operations owner | Not recorded | Not recorded | Not recorded | Not appointed |
| Finance and country tax | Finance/product-commercial owner; tax counsel | Not recorded | Not recorded; record each country's scope separately | Not recorded | Not appointed |
| Network and AAA | Network-operations owner | Not recorded | Not recorded | Not recorded | Not appointed |
| Communications | Communications/privacy owner | Not recorded | Not recorded | Not recorded | Not appointed |
| Support and incident command | Support lead; incident commander | Not recorded | Not recorded | Not recorded | Not appointed |
| Country launch | Country counsel and launch approvers for KE, UG, and TZ | Not recorded | Record an independent entry for each country | Not recorded | Not appointed |

## Independent review and sign-off log

Record the artifact version, reviewer's name and role, review result, conditions, evidence link, and date. These rows are review prompts derived from the Phase 0 exit conditions, not new approval standards. Leave a row `Not recorded` until a reviewer has completed and recorded the review.

| Review area | Reviewer(s) and role | Artifact/version reviewed | Result and conditions | Independence or conflict note | Evidence and date | Status |
|---|---|---|---|---|---|---|
| Product scope, story priorities, and acceptance catalog | Not recorded | Not recorded | Not recorded | Not recorded | Not recorded | Not recorded |
| Architecture, tenant/branch isolation, and application boundaries | Not recorded | Not recorded | Not recorded | Not recorded | Not recorded | Not recorded |
| Security controls and threat acceptance | Not recorded | Not recorded | Not recorded | Not recorded | Not recorded | Not recorded |
| Data governance, privacy, and country transfer/retention evidence | Not recorded | Not recorded; record KE, UG, and TZ separately where applicable | Not recorded | Not recorded | Not recorded | Not recorded |
| Finance, pricing, tax, payment, and FX evidence | Not recorded | Not recorded; record country-specific evidence separately | Not recorded | Not recorded | Not recorded | Not recorded |
| Design-system and accessibility acceptance | Not recorded | Not recorded | Not recorded | Not recorded | Not recorded | Not recorded |
| Network, operations, reliability, recovery, and support readiness | Not recorded | Not recorded | Not recorded | Not recorded | Not recorded | Not recorded |

## Phase 0 gate record

Complete this section only after the evidence above and all other plan exit criteria have been assessed.

| Gate | Record |
|---|---|
| Phase 0 exit assessment and supporting evidence | Not recorded; see the [story map exit review](PRIORITIZED-V1-STORY-MAP.md) |
| Open decisions with accountable owners and evidence plans | Not recorded; reconcile with [`DECISION-REGISTER.md`](DECISION-REGISTER.md) |
| Independent product and security sign-off | Not recorded |
| User authorization to begin implementation | Not granted; obtain explicit authorization after the exit conditions are accepted |
| Phase 0 disposition, approver, and date | In progress; no acceptance recorded |

## Related records

- [Implementation plan — Phase 0](IMPLEMENTATION-PLAN.md#phase-0--close-planning-and-turn-decisions-into-build-contracts)
- [Prioritized v1 story map and Phase 0 exit review](PRIORITIZED-V1-STORY-MAP.md)
- [Decision register](DECISION-REGISTER.md)
- [Repository implementation status](IMPLEMENTATION-STATUS.md)
