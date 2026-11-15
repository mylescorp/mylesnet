# Standard pricing and tax approval worksheet

**Task:** MN-PLAN-027

**Plan phase:** Phase 0 — close planning and turn decisions into build contracts

**Status:** Owner worksheet only; local price values, tax rules, and commercial approvals remain open.

## Purpose and boundary

This worksheet organizes the remaining country-specific decisions needed to publish MylesNet standard Hotspot/PPPoE usage prices and issue compliant subscription invoices in Kenya, Uganda, and Tanzania. It is a working record for appointed product/commercial, finance/accounting, and country tax/legal owners.

It does not select local rates, tax treatments, invoice terms, or FX policy; provide legal/tax advice; create a cost or revenue forecast; change an approved meter formula; or authorize product implementation or collection. Fill values only from dated evidence and signed owner decisions. No invoice or tax amount is calculated in this packet.

## Approved rules to preserve

These inputs are approved and are not reopened by this worksheet:

- Standard Hotspot usage is 3% of audited confirmed Hotspot sales before provider fees, less refunds and reversals; pending and failed payments are excluded.
- Standard PPPoE usage uses a USD $0.25 reference per distinct non-test account with an eligible active PPPoE entitlement at any time during a billing month. Count each account once at the full unit; do not prorate.
- Combine Hotspot and PPPoE on one monthly subscription invoice. Apply one $5-equivalent minimum to the combined pre-tax usage charge in every paid month, including a zero-usage month.
- Publish fixed, versioned local-currency PPPoE rates and fixed local equivalents of the monthly minimum after validating the equivalents. Standard rates are before tax; itemize applicable taxes separately by billing legal entity.
- For cross-currency Hotspot fees, calculate by sale currency, then use the approved official central-bank reference rate at invoice issuance to convert to the selected billing legal entity's invoice currency. Show and preserve the applied rate and as-of time.
- Standard billing is monthly with no annual-prepayment discount. Give at least 30 days' notice for a published rate change and apply it prospectively from a future billing cycle, never retroactively.
- A tenant may have multiple branches/legal entities but has one MylesNet subscription invoice. The selected billing legal entity determines the invoice currency; do not convert or draw a wallet across legal entities. Standard offers impose no hard subscriber/router caps or per-staff/per-voucher charges.
- Enterprise remains custom-quoted for at least 10,000 active subscribers or a documented multi-region/regulatory need; the approved quote fields and feature boundaries remain in the canonical decisions.

See [MN-DEC-022](DECISION-REGISTER.md), [READINESS-DECISIONS.md](READINESS-DECISIONS.md), [CORE-FLOWS-AND-DATA.md](CORE-FLOWS-AND-DATA.md), and [TECHNICAL-SPECIFICATION.md §14.5](TECHNICAL-SPECIFICATION.md#145-saas-billing-entitlements-and-usage-lifecycle).

## Country price and tax sign-off register

Complete one record per operating and billing legal entity. Currency labels are approved working metadata; all amounts, tax treatment, and invoice-compliance claims remain open.

| Country / billing currency | Fixed PPPoE rate per eligible account-month | Fixed combined monthly minimum | FX basis and rounding used to validate fixed values | Tax treatment and rate | Invoice entity/registration and fields | Effective date and notice | Owner evidence / status |
|---|---|---|---|---|---|---|---|
| Kenya / KES | Open | Open | Open; cite source, pair/direction, observation date, calculation, and rounding | Open; finance and country counsel approval required | Open; confirm seller, registration, numbering, receipt/tax fields, language, and any fiscalization | Open; versioned prospective schedule; at least 30 days' notice | Open |
| Uganda / UGX | Open | Open | Open; cite source, pair/direction, observation date, calculation, and rounding | Open; finance and country counsel approval required | Open; confirm seller, registration, numbering, receipt/tax fields, language, and any fiscalization | Open; versioned prospective schedule; at least 30 days' notice | Open |
| Tanzania / TZS | Open | Open | Open; cite source, pair/direction, observation date, calculation, and rounding | Open; finance and country counsel approval required | Open; confirm seller, registration, numbering, receipt/tax fields, language, and any fiscalization | Open; versioned prospective schedule; at least 30 days' notice | Open |

Do not use an indicative public FX observation as a fixed local price without Finance approval. Keep the approved fixed rate schedule separate from the invoice-time conversion snapshots used for Hotspot sale currencies. The [FX source inventory](FX-RATE-SOURCE-PUBLIC-EVIDENCE.md) and [conversion provenance contract](FX-CONVERSION-PROVENANCE-CONTRACT.md) capture evidence and open source/quote/freshness/rounding choices; neither selects a series or local amount.

## Decision and evidence checklist

| Decision | Required owner input | Acceptance evidence before price publication or invoicing | State |
|---|---|---|---|
| Fixed PPPoE price in each local currency | Approved amount per billable account-month, comparison to the USD reference, market/cost context, rounding, and effective date | Reproducible calculation and country-specific Finance plus Product/Commercial approval | Open |
| Fixed monthly minimum in each local currency | Approved local equivalent of the $5 floor and confirmation it applies once to combined Hotspot/PPPoE before tax, including a paid zero-usage month | Reproducible example for zero, below-floor, and above-floor usage; Finance approval | Open |
| Hotspot service-fee tax basis | Country-specific tax classification and the taxable base for MylesNet's service fee. Analyze the fee's tax treatment separately from its approved usage-meter base: confirmed Hotspot sales before provider fees, less refunds and reversals. Tax advice does not change that meter formula. | Dated counsel/tax advice and Finance-approved examples showing the fee tax calculation separately from the unchanged usage-meter calculation; no tax percentage is inferred here | Open |
| Tax application and invoice presentation | Applicable indirect/direct tax rules by MylesNet billing entity; inclusive/exclusive display; tax lines, currency, rounding, credits/corrections, and required disclosures | Country counsel and Finance sign-off tied to current law and entity registration; compliant sample invoices | Open |
| FX for cross-currency Hotspot fees | Authoritative central-bank series by invoice use case, quote direction, cross-rate/inversion, timestamp, freshness/missing behavior, precision, and rounding | Finance-approved source-to-invoice example with an immutable conversion snapshot; link the approved series and policy | Open |
| Price schedule governance | Named approvers, version identifier, notice channel, effective-cycle rule, migration of calculator/marketing copy, and Enterprise exception authority | Approved change workflow and evidence of at least 30 days' prospective notice; no retroactive repricing | Open |
| Calculator and quote consistency | Pre-tax public estimate rules, local-currency selection, manual selector, and exact Enterprise qualification/quote fields | Product/Finance sign-off that public examples agree with approved price versions; never publish open values as final | Open |

Country counsel and Finance must verify the current tax and invoicing requirements before any public price or invoice behavior is approved. The country-pack `CP-TAX-01` register already identifies tax classification, registrations, invoice fields, numbering, language, corrections, retention, and e-invoicing/fiscalization as open.

## Reproducible invoice scenarios for owner review

After the country table is filled, prepare dated examples from the approved inputs. Keep the source amounts, units, price version, tax assumptions, FX evidence, and calculation steps beside each result.

1. A paid month with no usage: show the fixed local minimum once, then apply the separately approved tax treatment.
2. A paid month with PPPoE usage below and above the minimum: count distinct eligible accounts once each at the fixed local rate; do not prorate a partial month.
3. Mixed Hotspot and PPPoE usage: show the 3% eligible Hotspot base and PPPoE amount combined before applying one minimum, not one floor per meter.
4. Refund/reversal and provider-fee case: demonstrate the approved Hotspot basis after refunds/reversals and before provider fees; record what remains unresolved if country tax advice treats the base differently.
5. Cross-currency Hotspot sale: show original sale currency, selected billing entity/invoice currency, approved invoice-issuance FX snapshot, and separate tax lines. Do not change the fixed PPPoE price through this conversion.
6. Multi-branch or multi-entity tenant: show the approved tenant-wide aggregation and the selected billing legal entity/currency; never combine a different entity's wallet or issue cross-entity value conversion.
7. Rate change: show old/new price versions, effective future billing cycle, notice date/channel, and preserved historical invoices.
8. Enterprise quote: verify the approved threshold/qualification and required quote fields while keeping the negotiated price, cadence, and exceptions subject to separate approval.

These are future review examples, not calculations or tests performed in this task. Do not fill sample numbers from a current public exchange table and present them as a selected schedule.

## Roles and closure

- **Product/commercial owner:** confirms standard offer presentation, price publication, calculator behavior, and Enterprise quote workflow.
- **Finance/accounting owner:** validates local fixed amounts, invoice calculations, ledger and tax-line interaction, FX evidence, rounding, versioning, and reconciliation.
- **Country counsel/tax adviser for KE, UG, and TZ:** provides entity-specific tax classification, registration, invoice/fiscalization, retention, and disclosure advice.
- **Platform/release operations owner:** confirms a controlled price-version rollout and that customer-facing calculator, invoices, notices, and reports use the same approved effective schedule.
- **Security/data owner:** confirms invoice and conversion evidence protection/retention where required.

The [Phase 0 owner appointment and sign-off register](PHASE-0-OWNER-APPOINTMENT-REGISTER.md) is the source for recording individual appointments and acceptance. This worksheet assigns no individual.

## Related artifacts

- [Decision register — MN-DEC-015 and MN-DEC-022](DECISION-REGISTER.md)
- [Country and operations packs — `CP-TAX-01`](COUNTRY-AND-OPERATIONS-PACKS.md)
- [Central-bank FX source evidence](FX-RATE-SOURCE-PUBLIC-EVIDENCE.md)
- [FX rate observation and conversion provenance contract](FX-CONVERSION-PROVENANCE-CONTRACT.md)
- [Technical specification — usage pricing and invoicing](TECHNICAL-SPECIFICATION.md#145-saas-billing-entitlements-and-usage-lifecycle)
- [Phase 0 implementation plan](IMPLEMENTATION-PLAN.md#phase-0--close-planning-and-turn-decisions-into-build-contracts)
- [Implementation status](IMPLEMENTATION-STATUS.md)
