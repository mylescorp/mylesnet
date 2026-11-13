# Phase 0 Country Legal and Data-Protection Evidence Plan

**Task:** MN-PLAN-020 · **Plan phase:** Phase 0 — close planning and turn decisions into build contracts<br>
**Date:** 12 November 2026<br>
**Status:** Evidence request and review plan; MN-DEC-018 remains open.<br>
**Scope:** Kenya, Uganda, and Tanzania; privacy roles, data transfers, retention, notices, rights handling, and incident obligations.

## Purpose and boundary

Turn the open country privacy, transfer, and retention gate into a reviewable request for qualified local counsel and the project owners. This document is a planning checklist, not legal advice, a compliance determination, a transfer approval, or permission to process production personal data. It does not decide pooled collections, payment licensing, tax, commercial terms, or provider selection; those remain in their existing country and payment decision work.

The official source inventory below was checked on 12 November 2026. Counsel must verify current consolidated law, amendments, commencement, regulator directions, applicability, and required filings at the time of advice. A law or regulator page does not establish MylesNet's compliance or the applicability of any rule to a particular data flow.

## Phase 0 acceptance criteria

This task contributes to Implementation Plan Phase 0 work items 5–7. The deliverable is a preparation packet: it must identify current official source starting points, cover the relevant data flows and retention classes, provide a country-counsel questionnaire and sign-off record, and preserve unresolved decisions as open. The packet itself does not satisfy the later country legal or Phase 0 exit gate.

- The official source inventory, source review date, and revalidation limits are recorded for Kenya, Uganda, and Tanzania.
- The product data-flow inventory, country-counsel questionnaire, complete retention-class mapping, and sign-off record are present.
- The plan distinguishes source observations from legal conclusions, does not invent roles, mechanisms, periods, deadlines, or approvals, and links the Phase 0 and country gates.

The later MN-DEC-018 and country-readiness gate requires named qualified local counsel and accountable privacy/data owners; written current advice for each country; approved roles, filings, transfers, retention, notices, rights and incident workflows; and separately signed country go/no-go evidence. Those external decisions remain open after this packet is delivered.

## MylesNet facts counsel should assess

Use the approved architecture as planning facts, while keeping unvalidated items marked as candidates:

- A tenant can operate across Kenya, Uganda, and Tanzania. Branch, legal-entity, and subscriber records may be connected across those locations.
- WorkOS AuthKit is the staff-identity provider; internal membership and authorization records remain authoritative.
- Convex is the shared SaaS application backend direction. Convex Europe (Ireland) is a leading managed-region candidate, and Vercel Dublin (`dub1`) is a leading function-region candidate. Neither is a selected production region or residency claim.
- FreeRADIUS with a separate PostgreSQL AAA store is the network-access direction. Its hosting and operations boundary still needs country and region review.
- Email, SMS, payment, storage, and support providers are not all selected. Provider processing locations, subprocessors, remote support access, backups, and onward transfers must be confirmed once the provider inventory is available.
- The platform must support purpose-limited collection, access and export controls, country/category retention, legal holds, and deletion or anonymization after the lawful retention window. Exact periods remain open.

Counsel should assess the actual data-flow map, contracts, system locations, and access paths before approving any conclusion. “Stored in a region” and “accessed or processed from a region” must be mapped separately for review.

## Official source inventory

These links are starting points for counsel, not exhaustive or conclusive authority. Verify the latest consolidated texts, amendments, subsidiary legislation, regulator notices, and any sector-specific rules.

| Country | Primary law and regulator sources | Source signal for counsel to verify |
|---|---|---|
| Kenya | [Data Protection Act, Cap. 411C — Kenya Law](https://new.kenyalaw.org/akn/ke/act/2019/24/eng%402022-12-31/source.pdf); [Data Protection (General) Regulations, 2021 — Kenya Law](https://new.kenyalaw.org/akn/ke/act/ln/2021/263/eng%402022-01-14/source) | The Act includes Part VI on transfers outside Kenya; the General Regulations include Part VII on such transfers. Counsel must determine which route, safeguards, records, consent, filings, and onward-transfer controls apply to each MylesNet flow. |
| Uganda | [Data Protection and Privacy Act, Cap. 97 — NITA-U laws and regulations](https://www.nita.go.ug/laws-and-regulations); [Data Protection and Privacy Regulations, 2021 — Personal Data Protection Office](https://pdpo.go.ug/media/2022/03/Data_Protection_and_Privacy_Regulations-2021.pdf); [PDPO official information center](https://pdpo.go.ug/knowledge-center) | Regulation 30 addresses processing or storage of personal data outside Uganda. Counsel must verify current adequacy notices, proof or consent requirements, onward processing, registration, and later regulatory guidance. |
| Tanzania | [Personal Data Protection Act, 2022 — PDPC](https://pdpc.go.tz/the-personal-data-protection-act-2022/); [Personal Data Collection and Processing Regulations, 2023 — PDPC](https://pdpc.go.tz/publications/the-personal-data-protection-personal-data-collection-and-processing-regulations-2023/); [Cross-border data transfer permit service — PDPC](https://pdpc.go.tz/services/cross-border-data-transfer-permit/); [PDPC about the Act and Commission](https://pdpc.go.tz/about-us/) | The PDPC publishes a cross-border permit service. Counsel must confirm the statutory basis, jurisdictional scope, permit process and conditions, including whether any distinct Zanzibar analysis is required for planned operations. |

The references were accessed on 12 November 2026. Uganda's PDPO information center and Tanzania's PDPC site publish notices and resources; recheck them when counsel is appointed and before country approval. Kenya Law and regulator publications should likewise be rechecked for amendments and current notices.

## Data-flow inventory for legal review

The product data map is in [`DATA-OWNERSHIP-AND-CLASSIFICATION.md`](DATA-OWNERSHIP-AND-CLASSIFICATION.md). Confirm or expand this inventory with architecture and provider owners; do not infer that a provider location or role is settled.

| Flow | Data and purpose to map | Required counsel determination |
|---|---|---|
| Public signup, inquiry, and marketing | Owner business contact, account identity, country/branch, consent and referral source; marketing lead data | Controller/processor roles, lawful grounds and notices, marketing consent/objection, retention, registration, and whether any location or local-presence rule applies. |
| Staff identity and tenant administration | WorkOS identity, membership, role/scope, recovery, sign-in and security audit data | Roles of MylesNet, tenant, and identity provider; required identity/transfer notice, access rights, security reporting, retention, and cross-border support access. |
| ISP subscriber and service records | Names/contact details, service account, entitlement, usage, vouchers, consent, support cases and portal activity | Whether ISP and MylesNet roles vary by purpose; tenant instructions, direct requests, lawful basis, sensitive/minor data handling if relevant, sector rules, retention, and deletion/export duties. |
| AAA and network operations | Authentication/accounting records, NAS/router metadata, session identifiers, access/security events | Local telecom or communications confidentiality rules, data localization/retention duties, lawful access requests, breach handling, and separation from SaaS records. |
| Payments and financial audit | Transaction references, settlement/reconciliation, invoices, refunds, credits and audit history; no raw card credentials | Controller/processor roles, privacy disclosures, financial-record retention, data-subject limits/exception handling, and interaction with legal holds. Payment licensing and pooled-collection approval are tracked separately. |
| Messaging and delivery | Contact address/phone, consent/suppression state, service notice or campaign metadata, delivery outcome | Required service versus marketing distinction, consent/objection, STOP handling, provider transfers/subprocessors, message-content minimization, and retention. |
| Support, security telemetry, exports, and backups | Scoped support session, redacted logs/traces, export files, incident evidence, backup copies | Operator access from each country, transfer status, log/backup retention, privilege and notice requirements, export access/deletion propagation, and incident records. |

For each flow record: data subjects; purposes; fields/categories; source; controller and processor; system and physical location; access locations; each recipient/subprocessor; retention class; legal basis; disclosure; transfer mechanism; onward-transfer rules; security controls; rights-request owner; and evidence link. Include third-party access and disaster-recovery copies, not only primary database location.

## Country-counsel questionnaire

Ask counsel to answer each item in writing for each country and identify the exact legal source, section/regulation, regulator guidance, assumption, and date relied on.

1. **Scope and entity roles:** Which country laws apply to MylesNet, each ISP tenant, and each provider for each flow? How do citizenship/residence, branch/entity establishment, target users, and processing locations affect scope? Determine when each party is a controller, joint controller, processor, or another role. Identify required registration, DPO/representative, local contact, periodic report, and renewal obligations.
2. **Data transfer and location:** Which operations count as a cross-border transfer or overseas processing, including remote operator access, provider support, telemetry, backups, email/SMS, and onward subprocessors? For every planned destination (including Ireland/Dublin candidates), specify the permissible mechanism, assessment, contract terms, consent/notice or regulator approval, permit/registration, evidence, validity period, and change trigger. Identify any category or sector that must remain local.
3. **Retention and deletion:** For every `RET-*` class below, identify mandatory minimum/maximum periods, applicable legal basis and source, start/reset event, required form (identifiable, pseudonymized, aggregate), deletion or anonymization outcome, legal-hold exception, and backup/archive expiry. Resolve conflicts across multiple countries/legal entities and distinguish platform, ISP, financial, communications, AAA, audit, and security records.
4. **Privacy notice and data-subject rights:** Specify required notice timing/content/language, lawful basis and consent form, rights, identity verification, response/extension deadlines, fee or refusal grounds if any, complaint route, and controller/processor cooperation. Define which party responds when a subscriber contacts MylesNet directly.
5. **Security incidents and assessments:** Define when an event is a reportable breach, who must notify which regulator/data subjects/tenants, time limits and required content, evidence retention, cross-border coordination, and when a DPIA or regulator consultation is required. Confirm how tenant notification fits MylesNet's incident runbook.
6. **Telecommunications and children:** Determine whether ISP, RADIUS, subscriber traffic/session metadata, communications secrecy, lawful interception, telecom licensing, or records-access laws add requirements beyond general data protection. Confirm whether minors may use subscriber portals and the appropriate controls, notices, and consent.
7. **Contract and design implications:** Provide approved clauses and required product behavior for tenant data-processing terms, provider/subprocessor agreements, data export/deletion, support access, breach notification, localization, audit, and regulator cooperation. Identify any architecture or service that must change before processing.

Do not treat consent as the default legal basis or as a substitute for transfer review. Counsel must evaluate the applicable lawful grounds and confirm whether a proposed transfer route is available for the specific data and purpose.

## Retention classes to resolve

For each row, counsel and the data owner must complete the required period, purpose/legal basis, trigger, final disposition, hold rule, backup expiry, evidence, approver, and review date. Do not replace `Open` with an estimate.

| Existing class | Records in scope | Required review |
|---|---|---|
| `RET-IAM` | Identity, membership, authentication, recovery | Separate operational identity lifetime, access revocation, and security/audit history. |
| `RET-SUBSCRIBER` | Subscriber profile, service, and contact data | Identify active-service, cancellation, dispute, and renewal triggers; reconcile export and deletion obligations. |
| `RET-FINANCIAL` | Platform/tenant ledgers, invoices, refunds, tax and reconciliation | Obtain country/legal-entity requirements and immutable-history exceptions; coordinate with MN-DEC-015 without setting rates or tax policy here. |
| `RET-RADIUS` / `RET-NETWORK` | AAA, sessions, router/NAS configuration, and network jobs | Separate credential reference, authentication/accounting, security evidence, and decommissioned device data; identify telecom-specific rules. |
| `RET-SECRET` / `RET-SECURITY` / `RET-SECURITY-AUDIT` | Secret lifecycle metadata, callback/security events, role changes and support grants | Preserve only required audit/proof; define access, incident, legal-hold, and deletion requirements. |
| `RET-AUDIT` | General business-operation audit records not covered by another category | Confirm lawful retention, actor/action evidence, correction history, access, and hold requirements. |
| `RET-CONSENT` / `RET-COMMUNICATION` | Consent, suppression, notices, campaigns and delivery evidence | Retain the minimum evidence needed for purpose, opt-out, service notice, and disputes; confirm requirements by channel/country. |
| `RET-TENANT` / `RET-OPERATIONAL` | Tenant settings, domains, jobs and equipment | Align operational lifecycle with tenant cancellation, recovery, and legal holds. |
| `RET-EXPORT` / `RET-TELEMETRY` | Export files, temporary data, logs, traces, metrics, and incidents | Bound temporary copies; include providers, caches, archives, backup expiry, identifiers, and access logs. |
| `RET-REFERENCE` | Private screenshots and design/reference captures | Keep private source captures local-only under existing rules; set a separate local owner and retention policy, never include sensitive source data in public documentation. |

## Evidence record and sign-off template

Create one record per country. Store the counsel memorandum and approval evidence in the approved private legal workspace, not in public Git if it contains privileged or personal information. The repository should retain only a sanitized decision summary and a reference identifier.

| Field | Required value |
|---|---|
| Country and geographic scope | `KE`, `UG`, or `TZ`; state Mainland/Zanzibar scope where applicable. |
| Counsel | Named qualified local counsel, firm, conflict check, engagement reference, advice date, and source cut-off date. |
| Processing map | Versioned data-flow map and list of systems, locations, recipients, subprocessors, support access, and transfer paths reviewed. |
| Roles and filings | Approved party roles by flow; registration/DPO/representative/reporting decision and evidence. |
| Transfer decisions | Approved legal basis/mechanism and required safeguards, permits, filings, contract instruments, notice/consent, onward-transfer limits, expiry and change triggers. |
| Retention schedule | Country-specific table keyed to each `RET-*` category, with basis, duration, trigger, deletion/anonymization, legal hold, backup expiry, and owner. |
| Rights/notices/incidents | Required notice/consent, rights response workflow/deadline, breach decision tree/notification timing, and named operational owners. |
| Residual issues | Open question, risk, interim disabled processing, accountable person, dependency, and revisit date. |
| Approvals | Counsel's written conclusion; privacy/data owner; security/operations owner; project owner; separate country go/no-go approver. |

## Decision and release gates

- MN-DEC-018 stays **Open** until all required country evidence and accountable owner appointments are reviewed and accepted. This plan does not close the decision.
- Do not claim Ireland or Dublin is adequate, approved, or a live data-residency region. Do not enable a data flow until counsel has approved its destination, recipients, legal mechanism, safeguards, and filings.
- Do not set statutory retention periods, promise a response deadline, or decide controller/processor roles from this packet. Keep every unknown explicit and route legal interpretations to local counsel.
- Incorporate approved results into `READINESS-DECISIONS.md`, `DECISION-REGISTER.md`, `COUNTRY-AND-OPERATIONS-PACKS.md`, the data ownership map, privacy/terms requirements, provider contracts, and the relevant implementation gates. Record a new decision ID where counsel resolves a material product/architecture choice.
- A country go/no-go and production processing require separate approvals under `AGENTS.md` and the implementation plan. This document does not grant either.
