# MylesNet Phase 0 Execution Kickoff Plan

**Date:** 11 November 2026<br>
**Plan phase:** Phase 0 — close planning and turn decisions into build contracts<br>
**Status:** Kickoff plan prepared; Phase 0 remains in progress<br>
**Boundary:** Planning and repository-foundation work only. This document does not authorize product implementation, live integrations, infrastructure provisioning, or deployment.

## Purpose

Translate the approved [end-to-end implementation plan](IMPLEMENTATION-PLAN.md) into an ordered set of reviewable Phase 0 tasks, identify the first practical work, and make the gate into implementation explicit. The implementation plan remains the phase and acceptance authority; this document is an execution aid.

## Repository and project baseline

- The checkout is a setup-only pnpm/Turborepo monorepo. `apps/web` contains a planning placeholder; `apps/admin`, `apps/storybook`, `convex/`, and `packages/design-system/` are reserved boundaries without product routes, server functions, tokens, or UI components.
- The root scripts cover `dev`, `build`, `typecheck`, and `clean`. The repository does not yet contain the CI, environment matrix, secret-management workflow, telemetry contract, or security controls required by Phase 1.
- The product scope is four related surfaces—public marketing/signup, platform administration, ISP tenant console, and subscriber/captive portals—plus an independently operated RADIUS/network data plane.
- The selected direction is `apps/web`, `apps/admin`, and `apps/storybook`; one shared Convex application deployment with strict tenant/branch authorization; WorkOS AuthKit for staff identity; FreeRADIUS 3.2.x with a separate PostgreSQL AAA store; and the approved MylesNet Design System v1.0. Convex Europe (Ireland), Vercel Dublin, and payment/messaging vendors remain candidates or provider decisions where the charter says so; they are not evidence of live production services.
- The planning inventory reports 308 public-site routes and 73 tenant reference route/view entries. The public screenshot manifest has 325 capture records because it also includes UI states; these are not 325 public URLs. Reference galleries and the preserved design-system archive are local-only evidence; their sample data and files must remain out of public commits. Subscriber/captive and platform-admin workflows still need approved story/state catalogs.
- `docs/IMPLEMENTATION-STATUS.md` correctly records Phase 0 as active and later phases as not started. Phase 0 has existing governance work in progress; a complete roadmap or a shell page does not satisfy its exit criteria.

## Source precedence and reconciliation findings

Use the project charter in `AGENTS.md` and the latest approved decisions in `READINESS-DECISIONS.md` as the authority. Use `TECH-STACK.md`, `DESIGN-SYSTEM-HANDOFF.md`, and the implementation plan for their approved contracts. Treat proposal/open wording in older or master planning documents as material to reconcile, not as a reason to reopen a settled decision.

The review found these concrete Phase 0 reconciliation items:

| Finding | Documents/sections | Phase 0 treatment |
|---|---|---|
| The master technical specification still offers a single-app layout, Go RADIUS, dedicated Convex, alternative hosting, and SSO/SCIM as open or optional. | `TECHNICAL-SPECIFICATION.md` §§3, 12, 14.2, 14.11; `AGENTS.md`; `TECH-STACK.md`; `READINESS-DECISIONS.md` | Reconcile to the approved app split, shared Convex boundary, FreeRADIUS 3.2.x direction, and SSO/SCIM deferred beyond v1. Keep region, hosting contract/cost, and provider readiness labeled as candidates where approval is still pending. |
| Multi-branch and cross-country tenant support is still described as an MVP question in the technical specification. | `TECHNICAL-SPECIFICATION.md` §§14.2 and 14.11; `AGENTS.md`; `READINESS-DECISIONS.md` | Carry forward multi-branch v1 and strict branch scope. Preserve open implementation details such as resource-level branch assignment, indexes, noisy-neighbor budgets, and legal transfer evidence. |
| Plan applicability examples still label white-label as a provisional plan entitlement, and older domain wording is inconsistent. | `TECHNICAL-SPECIFICATION.md` §14.1; `DESIGN-SYSTEM-HANDOFF.md` §7; `MYLESNET-DESIGN-SYSTEM-V1.0-AUDIT.md` | Reconcile to same-feature tiers and the approved all-tier branding/domain rules. Keep custom-domain DNS, verification, TLS, and prior-callback migration in the separate domain control plane. Resolve the package's stale fields before runtime integration. |
| The technical specification calls approved targets and scope “open” alongside matters that genuinely remain undecided. | `TECHNICAL-SPECIFICATION.md` §§10, 12, 14.11; `READINESS-DECISIONS.md`; `IMPLEMENTATION-PLAN.md` | Record decisions at item level. Carry forward approved login, RADIUS/captive availability, and payment activation/reconciliation targets; do not invent still-open latency, commercial values, retention periods, RPO/RTO, provider contracts, budgets, or owners. |
| Product-scope delivery stages are expressly proposals and do not match the approved phase roadmap in every detail. | `PRODUCT-SCOPE.md` §“Suggested delivery stages”; `IMPLEMENTATION-PLAN.md` | Use the implementation plan's phase map, dependencies, and exit gates as the sole execution sequence. |

The v1.0 design audit already records source-package gaps: schema IDs, secondary-accent derivation, shape-preset approval, missing dark-output values, contrast/chart coverage, white-label/domain fields, and missing official logo/font assets. Keep these as explicit Phase 2 prerequisites; do not silently close them during Phase 0.

## Ordered Phase 0 work

The tasks below are the proposed next work items. They are documentation and decision-closure work unless a later user instruction explicitly authorizes a different scope.

| Order / task | Work and outputs | Acceptance evidence | Dependencies |
|---|---|---|---|
| **1 — MN-PLAN-003: Decision register and canonical reconciliation** | Create a register with stable ID, source, decision state (approved/candidate/open/rejected), owner, evidence, dependency, and revisit trigger. Reconcile stale canonical text without erasing useful history. Record approved v1 scope separately from external/legal/commercial decisions. | Every Phase 0 contradiction above is resolved or explicitly flagged with the authoritative source; no unresolved item is presented as approved; linked canonical documents agree on the decision. | This kickoff plan; existing Phase 0 requirements. |
| **2 — MN-PLAN-004: Requirement and route/action traceability** | Build stable requirement/story IDs and map public, platform, tenant, subscriber, captive, and network operations to actor, permission, tenant/branch scope, plan/lifecycle applicability, data class, audit event, reference evidence, and acceptance cases. Preserve static/structured treatment for public routes unless CMS needs are approved. | Reconciled counts and scope for the 308 public routes and 73 tenant reference entries; platform and subscriber/captive proposed journeys have approved catalogs; exports and destructive operations have explicit access contracts. No private reference data is copied into the catalog. | MN-PLAN-003 for settled scope and vocabulary. |
| **3 — MN-PLAN-005: Architecture decision records and contracts** | Prepare the Phase 0 ADR set: app/session/domain split; WorkOS identity and organization mapping; Convex tenant/branch enforcement and realtime revocation; data region; RADIUS source of truth, PostgreSQL store, outbox, and outage behavior; payment ledger and country gates; custom domains/DNS/TLS/callback transition; theme compiler/schema; storage/export/backup; deployment/rollback; observability/SLO measurement. | Each ADR records context, approved decision or explicit open question, alternatives, security/operational tradeoffs, owner, evidence, and revisit trigger. Required interface and sequence contracts are linked from the traceability catalog. | MN-PLAN-003; relevant MN-PLAN-004 requirements. |
| **4 — MN-PLAN-006: Data, event, and threat contracts** | Define the data dictionary/ownership map, data classification, threat model, API/error/event envelope backlog, idempotency/retry/reconciliation expectations, and safe synthetic-data rules. Include tenant/branch scope, privacy, retention, export, audit, and deletion/legal-hold responsibilities. | Every critical record/event has an authoritative owner, scope, classification, retention decision or named open owner, and failure/recovery behavior. Threat scenarios cover tenant escape, privilege escalation, payment replay, RADIUS/router boundaries, support access, secrets, and reference assets. | MN-PLAN-003 and the relevant traceability/ADR decisions. |
| **5 — MN-PLAN-007: Country and operations packs** | For Kenya, Uganda, and Tanzania, record country/legal entity, currency/tax/invoice, data transfer/retention, language/timezone, payment and payout capability, messaging, support, and launch evidence. Add owners and evidence needed for provider, lab, recovery, staffing, cost, and migration questions. | Country enablement remains disabled until its legal/provider/operational evidence is signed. Candidate providers/regions are clearly distinguished from selected and approved production services. Open commercial and legal values have an accountable owner and revisit condition. | MN-PLAN-003; relevant architecture and data decisions. |
| **6 — MN-PLAN-008: Prioritized story map and Phase 0 exit review** | Order the v1 story backlog by dependency and risk; confirm engineering/release/security/accessibility/data owners and working agreements; consolidate links to artifacts in `IMPLEMENTATION-STATUS.md`. Review all Phase 0 exit criteria. | No material contradiction remains about scope, service boundaries, roles, currency/money rules, or network outage behavior; unresolved external values have explicit owners and do not block safe development environments; handoffs/status link the evidence; user explicitly approves moving from planning into implementation. | MN-PLAN-003 through MN-PLAN-007. |

Do not assign calendar estimates until team capacity, budget, external access, and start date are known. Phase 0 may continue producing planning documents while the product remains unauthorized.

## Phase dependencies after Phase 0

Follow `IMPLEMENTATION-PLAN.md` §§4–5 exactly:

1. **Phase 1 — Workspace, environments, and security spine** follows Phase 0 only after explicit implementation authorization. Recheck pinned runtime/tool versions; establish reproducible local/CI setup, environment separation, secret boundaries, redacted telemetry, and the agreed security spine. Do not add feature workflows as part of this foundation phase.
2. **Phases 2 and 3 — Design system/shells and identity/tenant/RBAC** depend on Phase 1 and establish shared UI and authorization foundations before broad features. Their interfaces and exit evidence gate the platform, public, and tenant foundations.
3. **Phases 4–6 — Platform lifecycle, public signup/domains, and tenant core operations** follow the design and identity foundations. Phase 5 depends on platform lifecycle as well as design and identity; Phase 6 depends on platform lifecycle, design, and identity.
4. **Phases 7 and 8 — RADIUS/router lab and finance/payment rails** follow tenant core operations and must prove their isolated contracts before subscriber activation.
5. **Phase 9 — Subscriber/captive portals** depends on both network and finance/payment proofs. **Phase 10 — Communications/support/analytics** depends on platform, tenant, network, finance, and portal foundations.
6. **Phase 11 — Hardening/legal/operations** depends on public, network, finance, portal, and communications/support/analytics readiness. **Phase 12 — Country pilots/GA** depends on Phase 11 and country-specific go/no-go evidence.

No dependency is complete merely because a document, screen, or demo exists; use the evidence and exit gate named for each phase.

## Gates and work that must stay open

The following remain decisions or evidence gates, not assumptions for the kickoff plan to fill in:

- Tier names/prices/quotas/tax treatment and detailed plan behavior.
- Country counsel approvals, data-transfer safeguards, precise retention schedules, and legal terms.
- Payment provider contracts/sandboxes, fee schedules, direct-versus-pooled activation, settlement/refund/payout capabilities, and merchant-account ownership.
- Email/SMS/WhatsApp vendor and country delivery evidence; DNS registrar and TLS automation proof.
- Hosting/region contract, latency, cost, capacity, SLO definitions outside already approved targets, and scale budgets.
- Backup frequency, RPO/RTO, disaster-recovery proof, incident/on-call owners, support staffing, migration sources, and public-launch cohort/date.
- The user's explicit transition from planning into implementation; production deployment remains a separate approval gate.

## Documents reviewed for this kickoff

The review covered the repository charter and scaffold (`AGENTS.md`, `README.md`, workspace/package manifests, app/backend/package READMEs, and the current placeholder web page); the implementation roadmap and status ledger; all canonical product, architecture, access, flow, readiness, stack, and design-system documents; the route/reference review notes; and existing agent handoffs. Private screenshot and archive paths remain ignored/local-only and were not copied into this plan.

The main planning sources are `AGENTS.md`, `IMPLEMENTATION-PLAN.md`, `IMPLEMENTATION-STATUS.md`, `READINESS-DECISIONS.md`, `TECHNICAL-SPECIFICATION.md`, `TECH-STACK.md`, `SAAS-ARCHITECTURE.md`, `PRODUCT-SCOPE.md`, `CORE-FLOWS-AND-DATA.md`, `ROUTES-AND-ACCESS.md`, `DESIGN-SYSTEM-HANDOFF.md`, and `MYLESNET-DESIGN-SYSTEM-V1.0-AUDIT.md`.

## Immediate next action

Start **MN-PLAN-003**, the Phase 0 decision register and canonical reconciliation. Keep Phase 0 marked **In progress** until all six work items have evidence and the stated exit gate is met. Do not start Phase 1 or product implementation before the explicit user approval required by the charter.
