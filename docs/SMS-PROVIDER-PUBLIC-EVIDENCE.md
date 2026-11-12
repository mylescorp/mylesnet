# SMS provider public evidence

**Task:** MN-PLAN-014 · **Plan phase:** Phase 0 — close planning and turn decisions into build contracts<br>
**Evidence checked:** 12 November 2026<br>
**Status:** Public documentation comparison only; no provider selected and no messages sent.

## Scope and boundary

This note captures public integration, trial-mode, delivery-report, and country/sender evidence for Africa's Talking and Infobip. Twilio remains a pricing benchmark under MN-DEC-014; this task does not repeat or refresh its prices. Public documentation can establish documented product behavior and declared service availability. It does not establish account eligibility, live route quality, handset delivery, local legal approval, data-transfer acceptability, contract terms, or production readiness.

No provider account, credential, sandbox, webhook endpoint, or SMS destination was used. No SMS was sent. This is a planning artifact only; it does not select a vendor or authorize a pilot.

## Evidence comparison

| Topic | Africa's Talking | Infobip | Planning consequence |
|---|---|---|---|
| Public service/API evidence | Its official Help Center sandbox guide describes a free simulator with SMS short-code resources and callback/webhook setup. The provider's developer SMS pages returned HTTP 403 during this review, so current API schemas, authentication details, and callback contracts were not independently inspected. | Current SMS API v3 documentation describes sending, message IDs, asynchronous status, delivery reports, and webhook configuration. A `PENDING_ACCEPTED` response means queued/accepted, not handset delivery. | Do not design provider-specific production contracts from quickstarts alone. Obtain current account/API documentation and confirm the callback security model and payload version before implementation. |
| Safe test boundary | The sandbox guide states that development interactions occur in its simulator rather than on a handset. This is the clearest no-handset test path found in this comparison. | Trial documentation requires a verified recipient for self-signup accounts. The SMS quickstart names `InfoSMS`, while another official delivery-report tutorial names `ServiceSMS`; provider trial documentation also lists multiple shared sender examples. The exact sender and recipient limits vary by account/channel. | AT simulator testing can avoid sending to a handset if provider account setup is later approved. Infobip trial testing is an actual message to a verified phone destination, not an offline simulator; use only a specifically authorized controlled test destination. Neither path was exercised here. |
| Delivery evidence and callbacks | An official 2018 Help Center article distinguishes the initial API response, provider handoff to a telco, and a later delivery report. It describes handset success as a report status, but the article is historical and current developer pages were inaccessible in this review. | The quickstart documents final DLR webhooks and distinguishes `DELIVERED_TO_HANDSET` from `DELIVERED_TO_OPERATOR`; a publicly reachable URL must return HTTP 200. The current reporting guide documents up to 20 retries after the initial webhook attempt, with the final retry 41 hours 30 minutes after the initial attempt; events can be lost after the retry window. Polling and logs cover the previous 48 hours, with polling reports consumed on retrieval. | A send API success is not customer delivery proof. Any future callback consumer needs message-ID correlation, idempotent event handling, durable acknowledgement, monitored recovery, and a documented fallback/reconciliation path. Confirm AT's current callback behavior and both providers' callback authentication, signatures, replay controls, and retention before implementation. |
| Declared country availability | The provider's 20 May 2025 country/service list declares bulk SMS in Kenya, Uganda, and Tanzania; it also lists two-way SMS in Kenya and shortcode service in Uganda and Tanzania. | The reviewed public material includes Kenya, Uganda, and Tanzania sender-registration guides. The coverage page says account-specific coverage and rates may require provider support; this review did not establish named operator routes or guaranteed service in all three countries. | Provider-published availability is not evidence of coverage on each target MNO, successful handset delivery, latency, or price. Obtain country- and MNO-specific routing confirmation and comparable quotes. |
| Sender registration evidence | A 2020 Kenya/Uganda Help Center article exists, but its published timelines and price details are stale and were not reused. Current sender requirements, approval lead times, and Tanzania rules need written confirmation. | Public guides describe country-specific restrictions. Kenya's guide says sender names must be linked to the company/person and documents Safaricom separation for transactional and promotional senders; it also warns against registering one sender through multiple aggregators simultaneously. Uganda's guide includes sender format and volume statements, including an MTN Uganda threshold; Tanzania's guide disallows generic/numeric senders and notes operator-specific underscore restrictions. | These are provider-published instructions, not counsel advice or confirmation of current telco rules. Obtain written, use-case-specific approval, costs, and timelines for each country and traffic class before relying on a sender ID. |
| Cost, delivery quality, privacy, and support | No current country quote or MylesNet handset test was obtained. | No MylesNet quote, account-specific route list, handset pilot, SLA, data-location/retention terms, or support commitment was obtained. | Keep provider choice open. Compare cost per delivered segment, delivery rate and latency by MNO, sender cost/timeline, throughput, retries, consent/STOP handling, data terms, and support on the same approved templates and destinations. |

## Safe evaluation sequence

1. Appoint the communications/privacy owner and country counsel named as open roles in `COUNTRY-AND-OPERATIONS-PACKS.md`.
2. Request written Kenya, Uganda, and Tanzania coverage, sender-registration, tariff, throughput, DLR, retention, security, data-location, support, and contract details from both candidates. Ask for transactional and promotional traffic rules separately.
3. Review trial terms and callback authentication before configuring any test account or endpoint. Use Africa's Talking's simulator for no-handset flow checks if the account and test environment are approved. For Infobip, first obtain explicit authorization for an owned/controlled test number; its documented trial flow sends to verified phone recipients.
4. If a later user-approved pilot is authorized, use synthetic content and controlled destinations, record each country/MNO, timestamp, sender, segment count, provider acceptance, final DLR status, and observed handset arrival. Do not use subscriber numbers or credentials.
5. Compare provider-confirmed handset delivery and delivered-message cost using the same approved test matrix. Obtain legal/privacy review, vendor terms, data-transfer and retention decisions, opt-out behavior, and an accountable decision before selecting a provider.

The existing product policy remains in force: service notices and promotional messaging need separate treatment; promotional opt-in and STOP suppression must be enforced, and sensitive credential delivery needs the relevant privacy/security review. This public-doc review does not approve SMS content or consent language.

## Open evidence and decisions

- No SMS provider is selected. MN-DEC-014 remains a candidate decision and CP-COMMS-01 remains open in all three countries.
- No comparable quotes, account-specific country/MNO route lists, delivery tests, sender approvals, throughput commitments, or support/SLA terms were obtained.
- Current Africa's Talking API and callback details remain unverified because the official developer pages returned HTTP 403 to this review tool; that access limitation does not show that the docs or API are unavailable to an account holder.
- Infobip's public trial examples use different test sender names. Confirm the account-specific sender and destination restrictions before any test.
- Callback authentication/signature, replay defense, provider retry details for Africa's Talking, privacy/data location, retention, and regional legal requirements still need evidence.
- No external service, credential, callback, router, or live communications integration was configured.

## Official sources

Sources below were reviewed on 12 November 2026. Where the source has a publication date, it is shown; undated vendor documentation may change and must be rechecked before use.

### Africa's Talking

- [Sandbox setup and simulator](https://help.africastalking.com/en/articles/1170660-how-do-i-get-started-on-the-africa-s-talking-sandbox) — 26 February 2024; states simulator interactions do not occur on a handset and discusses callback/webhook setup.
- [Countries and live products](https://help.africastalking.com/en/articles/2727792-which-countries-are-africa-s-talking-products-in) — 20 May 2025; provider-declared country/service matrix.
- [Why messages fail and delivery-report stages](https://help.africastalking.com/en/articles/742491-why-did-my-messages-fail) — 18 October 2018; historical status explanation, not treated as current implementation guidance.
- [SMS sending guide](https://developers.africastalking.com/docs/sms/sending) and [delivery-report guide](https://developers.africastalking.com/docs/sms/delivery-reports) — official developer pages returned HTTP 403 to this review tool on 12 November 2026.
- [Kenya/Uganda sender-ID setup](https://help.africastalking.com/en/articles/407085-how-do-i-set-up-my-sender-id-in-kenya-or-uganda) — 2020 article; retained as a pointer only. Old price/timeline statements were excluded from current comparisons.

### Infobip

- [SMS API quickstart and delivery report](https://www.infobip.com/docs/developer-resources/quickstart) — documents `InfoSMS`, verified-recipient trial use, acceptance vs handset-delivery statuses, callback requirements, and delivery report fields.
- [Receive an SMS delivery report](https://www.infobip.com/docs/tutorials/receive-sms-delivery-report-using-infobip-api) — tutorial uses `ServiceSMS` and a verified recipient during trial, illustrating that account/tutorial examples should be confirmed against current account configuration.
- [Free trial limits](https://www.infobip.com/docs/get-started/build-and-go-live/free-trial) — provider's current trial limitations and shared test-sender examples.
- [Reporting webhooks](https://www.infobip.com/docs/reporting/webhooks) — retry schedule, event-loss boundary, and polling/log retention windows.
- [SMS delivery reports](https://www.infobip.com/docs/sms/message-delivery) — webhook payload and setup details.
- Sender registration: [Kenya](https://www.infobip.com/docs/essentials/africa-registration/kenya-letter-of-authorization-loa-guidelines), [Uganda](https://www.infobip.com/docs/essentials/africa-registration/uganda-letter-of-authorization-loa-guidelines), and [Tanzania](https://www.infobip.com/docs/essentials/africa-registration/tanzania-letter-of-authorization-loa-guidelines).
- [SMS coverage and connectivity](https://www.infobip.com/docs/sms/sms-coverage-and-connectivity) — public overview; account-specific country coverage/rates require provider confirmation.

## Related requirements

- [Country and operations packs — CP-COMMS-01](COUNTRY-AND-OPERATIONS-PACKS.md)
- [Decision register — MN-DEC-014](DECISION-REGISTER.md)
- [Readiness decisions — messaging comparison](READINESS-DECISIONS.md)
- [API/error/event backlog — EVT-005](API-ERROR-EVENT-CONTRACT-BACKLOG.md)
- [Implementation plan — Phase 0](IMPLEMENTATION-PLAN.md#phase-0--close-planning-and-turn-decisions-into-build-contracts)
