# Hosting cost and capacity evidence plan

**Task:** MN-PLAN-025

**Plan phase:** Phase 0 — close planning and turn decisions into build contracts

**Evidence checked:** 13 November 2026

**Status:** Public-source planning evidence; no hosting plan, region, budget, contract, or capacity approval is recorded.

## Purpose and boundaries

This document organizes public cost and plan-limit evidence for the current hosting candidates: Vercel for the Next.js applications and Convex for the shared application backend. It supplies a reproducible workload model for the appointed owners to complete. It is not a cost estimate, vendor selection, production-region decision, legal review, capacity demonstration, or authorization to create production resources.

The approved direction remains one shared Convex deployment for v1, with Vercel Dublin (`dub1`) and Convex Europe (Ireland) as candidates only. The application includes `apps/web`, `apps/admin`, and `apps/storybook`; production hosting boundaries and plan/project counts still need architecture confirmation. Keep RADIUS/AAA, payment providers, identity, email/SMS, DNS, object storage, and support staffing in separate cost lines. This source review does not price them.

## Public provider evidence

The sources below were reviewed on 13 November 2026. Prices are snapshots; recheck the live provider pages and obtain written terms before making a budget or plan decision.

| Provider/source | Published evidence observed | What it establishes | What it does not establish |
|---|---|---|---|
| [Vercel plans and pricing](https://vercel.com/pricing), [Spend Management](https://vercel.com/docs/spend-management) | The public page lists Pro at $20/month with $20 of included usage credit. Prices are in USD, excluding applicable taxes, and usage-based charges continue beyond the credit. Spend Management is available on Pro at no extra cost; the set amount applies to metered usage after monthly credit, while seats and separate add-ons are excluded. Reaching the threshold can pause production deployments; checks can take several minutes, and storage charges continue. | A public starting point for the team plan, credit, and spend-alert/pause controls. | Total monthly cost, additional developer-seat costs, eligibility for every feature, quote terms, billing-address taxes, credit treatment for each workload, or a guaranteed maximum bill. Pausing production makes websites/APIs unavailable and is not a safe hard cap for an availability-sensitive service. |
| [Vercel Dublin regional pricing](https://vercel.com/docs/pricing/regional-pricing/dub1) | Dublin (`dub1`) managed-infrastructure pricing is available only to Pro users. The page reports (USD): Fast Data Transfer $0.15/GB; CDN requests $2.40/million; Fluid Active CPU $0.168/hour; Fluid Provisioned Memory $0.0139/GB-hour; Fast Origin Transfer $0.06/GB; and Service Requests $0.60/million. The page says applicable taxes are excluded and was last updated 14 September 2026. | Region-specific marginal rates for the published metered resources, subject to a current-plan recheck. | MylesNet traffic, the amount of the Pro credit consumed by a workload, the final invoice, all add-ons, or production latency/capacity. These rates alone do not select Dublin. |
| [Vercel region list](https://vercel.com/docs/regions) and [limits](https://vercel.com/docs/limits) | The region list maps `dub1` to Dublin, Ireland (`eu-west-1`). The limits page publishes plan-specific limits, including a 2,048-route-per-deployment limit and a 45-minute build-time limit for Pro; function limits and features depend on plan. | Region identifier and public plan limits that must be compared with the actual deployment and traffic model. | The application’s route compilation, server-function performance, failover behavior for a selected plan, or a measured service-level result. |
| [Convex plans and resource pricing](https://www.convex.dev/pricing), [usage limits](https://docs.convex.dev/production/state/limits) | The public page lists Professional at $25 per developer/month and Business & Enterprise with a $2,500 monthly minimum. The resource-price selector was displaying US East at 1.0× when checked. Professional’s displayed included quantities/rates included 25 million function calls, then $2 per additional million; 250 action-compute GB-hours, then $0.30/GB-hour; 50 GB database storage, then $0.20/GB; 100 GB file storage, then $0.03/GB; 50 GB database I/O, then $0.20/GB; 50,000 search query-GB units, then $0.10 per additional 1,000 units; and 50 GB egress, then $0.12/GB. The page says included amounts are shared across regions. The limits page says Professional uses S256, with a 10,000 concurrent-session limit, up to 256 concurrent queries/mutations, and 512 concurrent actions; team developers are limited to 1–20. | Public baseline price/resource metrics and plan capacities to compare with measured application workloads. | A negotiated quote, fit for MylesNet’s workload, or measured capacity. The published Convex session limit is not an ISP network subscriber-session limit, and subscriber account totals cannot be compared to it without workload/concurrency evidence. Recheck values in the live region selector and written quote. |
| [Convex region documentation](https://docs.convex.dev/production/regions) | It lists Europe (Ireland) as `aws-eu-west-1`, with a 30% regional surcharge; usage outside the US is billed at 1.3× standard rates. It says deployment region cannot be changed in place; moving requires a new deployment/project and data export/import. | An Ireland usage multiplier and a material region-migration constraint for planning. | The treatment of every Professional included allowance or fixed developer charge under an Ireland deployment; obtain confirmation before financial modeling. It also does not establish data-transfer approval, latency, or regional availability guarantees. |

## Cost-model worksheet

The appointed finance/commercial and platform/operations owners should complete the following inputs for each scenario. Use measured synthetic or approved test workloads where available. Preserve source, collection period, method, and assumptions beside every value. Do not derive production load solely from subscriber or tenant counts.

| Scenario | Tenant / subscriber / branch volume | Peak concurrency and request mix | Duration and source | Status |
|---|---|---|---|---|
| Local development and previews | Not supplied | Not supplied | Local/preview evidence when authorized | Open |
| Controlled pilot | Not supplied | Not supplied | Pilot size and countries require owner approval | Open |
| Planned launch cohort | Not supplied | Not supplied | Cohort, country sequence, and date open | Open |
| 12-month planning target | The approved planning target is 1,000 active paying ISP tenants and 100,000 active subscriber accounts, twelve months after public launch. | Peak ratios, branches, usage mix, and growth curve not supplied | Planning target from `READINESS-DECISIONS.md`; it is not a measured workload | Open model |
| Stress and recovery case | Largest-tenant, synchronized peak, backup/restore and failover sizes not supplied | Not supplied | Must include growth headroom and failure-mode traffic | Open |

For each scenario, record at least:

- **Vercel:** number of teams/projects and paid developer seats; cached/static versus dynamic request counts; request regions and function region; Dublin CDN requests and transfer GB; function invocations; Active CPU hours; provisioned GB-hours; ISR reads/writes; image transformations; build frequency/duration; logs/observability, storage, protection add-ons, and spend alerts. Model no-credit/overage and spend-triggered production-pause scenarios; the pause is delayed and causes an outage, so it is not a risk-free spending cap.
- **Convex:** developer seats and plan; function calls by query/mutation/action; action-compute GB-hours; database and file storage; database I/O; search storage/queries; egress; live sessions and subscriptions; backups, restore/export operations, and log retention. Identify the Ireland pricing selector result or attach a written quote.
- **Shared inputs:** synthetic workload source; expected largest tenant and branch count; active and peak users; data sizes and retention assumptions; country/region placement; growth curve; reliability and recovery scenarios; billing currency/tax; named owner and evidence date.

Keep service-objective calculations with MN-PLAN-019 and its owner. This packet consumes approved measurement assumptions when supplied; it does not set SLOs or alter the published targets.

### Calculation method

1. Price Vercel’s applicable subscription, seats, project add-ons, and metered usage at the selected plan and actual region. Apply credits only as the provider’s then-current billing terms permit; model spend controls, a no-credit/overage case, and the service impact of any spend-triggered production pause.
2. Price Convex at the selected plan using the actual region’s usage rates, included allowances, developer count, and quoted support/contract items. For Ireland, reconcile the published 1.3× usage statement with the current included-usage mechanics before treating any result as a budget.
3. Add the providers to a monthly USD subtotal, then list tax, currency conversion, and excluded services separately. Do not combine costs from different deployment scenarios or assume annual discounts.
4. Show low/base/high cases with input provenance and sensitivities. At minimum vary dynamic request share, active CPU duration, data egress, Convex action compute, retained data, and peak concurrency. A workload change must update the model rather than silently changing assumed unit prices.
5. Reconcile estimates to provider usage dashboards/invoices only after an authorized, isolated environment exists. No provider account, deployment, or billing change was made for this evidence review.

No numeric total is provided because traffic, tenant mix, user seats, resource rates, storage retention, architecture deployment count, limits, and account quotes remain unknown. The resource unit prices above are not a total-cost estimate.

## Evidence and approvals still required

| Evidence / decision | Accountable role to appoint | Acceptance evidence | Current state |
|---|---|---|---|
| Workload volumes, largest tenant, peak profile, traffic/cache mix, and growth curve | Product owner and engineering/architecture lead | Signed scenario inputs tied to route/action flows and the approved 12-month target | Open |
| Per-region request, compute, storage, egress, retention, build, and team-seat forecast | Platform/release operations and finance owner | Reproducible spreadsheet/model with units, source data, assumptions, sensitivity cases, and date | Open |
| Vercel Pro/Enterprise eligibility, Dublin rates, security/operations features, credits, limits, invoice taxes, and spend guardrails | Platform/release operations and finance owner | Current provider documentation plus written quote/account confirmation for required features | Open |
| Convex plan and Ireland-specific pricing, included allowance treatment, workload limits, backup/restore and support terms | Platform/operations and finance owner | Live regional selector evidence or written quote, usage forecast, and plan-limit confirmation | Open |
| KE, UG, TZ latency, data-transfer, retention, capacity, recovery, contract, and legal review | Country counsel, data/privacy, security, and operations owners | Country-specific evidence and approvals; measured latency and failure/recovery results in approved environments | Open |
| Production provider/region selection and budget | Project owner with appointed accountable owners | Reconciled cost model, legal/provider approval, workload and failover evidence, and explicit decision record | Not selected |

## Next steps

1. Appoint the workstream owners using the [Phase 0 owner appointment and sign-off register](PHASE-0-OWNER-APPOINTMENT-REGISTER.md).
2. Define workload scenarios from the route/action catalog and approved commercial targets, then collect measurements from a synthetic environment only after the relevant environment work is authorized.
3. Request current plan, regional pricing, feature, service-limit, support, and contract details from both providers; confirm the Convex Ireland surcharge/included-usage mechanics.
4. Reconcile the resulting estimates with recovery, SLO, legal-transfer, tax, and country evidence in [`ADR-0004`](adr/ADR-0004-data-region.md), [`ADR-0009`](adr/ADR-0009-storage-exports-and-backups.md), [`ADR-0011`](adr/ADR-0011-observability-and-slo-measurement.md), and MN-DEC-019 before any region or plan decision.
5. Recheck all time-sensitive sources immediately before the owner decision. Public documentation is discovery evidence, not a contract, measured capacity proof, legal approval, or production authorization.

## Sources

- Vercel: [Plans and pricing](https://vercel.com/pricing); [Dublin regional pricing](https://vercel.com/docs/pricing/regional-pricing/dub1); [regions](https://vercel.com/docs/regions); [limits](https://vercel.com/docs/limits); [spend management](https://vercel.com/docs/spend-management).
- Convex: [Pricing](https://www.convex.dev/pricing); [regions](https://docs.convex.dev/production/regions); [usage limits](https://docs.convex.dev/production/state/limits).
- MylesNet: [Implementation plan](IMPLEMENTATION-PLAN.md); [regional data ADR](adr/ADR-0004-data-region.md); [storage and recovery ADR](adr/ADR-0009-storage-exports-and-backups.md); [SLO/observability ADR](adr/ADR-0011-observability-and-slo-measurement.md); [readiness decisions](READINESS-DECISIONS.md); [decision register](DECISION-REGISTER.md).
