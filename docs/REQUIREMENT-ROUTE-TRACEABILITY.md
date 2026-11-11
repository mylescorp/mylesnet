# Requirement and route/action traceability catalog

**Task:** MN-PLAN-004 · **Plan phase:** Phase 0 — Close planning and turn decisions into build contracts  
**Status:** Planning artifact; catalog content is in review. No product behavior is implemented or authorized.  
**Depends on:** MN-PLAN-003 decision register and canonical reconciliation.  
**Authority:** Project charter in `AGENTS.md`, approved decisions in `READINESS-DECISIONS.md` and `DECISION-REGISTER.md`, then the canonical requirement documents linked below.

## Purpose and use

This catalog gives requirements, journeys, actions, and route references stable IDs so later architecture and implementation work can cite one contract. Public and tenant rows describe route coverage and observed views; they do not assert that every referenced interaction was verified. Platform-admin and subscriber/captive journey catalogs turn the approved product direction into acceptance cases while leaving unresolved route naming, hosting, provider, commercial, and legal choices visible.

The public sitemap inventory is 308 URLs: the homepage plus 307 non-homepage routes. The private public-site capture manifest has additional UI-state captures; those are not sitemap routes. The tenant review has 73 route/view entries, including theme and tab states, a captured 404, and one external router-console reference. Those 73 entries are not 73 approved application routes. The exact inventories below contain route strings or sanitized route templates only; they omit screenshot files, image metadata, page contents, account values, and other private reference data.

### Catalog conventions

- Requirement IDs (`REQ-*`), journey IDs (`J-*`), acceptance IDs (`AC-*`), and route IDs (`PUB-*`, `TEN-*`) are stable references. Route IDs are deterministic hashes of the canonical route or sanitized view descriptor; the route string remains the human-readable key.
- A route row inherits the actor, permission, tenant/branch scope, lifecycle/plan applicability, data class, audit policy, and acceptance case from its linked requirement. The requirement tables define those contracts once instead of repeating them inconsistently across hundreds of routes.
- `Tenant + assigned branch` means every operation derives tenant and branch access from verified server-side identity, membership, role, and resource scope. The exact resource-to-branch assignment and shared-resource rules remain open under MN-DEC-016; a client-supplied tenant or branch ID never authorizes access.
- All three SaaS tiers expose the same features. Quotas may block new additions while preserving existing records and service. Tier names, prices, quota values, tax handling, and exact entitlement values remain open. Lifecycle restrictions follow the approved tenant lifecycle; a route being listed does not bypass suspension or other server-side state.
- Audit events record actor, tenant/branch where applicable, resource, action, timestamp, reason where required, and request/job/correlation reference. Redact secrets and avoid copying sensitive field values into this planning catalog.
- Proposed platform navigation names are catalog keys, not finalized URLs. No CMS is assumed. Exact subscriber portal host/navigation remains open.

## Cross-cutting architecture contracts

Route rows inherit their requirement contracts above; those requirements also inherit the interface and sequence contracts below. Use this map when a route/action touches more than one service boundary. It keeps the 308 public routes and 73 tenant reference entries linked through their requirement IDs without duplicating an architecture contract on each route row.

| Requirement/journey family | Interface and sequence contracts |
|---|---|
| Public/signup and tenant provisioning (`REQ-PUB-*`, `REQ-SIGNUP-01`) | [`IC-APP-01/02`, `IC-IAM-01`, `SEQ-SIGNUP-01`](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md#interface-contract-registry) |
| Platform and tenant staff identity, roles, and revocation (`REQ-PLAT-04`, `REQ-TEN-10`, `REQ-IAM-01/02`) | [`IC-APP-02`, `IC-IAM-01/02`, `IC-DATA-01`, `SEQ-IAM-01/02`](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md#sequence-registry) |
| Tenant access, branch scope, exports, and audit (`REQ-PLAT-02/05/07`, `REQ-TEN-02/09/13`) | [`IC-DATA-01`, `IC-EXPORT-01`, `IC-STORAGE-01`, `SEQ-SUPPORT-01`, `SEQ-EXPORT-01`](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md#interface-contract-registry) |
| Tenant domains and branding (`REQ-TEN-11`, public and portal host resolution) | [`IC-APP-02`, `IC-DOMAIN-01`, `IC-THEME-01`, `SEQ-DOMAIN-01`](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md#sequence-registry) |
| Platform billing and tenant subscriber payments (`REQ-PLAT-03`, `REQ-TEN-07/08/12`, `REQ-SUB-02`, `REQ-CAP-01`) | [`IC-PAY-01`, `IC-LEDGER-01`, `IC-EVENT-01`, `SEQ-PAY-01`, `SEQ-PLAN-01`](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md#interface-contract-registry) |
| Network sessions, accounting, and router commands (`REQ-TEN-05`, `REQ-NET-01/02`) | [`IC-RADIUS-01`, `IC-ROUTER-01`, `IC-DATA-01`, `IC-OBS-01`, `SEQ-RADIUS-01`, `SEQ-ROUTER-01`](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md#sequence-registry) |
| Lifecycle, deletion, backup, and recovery (`REQ-PLAT-02/03/07`, `REQ-TEN-12/13`, `AC-D-03`) | [`IC-STORAGE-01`, `IC-RECOVERY-01`, `IC-MIGRATION-01`, `SEQ-OFFBOARD-01`, `SEQ-RESTORE-01`](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md#interface-contract-registry) |
| Service health and operational reporting (`REQ-PLAT-01/06`, `REQ-TEN-01`, `REQ-NET-01/02`) | [`IC-EVENT-01`, `IC-OBS-01`, `IC-INCIDENT-01`](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md#interface-contract-registry) |

The full contract definitions and their limits are indexed in the [ADR set](adr/README.md). Contract links do not close open provider, branch-assignment, regional, design-system, legal, retention, RPO/RTO, or SLO measurement decisions.

## Public and signup requirements

| Requirement | Actor and permission | Scope and applicability | Data class and audit | Acceptance case |
|---|---|---|---|---|
| REQ-PUB-01 — Core marketing and product pages | Anonymous visitor; public read | Public, structured/server-rendered content; all visitors | Public content; no per-view audit | AC-PUB-01: route is discoverable, responsive, and has correct canonical metadata; CMS/editor access is not inferred. |
| REQ-PUB-02 — Feature pages | Anonymous visitor; public read | Public, static/structured; all visitors | Public content; no per-view audit | AC-PUB-01, AC-PUB-02: route maps to an approved v1 feature family and does not promise an unapproved provider, SLO, or capability. |
| REQ-PUB-03 — Solutions and comparisons | Anonymous visitor; public read | Public, structured; all visitors | Public content; no per-view audit | AC-PUB-01, AC-PUB-02: route remains in the inventory and claims are traceable to approved scope. |
| REQ-PUB-04 — Resources, blog, changelog, and integrations directory | Anonymous visitor; public read | Public, structured; all visitors | Public content; no per-view audit. Publication changes, if a CMS is later approved, require versioned publish audit. | AC-PUB-01, AC-PUB-02: routes stay static/structured unless an editorial need and CMS decision are approved. |
| REQ-PUB-05 — Country and market pages | Anonymous visitor; public read | Public; country-specific launch/payment statements must respect that country's approval gate | Public content; no per-view audit | AC-PUB-02: no candidate provider, data region, or legal status is represented as approved/live. |
| REQ-PUB-06 — Public tools and calculators | Anonymous visitor; public use | Public; inputs remain in the browser unless a later privacy-reviewed need says otherwise | Public or transient input; no retained PII by default | AC-PUB-01: tool behavior is bounded, accessible, and does not silently retain inputs. |
| REQ-PUB-07 — Legal, privacy, affiliate, and contact content | Anonymous visitor; public read or explicit contact submission | Public content; submissions are purpose-limited and country-aware | Public content; contact form may contain PII and receives a submission audit/event | AC-PUB-02: consent and contact data are limited to the stated purpose; legal copy is not invented by implementation. |
| REQ-SIGNUP-01 — ISP signup, verification, and recovery | Prospect / initial Owner; public signup actions | One tenant per verified Owner identity; duplicate business names allowed; reserved subdomains blocked; self-service after email verification. Trial and lifecycle rules apply as approved. | Identity/contact PII, consent, tenant setup and provisioning audit; no government business-registration ID in v1 | AC-SIGNUP-01: verify email before idempotent tenant provisioning; show recoverable progress/support state; never report ready before tenant and Owner access succeed. |

## Platform-admin requirements and journey catalog

Platform route groups are proposed in `ROUTES-AND-ACCESS.md` and `TECHNICAL-SPECIFICATION.md` §5.3. Platform administrators use a distinct MFA-protected platform identity/role. They have no routine tenant-data access; tenant records are visible only through the approved, scoped support boundary.

| Requirement / journey | Actor and permission | Scope and lifecycle/plan | Data class and audit | Acceptance case |
|---|---|---|---|---|
| REQ-PLAT-01 — Service and onboarding overview | Platform Owner, Operations, Finance, Support, Security Admin, Auditor; role-scoped `read` | Platform aggregates; minimize subscriber PII; show current source timestamps | Platform operations and aggregate tenant metadata; privileged filter/export audited | AC-PLAT-01: health, lifecycle, queue, incident, onboarding, and billing summaries are visible only to assigned platform roles and do not expose unnecessary subscriber data. |
| REQ-PLAT-02 — Tenant directory and lifecycle | Platform Owner; Operations within assigned permissions; separate `read`, `create/provision`, `suspend`, `reactivate`, `offboard`, and platform-scope `export` actions | One tenant record at a time or bounded platform query; approved trial/active/grace/restricted/suspended/offboarding lifecycle | Tenant metadata and limited Owner contact PII for authorized operational reads; platform export is limited to non-personal tenant metadata; lifecycle, reason, approver, platform export/offboard events audited | AC-PLAT-02: actions validate lifecycle and permission server-side, show impact/recovery path, preserve retention duties, and never treat a platform role or support grant as tenant-data export authority. |
| REQ-PLAT-03 — SaaS plans, entitlements, subscriptions, and invoices | Platform Owner manages plan policy; Platform Finance reads/reconciles/adjusts within approval; Auditor read-only | MylesNet platform ledger only; same features across tiers; exact price/quota/tax values remain open | Financial and tenant subscription metadata; immutable invoice/ledger effects; refunds/credits use approved reversals | AC-PLAT-03: platform subscription accounting stays separate from tenant subscriber collections; no unapproved tier value or pooled collection is activated. |
| REQ-PLAT-04 — Platform operators and access | Platform Owner / authorized Security Admin; `operator.manage`, `role.assign`, session revoke within ceiling; Auditor read-only | Platform identities only; MFA required; no self-escalation | Security/access and identity data; invite, role, MFA, recovery, session, and approval events audited | AC-PLAT-04: every platform admin has MFA; role changes and revocations are enforced immediately and leave an immutable audit record. |
| REQ-PLAT-05 — Tenant support grant | Platform Support requests; tenant Owner approves; Platform Security Admin and a distinct authorized approver handle emergency path | Tenant/resource-scoped read-only grant, max one hour; only explicitly approved notes and non-sensitive profile/contact corrections can be writable; no finance, roles, secrets, network, messaging, exports, delete, or suspension | Tenant data only within grant; reason, case, requested scope, approval, start, expiry, reads/writes, revoke, and notification audited | AC-PLAT-05: no data access before approval; banner and audit are visible; grant expires; emergency path has two distinct approvers and notifies tenant. |
| REQ-PLAT-06 — Integrations and operations | Platform Owner / Operations / assigned Finance or Support roles; status read, assigned operational actions, `secret.rotate` only | Platform service/provider/job scope; candidates remain disabled until approved; secrets never read back | Operational/security metadata and secret references; rotation, retry, maintenance, and incident actions audited | AC-PLAT-06: status is distinguishable from provider approval; a secret can be written/rotated but not revealed; retries are bounded and attributable. |
| REQ-PLAT-07 — Platform audit and policy | Platform roles by assigned read permission; policy changes by authorized owner | Platform audit is append-only; bounded time/resource queries; export is a separately granted platform-scope action | Security/audit; export request and result audited without exporting tenant records or secret values | AC-PLAT-07: audit events cannot be edited/deleted; platform audit exports are scoped, justified, MFA-protected, and logged; tenant data requires the separate Owner-authorized export contract. |
| REQ-PLAT-08 — Marketing CMS (candidate only) | Candidate platform editorial roles; no CMS permission is approved yet | Only if a CMS decision is approved; tenant staff cannot publish public marketing content | Public content plus author/version history; draft/review/publish/unpublish/schedule/slug redirects would be audited | AC-PLAT-08: remains excluded from implementation until MN-DEC-020 is resolved with an approved editing need, owner, and workflow. |

| Journey | Required stages and states | Acceptance case |
|---|---|---|
| J-PLAT-01 — Inspect service health | Overview → source timestamp and lifecycle summaries → authorized detail view → bounded drill-down | AC-PLAT-01 |
| J-PLAT-02 — Provision and manage a tenant | Search or onboarding queue → tenant detail → invite/provision/retry or lifecycle action → reason/impact confirmation → result and audit history | AC-PLAT-02 |
| J-PLAT-03 — Reconcile MylesNet subscription | Tenant billing summary → platform invoice/payment evidence → reconcile or approved adjustment → immutable result | AC-PLAT-03 |
| J-PLAT-04 — Review platform access | Operator/role/MFA list → review assignment → revoke/change with authorization → immediate effective access → audit | AC-PLAT-04 |
| J-PLAT-05 — Request and approve support | Case/reason/scope request → Owner approval or denied state → visible bounded session → expiry/revoke → tenant notification and audit | AC-PLAT-05 |
| J-PLAT-06 — Respond to service/provider issue | Status/job/incident → assigned retry/rotation/maintenance action → bounded result → follow-up audit | AC-PLAT-06 |
| J-PLAT-07 — Review audit and policy | Platform-scoped search → redacted detail → separately authorized platform-audit export if needed → export receipt/audit | AC-PLAT-07 |

## Tenant-console requirements

The role names and ceilings below follow the charter and technical specification §§4 and 6. The Owner is fixed and tenant-wide. Admin, functional, and custom roles may have tenant-wide or branch scope, with no self-escalation. Each request and live subscription re-evaluates current membership, tenant state, permission, branch scope, and support grant where relevant.

| Requirement | Actors and permission | Scope, tier, and lifecycle | Data class and audit | Acceptance case |
|---|---|---|---|---|
| REQ-TEN-01 — Overview and live dashboard | Owner/Admin and assigned functional roles; `read` for each displayed dataset | Authorized tenant/branch aggregate; all tiers; no mutation from summary card | Operational aggregates and potentially PII-minimized activity; privileged drill-down audited | AC-TEN-01: aggregates are timestamped, branch-scoped, permission-filtered, and do not imply stale data is live. |
| REQ-TEN-02 — Subscribers and service accounts | Owner/Admin; assigned Support/Network/Finance roles per field permission; separate `read/create/update/deactivate` | Tenant + assigned branch; all tiers; quota can block new additions while existing accounts/services remain | PII, service/access data, and credentials; changes, credential reset, export, and activation audited | AC-TEN-02: own-tenant/branch filtering applies on reads/writes/jobs; masking and lifecycle rules are respected. |
| REQ-TEN-03 — Leads and sales pipeline | Owner/Admin; Sales for assigned leads; other roles only assigned `read` | Tenant + assigned branch; all tiers | Prospect PII and sales data; conversion/archive/import and assignment changes audited | AC-TEN-03: import has preview/validation and duplicate handling; conversion preserves history and scope. |
| REQ-TEN-04 — Support tickets and feedback | Owner/Admin; Support for assigned tickets; Subscriber only for own ticket; other roles by permission | Tenant + assigned branch, or subscriber's own records | PII and support content; access, assignment, status, message, export, and resolution audited | AC-TEN-04: tenant staff see only permitted tickets; subscriber portal exposes only the caller's own ticket and approved fields. |
| REQ-TEN-05 — Plans, sessions, NAS, and network operations | Owner/Admin; Network Operator for assigned technical fields/commands; limited status views for other roles | Tenant + assigned branch/NAS; all tiers; country/device/network activation gates apply | Network/AAA metadata, sensitive credentials and session state; commands, disconnect, secret rotation, and failures audited | AC-TEN-05: tenant/NAS mapping is server-verified; command preview, permission, MFA/high-risk grant, timeout and result are recorded; router secrets never read back. |
| REQ-TEN-06 — Equipment, CPE, and fiber inventory | Owner/Admin; Network Operator and assigned staff according to field permission | Tenant + assigned branch; all tiers | Operational/network metadata and subscriber linkage; assignment, firmware/policy change, disposal/archive audited | AC-TEN-06: archive preserves history; firmware and remote commands remain subject to lab/device approval and cannot expand the allowed router tunnel. |
| REQ-TEN-07 — Tenant finance and ledger | Owner/Admin; Finance for assigned finance actions; other roles masked/read-only by permission | Tenant + billing legal entity/branch; exact currency per transaction; pooled collection disabled until country approval | Financial PII, payment references and immutable ledger; manual entry, reconciliation, refund, credit, reversal, and export audited | AC-TEN-07: settled facts are not edited/deleted; correction is a linked reversal/credit; provider evidence is idempotent; tenant and platform ledgers never mix. |
| REQ-TEN-08 — Vouchers, agents, campaigns, and communications | Owner/Admin; Sales/Support by assigned work; Owner-approved `send`/bulk grant | Tenant + assigned branch/audience; consent and country/provider capability apply; all plan tiers | PII, consent, voucher secrets, message content/delivery; issue, void, export, audience preview, approval, send, and opt-out audited | AC-TEN-08: no send without consent/suppression/cost checks and required approval; issued/redeemed vouchers retain history. |
| REQ-TEN-09 — Analytics and reporting | Owner/Admin; Finance/Network/Support/Analyst only approved datasets | Tenant + branch scope; bounded date range; all tiers; exports separately granted | Aggregate or sensitive finance/network/PII; query is permission-scoped, export separately audited | AC-TEN-09: reports show source/as-of information; no cross-branch or cross-tenant result leaks through filters or exports. |
| REQ-TEN-10 — Staff, roles, and membership | Owner; Admin within own ceiling; Auditor/other staff read only if granted | Tenant-wide Owner; tenant-wide or selected-branch assignments for others | Identity, role, permission, branch, audit data; invite, role, scope, deactivation, and recovery audited | AC-TEN-10: exactly one active Owner; no self-escalation; immediate session/subscription revocation; custom-role deletion blocked while assigned. |
| REQ-IAM-01 — Tenant ownership transfer | Sole current Tenant Owner initiates with `owner.transfer`; incoming Owner verifies and accepts; both complete MFA step-up (incoming Owner enrolls if needed) | One tenant; transfer keeps exactly one active Owner; apply approved former-Owner role choice immediately | Identity, membership, role, verified contact, and security data; transfer request, acceptance, atomic role change, notices, and result audited | AC-IAM-01: reject an unverified or ineligible incoming identity; grant no Owner access before acceptance and MFA; atomically transfer ownership and former-Owner role (tenant-wide Admin by default, narrower role, or remove); maintain exactly one active Owner; immediately recompute access; notify verified tenant contacts. |
| REQ-IAM-02 — Tenant Owner recovery after loss of verified methods | Owner claimant; trained Platform Support verifies and initiates; a distinct MFA-protected Platform Security Admin independently approves high-risk recovery when required | One tenant owner identity; verified email/phone self-service first; manual recovery only when needed; recovery does not grant routine tenant-data support access | Identity and business-status evidence, trusted contact data, security case, and audit; evidence access minimized and notices go only to previously verified contacts | AC-IAM-02: when all verified methods are lost, verify current business status plus claimant identity/authority and call an existing trusted tenant contact; apply 24-hour default or 72-hour high-risk cooling-off. If no trusted contact is reachable, classify high risk, require distinct Security Admin approval plus a current official business-register record or formal company resolution and live government-ID check with independent evidence review. Notify prior verified Owner contacts at request and decision, provide a dispute path, never use newly supplied contacts for notices, and record the full audit trail. |
| REQ-TEN-11 — Tenant settings, domains, security, and integrations | Owner; Admin within ceiling; domain/network/finance settings by assigned permission; high-risk secret operations Owner-approved | Tenant and relevant legal entity/branch; branding/custom domains available on every tier; country/provider settings gated | Tenant configuration, PII, provider references/secrets; changes, verification, rotation, and security actions audited | AC-TEN-11: DNS verification precedes HTTPS/domain change; prior host remains through callback migration; secrets are write/rotate-only; all changes are tenant-scoped. |
| REQ-TEN-12 — MylesNet subscription settings | Owner; assigned Finance can read/reconcile only within policy | Platform subscription associated with tenant billing legal entity; all tiers; exact prices/quotas remain open | Platform financial metadata; plan/cancel/auto-pay and billing actions audited in platform ledger | AC-TEN-12: only Owner confirms high-risk plan/auto-pay actions with MFA; exact currency and approved lifecycle apply; no cross-entity wallet draw. |
| REQ-TEN-13 — Tenant audit, referrals, notices, and feedback | Owner/Admin and role-scoped readers; subscriber may create own feedback/read public product notice | Tenant + branch or subscriber-own scope; feedback/referral features only as approved | Audit, PII, referral and product notice data; edits to operational feedback/referral state audited; audit itself append-only | AC-TEN-13: histories stay immutable where required and reads remain scoped/masked. |

### Owner transfer and recovery journeys

The transfer and recovery contracts follow [`CORE-FLOWS-AND-DATA.md`](CORE-FLOWS-AND-DATA.md) “Tenant ownership transfer,” [`READINESS-DECISIONS.md`](READINESS-DECISIONS.md) “Tenant authorization and ownership,” and [`IMPLEMENTATION-PLAN.md`](IMPLEMENTATION-PLAN.md) Phase 3 work items 8–9.

| Journey | Required stages and states | Acceptance case |
|---|---|---|
| J-IAM-01 — Transfer tenant ownership | Current Owner requests and MFA-confirms → incoming Owner verifies, accepts, and MFA-confirms/enrolls → chooses former Owner's post-transfer access → atomic ownership/role change → immediate access recomputation → verified-contact notifications and audit; if the current Owner cannot initiate, route to J-IAM-02 | AC-IAM-01 |
| J-IAM-02 — Recover tenant ownership | Verified email/phone self-service → if all methods are lost, support verifies business and claimant authority → trusted-contact callback → 24-hour default/72-hour high-risk cooling-off and prior-contact notices → if no trusted contact is reachable, high-risk evidence and distinct Security Admin approval → decision/dispute path and audit; no routine tenant-data grant | AC-IAM-02 |

## Subscriber, captive, and network journeys

Subscriber journeys use a separate identity from staff. Subscriber reads and writes are self-scoped to their verified identity and resolved tenant. Captive pre-auth access is limited to public tenant portal content and a short-lived checkout/voucher flow. Payment redirect pages are not proof of payment; only a valid provider confirmation or reconciliation event can create a ledger effect or activate access.

| Requirement / journey | Actor and permission | Scope/applicability | Data class and audit | Acceptance case |
|---|---|---|---|---|
| REQ-SUB-01 / J-SUB-01 — Sign in and account recovery | Subscriber; own identity only | Own subscriber identity, tenant; staff AuthKit and roles are separate | Contact/authentication PII and security events; recovery and credential changes audited | AC-SUB-01: no account enumeration; verified recovery only; no cross-subscriber or staff-data access. |
| REQ-SUB-02 / J-SUB-02 — Service, invoice, and renewal | Subscriber; `read own`, initiate eligible payment/renewal | Own tenant account/service; published plans only; payment method/country must be enabled | PII, service and financial records; payment intent/result and profile/consent changes audited | AC-SUB-02: show pending/success/failure/expired states; retry is safe; receipt reflects immutable ledger state. |
| REQ-SUB-03 / J-SUB-03 — Own support and contact preferences | Subscriber; own ticket and permitted contact/preferences update | Own tenant/account only | PII and support content; consent/preferences and ticket changes audited | AC-SUB-03: only permitted self-service fields are writable; STOP/suppression is honored. |
| REQ-CAP-01 / J-CAP-01 — Captive plan purchase | Anonymous visitor or subscriber; view public portal and create own checkout intent | One resolved tenant/portal domain; country/provider enablement and published plan apply | PII/payment reference and service intent; checkout attempt/result audited, secrets excluded | AC-CAP-01: duplicate prompts/activation prevented; signed confirmation or reconciliation event is required; retry/failure has safe recovery. |
| REQ-CAP-02 / J-CAP-02 — Voucher redemption or return login | Subscriber/voucher holder; prove voucher ownership or subscriber credentials | One tenant and verified NAS mapping; voucher validity/plan state applies | Voucher credential, subscriber PII and network access; redemption/denial/session event audited without logging secret | AC-CAP-02: voucher cannot be reused beyond policy; tenant/NAS and expiry/quota checks precede access; no cross-tenant lookup. |
| REQ-NET-01 / J-NET-01 — RADIUS authentication/accounting | Trusted NAS and FreeRADIUS service; system policy decision | NAS maps server-side to one tenant; PostgreSQL AAA is authoritative on request path; Convex is not synchronous dependency | Network/AAA credentials, accounting and session data; starts/interim/stops idempotent and security events audited | AC-NET-01: policy uses tenant, entitlement, expiry/quota/session constraints; on RADIUS loss existing sessions continue, new logins reject, never fail open. |
| REQ-NET-02 / J-NET-02 — Router enrollment and commands | Tenant Owner/Network Operator with permission; system worker executes approved job | Tenant + assigned branch/NAS; RouterOS 7 lab/provisioning gate; tunnel limited to RADIUS and CoA/Disconnect | Network/security metadata and secret references; issue, rotate, revoke, command, timeout, retry and result audited | AC-NET-02: single-use expiring least-privilege provisioning; block WinBox/SSH/RouterOS API over tunnel; do not connect real routers during planning. |

| Journey | Ordered states and required outcome | Acceptance |
|---|---|---|
| J-SUB-01 — Sign in/recover | Identify → verify → authenticated own-account view or safe recovery/pending/denied state | AC-SUB-01 |
| J-SUB-02 — Renew service | Own service → eligible plan → payment intent → pending → provider confirmation/reconciliation → receipt and activation or safe failure/retry | AC-SUB-02 |
| J-SUB-03 — Contact/support | Own profile/preferences → permission and consent check → save with audit, or own-ticket create/read/update | AC-SUB-03 |
| J-CAP-01 — Buy a package | Tenant portal → plan/terms → minimal identity → expiring checkout intent → provider handoff → pending → verified confirmation/reconciliation → one ledger effect, entitlement and activation; otherwise failure/expiry/retry | AC-CAP-01 |
| J-CAP-02 — Redeem voucher or return | Tenant portal → voucher/login verification → tenant/NAS and plan checks → idempotent redemption/session activation or explicit denial | AC-CAP-02 |
| J-NET-01 — Authenticate/account | NAS identity/secret → server-side tenant mapping → policy check → accept/reject; accounting events persist idempotently; outage fails closed for new sessions | AC-NET-01 |
| J-NET-02 — Enroll/control router | Owner-authorized request → preview scope → one-time expiring config → isolated lab validation → narrowly allowed command → result/timeout and audit → revoke/rotate | AC-NET-02 |

## Cross-cutting export and destructive-action contracts

| Action class | Required actor/permission and scope | Required controls and immutable history | Acceptance |
|---|---|---|---|
| Tenant sensitive export (subscriber, payment, finance, or similarly sensitive data) | Explicit `export` permission granted/approved by the Tenant Owner; the caller must also have read access to each included field and branch | MFA step-up; bounded date/filter/record set; purpose/reason; field masking/minimization; rate limit; safe file URL and expiry; actor, approver, scope, time and result audit. Never infer export from view permission. | AC-X-01: unauthorized, over-broad, expired, cross-tenant or out-of-branch exports fail closed and leave an audit denial/result without exposing data. |
| Platform-scope export (tenant directory metadata, platform finance, platform audit/policy) | Explicit platform `export` permission for platform-owned records and non-personal tenant metadata only | Bounded purpose/scope; MFA for sensitive output; reason and independent approval where platform policy requires; audit of request, approval, generated file and access; exclude all tenant personal data, subscriber/customer records, tenant files, tenant ledger data, and any data obtained under a SupportGrant | AC-X-02: platform export is limited to platform-owned records and non-personal tenant metadata; a support grant never authorizes tenant-data export. |
| Tenant-data export executed through platform tooling | Tenant Owner initiates or separately authorizes the exact tenant/branch export scope and grants `export`; a platform executor must also hold the separate `offboard` permission when executing an offboarding export. This is not a support session or support-grant action. | MFA; exact tenant/branch/date/record scope and purpose; least-privilege fields; reason and approval; safe file expiry; actor, Owner approver, generation, and access audited. SupportGrant context is absent and cannot obtain or widen the export. | AC-X-03: exports require explicit Owner authorization and tenant `export` access; they cannot be initiated or retrieved through `support.start`, `support.approve`, or a SupportGrant. |
| Operational delete/deactivate/archive | Resource-specific `delete/archive` permission inside current tenant/branch scope; high-risk/bulk action requires Owner grant and MFA | Preview affected records/services; confirmation and reason; preserve references/history; audit before/after summary. Use deactivate/archive where a resource has dependent history. | AC-D-01: destructive operation cannot cross tenant/branch scope, bypass lifecycle, or erase mandatory financial, security, legal, or audit history. |
| Financial, payment, consent, access, or audit correction | Explicit `reverse`, `refund`, `credit`, `void`, `revoke`, or equivalent permission; approvals per resource; MFA for high-risk action | Append linked reversal/adjustment/deactivation, reason and provider evidence; preserve original event; prevent duplicate effect with idempotency; audit both entries. Audit events themselves cannot be changed/deleted. | AC-D-02: settled financial facts and audit records remain immutable; retry/correction cannot double-post or erase evidence. |
| Tenant offboarding / account deletion | Authorized platform lifecycle action; Owner follows tenant export/cancellation path; legal hold/retention policy applies | Impact preview, reason, approval, export opportunity, state transition, retention/legal hold and deletion job audit; exact schedule remains open pending country counsel | AC-D-03: service shutdown is distinguishable from record erasure; legal hold and mandatory retention override deletion; no exact retention period is invented. |

## Public sitemap route inventory

The canonical set is 308 unique public URL paths (homepage `/` plus 307 non-homepage sitemap paths). `REQ-*` links route rows to the requirement contract above. The reference gallery's additional state captures are excluded. Stable IDs are the first 10 uppercase hexadecimal characters of SHA-256 over the exact normalized path.

| Route ID | Public URL path | Requirement | Page family |
|---|---|---|---|
| PUB-8A5EDAB282 | `/` | REQ-PUB-01 | Home |
| PUB-1E617A77AE | `/bandwidth-calculator` | REQ-PUB-06 | Tool/calculator |
| PUB-A4CABCE860 | `/bangladesh` | REQ-PUB-05 | Country/market |
| PUB-AB5746DC9D | `/become-an-affiliate` | REQ-PUB-07 | Legal/contact/affiliate |
| PUB-B40437E2B9 | `/benin` | REQ-PUB-05 | Country/market |
| PUB-8CAAFE4386 | `/blog` | REQ-PUB-04 | Blog |
| PUB-4051E03ACB | `/blog/automatic-invoicing-software` | REQ-PUB-04 | Blog |
| PUB-E21342D59E | `/blog/automatic-reconnection-after-mpesa-payment` | REQ-PUB-04 | Blog |
| PUB-82D3BECA8D | `/blog/best-isp-billing-system-in-kenya` | REQ-PUB-04 | Blog |
| PUB-6066C47291 | `/blog/captive-portal-mobile-money-instant-internet-access` | REQ-PUB-04 | Blog |
| PUB-C0F8926AB5 | `/blog/centipid-billing-guide` | REQ-PUB-04 | Blog |
| PUB-921EE00635 | `/blog/centipid-billing-system-2026-smarter-isp-management` | REQ-PUB-04 | Blog |
| PUB-85079C23A4 | `/blog/cloud-billing-software-wireless-isps-kenya-no-local-server` | REQ-PUB-04 | Blog |
| PUB-DA4C90958A | `/blog/free-open-source-tr-069-acs-genieacs-vs-paid-platforms` | REQ-PUB-04 | Blog |
| PUB-E5EA31B95F | `/blog/hotspot-billing-software` | REQ-PUB-04 | Blog |
| PUB-281B735E7B | `/blog/hotspot-billing-system-cyber-cafe-estate-wifi-nairobi` | REQ-PUB-04 | Blog |
| PUB-21878B3B61 | `/blog/how-many-public-ip-addresses-does-an-isp-need` | REQ-PUB-04 | Blog |
| PUB-6E17B3F901 | `/blog/how-to-integrate-mpesa-with-a-billing-system` | REQ-PUB-04 | Blog |
| PUB-7ECE9D0B17 | `/blog/how-to-manage-isp-customers-and-billing` | REQ-PUB-04 | Blog |
| PUB-5ADE39E53E | `/blog/how-to-pay-safaricom-wifi-via-mpesa` | REQ-PUB-04 | Blog |
| PUB-504FDDADDE | `/blog/internet-vouchers` | REQ-PUB-04 | Blog |
| PUB-D9DEFED1A6 | `/blog/isp-billing-bandwidth-management-software` | REQ-PUB-04 | Blog |
| PUB-90B955A3DD | `/blog/isp-billing-software` | REQ-PUB-04 | Blog |
| PUB-355F1290AF | `/blog/isp-billing-system-indonesia` | REQ-PUB-04 | Blog |
| PUB-84BDEBB609 | `/blog/isp-management-software` | REQ-PUB-04 | Blog |
| PUB-6FB56F248F | `/blog/isp-network-monitoring-on-a-small-budget` | REQ-PUB-04 | Blog |
| PUB-E998EE39AC | `/blog/isp-subscriber-management-system` | REQ-PUB-04 | Blog |
| PUB-A736E95A21 | `/blog/lipa-na-mpesa-portal` | REQ-PUB-04 | Blog |
| PUB-A61C6D61D2 | `/blog/m-pesa-business-number` | REQ-PUB-04 | Blog |
| PUB-726A00F0E1 | `/blog/mikrotik` | REQ-PUB-04 | Blog |
| PUB-6815A544CA | `/blog/mikrotik-billing-system` | REQ-PUB-04 | Blog |
| PUB-97CF5C7426 | `/blog/mikrotik-radius-server` | REQ-PUB-04 | Blog |
| PUB-D231C44B45 | `/blog/mpesa-business-email` | REQ-PUB-04 | Blog |
| PUB-7B0EA769CF | `/blog/network-access-control` | REQ-PUB-04 | Blog |
| PUB-7046425559 | `/blog/pay-bill-equity-bank` | REQ-PUB-04 | Blog |
| PUB-D3EEDA2BEE | `/blog/safaricom-paybill-application` | REQ-PUB-04 | Blog |
| PUB-01F413B7CC | `/blog/when-you-need-billing-crm-and-nms-separately-not-in-one-platform` | REQ-PUB-04 | Blog |
| PUB-3E93F312C0 | `/blog/winbox` | REQ-PUB-04 | Blog |
| PUB-1532677781 | `/burkina-faso` | REQ-PUB-05 | Country/market |
| PUB-3E3CB8F8AF | `/cameroon` | REQ-PUB-05 | Country/market |
| PUB-1A0ECF5052 | `/cgnat-calculator` | REQ-PUB-06 | Tool/calculator |
| PUB-9433DB80E9 | `/changelog` | REQ-PUB-04 | Changelog |
| PUB-7EAA6E0255 | `/changelog/15-day-subscription-packages-for-pppoe-plans` | REQ-PUB-04 | Changelog |
| PUB-A9507AF0B9 | `/changelog/accounting-and-mpesa-records-are-now-retained-for-up-to-three-years-in-compliance-with-cak` | REQ-PUB-04 | Changelog |
| PUB-371A365180 | `/changelog/accurate-mine-ticket-counts-and-resolved-ticket-metrics-when-tickets-are-reopened` | REQ-PUB-04 | Changelog |
| PUB-10F9DDB61B | `/changelog/accurate-whatsapp-message-statuses-now-distinguish-sent-delivered-failed-and-pending-messages-with-retries-limited-to-failed-messages` | REQ-PUB-04 | Changelog |
| PUB-5556AD2A31 | `/changelog/active-plan-view-for-quickly-finding-active-subscribers` | REQ-PUB-04 | Changelog |
| PUB-FE304BAA08 | `/changelog/admin-alerts-when-an-m-pesa-payout-fails-because-of-an-invalid-recipient` | REQ-PUB-04 | Changelog |
| PUB-42F9A16EF2 | `/changelog/agent-sales-revenue-and-commissions-now-use-the-price-paid-when-each-voucher-batch-was-issued-keeping-historical-reports-accurate-after-package-price-changes` | REQ-PUB-04 | Changelog |
| PUB-D4F62049C2 | `/changelog/an-invoices-quotif-unpaidquot-panel-now-says-when-the-subscribers-plan-has-already-expired-instead-of-showing-a-date-that-seemed-to-clash-with-the-due-date` | REQ-PUB-04 | Changelog |
| PUB-7F4E22D47F | `/changelog/analytics-networking-graphs-now-fit-phone-screens-and-display-date-labels-without-being-cut-off` | REQ-PUB-04 | Changelog |
| PUB-8EA8DAD1A9 | `/changelog/anti-sharing-rules-now-apply-correctly-after-first-time-device-provisioning` | REQ-PUB-04 | Changelog |
| PUB-8944C7C4D1 | `/changelog/azampay-callbacks-with-valid-rsa-signatures-are-now-accepted-preventing-legitimate-payments-from-being-rejected` | REQ-PUB-04 | Changelog |
| PUB-EBD271EDF0 | `/changelog/azampay-client-secret-can-now-be-up-to-1000-characters` | REQ-PUB-04 | Changelog |
| PUB-47370999FE | `/changelog/back-and-refresh-buttons-in-the-installed-app` | REQ-PUB-04 | Changelog |
| PUB-1A1D7E76ED | `/changelog/batch-voucher-printing-now-uses-the-selected-batch-so-the-generated-pdf-matches-the-vouchers-chosen-for-printing` | REQ-PUB-04 | Changelog |
| PUB-D42F12A186 | `/changelog/bluetooth-thermal-printing-for-58-mm-voucher-slips-from-a-phone` | REQ-PUB-04 | Changelog |
| PUB-A7F3928EDD | `/changelog/branch-specific-routeros-cve-checks-for-stable-long-term-and-v6-releases-with-upgrade-guidance-tailored-to-each-branch` | REQ-PUB-04 | Changelog |
| PUB-43791FD91E | `/changelog/bulk-compensation-for-selected-subscribers-from-the-users-list-with-readable-times-in-outage-credit-sms-messages` | REQ-PUB-04 | Changelog |
| PUB-15EFFABC8C | `/changelog/bulk-compensation-for-subscriber-groups-like-hotspot-and-pppoe` | REQ-PUB-04 | Changelog |
| PUB-DC2B6419BF | `/changelog/bunicom-sms-gateway-support-for-sending-bulk-messages-in-kenya` | REQ-PUB-04 | Changelog |
| PUB-DFC5D48A91 | `/changelog/buy-your-own-domain-and-manage-its-dns-from-settings` | REQ-PUB-04 | Changelog |
| PUB-1705BA9ACF | `/changelog/calendar-based-analytics-ranges-for-daily-weekly-monthly-and-yearly-views-with-synchronized-filters-and-hourly-detail-in-daily-analytics` | REQ-PUB-04 | Changelog |
| PUB-C98FB06235 | `/changelog/campaign-support-in-more-templates` | REQ-PUB-04 | Changelog |
| PUB-82B25D16BC | `/changelog/captive-portal-checkout-now-surfaces-the-phone-numberpackage-selection-more-prominently-so-its-easier-to-find-during-signup` | REQ-PUB-04 | Changelog |
| PUB-8B61974946 | `/changelog/clear-payment-success-results-and-next-step-guidance-after-purchasing-a-domain` | REQ-PUB-04 | Changelog |
| PUB-B1FBB83465 | `/changelog/clearer-campaign-lists-with-user-friendly-audience-and-status-labels-queued-campaign-totals-sentfailed-message-counts-and-dark-mode-status-chips` | REQ-PUB-04 | Changelog |
| PUB-96AB93374E | `/changelog/clearer-instructions-for-sending-and-entering-the-email-verification-code-when-buying-a-domain` | REQ-PUB-04 | Changelog |
| PUB-27654BF063 | `/changelog/clearer-payment-error-messaging-and-setup-guidance-for-hubtel-ip-allow-listing` | REQ-PUB-04 | Changelog |
| PUB-329636B3CA | `/changelog/complete-leads-kanban-columns-now-show-their-available-cards-accurate-per-stage-counts-and-truncation-indicators-for-stages-with-more-than-the-visible-limit` | REQ-PUB-04 | Changelog |
| PUB-C75204072E | `/changelog/configurable-voucher-print-limits-of-50-100-200-500-or-1000-vouchers` | REQ-PUB-04 | Changelog |
| PUB-AD92AB1234 | `/changelog/confirmation-feedback-after-sending-credentials-and-other-partial-page-actions` | REQ-PUB-04 | Changelog |
| PUB-F181FC6B0A | `/changelog/connect-telegram-in-settings-operator-alerts-to-get-router-down-and-up-alerts-there-and-choose-which-channels-smswhatsapp-email-telegram-receive-them` | REQ-PUB-04 | Changelog |
| PUB-753C8D9416 | `/changelog/connection-status-on-the-tv-payment-page-and-package-expiry-details-after-purchase` | REQ-PUB-04 | Changelog |
| PUB-211CB56CC5 | `/changelog/consistent-30-day-revenue-figures-across-the-packages-list-plan-details-and-revenue-chart` | REQ-PUB-04 | Changelog |
| PUB-1791669746 | `/changelog/correct-memory-and-disk-totals-in-diagnostics-for-rb5009-routers` | REQ-PUB-04 | Changelog |
| PUB-B3860D6747 | `/changelog/credit-notes-for-full-or-partial-invoice-adjustments-including-numbered-pdfs-customer-portal-access-sage-synchronization-audit-history-and-wallet-credits-for-overpayments` | REQ-PUB-04 | Changelog |
| PUB-BD30564F9F | `/changelog/critical-security-findings-for-routers-running-vulnerable-routeros-versions-surfaced-on-router-pages-and-dashboards` | REQ-PUB-04 | Changelog |
| PUB-E17BBAB1B8 | `/changelog/daily-and-weekly-router-health-reports-covering-uptime-downtime-latency-recorded-outages-and-weekly-email-digests` | REQ-PUB-04 | Changelog |
| PUB-4DAA4283F9 | `/changelog/daily-sales-summary-emails-for-operators-to-monitor-sales-and-operator-activity` | REQ-PUB-04 | Changelog |
| PUB-8EC52A2981 | `/changelog/dalipay-payment-webhook-configuration-guidance-including-the-correct-callback-url-and-callback-secret-requirements` | REQ-PUB-04 | Changelog |
| PUB-A6FC1F9B5E | `/changelog/dark-mode-dropdown-menus-are-now-readable-instead-of-appearing-invisible` | REQ-PUB-04 | Changelog |
| PUB-92CFA530E1 | `/changelog/data-bundles-no-longer-expire-before-the-customers-first-login` | REQ-PUB-04 | Changelog |
| PUB-A1F2CF2863 | `/changelog/deleted-subscribers-payments-are-now-included-in-hotspot-and-pppoe-analytics-figures` | REQ-PUB-04 | Changelog |
| PUB-C2E781E048 | `/changelog/device-payments-now-show-as-connected-and-stk-payments-correctly-match-the-recorded-package-on-the-payment-page` | REQ-PUB-04 | Changelog |
| PUB-DA9315DE9A | `/changelog/domain-purchase-verification-emails-now-identify-the-recipient-as-the-domain-owner-instead-of-a-support-email` | REQ-PUB-04 | Changelog |
| PUB-632DD02B74 | `/changelog/dual-currency-pricing-for-zimbabwe-isps-displaying-amounts-in-us-and-zwg` | REQ-PUB-04 | Changelog |
| PUB-23187D9BAC | `/changelog/duplicate-phone-conflicts-now-show-a-friendly-validation-error-in-user-flows-and-users-agents-and-staff-get-a-toast-that-points-to-the-existing-record-holding-that-phone-number` | REQ-PUB-04 | Changelog |
| PUB-2351D136A7 | `/changelog/expiry-dates-when-importing-subscribers` | REQ-PUB-04 | Changelog |
| PUB-639BF4D24A | `/changelog/exporting-expired-or-active-subscribers-to-excel-now-preserves-the-selected-filter-instead-of-exporting-all-users` | REQ-PUB-04 | Changelog |
| PUB-EE1D9760CE | `/changelog/faster-alerts-for-signs-of-staff-account-misuse` | REQ-PUB-04 | Changelog |
| PUB-58D59139E8 | `/changelog/faster-payments-and-router-payments-page-loads-through-cached-historical-session-data-and-bounded-router-session-scans-with-graceful-fallback-when-weights-are-unavailable` | REQ-PUB-04 | Changelog |
| PUB-0CAAD36BAF | `/changelog/faster-routers-page-loading-for-users-with-many-mikrotiks` | REQ-PUB-04 | Changelog |
| PUB-237C160A34 | `/changelog/fimipay-mobile-money-payments-for-tanzania-available-through-settings-payments` | REQ-PUB-04 | Changelog |
| PUB-273B12F12C | `/changelog/fixed-outbound-ip-routing-for-hubtel-payment-gateway-calls-simplifying-gateway-allow-listing` | REQ-PUB-04 | Changelog |
| PUB-4281DCC5FB | `/changelog/flutterwave-as-a-selectable-payment-gateway-for-isps-with-payment-credits-issued-only-after-verification` | REQ-PUB-04 | Changelog |
| PUB-02288EFF8E | `/changelog/friendlier-ticket-labels-for-statuses-priorities-and-sources-with-brand-aligned-colours-and-dark-mode-support` | REQ-PUB-04 | Changelog |
| PUB-39D5815905 | `/changelog/full-figure-earnings-on-the-dashboard-such-as-1265-instead-of-13k` | REQ-PUB-04 | Changelog |
| PUB-454E95910F | `/changelog/fup-throttling-for-short-duration-plans-when-data-thresholds-are-reached-rather-than-expiring-the-voucher` | REQ-PUB-04 | Changelog |
| PUB-90EB7E186D | `/changelog/generate-voucher-codes-made-of-numbers-only` | REQ-PUB-04 | Changelog |
| PUB-39C2ACCE22 | `/changelog/grid-based-captive-portal-layout-that-shows-all-packages-in-the-viewport` | REQ-PUB-04 | Changelog |
| PUB-2D91DEEC2A | `/changelog/hap-lite-and-hap-mini-routers-can-now-be-provisioned` | REQ-PUB-04 | Changelog |
| PUB-D152D09075 | `/changelog/hotspot-ads-earn-when-subscribers-watch-a-video-ad` | REQ-PUB-04 | Changelog |
| PUB-0C0A7C07FE | `/changelog/hotspot-buy-button-no-longer-hangs-when-a-purchase-request-fails` | REQ-PUB-04 | Changelog |
| PUB-43EDE28D35 | `/changelog/hotspot-login-buttons-now-work-instead-of-silently-doing-nothing` | REQ-PUB-04 | Changelog |
| PUB-AEEE08786E | `/changelog/hotspot-payments-now-resolve-the-correct-payer-by-checkout-link-instead-of-merging-credits-by-phone-number-so-each-payment-applies-to-the-intended-user` | REQ-PUB-04 | Changelog |
| PUB-DDBC74C300 | `/changelog/hotspot-revenue-amount-used-to-calculate-the-3-fee-shown-on-centipid-invoices` | REQ-PUB-04 | Changelog |
| PUB-F83581DBC0 | `/changelog/hubtel-payments-are-more-reliable-customers-who-pay-are-connected-even-when-hubtel-is-slow-to-reply-and-they-are-never-sent-a-second-payment-prompt` | REQ-PUB-04 | Changelog |
| PUB-676C6BAF90 | `/changelog/hubtel-payments-now-go-through-add-your-hubtel-merchant-account-number-pos-sales-id-in-your-hubtel-payment-settings-and-payment-errors-now-show-hubtels-own-reason` | REQ-PUB-04 | Changelog |
| PUB-0513D34D7D | `/changelog/in-stock-equipment-can-now-be-saved-without-a-subscriber-avoiding-the-user-id-cannot-be-null-error` | REQ-PUB-04 | Changelog |
| PUB-E3F79002F1 | `/changelog/incorrect-add-a-domain-first-message-after-successfully-switching-to-a-custom-domain` | REQ-PUB-04 | Changelog |
| PUB-4D4AF2C739 | `/changelog/instant-sms-or-whatsapp-alerts-for-isp-owners-when-clients-renew-their-subscriptions` | REQ-PUB-04 | Changelog |
| PUB-3372C1145D | `/changelog/iotec-sms-gateway-selection-for-ugandan-isps-in-settings` | REQ-PUB-04 | Changelog |
| PUB-C2F2D2B737 | `/changelog/isp-to-isp-referrals-with-a-referral-list-email-invite-flow-referral-tracking-and-a-10-discount-for-the-referrer-when-the-referred-isp-pays-capped-at-100` | REQ-PUB-04 | Changelog |
| PUB-016BBB2766 | `/changelog/isps-in-tanzania-can-now-collect-vodacom-m-pesa-payments-directly-through-their-own-vodacom-merchant-account` | REQ-PUB-04 | Changelog |
| PUB-1258E62234 | `/changelog/kenyan-isp-compliance-workflows-covering-outage-credit-logs-complaint-references-and-subscriber-notices-before-package-price-changes` | REQ-PUB-04 | Changelog |
| PUB-A7AB20EAD6 | `/changelog/kiglo-market-messenger-is-now-available-as-an-sms-gateway-for-sending-messages` | REQ-PUB-04 | Changelog |
| PUB-44A95861A6 | `/changelog/loyalty-points-report-showing-subscriber-balances-points-issued-and-points-redeemed` | REQ-PUB-04 | Changelog |
| PUB-A0C736E424 | `/changelog/m-pesa-payments-through-vodacom-for-drc-based-isps` | REQ-PUB-04 | Changelog |
| PUB-AF2E50F632 | `/changelog/m-pesa-receipt-codes-can-no-longer-be-reused-to-connect-after-a-subscription-is-renewed` | REQ-PUB-04 | Changelog |
| PUB-D467979672 | `/changelog/mac-candidate-picker-now-prefers-the-active-dhcp-lease-over-a-stale-arp-entry-helping-bindings-use-the-correct-ip-address` | REQ-PUB-04 | Changelog |
| PUB-CA541831B5 | `/changelog/mac-candidate-selection-now-prefers-the-active-dhcp-lease-over-stale-arp-entries-preventing-bindings-from-picking-the-wrong-ip-address` | REQ-PUB-04 | Changelog |
| PUB-A947EA49A5 | `/changelog/manage-your-customers-terminals-over-tr-069-now-out-of-beta` | REQ-PUB-04 | Changelog |
| PUB-1A9B3292AC | `/changelog/mauritania-billing-now-uses-the-current-mru-currency-code-instead-of-retired-mro-preventing-incorrect-underbilling` | REQ-PUB-04 | Changelog |
| PUB-19D63402AD | `/changelog/mid-cycle-pppoe-plan-changes-with-prorated-upgrade-charges-or-downgrade-credits-while-keeping-the-existing-expiry-date` | REQ-PUB-04 | Changelog |
| PUB-50403B689C | `/changelog/missing-email-view-error-when-sending-payment-confirmation-emails` | REQ-PUB-04 | Changelog |
| PUB-A87DD8FC59 | `/changelog/missing-packages-are-now-created-automatically-during-subscriber-import` | REQ-PUB-04 | Changelog |
| PUB-0251BB6E40 | `/changelog/mnotify-sms-gateway-support-for-ghana-enabling-automated-invoices-payment-reminders-and-service-alerts` | REQ-PUB-04 | Changelog |
| PUB-AC8743F82E | `/changelog/mobivas-sms-gateway-support` | REQ-PUB-04 | Changelog |
| PUB-9A4A1CCD03 | `/changelog/monnify-settings-now-use-the-live-api-exclusively-removing-the-sandbox-environment-choice` | REQ-PUB-04 | Changelog |
| PUB-941D335745 | `/changelog/monthly-pppoe-plans-now-expire-on-the-same-calendar-date-in-the-following-month-including-during-31-day-months` | REQ-PUB-04 | Changelog |
| PUB-13FC3BFB11 | `/changelog/monthly-subscriptions-can-now-be-paid-using-mobile-money-in-uganda` | REQ-PUB-04 | Changelog |
| PUB-8412AE903D | `/changelog/more-reliable-router-connectivity-on-challenging-networks` | REQ-PUB-04 | Changelog |
| PUB-BD39BF9878 | `/changelog/mtn-sms-api-v3-as-an-available-sms-gateway` | REQ-PUB-04 | Changelog |
| PUB-C0FB0452DE | `/changelog/nalo-solutions-is-now-available-as-an-sms-gateway-for-ghana` | REQ-PUB-04 | Changelog |
| PUB-E57223DC3A | `/changelog/new-device-login-alerts-now-use-the-platform-mailer-avoiding-failures-caused-by-isp-specific-smtp-servers` | REQ-PUB-04 | Changelog |
| PUB-0EEE1741C6 | `/changelog/nextsms-and-textbee-now-appear-in-the-messaging-marketplace` | REQ-PUB-04 | Changelog |
| PUB-6477921E3A | `/changelog/on-demand-winbox-access-router-ports-stay-closed-until-opened-in-the-app-then-remain-available-for-60-minutes-without-requiring-a-fixed-source-ip` | REQ-PUB-04 | Changelog |
| PUB-63BE00FC50 | `/changelog/open-a-routers-web-interface-from-centipid` | REQ-PUB-04 | Changelog |
| PUB-0AF6202D94 | `/changelog/orange-sms-gateway-support-for-guinea-including-gateway-listing-for-billing-users` | REQ-PUB-04 | Changelog |
| PUB-6A9DCE42D0 | `/changelog/outage-compensation-is-now-capped-at-the-time-a-subscriber-had-left` | REQ-PUB-04 | Changelog |
| PUB-FA97B1AE33 | `/changelog/outage-credit-logging-and-complaint-references-for-regulatory-compliance` | REQ-PUB-04 | Changelog |
| PUB-31FF51F116 | `/changelog/pandora-sms-is-now-available-in-the-gateway-marketplace` | REQ-PUB-04 | Changelog |
| PUB-81A7C8D31F | `/changelog/passwordless-magic-link-login-on-the-tenant-login-page-subdomain` | REQ-PUB-04 | Changelog |
| PUB-7DF3A1B05A | `/changelog/past-dates-are-now-disabled-in-the-subscriber-expiry-date-picker-so-they-cant-be-selected-before-validation` | REQ-PUB-04 | Changelog |
| PUB-2B0216D1D6 | `/changelog/pawapay-availability-across-seven-additional-supported-countries-with-country-specific-currencies-and-mobile-money-provider-options` | REQ-PUB-04 | Changelog |
| PUB-EDF29497BB | `/changelog/pay-out-to-a-sacco-or-shared-paybill-kenya` | REQ-PUB-04 | Changelog |
| PUB-7D1BF0A907 | `/changelog/payer-phone-numbers-now-appear-in-the-dashboard-payment-feed-instead-of-hotspot-usernames` | REQ-PUB-04 | Changelog |
| PUB-9FCCA42945 | `/changelog/payfast-payment-gateway-for-south-african-tenants-with-payfast-set-to-live-mode` | REQ-PUB-04 | Changelog |
| PUB-F11E159608 | `/changelog/payments-page-scrolling-no-longer-jumps-back-to-the-top-while-reviewing-entries-and-invoices-now-include-the-hotspot-amount-with-separate-voucher-and-mobile-money-charges` | REQ-PUB-04 | Changelog |
| PUB-FB5F0F3511 | `/changelog/payments-refresh-automatically-when-new-payments-come-in-so-the-page-stays-up-to-date-without-a-manual-refresh` | REQ-PUB-04 | Changelog |
| PUB-3CF201F07C | `/changelog/paynamics-payment-gateway-support-for-philippine-isps-including-gcash-maya-and-card-payments` | REQ-PUB-04 | Changelog |
| PUB-A16F952B52 | `/changelog/paynow-payments-for-zimbabwe` | REQ-PUB-04 | Changelog |
| PUB-405115E787 | `/changelog/payout-details-in-the-payments-export` | REQ-PUB-04 | Changelog |
| PUB-CD4B6E1205 | `/changelog/payswitch-theteller-payment-gateway-for-ghana-supporting-mobile-money-and-card-payments-from-settings-payments` | REQ-PUB-04 | Changelog |
| PUB-0327A56E29 | `/changelog/phone-numbers-in-the-overview-renewals-due-list-now-hide-middle-digits-for-greater-privacy` | REQ-PUB-04 | Changelog |
| PUB-13ECAE45BC | `/changelog/platform-copilot-actions-are-restricted-to-platform-tools-impersonated-subscriber-portals-are-read-only-receipt-login-is-rate-limited-csv-exports-also-protect-against-formula-injection-and-wifi-passphrases-are-masked-after-in-app-actions` | REQ-PUB-04 | Changelog |
| PUB-998BBFCDD9 | `/changelog/pppoe-customers-now-see-only-pppoe-packages-in-their-dashboard-top-up-options` | REQ-PUB-04 | Changelog |
| PUB-4991EBFDAD | `/changelog/pppoe-package-assignment-for-converted-leads-direct-access-to-the-subscriber-form-after-conversion-and-corrected-at-days-left-rounding-in-reminder-sms` | REQ-PUB-04 | Changelog |
| PUB-1434EB1510 | `/changelog/pppoe-payments-are-now-classified-correctly-instead-of-showing-up-as-hotspot-payments-for-pppoe-users-paying-via-m-pesa` | REQ-PUB-04 | Changelog |
| PUB-D5400817F9 | `/changelog/pppoe-sign-in-with-username-and-password-instead-of-otp-plus-unique-account-numbers-and-usernames-across-subscribers` | REQ-PUB-04 | Changelog |
| PUB-D3D6858315 | `/changelog/pppoe-user-creation-now-includes-a-location-field-and-edited-or-expiry-changed-clients-keep-the-bandwidth-from-their-selected-package-instead-of-falling-back-to-1m1m` | REQ-PUB-04 | Changelog |
| PUB-295C27B3B6 | `/changelog/price-change-notices-now-reach-all-pppoe-subscribers-on-the-affected-plan-including-legacy-profile-based-subscribers-while-excluding-hotspot-subscribers` | REQ-PUB-04 | Changelog |
| PUB-6D99D6DA58 | `/changelog/price-change-sms-are-no-longer-sent-to-subscribers-scheduled-to-move-to-another-plan-while-subscribers-whose-scheduled-plan-was-deleted-still-receive-the-notice` | REQ-PUB-04 | Changelog |
| PUB-4539786E58 | `/changelog/price-change-sms-notices-now-exclude-subscribers-already-scheduled-to-move-to-another-plan` | REQ-PUB-04 | Changelog |
| PUB-015E135DBF | `/changelog/radius-reason-tooltips-on-active-sessions-with-meanings-shown-next-to-each-reason-on-the-user-profile` | REQ-PUB-04 | Changelog |
| PUB-805DBB1E74 | `/changelog/read-a-routers-live-log-on-its-page-and-ask-the-copilot-about-any-line` | REQ-PUB-04 | Changelog |
| PUB-3C3DB109B5 | `/changelog/readable-expense-categories-such-as-centipid-subscription-and-sms-across-tables-filters-chips-and-forms-app-generated-expense-types-are-filterable-and-read-only-when-edited-and-record-counts-use-correct-plurals` | REQ-PUB-04 | Changelog |
| PUB-8DF4834F52 | `/changelog/readable-lead-sources-and-stages-with-plan-prices-shown-in-the-isps-currency` | REQ-PUB-04 | Changelog |
| PUB-D5928F975D | `/changelog/readable-permission-labels-and-corrected-guidance-on-the-staff-roles-tab` | REQ-PUB-04 | Changelog |
| PUB-0AF59F4C38 | `/changelog/record-installation-fees-collected-on-site` | REQ-PUB-04 | Changelog |
| PUB-760F989560 | `/changelog/recurring-sms-and-email-campaigns-continue-scheduling-future-runs-after-a-failed-send-and-resaving-a-failed-campaign-with-a-future-date-makes-it-active-again` | REQ-PUB-04 | Changelog |
| PUB-5278B08043 | `/changelog/registration-no-longer-re-runs-dns-email-validation-at-the-final-isp-signup-step-which-could-block-clients-from-finishing-sign-up` | REQ-PUB-04 | Changelog |
| PUB-FEF168DB74 | `/changelog/reliable-fup-usage-resets-for-pppoe-clients-from-their-profiles` | REQ-PUB-04 | Changelog |
| PUB-9398B474B6 | `/changelog/rename-tr-069-devices-directly-from-the-device-page` | REQ-PUB-04 | Changelog |
| PUB-29E79550BE | `/changelog/renewal-expenses-for-non-kenyan-isps-are-now-recorded-in-the-isps-local-currency-using-the-payments-exchange-rate` | REQ-PUB-04 | Changelog |
| PUB-EB17FF98DD | `/changelog/restored-the-send-voucher-action-on-the-customer-profile-page-so-vouchers-can-be-generated-and-sent-from-the-profile-screen-again` | REQ-PUB-04 | Changelog |
| PUB-12DF921180 | `/changelog/revenue-dashboard-with-a-date-based-view-for-tracking-income-by-day-and-specific-dates` | REQ-PUB-04 | Changelog |
| PUB-879E8FB380 | `/changelog/role-based-controls-to-hide-subscriber-totals-from-technicians-and-other-staff` | REQ-PUB-04 | Changelog |
| PUB-8A0D30BA24 | `/changelog/router-based-filtering-for-payments-active-users-and-all-users-including-mikrotik` | REQ-PUB-04 | Changelog |
| PUB-F1051A5FF7 | `/changelog/router-deletion-when-ip-bindings-are-missing` | REQ-PUB-04 | Changelog |
| PUB-5E8F6A678C | `/changelog/scheduled-packages-now-appear-at-their-correct-times` | REQ-PUB-04 | Changelog |
| PUB-366CE57362 | `/changelog/searchable-portal-template-selectors-with-readable-theme-names-plus-design-system-colours-for-router-health-warnings` | REQ-PUB-04 | Changelog |
| PUB-C5E88FDC69 | `/changelog/segment-recipient-counts-now-show-correctly-instead-of-always-returning-0` | REQ-PUB-04 | Changelog |
| PUB-F4A2A48108 | `/changelog/separate-todays-pppoe-hotspot-and-total-revenue-on-the-overview-dashboard` | REQ-PUB-04 | Changelog |
| PUB-A9E1AAF029 | `/changelog/single-month-payment-and-daily-collections-drill-down-for-each-mikrotik-router` | REQ-PUB-04 | Changelog |
| PUB-A32475DFCE | `/changelog/sla-breach-alerts-for-support-tickets-notifying-the-assignee-or-isp-admins-when-unassigned` | REQ-PUB-04 | Changelog |
| PUB-0517A6FACD | `/changelog/smartolt-integration-for-onu-setup-with-vlan-based-configuration-available-to-more-isps` | REQ-PUB-04 | Changelog |
| PUB-5C1DF405FF | `/changelog/sms-sent-through-africas-talking-now-use-the-correct-country-prefix-and-report-rejected-messages-accurately-instead-of-marking-them-as-delivered` | REQ-PUB-04 | Changelog |
| PUB-333F2630EE | `/changelog/staff-can-now-get-an-sms-when-a-support-ticket-is-assigned-to-them-turn-it-on-under-settings-operator-alerts` | REQ-PUB-04 | Changelog |
| PUB-8B4FDD6DBD | `/changelog/subscriber-imports-now-create-new-subscribers-when-only-the-phone-number-matches-an-existing-subscriber-without-overwriting-the-existing-account` | REQ-PUB-04 | Changelog |
| PUB-C7CC5C7B33 | `/changelog/subscriber-imports-now-create-new-subscribers-when-only-the-phone-number-matches-an-existing-subscriber-without-overwriting-the-existing-record` | REQ-PUB-04 | Changelog |
| PUB-D81DDF69A0 | `/changelog/subscriber-invoices-and-payments-can-sync-with-zoho-books` | REQ-PUB-04 | Changelog |
| PUB-8A68CD642F | `/changelog/subscriber-list-empty-states-now-offer-relevant-actions-phone-tap-targets-meet-the-44px-minimum-colors-use-design-tokens-and-silent-activity-text-handles-singular-days` | REQ-PUB-04 | Changelog |
| PUB-9B3CAA32CC | `/changelog/subscriber-reference-panel-now-includes-locationaddress-bridge-ports-are-shown-in-an-accordion-and-the-print-dropdown-handles-collisions-more-gracefully` | REQ-PUB-04 | Changelog |
| PUB-1795C69359 | `/changelog/subscriber-wifi-changes-now-wake-the-router-promptly-instead-of-waiting-for-the-next-scheduled-check-in` | REQ-PUB-04 | Changelog |
| PUB-0C75A11D30 | `/changelog/subscribers-remain-attached-when-their-plan-is-renamed-preserving-subscriber-counts-and-ensuring-future-plan-speed-burst-and-device-changes-reach-their-routers` | REQ-PUB-04 | Changelog |
| PUB-EDAEAE02D4 | `/changelog/support-phone-numbers-displayed-as-text-on-every-captive-portal-theme-making-help-contacts-easier-to-find` | REQ-PUB-04 | Changelog |
| PUB-FBF8D55165 | `/changelog/taifa-mobile-as-an-sms-gateway-option-for-isp-notifications` | REQ-PUB-04 | Changelog |
| PUB-F27F3361D4 | `/changelog/tanzania-sms-kibonet-added-nextsms-now-actually-delivers` | REQ-PUB-04 | Changelog |
| PUB-FC10A6DA95 | `/changelog/tanzanian-isps-can-now-collect-payments-with-pluspesa-which-sends-subscribers-a-pin-prompt-for-m-pesa-tigo-pesa-airtel-money-halopesa-or-azampesa` | REQ-PUB-04 | Changelog |
| PUB-9730EB8560 | `/changelog/termii-sms-gateway-support-for-nigeria-including-gateway-selection-from-the-billing-settings` | REQ-PUB-04 | Changelog |
| PUB-B9039A15EA | `/changelog/the-network-copilot-can-now-make-changes-for-you-answer-more-questions-and-answers-them-better` | REQ-PUB-04 | Changelog |
| PUB-98CCA3A282 | `/changelog/top-customers-value-rank-list-in-analytics` | REQ-PUB-04 | Changelog |
| PUB-EE18CB8BC0 | `/changelog/tr-069-commands-such-as-reboots-and-wifi-changes-now-reach-devices-within-seconds-where-the-network-allows-it-instead-of-waiting-up-to-5-minutes-for-the-next-check-in` | REQ-PUB-04 | Changelog |
| PUB-A7EDF09569 | `/changelog/tr-069-device-lists-no-longer-expose-router-credentials-or-customers-home-network-data-to-read-only-staff` | REQ-PUB-04 | Changelog |
| PUB-CE8E4D61BA | `/changelog/tv-remote-navigation-reaching-the-paybill-card-and-device-details-below-the-pay-button` | REQ-PUB-04 | Changelog |
| PUB-6F49297C2B | `/changelog/ugandan-iotec-collections-now-record-using-the-caller-generated-external-id-and-requested-amount-and-treat-senttovendor-as-an-accepted-collect-status-so-payments-arent-left-unrecorded` | REQ-PUB-04 | Changelog |
| PUB-12BE22B5BA | `/changelog/unused-isp-referral-discounts-now-carry-over-to-the-next-invoice-with-clearer-pending-discount-amounts-and-capped-savings` | REQ-PUB-04 | Changelog |
| PUB-8CE324411C | `/changelog/updated-symfony-mime-dependencies-across-php-repositories-to-address-cve-2026-45070-reducing-exposure-to-email-header-injection` | REQ-PUB-04 | Changelog |
| PUB-C9334B0E20 | `/changelog/voucher-balance-lookup-for-hotspot-users-including-current-data-usage-and-remaining-time` | REQ-PUB-04 | Changelog |
| PUB-9C9C8C2998 | `/changelog/voucher-print-pdfs-remain-accessible-when-the-live-connection-drops-or-the-ready-notification-is-missed-with-status-fallback-and-recent-download-links` | REQ-PUB-04 | Changelog |
| PUB-FE053BA7DC | `/changelog/voucher-records-now-show-which-staff-member-created-each-voucher` | REQ-PUB-04 | Changelog |
| PUB-A607554398 | `/changelog/whatchimp-as-a-whatsapp-gateway-providing-an-affordable-meta-verified-messaging-option` | REQ-PUB-04 | Changelog |
| PUB-F50B91D934 | `/changelog/winsms-support-as-an-sms-gateway-for-south-african-isps-including-gateway-listing-and-integration` | REQ-PUB-04 | Changelog |
| PUB-BFFB9EF11C | `/changelog/wireguard-tunnel-ips-are-now-released-when-a-nas-is-deleted-preventing-reused-addresses-from-colliding-with-another-peer` | REQ-PUB-04 | Changelog |
| PUB-2D8646881D | `/changelog/xendit-payment-gateway-support-for-isps-in-the-philippines-indonesia-thailand-malaysia-and-vietnam-including-local-payment-methods-such-as-gcash-maya-promptpay-fpx-cards-and-indonesian-e-wallets` | REQ-PUB-04 | Changelog |
| PUB-8281638077 | `/changelog/you-can-now-upload-firmware-for-a-router-model-and-push-it-to-one-tr-069-router-or-many-at-once-and-see-each-routers-new-version-when-it-finishes` | REQ-PUB-04 | Changelog |
| PUB-1D7118CACD | `/cidr-calculator` | REQ-PUB-06 | Tool/calculator |
| PUB-4EB9506365 | `/contact` | REQ-PUB-07 | Legal/contact/affiliate |
| PUB-B3570E32B8 | `/cote-divoire` | REQ-PUB-05 | Country/market |
| PUB-37F4A77914 | `/countries` | REQ-PUB-05 | Country/market |
| PUB-188E8FAFC3 | `/data-converter` | REQ-PUB-06 | Tool/calculator |
| PUB-4435A2E8BB | `/dbm-to-watt-converter` | REQ-PUB-06 | Tool/calculator |
| PUB-AC0A478254 | `/dns-lookup` | REQ-PUB-06 | Tool/calculator |
| PUB-0322E2E15D | `/dr-congo` | REQ-PUB-05 | Country/market |
| PUB-60464E7155 | `/egypt` | REQ-PUB-05 | Country/market |
| PUB-DE33593435 | `/features` | REQ-PUB-02 | Feature |
| PUB-A9CF7E58BA | `/features/affiliate-referrals` | REQ-PUB-02 | Feature |
| PUB-8246D41C89 | `/features/ai-assistant` | REQ-PUB-02 | Feature |
| PUB-73F1A8D2A9 | `/features/analytics-reports` | REQ-PUB-02 | Feature |
| PUB-CA2964189E | `/features/audit-logs-changelog` | REQ-PUB-02 | Feature |
| PUB-DA8C1C05BD | `/features/automated-payment-collection` | REQ-PUB-02 | Feature |
| PUB-E9731209F5 | `/features/captive-portal-promotions` | REQ-PUB-02 | Feature |
| PUB-C5AF81D015 | `/features/collections-withdrawals` | REQ-PUB-02 | Feature |
| PUB-CD52044BCE | `/features/custom-captive-portals` | REQ-PUB-02 | Feature |
| PUB-879BD6B1D3 | `/features/equipment-inventory` | REQ-PUB-02 | Feature |
| PUB-14E6F02C85 | `/features/expense-management` | REQ-PUB-02 | Feature |
| PUB-2D7086F0AD | `/features/fiber-map` | REQ-PUB-02 | Feature |
| PUB-DAABAE451E | `/features/ip-mac-bindings` | REQ-PUB-02 | Feature |
| PUB-F1EAE965C7 | `/features/leads-sales-pipeline` | REQ-PUB-02 | Feature |
| PUB-C9C8B47B6D | `/features/live-sessions-usage` | REQ-PUB-02 | Feature |
| PUB-CA8CA2A498 | `/features/mcp-integrations` | REQ-PUB-02 | Feature |
| PUB-4813BE5041 | `/features/mikrotik-provisioning` | REQ-PUB-02 | Feature |
| PUB-4FDA92150E | `/features/multi-branch-operations` | REQ-PUB-02 | Feature |
| PUB-DB52FCEED2 | `/features/notifications-reminders` | REQ-PUB-02 | Feature |
| PUB-8A2DC017C0 | `/features/package-management` | REQ-PUB-02 | Feature |
| PUB-0D1F0D5403 | `/features/payment-reconciliation` | REQ-PUB-02 | Feature |
| PUB-9CF672A490 | `/features/radius-authentication` | REQ-PUB-02 | Feature |
| PUB-5F119FF356 | `/features/recurring-invoicing` | REQ-PUB-02 | Feature |
| PUB-908AF46A25 | `/features/remote-winbox-access` | REQ-PUB-02 | Feature |
| PUB-B9E1F5B02D | `/features/revenue-reporting` | REQ-PUB-02 | Feature |
| PUB-188175CE23 | `/features/router-backups` | REQ-PUB-02 | Feature |
| PUB-30403AEAC3 | `/features/router-monitoring-alerts` | REQ-PUB-02 | Feature |
| PUB-ABD25B54DF | `/features/search-data-exports` | REQ-PUB-02 | Feature |
| PUB-DB80890DE0 | `/features/staff-roles-permissions` | REQ-PUB-02 | Feature |
| PUB-6E079EA82E | `/features/subscriber-communications` | REQ-PUB-02 | Feature |
| PUB-FF3C8B6ABC | `/features/subscriber-imports` | REQ-PUB-02 | Feature |
| PUB-5A985B1499 | `/features/subscriber-management` | REQ-PUB-02 | Feature |
| PUB-E92A5F0FD3 | `/features/subscriber-portal` | REQ-PUB-02 | Feature |
| PUB-097DAFB6AF | `/features/support-tickets` | REQ-PUB-02 | Feature |
| PUB-00B86CF99B | `/features/tr069-device-management` | REQ-PUB-02 | Feature |
| PUB-5E7D94B9AF | `/features/vouchers-agent-sales` | REQ-PUB-02 | Feature |
| PUB-DEA08A42DF | `/free-isp-billing-software` | REQ-PUB-01 | Core public |
| PUB-B39F81A2CF | `/freeradius-alternative` | REQ-PUB-01 | Core public |
| PUB-0767134D9F | `/ghana` | REQ-PUB-05 | Country/market |
| PUB-2E43152F05 | `/gpon-splitter-calculator` | REQ-PUB-06 | Tool/calculator |
| PUB-399104F2E9 | `/india` | REQ-PUB-05 | Country/market |
| PUB-1B4055599E | `/integrations` | REQ-PUB-04 | Integrations |
| PUB-509FDB8F03 | `/internet-billing-system` | REQ-PUB-01 | Core public |
| PUB-C4BEB0E349 | `/ipv6-subnet-calculator` | REQ-PUB-06 | Tool/calculator |
| PUB-04FD5B1DF2 | `/kenya` | REQ-PUB-05 | Country/market |
| PUB-9B581CCA60 | `/legal/affiliate-policy` | REQ-PUB-07 | Legal/contact/affiliate |
| PUB-2265FAF06E | `/link-budget-calculator` | REQ-PUB-06 | Tool/calculator |
| PUB-1BAE0DB629 | `/mac-address-lookup` | REQ-PUB-06 | Tool/calculator |
| PUB-AD139E279F | `/malawi` | REQ-PUB-05 | Country/market |
| PUB-23DCC3922E | `/mikrotik-router-monitoring` | REQ-PUB-01 | Core public |
| PUB-9F5E9C7BFB | `/mnsp-licence` | REQ-PUB-01 | Core public |
| PUB-8395D7E113 | `/nigeria` | REQ-PUB-05 | Country/market |
| PUB-98DAEB9652 | `/pakistan` | REQ-PUB-05 | Country/market |
| PUB-7394A2BB76 | `/pricing` | REQ-PUB-01 | Core public |
| PUB-0ECE7F7C30 | `/privacy` | REQ-PUB-07 | Legal/contact/affiliate |
| PUB-2686ACEE1D | `/republic-of-the-congo` | REQ-PUB-05 | Country/market |
| PUB-3127CE2CC6 | `/rwanda` | REQ-PUB-05 | Country/market |
| PUB-4B601221EC | `/senegal` | REQ-PUB-05 | Country/market |
| PUB-F7176092D1 | `/sierra-leone` | REQ-PUB-05 | Country/market |
| PUB-D3E0DF4C32 | `/sla-uptime-calculator` | REQ-PUB-06 | Tool/calculator |
| PUB-74A05F0A70 | `/solutions` | REQ-PUB-03 | Solution/comparison |
| PUB-4D27957905 | `/solutions/hotspot` | REQ-PUB-03 | Solution/comparison |
| PUB-CF0D5FE84D | `/solutions/pppoe` | REQ-PUB-03 | Solution/comparison |
| PUB-8858795B79 | `/solutions/vouchers` | REQ-PUB-03 | Solution/comparison |
| PUB-B72D599EC6 | `/south-africa` | REQ-PUB-05 | Country/market |
| PUB-3C05A5B60E | `/speedtest` | REQ-PUB-06 | Tool/calculator |
| PUB-67FB29D0D5 | `/subnet-calculator` | REQ-PUB-06 | Tool/calculator |
| PUB-59E6D69F6A | `/tanzania` | REQ-PUB-05 | Country/market |
| PUB-2DDA5C6B8E | `/terms` | REQ-PUB-07 | Legal/contact/affiliate |
| PUB-D823ED01B4 | `/thailand` | REQ-PUB-05 | Country/market |
| PUB-1491D206C3 | `/tools` | REQ-PUB-06 | Tool/calculator |
| PUB-8A8A4094D3 | `/tower-revenue-calculator` | REQ-PUB-06 | Tool/calculator |
| PUB-9CA9D9C72D | `/uganda` | REQ-PUB-05 | Country/market |
| PUB-FA9FF8FB6B | `/ups-runtime-calculator` | REQ-PUB-06 | Tool/calculator |
| PUB-3C85FA526D | `/vs/daloradius` | REQ-PUB-03 | Solution/comparison |
| PUB-C08815EABE | `/vs/mikhmon` | REQ-PUB-03 | Solution/comparison |
| PUB-826CBE7A69 | `/vs/phpnuxbill` | REQ-PUB-03 | Solution/comparison |
| PUB-53B0FB5ECA | `/vs/splynx` | REQ-PUB-03 | Solution/comparison |
| PUB-B563453706 | `/what-is-my-ip` | REQ-PUB-06 | Tool/calculator |
| PUB-B35D4B0C27 | `/zambia` | REQ-PUB-05 | Country/market |
| PUB-8A604F8A8D | `/zimbabwe` | REQ-PUB-05 | Country/market |

## Tenant reference route/view inventory

These 73 entries are sanitized observations from the local-only tenant reference manifest. Numeric/opaque route identifiers are replaced with `:id`; a small set of state labels (theme, tab, external reference) is kept because it describes the captured view, not an account. The external router console is explicitly excluded from the MylesNet admin surface; `/accounts` is retained as an observed 404 gap, not accepted route behavior. Stable IDs are the first 10 uppercase hexadecimal characters of SHA-256 over the exact sanitized descriptor.

| Route ID | Sanitized route/view descriptor | Requirement / classification |
|---|---|---|
| TEN-F25AC57297 | `/accounts` | Observed 404; not an accepted product route |
| TEN-6D06B3167A | `/active-users` | REQ-TEN-05 |
| TEN-48E47B4B0A | `/agents` | REQ-TEN-08 |
| TEN-CD157B6E2B | `/analytics` | REQ-TEN-09 |
| TEN-547B82221D | `/audits` | REQ-TEN-13 |
| TEN-D977833ED7 | `/campaigns` | REQ-TEN-08 |
| TEN-2D5CDBC894 | `/campaigns/create` | REQ-TEN-08 |
| TEN-9433DB80E9 | `/changelog` | REQ-TEN-13 |
| TEN-D1510E1C66 | `/cpe` | REQ-TEN-06 |
| TEN-6A3716A6AA | `/cpe/firmware` | REQ-TEN-06 |
| TEN-89347BB23A | `/dashboard` | REQ-TEN-01 |
| TEN-6B0718C4E4 | `/equipment` | REQ-TEN-06 |
| TEN-D8F2200F9B | `/equipment/create` | REQ-TEN-06 |
| TEN-ED86B41BB8 | `/expenses` | REQ-TEN-07 |
| TEN-4657BABCB7 | `/expenses/create` | REQ-TEN-07 |
| TEN-F70EDE6B71 | `/feedback-board` | REQ-TEN-13 |
| TEN-E5201E16EA | `/fiber/map` | REQ-TEN-06 |
| TEN-34858BDB2E | `/invoices` | REQ-TEN-07 |
| TEN-B8B637D9B6 | `/invoices/create` | REQ-TEN-07 |
| TEN-67C9231D54 | `/leads` | REQ-TEN-03 |
| TEN-0800246FF1 | `/leads/import-export` | REQ-TEN-03 |
| TEN-44130A1AC5 | `/loyalty-points` | REQ-TEN-07 |
| TEN-2BADF9B70C | `/nas` | REQ-TEN-05 |
| TEN-CC2354B71D | `/nas/:id` | REQ-TEN-05 |
| TEN-822EC53888 | `/nas/:id/web-console (external RouterOS login page)` | External RouterOS reference; excluded from the MylesNet admin surface |
| TEN-1A693EBD7A | `/nas/create` | REQ-TEN-05 |
| TEN-8577F46906 | `/network-map` | REQ-TEN-05 |
| TEN-2462A17D8B | `/outage-credits` | REQ-TEN-07 |
| TEN-02FE33ADB3 | `/packages` | REQ-TEN-05 |
| TEN-2FE75FF523 | `/packages/create` | REQ-TEN-05 |
| TEN-2686BDB4A6 | `/payments` | REQ-TEN-07 |
| TEN-D203EECEBE | `/payments/create` | REQ-TEN-07 |
| TEN-5FF473FC8F | `/payments/import-export` | REQ-TEN-07 |
| TEN-5C28AB56D1 | `/referrals` | REQ-TEN-13 |
| TEN-82C10F4691 | `/settings/ai` | REQ-TEN-11 |
| TEN-D4D3BB42C0 | `/settings/branding` | REQ-TEN-11 |
| TEN-E8A3FDC61F | `/settings/communications/email` | REQ-TEN-11 |
| TEN-50C2ED70D6 | `/settings/communications/sms` | REQ-TEN-11 |
| TEN-4F4913C8DC | `/settings/communications/whatsapp` | REQ-TEN-11 |
| TEN-95044E86BF | `/settings/developer` | REQ-TEN-11 |
| TEN-0BAF13392E | `/settings/domain` | REQ-TEN-11 |
| TEN-5DEB775AB6 | `/settings/integrations/accounting` | REQ-TEN-11 |
| TEN-C8F8150FE2 | `/settings/loyalty` | REQ-TEN-11 |
| TEN-EC7E16218E | `/settings/message-templates` | REQ-TEN-11 |
| TEN-4F9D6282FA | `/settings/network/hotspot` | REQ-TEN-11 |
| TEN-C13C5D7818 | `/settings/network/pppoe` | REQ-TEN-11 |
| TEN-56D1AE4AD4 | `/settings/notifications` | REQ-TEN-11 |
| TEN-158A90ABA6 | `/settings/payments` | REQ-TEN-11 |
| TEN-7BADE669E3 | `/settings/profile` | REQ-TEN-11 |
| TEN-5251FC7DCA | `/settings/security` | REQ-TEN-11 |
| TEN-5B5805E6CA | `/sms` | REQ-TEN-08 |
| TEN-8E2A2EA668 | `/sms/campaigns` | REQ-TEN-08 |
| TEN-52E37E6BEA | `/sms/campaigns/create` | REQ-TEN-08 |
| TEN-8DCED01964 | `/sms/create` | REQ-TEN-08 |
| TEN-D898A7755E | `/staff` | REQ-TEN-10 |
| TEN-ADA31E6D06 | `/staff (Roles tab)` | REQ-TEN-10 |
| TEN-88BBDCE1C1 | `/subscription` | REQ-TEN-12 |
| TEN-8377FBF08C | `/tickets` | REQ-TEN-04 |
| TEN-F6B5A0CD45 | `/tickets/analytics` | REQ-TEN-04 |
| TEN-D78B460CB8 | `/tickets/create` | REQ-TEN-04 |
| TEN-CBFABFF0A5 | `/tickets/import-export` | REQ-TEN-04 |
| TEN-954DAC294D | `/users` | REQ-TEN-02 |
| TEN-9FC3A00AA3 | `/users/compensate` | REQ-TEN-02 |
| TEN-1C345B4EA8 | `/users/create` | REQ-TEN-02 |
| TEN-7A42147C2B | `/vouchers` | REQ-TEN-08 |
| TEN-596E2853CD | `/vouchers/batches` | REQ-TEN-08 |
| TEN-4BAD30E379 | `/vouchers/sales` | REQ-TEN-08 |
| TEN-DD94229CC7 | `/vouchers/templates` | REQ-TEN-08 |
| TEN-4AEF7CB9B7 | `/whatsapp` | REQ-TEN-08 |
| TEN-22B595A4C5 | `/whatsapp/create` | REQ-TEN-08 |
| TEN-EDE21C974E | `dashboard theme: dark` | REQ-TEN-01 — dashboard presentation state |
| TEN-8060C1647C | `dashboard theme: light` | REQ-TEN-01 — dashboard presentation state |
| TEN-2A982E9FA5 | `dashboard theme: system` | REQ-TEN-01 — dashboard presentation state |

## Open decisions and implementation boundary

- **MN-DEC-016:** exact resource-to-branch assignments, shared-resource behavior, tenant-wide/unassigned semantics, indexes, and migration remain open. This catalog states the enforcement boundary, not a schema shortcut.
- **MN-DEC-020:** CMS/editorial workflow and content owners remain open; public URL coverage stays static/structured absent an approved editing need. Subscriber portal host and exact navigation also remain open.
- Platform route names/grouping and exact page-level permission bundles are proposals until the architecture/route ADRs and detailed role matrix are accepted. Charter role boundaries, MFA, support grants, tenant isolation, and high-risk controls are applied as approved constraints.
- Exact plan names/prices/quotas/taxes; country legal/provider approvals; retention periods; DNS registrar; external integration contracts; detailed branch mapping; and RADIUS lab coverage remain open in the decision register. This catalog does not choose them.
- The catalog is a planning contract and does not authorize product code, live provider/router connections, tenant provisioning, or deployment.

## Source references

- [`AGENTS.md`](../AGENTS.md) — project charter, roles, tenant isolation, financial/network invariants, and planning-only boundary.
- [`IMPLEMENTATION-PLAN.md`](IMPLEMENTATION-PLAN.md) §5 Phase 0, work item 4 and its exit gate.
- [`EXECUTION-KICKOFF-PLAN.md`](EXECUTION-KICKOFF-PLAN.md) — MN-PLAN-004 acceptance criteria and dependencies.
- [`DECISION-REGISTER.md`](DECISION-REGISTER.md) — approved/candidate/open decisions, especially MN-DEC-016 and MN-DEC-020.
- [`TECHNICAL-SPECIFICATION.md`](TECHNICAL-SPECIFICATION.md) §§5–6, 8 — surface requirements, CRUD/RBAC, approvals, data ownership.
- [`CORE-FLOWS-AND-DATA.md`](CORE-FLOWS-AND-DATA.md) — signup, tenant, subscriber, RADIUS, captive, finance and domain flows.
- [`ROUTES-AND-ACCESS.md`](ROUTES-AND-ACCESS.md) — observed tenant routes, proposed platform groups, subscriber/captive route families, and access rules.
- [`public-site-review.md`](public-site-review.md) and [`tenant-console-review.md`](tenant-console-review.md) — sanitized reference-review summaries and inventory counts.
- [`READINESS-DECISIONS.md`](READINESS-DECISIONS.md) — approved lifecycle, identity, financial, and network decisions plus open evidence gates.
