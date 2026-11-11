# Synthetic data and isolated environment rules

**Task:** MN-PLAN-006 · **Plan phase:** Phase 0 — close planning and turn decisions into build contracts<br>
**Status:** Planning rules only; no test data generator or service integration is implemented.<br>
**Authority:** `AGENTS.md`; `TECHNICAL-SPECIFICATION.md` §§9, 14.3–14.8; `DECISION-REGISTER.md`; the Phase 0 threat/data contracts.

## Environment classes

| Environment | Allowed data and credentials | External effects |
|---|---|---|
| Local developer | Deterministic synthetic fixtures; local-only fake keys; fake identity/provider/AAA/router services. | No production network, payment, email/SMS, customer domain, or provider callback. |
| Shared development | Synthetic or explicitly approved de-identified development records; credentials isolated to development services. | No real payment/collection, customer notification, production DNS, or router command. |
| Provider sandbox / integration | Synthetic records and provider-owned sandbox credentials; separate callback URL, WorkOS app, Convex deployment and keys. | Only documented sandbox effects; never reuse production credential, callback, or tenant data. |
| Staging / preproduction | Synthetic or separately approved test records; isolated domains, keys, queues and provider accounts. | Block payouts, real customer messages and network changes unless separately authorized and explicitly controlled. |
| Production | Real records only after all legal, security, provider, operational and user-approval gates are satisfied. | Governed by the production release and country authorization process; never a local default. |

## Synthetic fixture rules

- Generate fake identities and tenant/branch graphs. Use reserved non-routable domains such as `.test`/`.invalid`; do not copy actual names, phone numbers, email addresses, subscriber accounts, router IDs, IPs, credentials, payment references, or support cases.
- Use documentation-only IP ranges for simulated networks, not values from saved screenshots or customer devices. Fixtures must not route to a real router, NAS, provider or customer network.
- Create fake `tenantId`, `branchId`, WorkOS organization, subscriber, provider merchant, payment, NAS, session, and event identifiers with an unmistakable test prefix. A production parser must not accept a fake key as production authority.
- Store only intentionally synthetic contact data. Make locale/currency/timezone examples plausible for Kenya, Uganda, and Tanzania, but do not use them to represent actual people or make legal/tax assertions.
- Payment scenarios use a local stub or documented provider sandbox. Use no live card/mobile-money token, merchant account, payout destination, or production signature secret. Provide duplicate, replay, wrong-amount, wrong-currency, invalid-signature, delayed, reversed, and unknown-mapping fixtures.
- RADIUS/router scenarios use a local simulator or isolated lab only after that phase is authorized. Never check in real shared secrets, WireGuard private keys, router exports, customer public IPs, or real accounting packets.
- Fake secrets are generated locally and are distinct per environment. Keep them out of Git, `.env.example`, screenshots, traces, handoff, and logs. `.env.example` contains variable names and safe descriptions only.
- Use deterministic fixture seeds for repeatability, but ensure fixture identifiers and test keys cannot collide with real records. Reset/reseed is explicit and must never target production.
- Capture stale, invalid, revoked, over-privileged, branch-mismatched, duplicate/out-of-order, and recovery cases. Synthetic fixtures should make negative tests possible without relaxing authorization rules.
- Do not put private reference images, page screenshots, OCR/transcription, image names or metadata into generated fixtures or public commits. The local-only source may contain identifiable account data and must be treated as `RESTRICTED` until reviewed.
- Redact logs, generated artifacts, debug output, and bug reports. If a test accidentally contains real data or a secret, stop using the artifact, remove access, rotate the affected secret where applicable, and follow the incident process rather than copying it into a report.

## Environment isolation requirements

- Use separate WorkOS apps, Convex deployments, provider sandbox accounts, callback endpoints, domains, storage, queues, telemetry and credentials per environment.
- A local app must fail safely when environment identity is missing or production. Do not include production URLs, deployment IDs, API keys, router addresses, merchant IDs, or real callback targets in defaults.
- Make production credentials unavailable to local developer and preview jobs. Limit sandbox secrets to the relevant integration boundary; redact them from CI output and logs.
- Browser-visible configuration contains only public identifiers/URLs. Secret-bearing actions run server-side or in separately credentialed workers.
- Document environment promotion and data movement. Copying production data into lower environments requires separate privacy/security review, approval, minimization, irreversible de-identification evidence, and a specific purpose; it is not an assumed fixture source.
- Keep provider, identity, RADIUS, router, storage and observability secrets separate. Rotation or test reset in one environment must not affect another.

## Planning acceptance

Before implementation, Phase 1 must provide reproducible synthetic fixtures, isolated local bootstrap, environment access/secret inventory, safe callback and router stubs, redacted telemetry checks, and a guard preventing accidental production targets. MN-PLAN-006 records these rules only. No tests, fixtures, integrations, environments, keys, or production configuration were created as part of this planning task.

## Related artifacts

- [Data ownership and classification](DATA-OWNERSHIP-AND-CLASSIFICATION.md)
- [Threat model](THREAT-MODEL.md)
- [API/error/event backlog](API-ERROR-EVENT-CONTRACT-BACKLOG.md)
- [Implementation plan](IMPLEMENTATION-PLAN.md)
