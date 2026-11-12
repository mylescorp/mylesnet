# Public payment-provider documentation evidence

**Task:** MN-PLAN-012 · **Plan phase:** Phase 0 — close planning and turn decisions into build contracts<br>
**Evidence checked:** 11–12 November 2026<br>
**Status:** Public-document research only. No provider is selected or approved, and no payment call, account creation, sandbox transaction, or production transaction was made.<br>
**Authority:** [`AGENTS.md`](../AGENTS.md), `IMPLEMENTATION-PLAN.md` Phase 0 work items 5–6, [`DECISION-REGISTER.md`](DECISION-REGISTER.md) MN-DEC-013, and [`COUNTRY-AND-OPERATIONS-PACKS.md`](COUNTRY-AND-OPERATIONS-PACKS.md) CP-PAYMENT-01.

## Purpose and limits

Record what the currently listed payment candidates make publicly available to developers, and identify evidence still needed from each provider. The provider-document section records provider claims; the regulator section separately records public regulator-published listings. Neither establishes MylesNet's provider eligibility, product-specific approval, security, performance, fees, settlement, or contractual terms. The country pack's legal, provider, finance, privacy, and operations approvals remain open. Do not use API Explorer or invoke an endpoint to test these claims.

## Evidence summary

| Country / candidate | Public documentation observed | Safe testing evidence | What remains unverified |
|---|---|---|---|
| Kenya — Safaricom Daraja | The developer portal describes creating a sandbox app and simulating API calls. Its Transaction Status documentation presents the sandbox endpoint and describes status lookup as a secondary reconciliation path when callbacks are missing. | A provider-described sandbox path is publicly documented. No account, sandbox credentials, integration, or transaction was created or exercised here. The status-query endpoint alone does not prove that every collection product has an isolated sandbox. | Written merchant eligibility and production onboarding; supported collection products and channels for MylesNet; callback authentication/replay rules; limits, fees, settlement, reversals/refunds, dispute handling, support and service terms; verified sandbox behavior for every required flow. |
| Uganda — ioTec Pay | The API reference describes mobile-money collection, disbursement, transaction-status queries, OAuth client credentials, and configurable callback notifications. The public reference also includes sample/test transaction values and statuses. | Test examples are visible, but the reviewed public material does not establish a separate sandbox host, how test credentials are issued, or whether every listed flow is guaranteed to be non-financial. Treat the environment as unverified until ioTec confirms it in writing. | Merchant/account eligibility for MylesNet and the intended Uganda use; supported operators/currencies for the account; test-vs-live endpoint and credential separation; callback signature/authentication and replay controls; fees, settlement, reversals/refunds, disputes, support and service terms. |
| Tanzania — ClickPesa | Current official docs describe USSD-PUSH collection, payment-status lookup, mobile payout and bank payout APIs. The Quickstart explicitly says there is no sandbox, that requests including API Explorer use production and move real funds, and that API access is unavailable until KYC approval. ClickPesa's site states it has a Bank of Tanzania license; the separate regulator listing below names its trading entity but does not confirm current status or product-specific scope. | **No sandbox is documented.** Do not call the API, use API Explorer, or treat a preview endpoint as a sandbox. Obtain written provider confirmation of any safe, non-financial test arrangement before integration testing. | A safe test arrangement or isolated environment; merchant eligibility and KYC requirements; written terms, fees and settlement; refund/reversal/dispute behavior; callback security/replay rules; supported channels and payout conditions; current license scope and country legal review; service/support commitments. |
| Tanzania — Selcom (backup candidate) | The developer portal documents collection, checkout, callback, and transaction-status interfaces. It says API access/credentials require contacting its business team and describes signed request authentication. The reviewed public page does not document a sandbox. | No sandbox is documented on the reviewed page. This absence does not prove that Selcom has no sandbox; the provider must confirm environment separation, test credentials, and non-financial test behavior before any transaction testing. | Current product/version availability and MylesNet merchant eligibility; safe sandbox or test account; access and credential lifecycle; callback verification/replay rules; fees, settlement, refunds, payouts, disputes, support and written terms; independent country/legal approval. |

## Regulator-published provider listings

These are dated observations from regulator publications checked on 12 November 2026. They identify regulator-published entity or service listings only; they do not establish present license validity, the exact permission scope for a named API/product or merchant flow, MylesNet's eligibility, or legal approval. Country counsel and the payments owner must verify current records and the proposed use before any provider decision.

| Country / candidate | Regulator-published observation | Limit and follow-up |
|---|---|---|
| Kenya — Safaricom Daraja | The Central Bank of Kenya directory, dated as at 15 May 2026, lists Safaricom PLC as an authorized PSP. Its approved services include facilitating mobile-money payments and providing a platform to process payments on behalf of merchants. | This supports the legal-entity and broad merchant-processing listing. It does not approve Daraja or a specific MylesNet collection flow, merchant account, settlement arrangement, or contract. Verify current status, applicable products, and merchant eligibility with counsel and Safaricom. |
| Uganda — ioTec Pay | Bank of Uganda's September 2023 Financial Inclusion Newsletter reports that, as at 30 June 2023, IOTEC Limited was listed as a Payment Systems Operator (Small Funds Transfer) and a Payment Service Provider (Class A, Small Electronic Money Issuer). | This is a historical regulator publication, not current-status evidence. The current BoU directory page is linked below, but the public page content retrieved for this review did not expose a complete searchable entity list; current ioTec license status, conditions, and API/product scope remain to be confirmed. |
| Tanzania — ClickPesa | Bank of Tanzania's National Payment Systems Annual Report 2025 lists DLT CP Tanzania Company Limited, trading as Clickpesa, in Annex O, “Non-Bank Payment System Providers.” | The listing independently corroborates the provider's named regulator association, but does not establish the current license instrument or scope for a specific API, product, or MylesNet merchant flow. Counsel must verify the legal entity, status, conditions, and proposed use. |
| Tanzania — Selcom (backup candidate) | The same Bank of Tanzania report lists Selcom Paytech Limited in Annex O, “Non-Bank Payment System Providers.” | The listing does not establish the current license instrument or scope for a specific API, product, or MylesNet merchant flow. Counsel must verify the legal entity, status, conditions, and proposed use. |

## Source register

Provider sources below were opened on 11 November 2026. Regulator sources were opened on 12 November 2026. Dates inside each description are the publication or directory dates, not the access date.

| Provider | Source and supported observation |
|---|---|
| Safaricom | [Daraja Developer Portal](https://developer.safaricom.co.ke/) — describes sandbox app creation and simulation. [Transaction Status API](https://developer.safaricom.co.ke/apis/TransactionStatus) — status lookup and sandbox endpoint. |
| ioTec | [ioTec Pay API reference](https://pay.iotec.io/api-docs/index.html) — collections, disbursements, statuses, OAuth, callbacks, and sample/test transaction values. The reference is dynamically rendered; its public content did not establish sandbox-vs-live account setup. |
| ClickPesa | [ClickPesa Quickstart](https://docs.clickpesa.com/home/introduction) — explicit no-sandbox/live-funds warning and KYC/API-access constraint. [USSD-PUSH overview](https://docs.clickpesa.com/payment-api/mobile-money-payment-api/mobile-money-payment-api-overview) and [payment-status API](https://docs.clickpesa.com/api-reference/collection/querying-for-payments/querying-for-payments) — collection and reconciliation interfaces. [Payment gateway page](https://clickpesa.com/payment-gateway/) — provider's own licensing statement; the BoT listing below identifies the trading entity but still requires current scope/condition verification. |
| Selcom | [Selcom Developers API reference](https://developers.selcommobile.com/) — collection/checkout/status/callback documentation, signed requests, and business-team credential access. Search of the reviewed page found no sandbox entry; that is not proof a sandbox is unavailable. |

### Regulator sources

| Regulator | Source and supported observation |
|---|---|
| Central Bank of Kenya | [Directory of Authorized Payment Service Providers](https://www.centralbank.go.ke/wp-content/uploads/2026/07/Directory-of-Authorized-Payment-Service-Providers-17th-June-2026.pdf), marked as at 15 May 2026 — Safaricom PLC entry and its approved-services description. |
| Bank of Uganda | [Financial Inclusion Newsletter, September 2023, Volume 006](https://bouganda.com/bouwebsite/bouwebsitecontent/PaymentSystems/Newsletters/BoU-Financial-Inclusion-Newsletter-September-2023.pdf), page 4 — IOTEC Limited license categories and classes; the article states the listing snapshot is as at 30 June 2023. [Current BoU supervised PSP/PSO directory page](https://bou.or.ug/financial_infrastructure_innovation) — identified as the current directory page, but the full entity list was not extractable in this review. |
| Bank of Tanzania | [National Payment Systems Annual Report 2025](https://www.bot.go.tz/Publications/Regular/Annual%20Report/en/2026060211494565.pdf), Annex O, “Non-Bank Payment System Providers” — entries for DLT CP Tanzania Company Limited t/a Clickpesa and Selcom Paytech Limited. |

## Decision impact and next evidence

1. Keep all four candidates at their existing `Candidate` state. This research does not rank, select, or approve a provider.
2. Keep each country's collection and payout rails disabled. Public documentation does not close CP-PAYMENT-01.
3. Treat ClickPesa's API testing as a stop condition until the provider supplies an explicitly non-financial test arrangement or the product owner formally accepts a different safe verification strategy. Do not run real-money tests as a substitute for sandbox evidence.
4. Have the appointed payments/finance owner and country counsel request written country-specific eligibility, commercial terms, settlement/reconciliation, refund/payout, callback-security, test-environment, privacy/transfer, and support evidence from each candidate.
5. Have a named security owner review authentication, callback authenticity/replay handling, credential lifecycle, and webhook exposure before any integration plan is approved.
6. Have country counsel verify each candidate's current legal entity, regulator status, permission scope, and proposed merchant/API flow. The regulator publications above are supporting discovery evidence only; Uganda's ioTec listing is a 2023 snapshot.

## Related artifacts

- [Country and operations packs](COUNTRY-AND-OPERATIONS-PACKS.md), CP-PAYMENT-01
- [Decision register](DECISION-REGISTER.md), MN-DEC-013
- [Implementation plan](IMPLEMENTATION-PLAN.md), Phase 0 work items 5–6 and Phase 8 payment gates
- [API/error/event contract backlog](API-ERROR-EVENT-CONTRACT-BACKLOG.md), EVT-001 and RECON-001
- [Implementation status](IMPLEMENTATION-STATUS.md)
