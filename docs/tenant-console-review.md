# Tenant console reference review

Reviewed in the signed-in browser on 10 November 2026. This is a read-only UX/design reference. Full-page captures remain in the local-only `docs/reference-images/` gallery; use its `index.html` and `manifest.csv` to map captured routes to images.

## App shell and visual system

- Persistent left rail with Overview and grouped Customers, Network, Finance, Outreach, and Insights destinations; nested destinations expand under finance and communications. Settings has a separate settings navigation rail.
- Top bar combines breadcrumbs/page title, global subscriber/router/payment search, assistant action, Support, and the signed-in profile menu.
- Centered, bounded work areas use cards on a pale neutral page, subtle borders, rounded corners, compact metadata, and a strong primary-action accent. Tables and forms use compact labels and card sections.
- Theme selector exposes Light, Dark, and System. The dashboard was captured in each; System followed the operating-system preference. Light was restored after review.
- Tenant branding settings include display name/logo, primary color, font choice, support contacts, and optional terms. These values are described as applying to captive portal, vouchers, invoices, and outbound messages. Domain settings provide tenant URL controls; hotspot settings show templates and previews. No branding, domain, or template settings were changed.

## Route coverage

The local gallery records 73 route/view entries, including theme, staff-role, router-detail, and gated states. Captured routes include:

- Overview and customers: `/dashboard`, `/users`, `/users/create`, `/users/compensate`, `/leads`, `/leads/import-export`, `/tickets`, `/tickets/create`, `/tickets/import-export`, `/tickets/analytics`.
- Network: `/active-users`, `/packages`, `/packages/create`, `/nas`, `/nas/create`, `/network-map`, `/cpe`, `/cpe/firmware`, `/equipment`, `/equipment/create`, `/fiber/map`.
- Finance: `/payments`, `/payments/create`, `/payments/import-export`, `/invoices`, `/invoices/create`, `/outage-credits`, `/loyalty-points`, `/expenses`, `/expenses/create`.
- Vouchers and outreach: `/vouchers`, `/vouchers/sales`, `/vouchers/batches`, `/agents`, `/vouchers/templates`, `/whatsapp`, `/whatsapp/create`, `/sms`, `/sms/create`, `/sms/campaigns`, `/sms/campaigns/create`, `/campaigns`, `/campaigns/create`.
- Insights and account: `/analytics`, `/changelog`, `/staff`, `/subscription`, `/referrals`, `/audits`, `/feedback-board`.
- Settings: branding, domain, PPPoE, hotspot, payments, SMS/email/WhatsApp, message templates, loyalty, notifications, assistant, accounting, developer, profile, and security settings.

## Findings and limits

- `/accounts` returned the application's 404 screen. The security page showed a password confirmation/passkey gate; it was not bypassed.
- Payment settings showed a connected collection provider. No gateway configuration was opened.
- Some screens contain live data that changes between visits. Counts and finance amounts are point-in-time references, not stable requirements or fixtures.
- Export/download actions, external links, and state-changing controls were left untouched. Router setup was captured at its initial step only; no router credentials were entered.
- Captures include customer communications and sensitive-looking values. Treat the local reference gallery as confidential. This report does not repeat those values.
- Some regions were loading skeletons during capture. Route coverage records rendered evidence, not every internal behavior, underlying source, or design token.
