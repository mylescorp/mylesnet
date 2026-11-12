# Transactional Email Provider Public Evidence

**Task:** MN-PLAN-013 — record public transactional-email provider evidence
**Plan phase:** Phase 0 — close planning and turn decisions into build contracts
**Reviewed:** 12 November 2026
**State:** Candidate research only; no provider selected or approved for production

## Scope and decision boundary

This report records public provider documentation for the already approved direction:

- WorkOS's default sender remains responsible for identity-flow email such as verification, invitations, sign-in, and password reset.
- Resend remains a conditional candidate for MylesNet application email, subject to country-specific privacy/transfer review and delivery pilots.
- Amazon SES using the Ireland (`eu-west-1`) service region is the application-email fallback if Resend is not approved. The region is a candidate, not a data-residency or legal approval.

This report does not change those decisions, select a provider, create provider accounts, send email, configure WorkOS, or approve processing in Kenya, Uganda, or Tanzania. Facts below are observations from public provider pages. Their implications for MylesNet are identified separately as open validation work. List prices are point-in-time estimates and must be rechecked before procurement.

## Evidence summary

| Surface/provider | Public documentation observed | Implication and open validation |
|---|---|---|
| **WorkOS default identity sender** | WorkOS documents production identity messages sent from `workos-mail.com`, with SPF/DKIM/DMARC configured and sender reputation monitored. WorkOS says delivery from its shared domain is not guaranteed. The email page documents 30-day delivery history; the shared-domain view omits attempts the provider rejects before acceptance. | This matches the approved identity-email boundary. Confirm identity verification, invitations, sign-in, password reset, and recovery delivery to representative KE/UG/TZ inboxes before launch. The cited email documentation does not establish WorkOS message-content or metadata storage location, country transfer basis, or contracted retention; obtain those from current WorkOS terms/support. |
| **Resend application-email candidate** | Resend states customer data—including email content, delivery logs, webhook payloads, and account records—is stored in the United States. A selected sending region controls routing/sending, not storage. Its standard DPA includes EU SCCs; it does not sign country-specific DPAs. Free/Pro/Scale email and log retention is 30 days; account termination data deletion is within 90 days, dashboard account deletion takes 7 days, backups expire after a further 7 days, and an individual sent email has no self-service delete API. Resend advertises webhook notifications and message-level inspection/replay. The current public Scale list price is $90/month for 100,000 messages, with $0.90 per additional 1,000. | The Ireland send-region option does not resolve the documented US storage/processing. Counsel must decide whether the transfer and contract terms are acceptable for each launch country. Treat 30-day content/log retention and limited per-message deletion as data-lifecycle constraints. Keep sensitive message contents minimal, never include passwords, and keep one-time codes out of logs/analytics. Validate webhook signatures, event coverage, retry/replay behavior, idempotency, suppression handling, and actual inbox placement. The price excludes any taxes/add-ons and may change. |
| **Amazon SES Ireland fallback** | AWS lists SES regional endpoints and identifies Ireland as `eu-west-1`; SES identities, sending endpoints, suppression lists, credentials, and configuration sets are region-scoped. SES event publishing can route events to CloudWatch, Firehose, EventBridge, Pinpoint, or SNS. The current Essentials list price is $0.16 per 1,000 outbound emails for the first 10 million/month (about $16 at 100,000 sends, before message-data charges, tax, and other AWS services). Pro lists a separate $105/account/region monthly charge. | `eu-west-1` provides an Ireland SES sending endpoint and regional SES resources; this alone does not prove that all related account, support, telemetry, event-destination, or subprocessor data stays there, nor that transfers comply with each launch country's law. Review the full AWS service/contract data path and destination regions. Validate account sandbox status, sending limits, identities/DKIM, suppression and bounce/complaint events, IAM scope, regional alarms, and cost. Keep WorkOS identity email on the approved WorkOS default sender; SES is only the fallback for MylesNet application messages. |

## Provider separation and delivery contract to preserve

1. Keep identity-flow email and MylesNet application messages on separate paths. Do not configure a custom WorkOS email provider as part of this planning task.
2. Application messages include tenant-owner withdrawal verification codes and service/account notices. Use short-lived, single-use verification codes; never send a subscriber password by email or include secrets in telemetry. The application should record delivery status and provider correlation identifiers without retaining message bodies by default.
3. Treat provider acceptance, delivery, bounce, complaint, suppression, and application action as separate states. Provider acceptance is not proof that a person saw the email. A callback/webhook is untrusted input until authenticated, deduplicated, and correlated to an expected message.
4. Maintain provider adapters and environment-specific secrets as required by `TECH-STACK.md`; do not put credentials in browser code or local production configuration.

These are design constraints for later implementation, not an authorization to integrate or send messages now.

## Country evidence still required

For Kenya, Uganda, and Tanzania, the communications/privacy owner and country counsel must complete the following before production selection:

- Review controller/processor roles, DPA and subprocessors, international transfer safeguards, access/support locations, retention, deletion, and any country-specific restrictions for WorkOS, Resend, and the AWS SES account/region actually proposed.
- Run a controlled delivery pilot to representative networks and inboxes, measuring accepted, delivered, bounced, spam/complaint, and latency outcomes for identity and application templates separately. Public claims and message-provider dashboards do not establish local inbox performance.
- Confirm sender-domain ownership, SPF/DKIM/DMARC alignment, bounce/complaint suppression, rate limits, support response terms, incident notification, and account recovery for each environment.
- Obtain comparable written pricing at expected volumes (including message size, attachment/data charges, event destinations, support, retention, taxes, and optional features). Recheck public price pages when obtaining quotes.
- Decide the minimum necessary email content and the application's own event/audit retention; validate one-time-code expiry, resend limits, and delivery failure recovery in the security and API contracts.

Until these are reviewed and signed, the candidate choices remain disabled for production use. This report does not close `CP-COMMS-01` or any country launch gate.

## Source register

Official provider sources accessed 12 November 2026:

- [WorkOS email delivery](https://workos.com/docs/email) — default sender, shared-domain delivery limitations, email history, suppression and delivery-status behavior.
- [WorkOS custom email providers](https://workos.com/docs/authkit/custom-email-providers) — provider configuration and documented SES/Resend options. MylesNet is not adopting a custom WorkOS provider here.
- [WorkOS custom emails](https://workos.com/docs/authkit/custom-emails) — application-managed identity email flow when default WorkOS messages are disabled; this is not the selected v1 path.
- [Resend GDPR and data location](https://resend.com/security/gdpr) — storage region, transfer terms, and retention/deletion behavior.
- [Resend security](https://resend.com/security) — vendor-stated encryption, TLS, backups, and security program. Obtain current audit materials and contract evidence during due diligence.
- [Resend pricing](https://resend.com/pricing) — public plan/volume snapshot, which can change.
- [Resend webhooks](https://resend.com/features/webhooks) — advertised event notifications, observability, retries, and replay; verify exact API/security behavior during a later isolated implementation spike.
- [Amazon SES regions and endpoints](https://docs.aws.amazon.com/ses/latest/dg/regions.html) — regional service endpoints and region-specific setup.
- [Amazon SES event destinations](https://docs.aws.amazon.com/ses/latest/dg/event-publishing-add-event-destination.html) — available event destinations.
- [Amazon SES pricing](https://aws.amazon.com/ses/pricing/) — current public plan rates and ancillary pricing structure.

## Limitations

Public provider documentation does not establish MylesNet's eligibility, negotiated terms, legal compliance, country-specific transfer mechanism, inbox deliverability, production account state, or support commitment. Provider statements about certifications and security controls are not independent evidence of scope or suitability; request current reports and contract schedules. Recheck mutable pricing, regions, retention, and product behavior before implementation or purchase.
