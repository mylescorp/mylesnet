# FX rate observation and conversion provenance contract

- **Task:** MN-PLAN-016 · **Plan phase:** Phase 0 — close planning and turn decisions into build contracts
- **Status:** Planning contract only. No central-bank series, quote convention, fallback policy, rate value, API, or integration is selected or implemented.
- **Authority:** `AGENTS.md`; `DECISION-REGISTER.md` MN-DEC-015/MN-DEC-022; `IMPLEMENTATION-PLAN.md` §§5, 8, 10; `TECHNICAL-SPECIFICATION.md` §14.5; ADR-0006.

## Purpose and boundary

This contract defines the provenance that must travel with a foreign-exchange (FX) rate and any conversion that uses it. A rate observation is evidence from a named source series. A conversion snapshot is the record of how one amount was converted for a named business use. They are separate records: a later source correction must not rewrite a previously issued invoice or a previously generated report artifact.

The approved uses have different timing:

- **Platform invoice issuance:** convert Hotspot fees from their sale currencies into the selected billing legal entity's invoice currency using the official central-bank reference rate at invoice issuance. Preserve the rate, source, currency pair, and as-of time with the invoice and show them to the Owner.
- **Consolidated reporting:** convert totals using the latest official central-bank reference rate at report view time, with source and as-of metadata. Stale or missing rate behavior must be visible.

These conversions are not payment settlement, wallet movement, payout calculation, or authority to move value between legal entities. Original amounts and currencies remain authoritative. Platform subscription billing and subscriber-collection ledgers stay separate; no FX record permits a cross-entity wallet draw or silent conversion.

## Logical records

The fields below specify required meaning, not final database columns, wire names, IDs, or enum values. Physical schema, decimal encoding, indexing, retention, and API details remain future design work.

### Rate observation

Each observed rate must be attributable to one exact source series and source observation.

| Information | Required meaning |
|---|---|
| Observation identity | Stable identity for this immutable captured observation, distinct from the source's own row/reference. |
| Source identity | Institution, named dataset/series, source reference, and any source or adapter version needed to reproduce how the row was interpreted. The exact institutions and series remain open. |
| Currency pair | Explicit base and quote currency plus an explicit quote-direction convention. A pair label alone is insufficient. The permitted pair set and direction for each source remain open. |
| Rate meaning | Rate kind (for example, a published indicative, weighted, or other series) and the exact source-provided value. Keep the source's displayed value/text and a normalized calculation value linked; do not infer precision from display formatting. |
| Source time | Source business/effective date and source publication time/timezone when provided. Preserve what the source actually exposes; do not invent a timestamp or imply that publication time equals applicability time. |
| Retrieval evidence | Retrieval time in UTC, source page/document or record reference, and an integrity reference for the captured evidence where permitted. Whether raw source content may be retained, and its licensing and retention terms, remain open. |
| Interpretation | Version of the mapping/convention used to interpret the source field. A changed mapping creates a new interpretation/observation reference rather than silently changing an already-used value. |
| Correction lineage | Link a corrected or superseding observation to the prior one. Do not overwrite an observation that has been used by an issued invoice or persisted report. |

### Conversion snapshot

Every persisted conversion result must identify its purpose and the exact inputs and interpretation that produced it.

| Information | Required meaning |
|---|---|
| Snapshot identity and purpose | Stable identity plus a purpose such as invoice issuance or report view/export. The exact purpose vocabulary remains open. |
| Use context | Invoice and line reference for invoice conversion, or report definition, filters, scope, and generation/view reference for reporting. Use server-resolved tenant, legal-entity, and branch context where applicable. |
| Input | Original amount and currency, retained without replacement by a converted total. |
| Rate link | Exact immutable rate-observation identity, source identity, source value, pair, quote direction, rate kind, source date/as-of value, and retrieval time used. |
| Calculation | Conversion orientation/formula and the unrounded result or sufficient calculation evidence to reproduce it. Precision, decimal representation, and rounding rule/version remain open for Finance approval. Binary floating-point must not be assumed safe for money. |
| Output | Converted amount and target currency, together with the calculation/policy version and creation time in UTC. The final amount must follow an approved currency-aware calculation and rounding rule. |
| Freshness/result state | Whether the selected observation was accepted for the intended use under the then-approved policy. Stale, missing, ambiguous, unavailable, and superseded handling rules and their exact state values remain open. |

## Preservation and display requirements

- Preserve the observation and conversion inputs/outputs used for an issued invoice as invoice evidence. An invoice correction is a linked adjustment or credit note; it does not replace the original conversion snapshot.
- A persisted report/export must retain enough provenance to identify the rate observation and conversion rule used for its displayed totals. Whether ordinary dashboard views are persisted, regenerated, or content-addressed is an open snapshot-retention decision.
- A later source correction or policy change creates new evidence. It does not silently rewrite historical financial or exported-report facts. Recalculation, restatement, and notification rules require Finance approval.
- Displayed converted totals identify their target currency, source, rate, pair/direction, and source as-of time. Include retrieval/generation time where needed to distinguish source age from when the application observed it.
- A missing, ambiguous, unavailable, or policy-stale rate must never be represented as a successfully current conversion. The allowed operator/user behavior, fallback, blocking, and refresh policy remain open; there is no implied carry-forward or substitute source.
- Keep report and invoice access within the same tenant, branch, legal-entity, role, and export permissions as their underlying financial data. Do not use source metadata as authorization context.
- Rate-source evidence may be public, but a converted snapshot inherits the confidentiality of its amounts, report scope, tenant/entity context, and invoice. Final sensitivity labels and retention schedules require data/privacy and Finance owners.

## Decisions still open

This contract does not select or approve any of the following:

1. Central-bank institution, exact series, source access method, supported pair list, and any route for cross rates.
2. Base/quote direction, inverse-rate use, source rate type, observation date/time semantics, business-day cutoff, and whether a source value is applicable at invoice issuance or report view time.
3. Retrieval frequency, cache lifetime, source correction handling, stale thresholds, missing/unavailable behavior, fallback or no-fallback policy, and whether conversion blocks or visibly degrades.
4. Numeric representation, supported precision, intermediate precision, currency rounding, and how rates are rounded for calculation versus display.
5. Whether dashboard view conversions are transient, reproducible, or persisted; report/export snapshot retention and correction/restatement behavior.
6. Source licensing, permitted caching/archive, evidence integrity method, source/API terms, security controls, and accountable Finance, accounting, data, and operations owners.

These remain under MN-DEC-015 and MN-DEC-022. Public central-bank page evidence is tracked separately; evidence of a published table does not settle the above policy, provider-access terms, accounting treatment, or legal approval.

## Phase 0 contract acceptance

Before implementation, the Finance and engineering owners must review a source-to-snapshot example for each use (invoice issuance and report view/export), confirm the complete provenance and correction path, and resolve or explicitly gate every open choice above. Future acceptance cases must cover pair inversion/ambiguity, missing/stale/corrected observations, reproducible calculation and rounding, report-view changes, invoice immutability, tenant/entity isolation, and prevention of wallet/settlement conversion. This document does not constitute that approval and does not authorize product implementation.

## Related requirements

- [`IMPLEMENTATION-PLAN.md`](IMPLEMENTATION-PLAN.md) §§5, 8, 10
- [`TECHNICAL-SPECIFICATION.md`](TECHNICAL-SPECIFICATION.md) §14.5
- [`CORE-FLOWS-AND-DATA.md`](CORE-FLOWS-AND-DATA.md)
- [`DATA-OWNERSHIP-AND-CLASSIFICATION.md`](DATA-OWNERSHIP-AND-CLASSIFICATION.md)
- [`API-ERROR-EVENT-CONTRACT-BACKLOG.md`](API-ERROR-EVENT-CONTRACT-BACKLOG.md)
- [`ADR-0006`](adr/ADR-0006-payment-ledger-and-country-gates.md)
