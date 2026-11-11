# Route and access inventory

**Status: planning only.** Tenant routes below were recorded during the read-only reference platform dashboard review on 2026-11-10. They are reference routes, not a requirement to clone every internal behavior. Platform-admin routes are proposed because no super-admin panel was inspected. Confirm route names and workflows before implementation.

## Observed tenant-console routes

| Area | Reference routes |
|---|---|
| Overview and customers | `/dashboard`; `/users`; `/users/create`; `/users/compensate`; `/leads`; `/leads/import-export`; `/tickets`; `/tickets/create`; `/tickets/import-export`; `/tickets/analytics` |
| Network | `/active-users`; `/packages`; `/packages/create`; `/nas`; `/nas/create`; `/network-map`; `/cpe`; `/cpe/firmware`; `/equipment`; `/equipment/create`; `/fiber/map` |
| Finance | `/payments`; `/payments/create`; `/payments/import-export`; `/invoices`; `/invoices/create`; `/outage-credits`; `/loyalty-points`; `/expenses`; `/expenses/create` |
| Vouchers and outreach | `/vouchers`; `/vouchers/sales`; `/vouchers/batches`; `/agents`; `/vouchers/templates`; `/whatsapp`; `/whatsapp/create`; `/sms`; `/sms/create`; `/sms/campaigns`; `/sms/campaigns/create`; `/campaigns`; `/campaigns/create` |
| Insights and account | `/analytics`; `/changelog`; `/staff`; `/subscription`; `/referrals`; `/audits`; `/feedback-board` |
| Settings | `/settings/branding`; `/settings/domain`; `/settings/network/pppoe`; `/settings/network/hotspot`; `/settings/payments`; `/settings/communications/sms`; `/settings/communications/email`; `/settings/communications/whatsapp`; `/settings/message-templates`; `/settings/loyalty`; `/settings/notifications`; `/settings/ai`; `/settings/integrations/accounting`; `/settings/developer`; `/settings/profile`; `/settings/security` |

Reference limitations: `/accounts` showed a 404; the security route was gated by reauthentication; export paths can download data; and some pages showed loading states. Treat those as review notes, not proposed behavior.

Import note: v1 supports guided CSV imports with validation, duplicate checks, a dry-run preview, explicit commit, audit trail and safe batch rollback. The observed `import-export` paths are reference evidence only; catalog the v1 import entities, entry points and exact route actions before implementation. Named system-specific connectors are deferred beyond v1.

## Proposed platform-admin route groups

These are candidate navigation groupings for the approved platform surface. V1 includes platform administration and SaaS billing; exact route names, navigation, grouping, and action-level acceptance remain to be cataloged before implementation. The phase sequence is in [`IMPLEMENTATION-PLAN.md`](IMPLEMENTATION-PLAN.md).

| Group | Candidate pages/actions |
|---|---|
| Overview | Service health, tenant counts, onboarding queue, incidents, failed jobs, usage summaries |
| Tenants | Directory, tenant details, create/provision, domain status, lifecycle, limits, suspend/reactivate |
| SaaS plans and billing | Plan catalog, entitlements, tenant subscription state, invoices/collections, grace periods |
| Platform operators | Users, roles, MFA status, access reviews, session revocation |
| Support access | Requests, approvals, scoped impersonation/support sessions, expiry and audit history |
| Integrations | Provider configuration status, webhook health, RADIUS and router service health; secrets stay hidden |
| Operations | Queue/scheduler status, provisioning jobs, backups, incidents, maintenance state |
| Audit and policy | Platform audit events, retention configuration, export/incident records |

## Proposed subscriber and portal pages

- Subscriber web portal: sign in/OTP, service summary, renew or choose a plan, payment result, invoices/receipts, profile/contact preferences, and support.
- Captive portal: landing/plan selection, phone or account identification, voucher entry, payment handoff, success/failure/pending states, terms, and retry/recovery.
- Exact navigation and whether the subscriber portal is shared or tenant-branded remain open decisions.

## Role groups

The v1 built-in tenant staff and platform role names are approved; detailed permission matrices remain proposed pending action-level review.

| Role | Intended scope |
|---|---|
| Platform owner | Full platform control-plane administration and role delegation; tenant records only through explicit audited support access. |
| Platform operations | Tenant provisioning, domains, service health, and operational actions within assigned permissions. |
| Platform finance | SaaS subscription invoices, collections, credits/refunds, and platform finance reports. |
| Platform support | Tenant status and support metadata; subscriber data only with a Tenant Owner-approved support grant. |
| Platform Security Admin | Security/access reviews, independent approval of high-risk tenant-owner recovery, and documented emergency support-access exceptions. |
| Platform auditor | Read-only platform audit and policy records; no secret access or mutations. |
| Tenant owner | Exactly one active Owner per tenant; owns tenant configuration and staff delegation; no access to other tenants or platform control plane. |
| Tenant administrator | Tenant-wide operational configuration within the tenant; no platform controls. |
| Tenant finance | Payments, invoices, expenses, reconciliation, and finance reports. |
| Tenant network operator | Routers/NAS, plans/access profiles, sessions, network settings, and network reports. |
| Tenant support agent | Subscriber records, tickets, communications, and narrowly scoped service actions. |
| Tenant sales/agent | Leads and assigned voucher/sales work only, subject to tenant policy. |
| Tenant analyst/read-only | Read-only access to approved tenant lists and reports. |
| Subscriber | Own customer portal and service records only. |

Each tenant has exactly one active Owner. Owner is always tenant-wide; Admin, functional, and custom roles can be assigned tenant-wide or to selected branches. A staff member has at most one role assignment at a given scope; use a custom role to combine duties within that scope. A tenant-wide role applies to all branches; additional branch roles add permissions only in their selected branches. Subscriber access remains self-scoped and is managed separately from tenant staff roles.

## Authorization rules to decide and enforce

- Use permission actions, not role-name checks scattered through the UI. Roles are bundles of versioned permissions.
- Resolve tenant from verified host/domain and authenticated membership. Never trust a browser-submitted tenant ID as the access boundary.
- Apply tenant scope to database queries, exports, file URLs, cache keys, broadcasts, queue jobs, search, and webhook processing.
- Gate CSV upload, mapping, dry-run, commit, job/row-result access and rollback with explicit import permissions and verified tenant/branch scope. Ignore tenant or branch identifiers supplied in CSV content as authorization; resolve scope from the authenticated operation. Never allow a dry run to mutate tenant records.
- Staff membership removal/deactivation revokes access immediately: invalidate the membership and active sessions, terminate live subscriptions, clear tenant-scoped client data, and preserve the audit trail.
- Role, permission, and branch-scope changes are enforced immediately across requests and live subscriptions; clear data no longer permitted by the new scope while leaving the user signed in.
- Tenant Owners and Admins may invite staff and set initial role/branch scope. Admins cannot exceed their own permission ceiling; Owner approval is required for high-risk grants. Ordinary invitations cannot grant the Owner role; ownership changes use the separate transfer flow.
- The Owner or a tenant-wide Admin may deactivate non-Owner staff. A branch-scoped Admin may revoke access only in assigned branches. The Owner is protected from ordinary staff deactivation and can be replaced only through the ownership-transfer or recovery process.
- Separate view/create/update/delete/export/financial-adjustment/router-command/support-access privileges.
- Tenant Owner grants or approves high-risk permissions. Tenant Admin may compose and assign lower-risk custom roles only within their own permission ceiling; no user may grant or assign permissions that elevate themselves.
- Tenant Owner may customize built-in role permission bundles for their tenant, except for the fixed Owner role; changes do not alter platform templates or other tenants.
- Custom roles can be created from scratch or cloned from an existing role as an independent permission set, with no live inheritance; enforce the creator's permission ceiling and Owner approval for high-risk grants.
- Built-in roles cannot be deleted. The Owner or an Admin with role-management permission may delete a custom role only when no staff are assigned; Admins stay within their permission ceiling. Retain its history in the audit trail.
- Owner approval is required to grant permissions for withdrawals/payout destinations, refunds/credits, sensitive exports, credential/secret reveal or rotation, role delegation, bulk messaging, and destructive/bulk network actions.
- Require MFA at sign-in for platform administrators. Tenant staff may opt out at sign-in, but high-risk actions (payment changes, sensitive credential operations, role/permission changes, and destructive operations) require a server-verified TOTP challenge and are denied to staff without an enrolled factor. Use optional WorkOS MFA API enrollment for tenant staff; AuthKit `max_age` reauthentication alone does not prove MFA. Confirm the flow in a pre-build compatibility spike.
- Record actor, tenant, resource, action, timestamp, request/job ID, reason, and before/after summary for privileged changes without logging secrets.
- Define approval thresholds for refunds, manual credits, bulk messaging, tenant suspension, and network-wide router commands.
