# ADR-0006: Payment ledger, merchant models, and country gates

- **Date:** 11 November 2026
- **State:** Ledger separation and approval gates accepted; provider and per-country operation remain open
- **Owner:** Finance/payments and country legal owners not named; assign in MN-PLAN-008
- **Evidence:** `AGENTS.md`; `READINESS-DECISIONS.md`; `CORE-FLOWS-AND-DATA.md`; `TECHNICAL-SPECIFICATION.md` §§8–9, 14.4–14.5; `DECISION-REGISTER.md` MN-DEC-008/013/015/018

## Context

MylesNet subscription billing and ISP subscriber collections are different businesses and ledgers. Payment providers supply external evidence; application effects must be replay-safe, reconcilable, and auditable. Country/provider choices and pooled collection approvals are not complete.

## Decision

- Keep the MylesNet platform subscription ledger separate from every ISP tenant subscriber-collection ledger, with separate merchant accounts, permissions, reports, and reconciliation ownership.
- Support tenant-owned merchant accounts and a MylesNet-managed pooled collection model as product models. A country must keep pooled collection/payout disabled until its legal, provider, settlement, and operational approvals are recorded. Each tenant selects only an enabled model.
- Use provider adapters, signed callback verification, replay protection, immutable provider/payment references, idempotency, an exception queue, and reconciliation. A confirmed payment may cause at most one ledger effect and one subscriber activation. Corrections use linked reversals/adjustments, not mutation of settled history.
- Provider-confirmed payments are authoritative evidence; a browser redirect is not settlement. Reject or quarantine mismatched tenant/provider mapping, amount, currency, signature, or duplicate/out-of-order events for safe reconciliation.
- MylesNet platform subscription invoices use MylesNet merchant accounts, not ISP subscriber-collection accounts. Wallet auto-pay is Owner-controlled, MFA-protected, limited to the selected billing legal entity's exact currency, and cannot convert or draw from another entity.
- Keep Daraja (Kenya), iOtec (Uganda), and ClickPesa with Selcom as backup (Tanzania) as candidates only. No provider or country is enabled by this record.

## Alternatives considered

- **One shared ledger/account for platform subscriptions and tenant collections:** rejected; funds and obligations must remain distinguishable.
- **Direct-to-merchant only:** retained as an allowed tenant model, not mandated globally.
- **Enable pooled settlement in all countries by default:** rejected; each country has an independent legal/provider gate.
- **Use browser redirect as proof of payment:** rejected; only a verified callback or confirmed reconciliation event can settle.

## Security and operational tradeoffs

Adapter reuse lowers integration duplication but must not collapse merchant identity, currency, ownership, or ledger boundaries. Webhook ingestion needs rate limits, signature/key rotation, replay controls, durable acknowledgement, idempotent processing, visible exceptions, least-privilege secrets, and reconciliation. Pooled collection increases custody, settlement, refund, and legal complexity.

## Interfaces and sequences

See `IC-PAY-01`, `IC-LEDGER-01`, `SEQ-PAY-01`, and `SEQ-PLAN-01` in [`INTERFACE-AND-SEQUENCE-CONTRACTS.md`](INTERFACE-AND-SEQUENCE-CONTRACTS.md).

## Open items and revisit trigger

Obtain provider contracts/sandboxes, supported payment/refund/reversal capabilities, settlement and payout timing/fees, country legal approvals, exact reconciliation ownership, tax/accounting advice, and exception staffing. Plan names, prices, quota values, tax and overage remain open. Revisit a country only when its evidence pack is approved.
