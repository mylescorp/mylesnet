# Phase 0 Country Tax and Invoicing Evidence Plan

**Task:** MN-PLAN-021 · **Plan phase:** Phase 0 — close planning and turn decisions into build contracts<br>
**Date:** 13 November 2026<br>
**Status:** Public-source preparation packet; MN-DEC-015 and CP-TAX-01 remain open.<br>
**Countries:** Kenya, Uganda, and Tanzania, with Tanzania Mainland and Zanzibar tracked separately for VAT.

## Purpose and boundary

Prepare a reviewable packet for appointed country tax counsel and the finance/commercial owner to resolve the tax and invoicing questions attached to the approved MylesNet pricing model. This is public-source discovery, not tax or legal advice, a tax classification, a rate recommendation, an invoice-compliance determination, or permission to issue production invoices or connect to a fiscalization system.

The project has approved the Hotspot and PPPoE usage formulas, a combined monthly minimum, separate MylesNet and tenant financial ledgers, and separately itemized applicable taxes. The tax treatment of each supply, the Hotspot fee base, tax rates, local PPPoE equivalents and minimums, registration duties, invoice rules, and cross-border consequences remain open. This packet does not alter those decisions or the separate country legal/data-protection gate in MN-DEC-018.

Official-source pages and legislation below were reviewed on 13 November 2026. They are starting points only. Counsel must confirm the current consolidated law, amendments and commencement, gazetted directions and exclusions, administrative practice, the facts of the contracting entities and flows, and any sector-specific rules before giving advice. Revenue-authority guidance may be incomplete or lag an amendment.

## Phase 0 acceptance criteria

This task contributes to Implementation Plan Phase 0 work items 5–6 and country pack requirement `CP-TAX-01`.

- Record primary government or revenue-authority sources and the date checked for all launch countries; separately identify Tanzania Mainland and Zanzibar VAT sources.
- Map the distinct MylesNet subscription invoice, the Hotspot usage fee, PPPoE usage, Enterprise charges, and ISP-to-subscriber charges/receipts as questions for counsel, without deciding their tax characterization.
- Give counsel and the finance/commercial owner a concrete questionnaire and country sign-off record covering tax registration, rates and bases, invoicing/fiscalization, records, adjustments, and evidence owners.
- Keep every unsupplied value and approval explicitly open. Do not infer legal-entity establishment, customer location, taxable status, tax base, rate, threshold, filing deadline, record period, or e-invoicing integration permission.

Delivery of this packet does not close MN-DEC-015, CP-TAX-01, MN-DEC-018, or any country launch gate. Closure requires named owners and written, country-scoped counsel/finance approval against actual entity, supply, customer, payment, and system facts.

## Approved product facts and separate questions

Use these approved product facts as inputs, not as tax conclusions:

- MylesNet platform subscription invoices use MylesNet merchant accounts and a billing legal entity's exact currency. The SaaS subscription ledger is separate from each ISP's subscriber collection ledger.
- Hotspot usage is measured from audited confirmed sales before provider fees, less refunds and reversals. The standard usage fee is 3% of that defined measure.
- PPPoE usage has a USD $0.25 reference per billable account-month and is converted to validated fixed local-currency values. Hotspot and PPPoE charges share one monthly minimum. Applicable tax is listed separately per billing legal entity.
- Tenant subscriber plans, subscriber collections, refunds, credits, and payouts belong to a distinct tenant-side service and ledger. Tenant-owned and pooled collection models have different legal and tax facts; pooled collection remains disabled until separately approved.

Counsel and finance must determine whether these are distinct supplies or tax bases, how the Hotspot percentage is treated, whether any payment amount is held or collected for another party, and how customer, entity, country, and currency affect the invoice and return. The product facts do not settle those questions.

## Official public-source inventory

| Jurisdiction | Official starting points reviewed | What the source says for follow-up | Limits and required confirmation |
|---|---|---|---|
| Kenya | [KRA eTIMS overview](https://www.kra.go.ke/business/etims-electronic-tax-invoice-management-system/learn-about-etims/what-is-etims); [Tax Procedures Act, Cap. 469B, as published by Kenya Law](https://new.kenyalaw.org/akn/ke/act/2015/29/eng%402024-12-27/source); [Tax Procedures (Electronic Tax Invoice) Regulations, 2024, Legal Notice 64](https://new.kenyalaw.org/akn/ke/act/ln/2024/64/eng%402024-03-28/source); [KRA VAT on Digital Marketplace Supply FAQ](https://www.kra.go.ke/helping-tax-payers/faqs/vat-on-digital-marketplace-supply) | KRA's current eTIMS guidance describes onboarding and electronic invoicing for persons engaged in business whether or not VAT registered. The Tax Procedures Act and 2024 Regulations describe electronic invoicing through the Commissioner's system, invoice contents/transmission, exclusions, and exemptions. KRA's digital-marketplace guidance describes a separate simplified route for qualifying non-resident suppliers and invoicing expectations. | Determine the contracting entity's residence/permanent establishment and whether any supply falls under digital-marketplace or electronic-services rules. Confirm current scope and interaction between the 2024 regulations and KRA digital-supply guidance, VAT registration, B2B/B2C status, tax base, required receipt/invoice, currency, and record/return duties. The source set does not classify MylesNet's SaaS or Hotspot fee. |
| Uganda | [URA EFRIS guidance](https://ura.go.ug/en/efris/); [URA notice on additional taxpayers required to use EFRIS](https://ura.go.ug/en/additional-taxpayers-required-to-use-efris/) | URA explains VAT e-invoices, non-VAT e-receipts, and credit/debit notes in EFRIS. Its 11 August 2026 notice describes additional designated sectors, including information technology and communication, effective 1 July 2025, alongside the existing VAT-registered obligation; it also names an exclusion for non-resident digital-service providers required to pay digital services tax. | Determine entity and customer status, whether services fall under a digital-services regime, the relevant gazetted designation/exclusion, current VAT and e-invoicing law, registration/representative obligations, B2B/B2C treatment, and any filing/receipt rules. URA web guidance is not a substitute for reviewing current Acts, amendments, and Gazette notices with counsel. |
| Tanzania — Mainland | [Office of the Attorney General MIS, Value Added Tax Act, Cap. 148 R.E. 2023](https://oagmis.oag.go.tz/portal/acts/revised/253/download); [Office of the Attorney General MIS, Tax Administration (General) Regulations](https://oagmis.oag.go.tz/portal/legislation/965/download) | The VAT Act sets location rules for supplies to registered and unregistered persons and includes electronic-service categories such as software, websites/hosting, online intermediation, and advertising. The regulations describe the Electronic Fiscal Device Management System and fiscal receipts/invoices; they also describe conditions under which accounting software may be allowed to issue an electronically signed fiscal invoice. | Confirm the current statutory text and amendments, territorial scope, treatment of the proposed services and entity/customer types, VAT registration and tax-point rules, exemptions, fiscal-device/software approval, e-signature and system integration, corrections, downtime, retention, and records. Do not extend Mainland law assumptions to Zanzibar. |
| Tanzania — Zanzibar | [Zanzibar Revenue Authority (ZRA), VAT on Digital Service guidance](https://www.zanrevenue.org/tax/vat-on-digital-service); [ZRA, consolidated VAT Act No. 4 of 1998 (2025)](https://www.zanrevenue.org/resources/view/the-value-added-tax-act-no-4-of-1998-final-consolidated-2025); [ZRA VAT Regulations](https://www.zanrevenue.org/resources/view/value-added-tax-regulations-of-2012) | ZRA describes a separate VAT-on-electronic-services process for non-resident suppliers and separately administered Zanzibar VAT. Its guidance refers to amendments introducing sections 4A and 4B and describes different handling for registered and non-registered recipients. | ZRA says its 2025 consolidation covers amendments only through June 2025 and is for reference, not legal proceedings; its current resource list also contains 2026 materials. Counsel must check the latest Gazette/Finance Act and ZRA rules, territorial and customer-location tests, supplier/recipient registration, fiscalization, invoice/record requirements, and interactions with Mainland supplies. No Zanzibar tax or e-invoice result is approved here. |

### Evidence notes

- The jurisdictions' fiscalization systems are not interchangeable invoice templates. Ask counsel and the finance/engineering owners whether each applicable rule requires a tax authority system, an approved fiscal device/software, a signature or receipt identifier, or only a customer-facing invoice/receipt.
- A public list or regulator page is not proof of MylesNet registration, exemption, accepted API, licensed status, or system compatibility. No tax account, portal, provider, invoice, or live transaction was accessed or created for this review.
- Tanzania VAT evidence is split by Mainland and Zanzibar. Treat the legal entity, customer, branch, and place-of-supply mapping as an explicit design input pending counsel advice.
- Rates and thresholds are intentionally omitted. They can change and must be verified by counsel/finance from operative sources before price configuration or a tax claim is approved.

## Counsel and finance decision questionnaire

For each country and the two Tanzania VAT jurisdictions where relevant, provide written answers tied to actual entities, contract terms, flows, and current primary sources:

1. Which MylesNet and tenant legal entities contract, invoice, collect, hold, or remit money? What registrations, tax numbers, tax representatives, local establishments, or licenses apply?
2. How are each of these characterized: MylesNet subscription, Hotspot usage fee, PPPoE usage, Enterprise services, subscriber access packages, subscriber payment collection, provider fees, refunds, credits, payouts, and pooled collections?
3. For each flow, which party supplies what to whom, where is the customer located, what is the B2B/B2C or taxable-person status, and what evidence supports the place-of-supply result?
4. What taxes apply (including VAT or digital-services tax and withholding, if any)? Define the taxable base for the Hotspot percentage, the PPPoE local equivalent/minimum, discounts, refunds/reversals, and any amount collected for a tenant. Give effective dates and official evidence; do not infer that one country treatment applies in another.
5. What registration threshold, rate, tax point, filing/payment frequency, payment currency, return, invoice/receipt, and correction/credit/debit-note rules apply to each supply? Which values may be shown before owner approval?
6. Must invoices be generated or reported through eTIMS, EFRIS, an EFD/VFD or other authority system? What approved integration route, identifiers, signature, buyer/supplier fields, invoice numbering, offline, failure/retry, cancellation, and audit behavior are required?
7. What invoice language and currency are permitted? How should the selected MylesNet billing-entity currency and branch-local subscriber currency appear? What FX evidence/time, rounding, and tax-disclosure records are required?
8. What records, retention schedule, archive/access, taxpayer data handling, and audit evidence are required? Coordinate privacy, transfer, and retention conclusions with MN-DEC-018 and the approved classification map rather than inventing periods.
9. How do Mainland Tanzania and Zanzibar VAT registration, digital-service rules, EFD/fiscalization, invoice records, and cross-territory supplies differ? What current Zanzibar Gazette instrument controls each answer?
10. What residual legal/commercial risks, assumptions, disabled features, owner, approval date, evidence expiry/revisit trigger, and country go/no-go dependencies remain?

## Country tax and invoice sign-off record

Create one signed record per country, with separate records for Tanzania Mainland and Zanzibar where VAT treatment differs. Keep a row open until the evidence is supplied.

| Field | Required record |
|---|---|
| Country/jurisdiction and legal entity | Exact contracting/invoicing entity, registration status, branch/customer location facts, and responsible owner |
| Supplies reviewed | Each product/collection flow in the questionnaire; contracting party, recipient, consideration, payment path, and customer type |
| Authority and operative sources | Statute, amendment, Gazette notice, authority guidance, section/regulation, publication/effective date, link, and counsel's current-version check |
| Tax result | Tax classification, jurisdiction, registration, rate/base/tax point, currency/FX/rounding, exemptions, and reasoning; cite written counsel/finance approval |
| Invoicing/fiscalization result | Required document, fields, numbering, language, authority/system/device, technical approval, corrections, offline and retry procedure |
| Records and reporting | Required records and retention source, filing/payment deadlines, reconciliation evidence, and owner |
| Approval | Named country tax counsel, finance owner, approvers, signed date, residual risks, revisit/expiry date, and country gate status |

**Current state:** no named tax counsel or finance/commercial owner is recorded; no country tax conclusion or fiscalization integration is approved. All country sign-off rows remain open.

## Related requirements and records

- [MN-DEC-015 in the decision register](DECISION-REGISTER.md): fixed local rates/minimums, tax treatment, FX policy, quote semantics, precision, budget, and approval remain open.
- [Country and operations packs, CP-TAX-01](COUNTRY-AND-OPERATIONS-PACKS.md): country-scoped tax and invoicing approval gate.
- [FX source evidence](FX-RATE-SOURCE-PUBLIC-EVIDENCE.md) and [FX conversion provenance contract](FX-CONVERSION-PROVENANCE-CONTRACT.md): no series, quote, fallback, precision, or finality policy is selected.
- [Implementation plan, Phase 0](IMPLEMENTATION-PLAN.md#phase-0--close-planning-and-turn-decisions-into-build-contracts): source evidence is preparatory and does not satisfy Phase 0 owner appointments or user authorization.
- [Implementation status](IMPLEMENTATION-STATUS.md) and the task handoff in `agent-handoffs/`.
