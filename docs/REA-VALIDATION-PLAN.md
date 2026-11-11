# REA Use Plan for MylesNet

**Status:** Planning workflow; product implementation has not been authorized.
**Purpose:** Define where REA contributes evidence to MylesNet product decisions, implementation, and verification.
**Related roadmap:** [End-to-End Implementation Plan](IMPLEMENTATION-PLAN.md).
**Scope boundary:** REA is an evidence and environment tool. It does not replace source review, application tests, provider sandboxes, RouterOS/FreeRADIUS labs, legal approval, or production monitoring.

## 1. Why REA is in this project

MylesNet has three evidence sources that should stay distinct:

1. **Product/reference evidence:** captured Centipid marketing pages, tenant screens, theme states, and user-provided workflow screenshots. This describes visible layouts and observed behavior, not Centipid's hidden implementation or MylesNet's final requirements.
2. **MylesNet implementation evidence:** our own local, preview, and later production builds, using synthetic tenant data and controlled accounts. This shows whether the new product behaves as specified.
3. **External system evidence:** payment, messaging, identity, hosting, DNS, RouterOS, and RADIUS sandboxes/labs. Their authoritative results establish integration behavior; REA can help observe the web-facing parts but cannot approve a provider or prove network service quality by itself.

REA will help map evidence from the first category into explicit requirements, then observe the second category during development to find discrepancies. It may build an AgentEnv from a sanitized, authorized web recording when an agent needs a repeatable interface fixture. It is not a general codebase analyzer: use repository search, source review, and normal project tools for ordinary MylesNet source questions.

## 2. Operating rules

- **Reuse before recapture.** Start with `docs/reference-images/manifest.csv`, its gallery, public `route-manifest.csv`, `tenant-console-review.md`, and `public-site-review.md`. Do not repeat captures just to recreate existing evidence.
- **Read-only reference inspection by default.** Existing Centipid captures include private sample customer and financial data. Do not submit, save, export, pay, send messages, change settings, create users, or perform network actions against the reference tenant. The captured images remain local-only and must not be committed.
- **Mutating scenarios require an explicitly disposable/seeded target.** Before a capture that creates, edits, deletes, sends, charges, redeems, or provisions anything, confirm that the target and account are designated disposable or seeded for this test. Use synthetic values only. Never treat user credentials or verified production accounts as test fixtures.
- **Keep the capture bounded.** Name the question, target, route(s), action(s), evidence needed, and stop condition before invoking REA. Use passive inspection first; use an interactive scenario only for a specific behavior that passive evidence cannot answer.
- **Separate fact from inference.** Record whether a claim is a screenshot/DOM/network observation, a runtime action/result, static bundle candidate, an inference, or unknown. A missing observation is not proof a feature does not exist.
- **Minimize and protect data.** Do not put passwords, OTPs, tokens, personal subscriber details, financial identifiers, provider secrets, or full request/response bodies in reports, fixtures, PRs, or logs. Redact captured values and keep raw captures private/ignored unless they contain only synthetic data and their inclusion is approved.
- **Do not use REA for live infrastructure validation.** Real payment/payouts, customer routers, RADIUS production endpoints, production identity, or tenant data require their own controlled approval and runbooks. Use provider sandboxes and isolated network labs.
- **No production action is implied.** REA evidence never grants permission to create accounts, run live transactions, provision routers, enable a country, merge a PR, or deploy.

## 3. REA workflow and evidence record

For each REA investigation:

1. **Question:** Write the exact uncertainty (for example, “What does the failed-payment state show after returning from mobile money?”).
2. **Target:** Identify whether this is a saved reference page, an authorized browser tab, a local MylesNet build, a preview build, or a sandbox. Record environment and route, not credentials.
3. **Method:** Choose the least invasive method: reuse screenshots/manifest → passive browser/page inspection → bounded read-only navigation → scenario capture on a disposable app → AgentEnv export if repeatable behavior is needed.
4. **Capture:** Save only relevant screenshots, accessibility/DOM text, route transitions, or bounded network events. Prefer selecting exact views over exporting broad page/session data.
5. **Interpret:** Mark each finding `Observed`, `Inferred`, `Unknown`, or `Contradicted`. Record limitations and what REA did not cover.
6. **Apply:** Update a requirement, interaction contract, implementation task, bug, or acceptance criterion. Link evidence; do not paste private payloads.
7. **Handoff:** Record REA Evidence IDs (if returned), target/environment, date, observations, unknowns, artifact locations, and next action in the task handoff. Keep reusable synthetic AgentEnv bundles in an approved test-fixture location; keep private/raw captures ignored.
8. **Close:** Confirm the original question is answered or explicitly unresolved; stop/close owned capture environments and note cleanup outcome. Do not leave a live browser scenario running unintentionally.

### Evidence ledger fields

| Field | Required content |
|---|---|
| Finding ID | Stable ID, such as `REA-PORTAL-001`. |
| Question and product area | Specific uncertainty and affected route/flow. |
| Target/environment | Reference, local, preview, or sandbox; no secrets. |
| Evidence | REA Evidence ID and type, screenshot/artifact path, capture date. |
| Observation | What was directly seen, using redacted descriptions. |
| Interpretation | Inference separated from observation. |
| Coverage/limitations | Routes, states, identities, or payloads not inspected. |
| Requirement link | Plan/story/route/action/acceptance ID that uses the finding. |
| Privacy and state | Read-only or disposable seeded; sensitivity and storage decision. |
| Status/owner | Answered, partially answered, or unresolved; next owner/action. |

Use concise Markdown findings in the task handoff or associated Phase 0 evidence log. Do not make a broad raw-capture archive part of Git.

## 4. Project-specific usage by stage

### Stage A — Phase 0: close reference and requirement gaps

**Use now, while the project remains planning-only.**

1. Inventory the saved screenshot and route manifests before using browser tools. Confirm the 308 public routes and 73 tenant route/view entries have requirement/acceptance mappings; do not recapture the whole product.
2. Identify only material gaps in visible behavior needed for approved requirements: signup/provisioning status, custom domain DNS verification, monthly/annual subscription and wallet auto-pay, withdrawal fee/OTP confirmation, branch selection, staff scope, package FUP, voucher purchase, subscriber activation, and portal error/pending states.
3. For a missing **visual** state, use an already supplied reference image or ask for a screenshot. For a missing **behavior** state, first search the existing docs/captures; if still unknown, mark it open rather than interacting with the live tenant. Any future state-changing capture must use a disposable seeded environment.
4. Use passive REA browser analysis only when the exact authorized page is available and a question requires rendered structure or a current screenshot. Record evidence type and limitations. Static analysis of reference JavaScript may provide route/endpoint candidates, but it cannot be treated as a backend contract.
5. Feed results into `REQUIREMENT-ROUTE-TRACEABILITY.md`, `CORE-FLOWS-AND-DATA.md`, acceptance sequences, or an ADR. Update `IMPLEMENTATION-STATUS.md` and the task handoff with unresolved items.

**Exit:** each REA finding supports a named requirement or is recorded as non-blocking research; all claims derived from the reference are labeled as observation/inference/unknown. Existing captured route coverage is reused.

### Stage B — Phase 2: design system and shell review

After product work is authorized and shared shells exist:

1. Run the MylesNet local shell in synthetic data and capture agreed routes at desktop/mobile viewport sizes.
2. Review Light, Dark, and System; Comfortable/Compact density; tenant theme extremes; keyboard/focus/empty/loading/error states; and English/Kiswahili layouts against the design-system handoff and Storybook gallery.
3. Compare page geometry, navigation behavior, token usage, responsive states, and accessibility tree. Treat pixel differences as prompts for human review, not automatic proof of a defect.
4. Create an actionable issue with local route, expected behavior, screenshot, observed behavior, relevant token/component, and severity. Recheck after a fix.

**Exit:** critical shared-shell, theme, navigation, and accessibility discrepancies have recorded resolution or an explicit accepted exception. Ordinary source/token correctness is verified with project code and build checks, not REA alone.

### Stage C — Phases 5–6: public site and tenant console

1. Validate the local/preview route matrix against the approved 308-page content registry and 73-entry tenant route catalog. Check routes, redirects, shell selection, selected branch, permission-denied states, and responsive navigation.
2. Use REA screenshots/page trees to inspect representative pages per family, then sample all route families by checklist. Use automated route smoke/visual checks for exhaustive path coverage; REA screen inspection is supplemental and cannot replace them.
3. For role scenarios, use seeded test identities representing platform roles, tenant roles, branch roles, multi-tenant staff, and subscriber accounts. Do not use real customer accounts.
4. Observe only authorized local/preview request flows to help find mismatched frontend/backend expectations. Keep captured bodies/headers/credentials redacted. Convert approved contracts into source schemas and tests; do not infer authorization from UI visibility.
5. Build a MylesNet AgentEnv only when a bounded recorded flow materially helps coding agents work against a stable interface fixture. Use synthetic data and preserve exact known limitations. Do not make the AgentEnv the production backend or the sole integration test.

**Exit:** routes are mapped to product acceptance evidence; critical representative states match the design/behavior contract; no privacy/tenant data leaked into fixtures; contract gaps are either resolved in code/spec or recorded.

### Stage D — Phases 7–9: network, finance, and subscriber access

REA's role is limited to observing browser-facing setup flows and portal states on local/sandbox systems:

- **Router onboarding:** capture the MylesNet setup wizard, generated command display and completion/error states using a synthetic MikroTik lab router. REA can inspect the web journey; RouterOS configuration, WireGuard reachability, and RADIUS auth/accounting require the isolated router/RADIUS lab and network-specific verification.
- **Payments and withdrawals:** use provider sandboxes and synthetic merchant data to capture checkout, pending, callback result, fees, OTP, payout status, refunds and exception handling. Never send a real charge, refund, OTP, or payout through REA during research.
- **Captive portal:** inspect mobile and desktop viewport, tenant themes, voucher and payment-pending/success/failed states with a seeded test tenant. A browser success screen does not prove payment settlement or actual network activation; verify provider evidence, ledger entries, entitlement and FreeRADIUS behavior separately.
- **Subscriber credentials:** use synthetic identities. Ensure username/password are not retained in screenshots, HARs, recordings, console output, analytics, or AgentEnv bundles.

**Exit:** UI flow evidence agrees with provider sandbox and network-lab results; financial and network truth is independently confirmed; no live infrastructure has been modified.

### Stage E — Phases 10–12: communications, hardening, pilots, launch

1. Use REA on staging to inspect tenant-facing messaging preferences, notice/campaign approvals, support, analytics, export, cancellation and reactivation surfaces.
2. Verify test messages use provider sandbox/test destinations and promotional consent/STOP states. The actual delivery service, throughput, privacy location and sender approval are validated by provider evidence and legal review.
3. During pilots, restrict REA to authorized test tenants and read-only debugging unless the test case explicitly uses seeded disposable data. Do not expose private tenant data in shared reports.
4. Use REA captures to document reproducible frontend defects. Use observability, payment settlement reports, RADIUS metrics, uptime probes, restore drills, and support incident records as authoritative production evidence.
5. Archive only sanitized, relevant evidence and link it to release/go-no-go decisions. Never treat a polished screenshot or successful browser click as release approval.

**Exit:** REA findings are closed or accepted with a reason; production go/no-go is based on the full plan's legal, security, provider, network, SLO, and operational gates.

## 5. REA outputs and repository placement

| Output | Location / handling |
|---|---|
| Raw reference screenshots, private pages, HAR/session captures | Keep in existing ignored `docs/reference-images/` or secure local evidence storage; never add to public commits. |
| Sanitized evidence notes/findings | Agent handoff or a focused `docs/rea-evidence/` Markdown report when findings span tasks; link from `IMPLEMENTATION-STATUS.md` if it is a project gate. |
| Synthetic AgentEnv bundle | Dedicated test/fixture storage only after reviewing generated content for secrets/PII. Include README with capture scope, known unknowns, and regeneration steps. Commit only if fully synthetic and reasonably sized. |
| MylesNet source changes | Normal product source, PR, code review, and project checks. REA does not directly authorize edits or edits to a captured target. |
| Launch verification | Existing dashboards/runbooks/CI and country go/no-go record; link sanitized REA findings as supporting UI evidence only. |

If the REA server gives content-addressed Evidence IDs, record them in the handoff together with a durable sanitized artifact path. Do not assume MCP-session retention alone is a permanent archive.

## 6. Priority investigation queue

| Priority | Question | Why it matters | REA method | Result destination |
|---:|---|---|---|---|
| 1 | Are signup, tenant provisioning, branch/entity selection, and failure recovery fully specified? | Creates tenant, authority and billing context. | Reuse signup captures; passive inspect only for a named gap. | Requirement traceability and onboarding sequence. |
| 2 | How do package purchase, voucher redemption, confirmed payment, auto-created account and network activation states connect? | Core subscriber acquisition and access correctness. | Existing user-provided screenshots plus controlled synthetic portal scenario after build. | Subscriber/captive state machine and acceptance tests. |
| 3 | Which permissions and approvals guard refunds, exports, payouts, role changes and router actions? | High financial/security impact. | Existing screenshots as visual evidence; seeded role tests in MylesNet later. | RBAC matrix, API authorization tests and audit requirements. |
| 4 | Does the built shell meet approved design tokens, themes, localization and accessibility states? | Shared across all panels and portals. | Local/preview screenshots + accessibility/DOM views across representative routes. | Design issues and visual review evidence. |
| 5 | Do RADIUS, payment, messaging and realtime projections agree during failures/retries? | Screens alone cannot prove consistency or availability. | Observe browser-facing test flows; correlate IDs with provider/lab telemetry. | Integration verification report and incident runbooks. |
| 6 | Are all public routes and tenant route families present and canonical? | Large route scope creates omission risk. | Route inventory and automated local smoke; REA spot-checks representative rendered output. | Route coverage/status ledger. |

## 7. What REA will not decide or prove

- It cannot inspect Centipid server source code or reveal behavior not observed through the permitted target.
- Static bundle findings are candidates; they do not prove server authorization, payment semantics, route availability, or execution.
- A screenshot or successful UI action cannot prove ledger correctness, provider settlement, RADIUS authentication, CoA behavior, uptime, security, accessibility conformance, or legal compliance.
- AgentEnv is a bounded recording-derived fixture. It may omit unseen API branches, pagination, dynamic values, concurrency, authorization failures, or behavior outside the recorded session.
- It does not replace Playwright/unit/integration/security tests, provider certification, RouterOS compatibility lab, FreeRADIUS load/failover testing, counsel review, monitoring, or human design approval.

## 8. REA task completion checklist

- [ ] Question and phase identified; existing screenshot/route evidence reviewed first.
- [ ] Target is authorized, exact, and labeled local/reference/sandbox/staging/production.
- [ ] Scenario is read-only, or target is explicitly designated disposable/seeded before mutations.
- [ ] Capture is bounded and excludes unnecessary credentials, PII, payment data, and message bodies.
- [ ] Findings separate observations, inferences, unknowns, and contradictions.
- [ ] REA Evidence IDs/artifact paths and limitations are recorded in a handoff.
- [ ] Relevant requirement, acceptance criterion, issue, or route mapping is updated.
- [ ] Owned session/computer/capture was closed or cleanup state documented.
- [ ] Production readiness is based on the authoritative test/provider/network/legal evidence, not REA alone.
