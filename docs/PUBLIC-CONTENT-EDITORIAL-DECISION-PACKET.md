# Public Content Editorial Decision Packet

**Task:** MN-PLAN-018
**Plan phase:** Phase 0 — close planning and turn decisions into build contracts
**Status:** Decision support prepared; no CMS, route-level editing scope, or content owner is approved.
**Related decision:** [MN-DEC-020](DECISION-REGISTER.md#open-decisions)

## Purpose and boundary

This packet narrows the public-content portion of MN-DEC-020 into a reviewable decision. It uses the route catalog as its inventory and identifies where recurring editorial work may exist. A route family that looks like editorial content is only a candidate signal: this packet does not decide that any route must use a CMS.

All 308 public sitemap URLs remain in the first-release route scope as static or structured content. CMS behavior stays excluded unless an accountable owner demonstrates an editing need and the project approves the route set and workflow. Subscriber account-portal host and navigation remain separate open questions. This planning packet authorizes no product implementation, vendor selection, production service, or public content publication.

## Evidence baseline

`REQUIREMENT-ROUTE-TRACEABILITY.md` contains 308 public route rows. The counts below were recomputed from its route-family labels and reconcile with the route-family totals in `IMPLEMENTATION-PLAN.md` §5, Phase 5:

| Route family | URLs | Initial editorial signal | Required content control if editing is later approved |
|---|---:|---|---|
| Changelog (index and entries) | 177 | Strong candidate for recurring release publication; cadence and accountable editor are unknown. | Tie each entry to an approved, actually released change. Do not publish planned work as shipped. Preserve publication/version history. |
| Blog (index and articles) | 33 | Strong candidate for recurring authoring; cadence, authoring roles, and review need are unknown. | Preserve author/reviewer, publication time, locale, canonical metadata, and correction history. Substantive product and country claims need the relevant approval. |
| Feature pages | 36 | Product-claim copy may change, but update frequency and need for non-engineer editing are unknown. | Keep claims aligned to approved v1 scope and capability gates. |
| Solutions and comparisons | 8 | May need campaign updates; editing frequency is unknown. | Date/review factual and competitor claims; do not imply unapproved features or provider relationships. |
| Country/market pages | 24 | May need localized market copy; launch-country coverage and editorial ownership are unknown. | Country-specific payment, legal, tax, data-region, and availability statements must follow the applicable country approval gate. A route does not approve service in that country. |
| Core public pages | 6 | Product/industry pages may change with positioning; editing need is unknown. | Product claims must trace to approved scope. |
| Tools and calculators | 17 | Supporting copy may be editorial; calculation behavior and financial rules are product logic. | Keep formula, rate, eligibility, and privacy behavior under reviewed application/configuration contracts, not free-form page editing. |
| Legal, contact, and affiliate | 5 | Legal text needs controlled revisions; the inventory does not establish who may approve or publish it. | Require designated legal/policy approval and effective/version dates. Contact submissions remain a separate, purpose-limited PII workflow. |
| Integrations directory | 1 | Directory content may change; update frequency is unknown. | List only integrations and capability states approved for public representation. |
| Home | 1 | High-visibility summary page; cadence and editing owner are unknown. | Product, pricing, country, and availability statements must match their canonical approved sources. |
| **Total** | **308** | Candidate signals only; no route-level CMS approval. | Keep stable route IDs, paths, and acceptance mappings. |

The family counts classify inventory, not current source files, translation completeness, page quality, operational capability, or editorial ownership. The saved screenshot manifest includes UI-state captures beyond these sitemap URLs and is not a second public-route count.

## Content-management options

| Option | Fits when | Main trade-off | Evidence needed before selection |
|---|---|---|---|
| Repository-managed static or structured content | Updates are infrequent, engineering release review is acceptable, and content is naturally versioned with the site. | Editors depend on engineering releases; a publish UI is not provided. | Named reviewer/release owner, expected update cadence, preview process, and correction/rollback path. |
| Managed content records with an internal platform editorial workflow | Named non-engineer editors need recurring drafts, review, scheduling, and publication inside MylesNet. | Adds privileged UI, authorization, audit, workflow, storage, and operational ownership to the platform. | Approved route set, role matrix, separation of author/reviewer/publisher where needed, audit/version contract, preview isolation, and support owner. |
| External CMS connected to the public renderer | A content team needs a dedicated editorial product and the integration passes security, privacy, availability, cost, and portability review. | Adds an external identity/data boundary, provider dependency, preview/publish integration, and vendor/data-transfer review. | The internal workflow requirements above plus vendor evidence, data-location/retention terms, webhook/signature/retry contract, export/exit plan, and integration owner. |

No option is selected. A headless CMS is not required merely because blog or changelog routes exist; repository-managed content remains a valid option if it meets the appointed owners' workflow.

## Decision worksheet for MN-DEC-020

The project owner and appointed product/content owner should record each answer in `READINESS-DECISIONS.md` and `DECISION-REGISTER.md` before Phase 0 closes:

1. Which exact route IDs need editing, and which remain code-owned, static, structured, or tool behavior?
2. What publishing cadence, editor count, and non-engineer capability create the need? Who is accountable for each route family?
3. Which management option meets that need, and who owns its security, operations, backups, and eventual migration?
4. Who may author, review, approve, schedule, publish, unpublish, and correct content? Which page classes require product, legal, country, finance, or security approval?
5. What language/locale coverage is required at launch, and who owns translation review and missing-translation behavior?
6. Which timezone governs scheduled publication, and how do emergency correction and rollback work?
7. What preview experience, production-cache invalidation, analytics, redirect migration, and search/sitemap updates are required?

Until those answers are recorded, keep CMS implementation excluded and retain the 308 route inventory in scope as static or structured pages.

## Build-contract acceptance criteria if editing is approved

These are proposed acceptance criteria for the later Phase 5 contract. They do not themselves approve CMS work:

- Every inventoried public route has one stable requirement/route ID, canonical path, page family, selected content source, locale expectation, and accountable owner; uncovered, duplicate, or ambiguous routes are resolved before implementation.
- Only explicitly approved platform editorial principals can access draft, review, publish, schedule, unpublish, redirect, or rollback actions. Tenant staff, subscriber identities, and anonymous visitors cannot reach authoring data or mutations.
- Draft previews require authorization, are non-indexable, cannot enter shared public caches, and do not change production content. Publishing is server-authorized, idempotent, versioned, attributable, timestamped, and auditable.
- The approved workflow defines draft/review/publish/unpublish/schedule states, version history, correction and rollback behavior, slug collision rules, redirect ownership/expiry, and how sitemap, canonical, metadata, internal links, and search indexes follow publication changes.
- English/Kiswahili requirements and translation review are explicit; publication cannot silently show incomplete high-risk localized legal, pricing, provider, or country-availability claims.
- Pricing formulas/rates, calculator computation, tenant operational state, provider readiness, SLOs, and legal approval status remain governed by their canonical domain decisions. Editorial tools cannot change those controls by editing page copy.
- Legal/privacy terms, country-specific statements, financial/commercial claims, and public release notes have named approval sources before publication. Public copy cannot turn a candidate provider, region, capability, or target into a live/approved claim.
- Any external content service meets the chosen privacy, data-transfer, retention, secret handling, webhook authenticity/retry, availability, export, and exit criteria. No private reference screenshots or live account values enter authored content or public assets.

## Open items retained

- No route-level editing need, CMS decision, editorial-role grant, person/role assignment, publishing timezone, or vendor approval has been provided.
- `MN-DEC-020` stays open until the worksheet decisions are approved. The subscriber account-portal host/navigation question also stays open and is not resolved by this packet.
- Phase 0 remains active; this document is evidence for the decision process, not the missing owner appointment, sign-off, implementation authorization, or Phase 0 exit.
