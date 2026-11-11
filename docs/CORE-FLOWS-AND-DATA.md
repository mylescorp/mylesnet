# Core workflows and conceptual data model

**Status: planning only.** These are system behaviors to validate with product and network owners. They are not implemented flows or a final database schema.

## End-to-end workflows to specify

### ISP signup and tenant provisioning

1. Prospect creates a platform identity and verifies email before automatic tenant provisioning; phone may be added and verified later.
   - An identity that already owns an ISP tenant cannot provision a second tenant.
2. Signup captures legal/business name, country, timezone, billing currency, tenant subdomain, and consent. The subdomain is stable after signup; a verified Owner may later request a rename, retaining the old URL as a reserved redirect alias. The display name is independent.
3. Prospect selects the standard **Hotspot** and/or **PPPoE** usage mode or requests a custom **Enterprise** quote. Hotspot and PPPoE share the standard feature set; a tenant may use both. Start a 14-day trial after email verification without collecting a payment method. Before the first charge, require billing setup and explicit tenant confirmation. Standard usage is billed monthly with no annual-prepayment discount; Enterprise may negotiate annual commitments. The first successful paid invoice anchors monthly dates. If a cycle lacks the anchor day, use that cycle's last calendar day and return to the original anchor in the next cycle. Generate a monthly invoice. Mobile money, bank transfer, and supported card require the tenant to select **Pay**; only separately enabled wallet auto-pay charges automatically. Service continues through the paid period after cancellation. If billing is not complete at trial end, keep console and network service running during a seven-day completion grace, then suspend under the unpaid-tenant rules. Store platform billing separately from the ISP's subscriber ledger.
4. Provisioning creates the tenant record, mapped WorkOS organization, owner membership, default roles, default settings, and reserved tenant subdomain. Retain renamed subdomains as tenant-owned redirect aliases and never reuse them.
5. Owner completes onboarding: brand, service plans, payment-provider setup, NAS/router registration, and initial data import. Tenant MFA enrollment is optional; high-risk actions require MFA step-up.
6. Tenant becomes active only after provisioning and required owner actions succeed. Retry transient provisioning failures idempotently within a bounded policy; preserve progress, display status, and provide a support path after the threshold is reached. Exact retry/backoff bounds belong in the implementation runbook.

Block a repeated verified Owner identity from owning another tenant and block duplicate subdomains; allow duplicate business names when those identifiers are distinct. Do not collect government-issued business registration IDs during v1 signup. Define failed verification/provisioning recovery before implementation. Tenant signup is self-service after email verification, with the selected 14-day trial and lifecycle rules documented in `READINESS-DECISIONS.md`.

### Guided tenant CSV import (v1)

1. An authorized tenant staff member selects an entity and tenant/branch scope, downloads the relevant CSV template/instructions, or uploads a prepared CSV. The supported entity list and exact field mappings are defined before implementation; named system-specific connectors are deferred beyond v1.
2. The import service validates file type/size, headers, required fields, formats and tenant/branch references. It checks duplicates both within the uploaded file and against records already in the selected tenant scope. It never silently overwrites or merges existing records.
3. Show a non-mutating dry-run preview with rows ready to import, rows with errors/warnings, duplicate candidates, and actionable row-level reasons. The operator must skip or resolve invalid/duplicate rows according to the entity-specific policy before confirmation. A dry run writes no tenant records.
4. Require explicit confirmation to commit. Run the job idempotently within the verified tenant and branch scope; show progress, per-row outcomes and retryable failures. Record who uploaded, validated, committed or rolled back the job, the selected entity/scope, mapping/template version, source-file digest, timestamps and result counts in the audit trail. Do not copy row contents, credentials or other secrets into logs.
5. Offer safe rollback for records created by that import batch. Rollback must not erase audit or immutable financial history, overwrite later edits, or delete records already depended on by other work. If a row cannot be reversed safely, preserve it and present an audited exception/reconciliation path instead of forcing deletion.
6. Apply tenant/branch authorization, bounded file and row counts, secure temporary-file handling, retention rules, and queue/backpressure policy to every import job.

**Open before build:** supported entity coverage, template schemas, duplicate matching/resolution rules, file/batch limits and retention, partial-commit semantics, retry behavior, and safe rollback cutoffs. Existing subscriber/lead import requirements use this shared guided CSV contract. No named system connector ships in v1.

### Tenant domain configuration and migration

1. Tenant settings show the active domain and provide three choices: MylesNet subdomain, a domain the tenant owns, and **Buy a domain**.
2. For a tenant-owned domain, issue one domain-specific DNS **A** record with its name, value, and TTL; show `Awaiting DNS` until detected. The Tenant Owner selects **Verify DNS** to recheck the record. Provision HTTPS automatically after verification.
3. When changing domains, list previous addresses separately and keep them serving so payment callbacks, subscriber self-service and other registered integrations continue to work. Warn that some providers may require their own dashboard or support team to change callback URLs.
4. Stop serving a previous address only after the Tenant Owner confirms integrations have moved and explicitly selects **Stop serving**. Keep the hostname reserved for the same tenant and audit domain changes.
5. **Buy a domain** opens a supported external registrar. After purchase, the tenant returns to MylesNet DNS setup. Select the registrar link and any country availability rules before implementation.

Reference: [saved domain settings](reference-images/reference-domain-settings.png). Use its single-A-record, manual verification and previous-address migration behavior; do not copy its sample hostname or IP address.

### Public pricing, usage metering, and Enterprise quote

- The anonymous pricing calculator accepts visitor-entered monthly Hotspot revenue and billable PPPoE account estimates, applies the confirmed-sales/account-count rules and the $5-equivalent minimum once to combined usage, and shows a pre-tax estimate. It does not require sign-in.
- Select a display currency through coarse IP-based country lookup: KES in Kenya, UGX in Uganda, and TZS in Tanzania. If lookup fails or the visitor is outside those markets, use USD. Keep the manual selector visible; do not request browser GPS or store precise location.
- Bill Hotspot at 3% of audited, confirmed sales before provider fees, less refunds and reversals; exclude pending/failed transactions. Include confirmed cash, bank, manual mobile-money, and provider-confirmed sales under both merchant models.
- Bill PPPoE at the published fixed local-currency equivalent of USD $0.25 per distinct non-test account with a valid PPPoE entitlement at any point during the billing month. Count each account once at the full monthly rate; do not prorate. Exact fixed KES/UGX/TZS rates remain pending validation.
- If Hotspot fees are earned in a currency different from the selected billing legal entity, calculate in each sale currency, then convert to invoice currency using the official central-bank reference rate at invoice issuance. Show and lock the applied rate and as-of timestamp. List tax separately by billing entity; usage rates are before tax.
- Apply one fixed local-currency equivalent of the $5 monthly minimum to combined usage in every paid month, including zero usage. Exact KES/UGX/TZS minimums remain pending validation. No hard subscriber/router caps or per-staff/per-voucher charges apply; retain operational abuse safeguards.
- Give the Owner at least 30 days' notice of a rate change and make it effective only from a future billing cycle. Do not apply changes retroactively.
- Enterprise is custom-priced for 10,000+ active subscribers or a documented multi-region/regulatory requirement. Its quote form asks for ISP/company name, work email, countries served, estimated subscriber count, Hotspot/PPPoE usage, and a short needs summary. Do not request credentials or registration IDs. Enterprise may negotiate dedicated onboarding/account management, migration help, custom integrations/API/branding, and contract/support terms; dedicated infrastructure or stronger guarantees need separate validation.

### Tenant ownership transfer

1. The sole current Owner starts the transfer and completes MFA step-up.
2. The incoming Owner verifies identity, accepts the transfer, and completes MFA step-up (enrolling if needed) before receiving Owner access.
3. During acceptance, the incoming Owner chooses the former Owner's post-transfer role: default tenant-wide Admin, a narrower role, or no access.
4. Commit the ownership and former-Owner role change atomically so the tenant always has exactly one active Owner; apply the new permissions immediately and notify all verified tenant contacts.
5. If the current Owner cannot initiate the transfer, follow the established tenant-owner recovery process; support cannot bypass it.

### Subscriber and service setup

1. Staff, import, or the tenant-branded subscriber portal creates a subscriber under the resolved tenant.
2. Validate tenant-local identifiers and phone normalization; capture only necessary personal data and communication consent.
3. Assign a service type (Hotspot, PPPoE, or another approved type), plan, account credentials/profile, validity, quota, price, and optional router/NAS.
4. Show the subscriber a clear pending/active/suspended/expired state and send credentials only through approved channels.
5. All edits, credential resets, expiry changes, and manual adjustments create tenant audit records.

### RADIUS authentication and accounting

1. A NAS sends Access-Request/Accounting messages to the RADIUS service over an approved network path.
2. Validate NAS identity and shared secret, then map the NAS to exactly one tenant using server-side registry data.
3. Read credentials and access policy only for that tenant; apply plan state, expiry, quota, device/session limits, and policy attributes.
4. Accept or reject with a reason code that does not disclose sensitive account state to an unauthenticated caller.
5. Record Start, Interim-Update, and Stop accounting idempotently; derive active sessions from bounded, recoverable accounting state.
6. If policy requires disconnect/CoA, enqueue a tenant-authorized command and record request, response, timeout, and retry state.

FreeRADIUS 3.2.x uses a dedicated PostgreSQL AAA store; Convex is not on the RADIUS authentication or accounting request path. Define how approved tenant/branch policy state is synchronized to AAA, along with cache invalidation and accounting-event delivery through a Convex outage. On RADIUS loss, keep existing sessions and reject new logins; never fail open. No RADIUS process should receive WorkOS or platform-admin credentials. See [`DECISION-REGISTER.md`](DECISION-REGISTER.md).

### Captive-portal purchase and service activation

1. Router presents tenant-branded portal with available plans and terms.
2. Subscriber selects plan and supplies phone/account identity or voucher.
3. Portal creates a time-limited checkout intent and uses the tenant’s configured payment adapter.
4. Browser redirect alone never activates service. A verified, signed provider callback or confirmed reconciliation event is authoritative.
5. Idempotent payment processing records the transaction, allocates it to the intended subscriber/plan, grants access, and returns a safe success page.
6. Pending, cancelled, duplicate, delayed, reversed, and failed payments have distinct states and recovery instructions.

### Tenant finance and SaaS finance

- **Tenant ledger:** money collected by an ISP from subscribers, invoices/receipts, allocations, gateway fees, refunds, manual credits, expenses, reconciliation, and accounting exports.
- **Platform ledger:** money collected by the SaaS from ISP tenants for platform plans, subscription invoices, taxes/fees, credits, collections, and refunds.
- Never combine these ledgers or let a tenant operator view another tenant’s financial records. Represent corrections as linked reversal/adjustment entries rather than mutating settled history.

### MylesNet subscription invoice payment

1. Show the tenant's active-until date, amount due, due date, invoice status, invoice ID, manual **Pay** action and downloadable PDF. Render monetary amounts in the selected billing legal entity's local currency.
2. Support local mobile money and bank transfer, plus card payment where a provider supports it. Use MylesNet's own provider merchant accounts, separate from ISP subscriber collection accounts; provider adapters may be reused. MylesNet absorbs its own subscription-invoice processing costs; do not add a separate processing-fee line. Send unpaid-invoice reminders 7, 3 and 1 day before due; stop reminders after payment. Store platform subscription activity in the platform ledger, separate from subscriber collections.
3. Tenants using MylesNet-managed subscriber collections may opt in to wallet auto-pay; it is off by default. Only the Tenant Owner may enable or disable it, with MFA step-up. In countries where pooled collection is legally and operationally approved, attempt payment from the wallet belonging to the selected billing legal entity one day before the invoice due date. Require exact invoice-currency match; do not convert or draw from another entity. If the wallet is short, retry hourly until the invoice is covered. Retries continue after suspension while auto-pay remains enabled; when the overdue invoice is paid, automatically restore tenant console and network service. If the Owner disables auto-pay mid-retry, cancel future attempts immediately; preserve completed payments and leave the invoice available for manual payment. Do not expose this option to tenants using direct-to-merchant collection.
4. Preserve the invoice and payment history; show paid/pending state and provide the corresponding receipt/PDF. Never copy sample currency amounts or invoice identifiers from the reference platform reference.

Reference: [saved subscription and wallet auto-pay](reference-images/reference-subscription-wallet-auto-pay.png). Send invoice reminders 7, 3, and 1 day before due; stop after payment is recorded.

### Vouchers and agent sales

Define batch generation, code entropy, uniqueness, print/download permission, inventory reservation, agent assignment, sale/commission snapshot, redemption, quota/expiry, voiding, and reconciliation. Preserve the price and commission terms at issuance/sale so later package edits do not rewrite historical reports.

### Router lifecycle

Register router identity, model/RouterOS version, tenant/NAS mapping, address/connectivity method, capabilities, credential reference, and health state. Provisioning is queued, least-privilege, rate-limited, observable, and reversible where possible. Router secrets never appear in logs or browser responses. Decide whether devices connect outbound to a management service or require inbound access; do not assume public router IPs.

### Super-admin support access

Support access begins with a tenant, reason, ticket/reference, requested resource scope, and expiry. Apply the required approval; issue a time-bound grant; show a visible support-session banner; capture reads and writes; block or mask payment/router secrets; revoke on expiry; and make access reviewable by tenant and platform auditors.

## Conceptual entities

### Platform-control data in Convex

`PlatformUser`, `PlatformRole`, `Tenant`, `TenantDomain`, `TenantPlan`, `TenantSubscription`, `TenantEntitlement`, `TenantUsagePeriod`, `ProvisioningJob`, `SupportGrant`, `ProviderCredentialReference`, `PlatformAuditEvent`, and `PlatformInvoice/Payment`.

### Tenant operational data in Convex

`TenantSettings`, `TenantMember`, `TenantRole`, `Subscriber`, `ServiceAccount`, `ServicePlan`, `NAS/Router`, `AccessProfile`, `RadiusCredential`, `RadiusAccountingEvent`, `ActiveSessionProjection`, `PaymentAttempt`, `TenantPayment`, `LedgerEntry`, `Invoice`, `CreditNote`, `Expense`, `VoucherBatch`, `Voucher`, `SalesAgent`, `Campaign`, `MessageDelivery`, `SupportTicket`, `Equipment`, `AuditEvent`, and `ImportJob`.

These are proposed logical domains within the Convex application backend; they do not imply a separate physical database per tenant. Names are conceptual and may map differently after the data dictionary is reviewed. Every tenant-owned record must have an explicit ownership rule, retention class, sensitivity classification, and deletion/export behavior.

## Architecture decisions tied to the model

- V1 uses one shared Convex deployment with strict server-verified tenant and branch isolation; separate deployments are a possible later-tier option, not an open v1 choice. Define tenant-scoped indexes, noisy-neighbor controls, backup/restore, export/deletion, migrations and platform-wide analytics before implementation. See [`DECISION-REGISTER.md`](DECISION-REGISTER.md).
- Real-time projections in Convex must have a named source of truth. Do not write the same payment, subscription, or RADIUS record independently in Convex and a second database without an idempotent sync/outbox design.
- Network access/authentication is separate from ordinary dashboard queries and uses FreeRADIUS 3.2.x with PostgreSQL AAA. Preserve current sessions and reject new logins on RADIUS loss; never fail open. Define the remaining source-sync, cache invalidation, HA/recovery, and accounting-retention contracts separately.
- Tenant billing needs integer minor units or currency-aware decimal rules, explicit currency per transaction, idempotency keys, immutable settlement records, and deterministic reconciliation.
- Personally identifiable information, payment tokens, router credentials, and captive portal logs need separate access and retention rules.
