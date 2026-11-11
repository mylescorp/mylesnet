# Task Handoff: Reconcile public sitemap route counts

- **Task ID:** `MN-GOV-003`
- **Date:** `2026-11-11`
- **Agent/owner:** Coordinating agent
- **Plan phase:** Phase 0 — close planning and turn decisions into build contracts; affects Phase 5 public-site scope
- **Status:** `In progress`
- **Related requirements:** `IMPLEMENTATION-PLAN.md` Phase 0 route traceability and Phase 5 all-308-URL acceptance; `TECHNICAL-SPECIFICATION.md` §5.1
- **Dependencies:** Approved 308-URL public-sitemap scope; private local screenshot inventory

## Objective

Resolve the public route-family mismatch identified in PR #1's Greptile review. PR #1 is merged, so this correction is prepared in a separate follow-up branch and PR rather than altering the merged pull request.

## Work completed

- Reconciled the source-sitemap scope recorded in `public-site-review.md`: 307 non-homepage URLs plus the homepage, for 308 public pages total.
- Matched the route-family counts against the private capture inventory: 36 feature paths, 33 blog paths, 177 changelog paths, 4 solution paths, 4 comparison paths, 1 integrations directory, and 52 other paths. The capture inventory also contains UI states, so its screenshot-row total is not the sitemap URL total.
- Updated the implementation plan, technical specification, public-site review note, and Phase 0 status ledger to use the same counts.

## Implementation or documentation details

- **Files changed:** `docs/IMPLEMENTATION-PLAN.md`, `docs/TECHNICAL-SPECIFICATION.md`, `docs/public-site-review.md`, `docs/IMPLEMENTATION-STATUS.md`, `docs/agent-handoffs/2026-11-11-MN-GOV-003-coordinator.md`.
- **Approach:** Count sitemap URLs separately from screenshot UI-state captures. Use the private local manifest only as evidence; do not add it or its image files to Git.
- **Decisions made:** The 307 non-homepage paths are partitioned into the listed families; the homepage is a separate 308th public URL.
- **Decisions left open:** None for this count reconciliation.

## Verification and evidence

- **Checks run:** `git diff --check`; sanitized inventory aggregation confirmed 325 capture records, with family counts of 36 features, 33 blog, 177 changelog, 4 solutions, 4 comparison paths, and 1 integrations path; arithmetic confirmed the listed families plus 52 other paths total 307 non-homepage URLs; local Markdown links valid in all five changed documentation files; `AGENTS.md` has no prohibited reference name.
- **Checks not run:** Product tests/builds are not applicable to this planning-document correction; tests are not run unless requested.
- **Evidence:** Existing public-site review records the homepage plus all 307 non-root URLs from the source sitemap. A sanitized local inventory check confirmed the specialized route-family counts; 52 remaining non-homepage sitemap paths are the residual category. No screenshot or sample account data is added to the PR.
- **Failures:** None observed.

## Remaining work

### Done

- Reconciled family counts to 307 non-homepage paths plus one homepage.

### Pending

- Obtain an explicit numerical 5/5 confidence score for PR #4's latest head; the completed review of `82432f4` was approved with zero comments but did not report a score. Fix any new findings on the same branch and PR.
- Merge only after Greptile explicitly reports 5/5 confidence with zero comments and unresolved issues, and required checks pass.

### Blocked

- None. PR #1 is merged, so this correction is tracked on follow-up PR #4.

## Risks and handoff instructions

- **Risks/limitations:** The route manifest is local-only and its screenshot-record total includes UI states; do not describe screenshot rows as sitemap URLs.
- **Next action:** Request review of the current PR #4 head and record the explicit score; fix any findings on its branch, then merge only after its review and check gates pass.
- **User approval required:** None for the requested documentation correction; do not deploy.

## Delivery state

- **Commit/branch:** Changes are committed and pushed on `fix/pr1-route-count-greptile`.
- **PR/review:** [PR #4](https://github.com/mylescorp/mylesnet/pull/4) targets `main`; open. Greptile's review of `82432f4` approved with zero comments and the prior thread is resolved; no explicit numerical confidence score was reported. This handoff update requires review at the resulting latest head.
- **Merge:** Not merged.
- **Deployment:** Vercel preview is ready; not deployed to production.
