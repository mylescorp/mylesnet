# Task Handoff: Central-bank FX source public evidence

- **Task ID:** `MN-PLAN-015`
- **Date:** `2026-11-12`
- **Agent/owner:** `Codex`
- **Plan phase:** `Phase 0 — close planning and turn decisions into build contracts`
- **Status:** `In progress`
- **Related requirements:** `IMPLEMENTATION-PLAN.md` Phase 0 work item 5; `MN-DEC-015`; `MN-DEC-022`; `TECHNICAL-SPECIFICATION.md` §14.5; `CORE-FLOWS-AND-DATA.md` billing/reporting currency rules.
- **Dependencies:** MN-PLAN-007 country and operations packs, MN-PLAN-008 Phase 0 exit review; finance/tax owner and country counsel remain unassigned.

## Objective

Inventory official CBK, BoU, and BoT public evidence for currency rates relevant to KES, UGX, and TZS billing/reporting. Record rate-series descriptions, date/field formats, historical access, ambiguity, and evidence gaps. Keep rate-source selection, quote direction, fallback/finality, pricing, and tax policy open. No product integration or operational use is in scope.

## Work completed

- Reviewed official central-bank pages for Kenya, Uganda, and Tanzania and the Bank of Tanzania weekly financial-markets report.
- Documented that CBK publishes a daily indicative open-of-trade series and separately describes an interbank spot-trade weighted-average series; the documents do not establish these as interchangeable.
- Captured BoU's public description of daily indicative rates and interactive history, while recording that the live rows/schema were not inspectable in this review.
- Captured BoT's public buying/selling/mean/date fields, prior-rate date-filter page, and a weekly report's explicit TZS-per-unit-of-foreign-currency label, while keeping daily-series semantics open.
- Added source links and unresolved questions to the Phase 0 decision/canonical requirements. No rate value, source, conversion rule, or fallback was selected.

## Implementation or documentation details

- **Files changed:** `docs/FX-RATE-SOURCE-PUBLIC-EVIDENCE.md`; `docs/IMPLEMENTATION-PLAN.md`; `docs/CORE-FLOWS-AND-DATA.md`; `docs/TECHNICAL-SPECIFICATION.md`; `docs/DECISION-REGISTER.md`; `docs/READINESS-DECISIONS.md`; `docs/IMPLEMENTATION-STATUS.md`; `docs/agent-handoffs/2026-11-12-MN-PLAN-015-fx-source-evidence-codex.md`.
- **Approach:** Used public official institution material only, with dated snapshots and limitations. Distinguished source-series metadata from the product's approved general instruction to use official central-bank reference rates.
- **Decisions made:** None. `MN-DEC-015` remains open; no central-bank rate series, rate type, or fallback is approved.
- **Decisions left open:** Authoritative series per billing/reporting use, country/source rule for cross-currency conversion, direct pair and quote orientation, retrieval/cutoff/freshness, non-business days, correction/version history, unavailable/missing/stale behavior, precision/rounding, data-access terms, and named financial/legal owners.

## Verification and evidence

- **Checks run:** `git diff --check` passed. Local Markdown link scan checked 157 links; every target exists except three private reference-image paths already present in base `HEAD` (`CORE-FLOWS-AND-DATA.md` → `reference-domain-settings.png` and `reference-subscription-wallet-auto-pay.png`; `READINESS-DECISIONS.md` → `reference-domain-settings.png`). Conflict-marker scan passed. Official public-source review on 12 November 2026: CBK's page states that daily indicative rates reflect average participant buy/sell rates at the open of trade and advertises historical exports; its separate Forex page describes a weighted-average registered spot-trades series. BoU's current page describes daily indicative rates and interactive history, but no live schema was visible to the review tool. BoT's daily table includes buying, selling, mean, and transaction date; its weekly report explicitly labels its weekly series TZS per foreign-currency unit.
- **Checks not run:** No tests, build, rate harvesting, API integration, or numerical calculation was run. GitHub PR CI is pending creation of the task's own draft PR.
- **Evidence:** Direct official links, observations, dates, and limitations are in `docs/FX-RATE-SOURCE-PUBLIC-EVIDENCE.md`.
- **REA evidence:** Not used; public source documentation and page metadata answered this question. No rendered MylesNet or shipped-runtime evidence was required.
- **Failures:** No newly introduced local documentation-check failure. Some official pages time out or expose only application chrome in the review tool; this limits field-level verification and is not evidence that their rates or APIs are unavailable. The three missing private image links are pre-existing and unchanged.

## Remaining work

### Done

- Recorded official source descriptions and explicit unresolved FX source-selection questions without changing the approved conversion policy.
- Linked the evidence to Phase 0 billing and reporting requirements.

### Pending

- Run documentation checks, commit/push the focused branch, open this task's own draft PR, and verify current-head Gitleaks/Vercel checks.
- Finance/tax owner and country counsel to select permitted series and approve pair, timing, stale/missing-rate, pricing, tax, retention, and access terms before implementation.

### Blocked

- None for this evidence inventory. Final FX policy cannot be approved without the named finance/country decision owners and required product/legal review.

## Risks and handoff instructions

- **Risks/limitations:** CBK exposes two separately described rate calculation contexts. BoU live fields/schema were not visible in this review. BoT's weekly quote orientation is explicit but does not by itself prove the same daily-series contract. Public pages do not establish reuse rights, freshness guarantees, API support, or accounting/tax approval.
- **Next action:** Finish documentation checks, push the task branch, create this task's own draft PR, and keep it open/unmerged while the code-review deferral is active. Do not modify another task's PR.
- **User approval required:** Explicit user authorization before product implementation or external rate use; user notice before re-enabling code review.

## Delivery state

- **Commit/branch:** Pending; branch `codex/mn-plan-015-central-bank-fx-evidence`.
- **PR/review:** Pending own draft PR; code review deferred at the user's direction.
- **Merge:** Not merged.
- **Deployment:** Not deployed; no product service was implemented or enabled.
