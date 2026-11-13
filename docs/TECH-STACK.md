# MylesNet technical stack

**Status: planning and foundation setup only.** This is the selected implementation direction consolidated from the approved technical decisions. No product workflows, live provider integrations, network services, or production infrastructure are implemented. Provider, legal, latency, cost, and operations candidates remain subject to the validation gates below.

## Application and workspace

| Area | Selected direction | Role / boundary |
|---|---|---|
| Monorepo | pnpm workspaces + Turborepo | One dependency graph and task cache across applications and shared packages. |
| Web runtime | Next.js App Router, React, TypeScript | `apps/web` serves public marketing and subscriber-facing routes; `apps/admin` serves authenticated platform and ISP operator surfaces. `apps/storybook` is design documentation and visual review, not a product portal. Confirm route ownership before product implementation. |
| Realtime backend | Convex | Typed queries, mutations, actions, transactions, scheduled work, and live subscriptions for application data. |
| Identity | WorkOS AuthKit | Staff authentication, invitations, MFA, and organization identity. Organization membership alone is not application authorization. |
| Authorization | Server-enforced application RBAC | Explicit platform roles and tenant roles with tenant/branch scope on each operation and live subscription. |
| Design system | User-approved MylesNet Design System v1.0 | DTCG token source, generated semantic CSS/Tailwind mappings, accessible shared components, tenant theme validation, and consistent shells. `packages/design-system` is the only token source. |
| Styling | Tailwind CSS + shadcn/ui patterns + Lucide | Framework and primitives consume generated design-system CSS variables; no copied palettes or independent token files. |
| TypeScript configuration | Shared workspace package | Common strict compiler settings; application-specific extensions only where required. |
| Component documentation | Storybook app | Theme switcher, component states, accessibility and visual review. |

The current foundation pins Next.js `16.4.6`, React `19.3.1`, Convex `1.49.0`, WorkOS AuthKit Next.js `4.6.3`, TypeScript `7.0.2`, Turborepo `2.11.10`, and pnpm `11.25.0`. These are setup-time versions as of 11 November 2026. Recheck support, peer compatibility, security notices, and provider requirements before implementation kickoff.

## Application data and tenant boundaries

- Use one shared Convex deployment for v1 with strict logical tenant/branch isolation; dedicated deployments are a possible future isolation option after workload, security, legal, and cost review, not an included offer.
- Map each WorkOS organization to one internal `tenantId`. Resolve organization, active tenant, branch scope, account state, roles, and permissions on the server for every operation. Never authorize from a client-supplied tenant or branch ID.
- Keep public pages and captive portal assets lightweight and independently cacheable. Resolve tenant domains safely; unknown or suspended tenants fail closed.
- Keep payment ledgers, audit events, membership history, and security records append-only; use explicit reversals/corrections.
- Use country/category retention defaults and guardrails. Legal holds override deletion; tenant-selected shorter periods are allowed where lawful.
- Leading managed-region candidates are Convex Europe (Ireland) and Vercel Dublin (`dub1`). The [hosting cost and capacity evidence plan](HOSTING-COST-AND-CAPACITY-EVIDENCE.md) records current public prices/limits and the open workload/quote inputs. Validate measured regional latency, plan limits, data transfers, contracts, recovery, capacity, and country law before production; no hosting plan or production region is approved by the evidence packet.

## Network and subscriber access

| Area | Selected direction | Boundary |
|---|---|---|
| RADIUS | FreeRADIUS 3.2.x with dedicated PostgreSQL AAA store | Authentication/accounting stays off Convex's request path. Run redundant MylesNet-managed regional endpoints and durably deliver session projections to Convex. |
| Router support | MikroTik RouterOS first; standard RADIUS-compatible NAS where feasible | RouterOS 7 required for the onboarding tunnel. Do not offer remote router administration through the service tunnel. |
| Router provisioning | Tenant-scoped generated setup through WireGuard | Restrict to RADIUS authentication/accounting and CoA/Disconnect; block WinBox, SSH, and RouterOS API. Use expiring/revocable enrollment and audit events. |
| Subscriber access | PPPoE and hotspot/captive portal with vouchers | Tenant-branded hosted portals, auto-created subscribers and entitlements, confirmed-payment activation, generated credentials, and English/Kiswahili. |
| Policy and accounting sync | Isolated worker/outbox design | Retry safely, preserve accounting delivery, avoid duplicate financial or session effects, and keep network secrets out of Convex client-visible data. |

If RADIUS connectivity fails, keep current sessions active and reject new logins until service returns. Never fail open. Target monthly RADIUS and captive portal availability is 99.95%; these are planning SLOs, not a production promise.

## Payments and financial processing

- Implement provider adapters and separate subscriber collection accounts from MylesNet's own platform subscription merchant accounts.
- First country integration candidates: Daraja for Kenya, iOtec for Uganda, and ClickPesa for Tanzania with Selcom backup. Validate contracts, sandbox/live readiness, refunds, payout throughput, fees, and settlement before selection.
- Support both tenant-owned merchant accounts and pooled MylesNet collections. Ask during tenant onboarding. Pooled collection and payout remain disabled country by country until legal and provider approvals are documented.
- Process signed callbacks idempotently; each provider-confirmed payment must create one financial effect and at most one activation. Surface unmatched confirmations immediately while safe retries continue.
- Maintain an append-only ledger. Record manual cash, bank, and mobile-money receipts with a source reference and audit event. Full/partial refunds follow tenant approval rules; unsupported rails become visible exceptions.
- Support withdrawal fee rules per country and payout rail (fixed, percentage, or both), verified legal-entity destinations, owner email OTP for each request, and automatic principal restoration on provider-confirmed failure.
- Multi-currency reports can show separate totals or consolidated totals converted with the latest official central-bank reference rates. Always display rate and as-of time.

## Identity, messaging, and external providers

- Tenant staff use the approved built-in/custom RBAC model with branch scope; there is exactly one Owner per tenant. Platform administrators use separate platform roles and mandatory MFA.
- Subscriber login is email/phone plus password; sensitive actions require MFA step-up. Recovery, sole-owner transfer, high-risk access, and emergency support follow audited workflows in the technical specification.
- Use WorkOS's default identity sender. Resend is the conditional application-email candidate after country-specific transfer review and delivery pilot; use SES Ireland if approval fails. Public provider facts and open validation are tracked in [`EMAIL-PROVIDER-PUBLIC-EVIDENCE.md`](EMAIL-PROVIDER-PUBLIC-EVIDENCE.md); these are not provider approvals.
- Compare Africa's Talking and Infobip for SMS delivery/pricing/security; retain Twilio as a price benchmark. Use a registered tenant sender ID where approved, otherwise a configured fallback.
- Separate promotional opt-in and STOP suppression from required service messages. Never put credentials or message bodies into logs or analytics.
- Provider credentials belong in environment-specific secret storage. Local development uses development credentials and synthetic data only.

## Platform subscription and hosting

- Standard self-service modes are Hotspot and PPPoE, with the same standard feature set and usage metering that may be combined on one monthly invoice. Hotspot is 3% of confirmed audited sales before provider fees, less refunds/reversals; PPPoE is USD $0.25 reference per distinct non-test account with a valid PPPoE entitlement during the billing month, counted once without proration. There are no hard subscriber/router caps or per-staff/per-voucher charges.
- Apply one $5-equivalent monthly minimum to combined usage, including zero-usage paid months. Calculate Hotspot fees in sale currency, convert using the official central-bank rate at invoice issuance, and preserve the applied rate and timestamp. Exact KES/UGX/TZS unit rates and minimums need validation. Prices exclude tax; itemize applicable taxes separately by billing entity. MylesNet absorbs its own subscription invoice processing costs.
- Use monthly billing for standard usage plans; there is no standard annual discount. Enterprise is custom-quoted for 10,000+ active subscribers or documented multi-region/regulatory needs and may negotiate annual commitments and tailored services. Give at least 30 days' notice of rate changes and apply them prospectively.
- The public calculator is anonymous; visitors enter monthly Hotspot revenue and billable PPPoE account estimates, and it applies the confirmed-sales/counting rules and local minimum before tax. Choose KES/UGX/TZS using coarse IP country detection in launch markets; fall back to USD elsewhere or on lookup failure, with a visible manual selector and no GPS/precise-location storage.
- Start a 14-day trial after email verification without requiring a payment method. Require billing setup and confirmation before first charge; incomplete setup receives a seven-day completion grace. Failed invoice payment has a separate seven-day grace, followed by console/network suspension. See `READINESS-DECISIONS.md` for monthly renewal, auto-pay, cancellation, and retry behavior.
- Host Next.js on Vercel with Dublin functions as the leading candidate and static assets on the global CDN. Use separate MylesNet payment merchant accounts for subscription invoices.
- Keep the network plane independently deployable: RADIUS, RouterOS workers, and long-running provider work must not share browser sessions or broad credentials.
- Use structured logs, correlation IDs, metrics/alerts, provider reconciliation, backups, and restore drills. Sentry/OpenTelemetry are candidate observability tools pending privacy/cost review.

## Local development and repository shape

```text
apps/web/                         public marketing and subscriber routes
apps/admin/                       platform and tenant operator routes
apps/storybook/                   design-system documentation/visual review
packages/design-system/           tokens, theme tooling, generated CSS, and UI package
packages/typescript-config/       shared strict TypeScript settings
convex/                           application schema and server functions
docs/                             canonical planning and handoff documents
```

The exact internal design-system package layout and import direction are specified in `DESIGN-SYSTEM-HANDOFF.md` and `packages/design-system/README.md`. Keep captured references and source archives in ignored local-only paths; they contain live sample records and must not enter public commits.

## Delivery and verification

At implementation kickoff, verify tenant isolation, WorkOS organization context, RBAC/branch scopes, realtime update behavior, append-only accounting, payment idempotency, RADIUS authentication/accounting, router onboarding, portal activation, responsive/accessibility behavior, and recovery/retention workflows. Use synthetic data and a controlled router lab before any live provider or network rollout. Current stage checks validate only the monorepo foundation; they do not verify product behavior or production readiness.
