# Router lab and acceptance decision worksheet

**Task:** MN-PLAN-030

**Plan phase:** Phase 0 — close planning and turn decisions into build contracts

**Status:** Owner worksheet; supported devices, paths, credential lifecycle, and lab evidence remain open.

## Purpose and boundary

Use this worksheet with [ADR-0005](adr/ADR-0005-radius-and-router-boundary.md) and the `IC-RADIUS-01`, `IC-ROUTER-01`, `SEQ-RADIUS-01`, and `SEQ-ROUTER-01` contracts in [the interface and sequence catalog](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md). It records the lab and acceptance choices required by `MN-DEC-017` before any router-facing implementation is considered ready.

This worksheet selects no device/version, connectivity mode, source allowlist, provisioning-token lifetime, captive-portal outage policy, or acceptance result. It does not authorize router connections, provisioning, packet capture, payment, or production/network changes. Any future lab must be isolated, explicitly authorized after product implementation is approved, and use synthetic tenants, subscribers, credentials, and payment outcomes.

## Approved boundaries to preserve

- Use FreeRADIUS 3.2.x with a dedicated PostgreSQL AAA store. Keep Convex, WorkOS, browser authorization, and payment callbacks off the RADIUS request path.
- RouterOS 7 is the first-class MikroTik target. Require an operator-pasted provisioning command and use a separate least-privilege worker path.
- The WireGuard tunnel permits RADIUS authentication/accounting and CoA/Disconnect only. Block WinBox, SSH, and RouterOS API through the tunnel.
- Provisioning material is single-use, expiring, auditable, and revocable. Router credentials/secrets stay out of UI, logs, exports, screenshots, and this worksheet.
- If RADIUS is unavailable, retain existing sessions where possible, reject new logins, and never fail open. Provider return pages do not authorize payment or network activation without verified confirmation.
- Do not connect to real routers during Phase 0.

These boundaries are recorded in [MN-DEC-006/017](DECISION-REGISTER.md), [ADR-0005](adr/ADR-0005-radius-and-router-boundary.md), and [Technical Specification §5.7](TECHNICAL-SPECIFICATION.md#57-radius-and-router-operations).

## Lab inventory and topology approval

Record each lab device and network path separately. “RouterOS 7” alone is not a tested compatibility claim.

| Lab item | Proposed value or selection | Evidence required | Owner / approval | Status |
|---|---|---|---|---|
| MikroTik hardware model and board/revision | Open | Inventory and ownership confirmation | Network owner | Open |
| RouterOS version/build and upgrade path | Open | Exact version/build and rollback plan | Network owner | Open |
| PPPoE and Hotspot capability coverage | Open | Required feature/config matrix | Product and network owners | Open |
| Isolated AAA server and database topology | Open | Network/data-flow diagram; no production credentials | Network/platform operations | Open |
| Lab router, tenant, branch, subscriber, and NAS identities | Open | Synthetic fixtures and tenant-boundary plan | Network/security owners | Open |
| Test network, NAT/CGNAT/firewall, and egress/inbound assumptions | Open | Diagram and verified path evidence in the isolated lab | Network owner | Open |
| Test SIM/mobile network or alternate uplink, if required | Open | Disposable lab-only connectivity plan | Network owner | Open |
| Monitoring, packet/log redaction, cleanup, and stop condition | Open | Lab runbook with secret/PII safeguards | Security/operations owners | Open |

Do not put public IPs, shared secrets, private keys, production hostnames, or credentials in this worksheet. Keep exact sensitive lab values in an approved secret inventory/runbook.

## Connectivity, provisioning, and policy decisions

| Decision | Required owner input | Acceptance evidence before implementation | Status |
|---|---|---|---|
| Permitted network paths and source validation | RADIUS authentication/accounting, CoA/Disconnect, worker health/control paths, source identity and denial behavior | Allow/deny matrix proves only approved paths are reachable | Open |
| Inbound reachability versus router-initiated outbound | CGNAT, dynamic address, firewall, reconnect and tunnel rekey assumptions | Representative topology evidence for each supported deployment case | Open |
| RouterOS compatibility envelope | Supported model/build combinations and explicit exclusions | Lab matrix and tested-version policy; unsupported versions fail with clear guidance | Open |
| Provisioning-token lifecycle | Lifetime, single use, scope, replay protection, cancellation, expiry, rotation, and revocation | Threat review and lab evidence showing reuse/expired/revoked attempts fail | Open |
| Tenant/NAS identity binding | NAS identity, tenant mapping, shared-secret rotation, duplicate or moved router behavior | Positive and cross-tenant negative cases; no client-supplied tenant authority | Open |
| Policy synchronization and stale state | Versioning, projection delay, revocation propagation, cache invalidation and recovery | Measured propagation and stale-policy acceptance for authorized scenarios | Open |
| Accounting durability and recovery | Start/Interim/Stop identity, durable outbox, ordering, retry/drain and reconciliation ownership | Failure-injection evidence shows acknowledged events survive and are not duplicated | Open |
| CoA/Disconnect and router command guard | Allowed command catalogue, approvals, timeouts, result capture, retries, compensation | Unauthorized command/admin-port attempts denied; authorized changes audited | Open |

Policy-sync, accounting, and availability targets remain subject to the broader ADR-0005 and service-objectives owner decisions. This worksheet does not set their numeric values.

## Captive portal and outage cases

Record product/network behavior and the owner evidence needed for each case. Do not treat browser redirect, payment return, or UI state as provider confirmation.

| Scenario | Owner decision / expected behavior | Required lab or sandbox evidence | Status |
|---|---|---|---|
| Router cannot reach hosted captive portal | Open: safe fallback, user guidance, and retry path | Synthetic outage scenario; no real subscriber traffic | Open |
| Provider checkout pending, failed, expired, or browser returns early | Open: preserve pending/failed state until verified callback or reconciliation | Approved provider sandbox evidence; no financial transaction | Open |
| Confirmed payment while router/portal is disconnected | Open: idempotent recovery and eventual entitlement/session behavior | Synthetic delayed-event/reconnect evidence | Open |
| Template update is partially applied or router is offline | Open: version visibility, retry, previous-version behavior, and safe rollback | Isolated template sync failure and rollback record | Open |
| RADIUS unavailable while NAS remains online | Preserve current sessions where possible; reject new logins; never fail open | Isolated failure and recovery evidence | Open |
| Router reconnects after policy/accounting backlog | Open: replay, deduplication, stale policy and reconciliation behavior | Bounded recovery exercise with durable event evidence | Open |

## Acceptance record

For every supported device/path combination, record the requirement ID, preconditions, synthetic fixture, action, expected result, negative/abuse case, evidence artifact, reviewer, and unresolved limitations. At minimum, the future isolated lab should demonstrate:

1. The approved RADIUS path works while WinBox, SSH, and RouterOS API remain unreachable through the tunnel.
2. Unapproved source, tenant/NAS mismatch, expired/reused/revoked provisioning token, and unauthorized CoA/Disconnect are rejected and audited.
3. RADIUS loss preserves existing sessions where possible and rejects new logins; recovery does not create duplicate accounting or silently apply unapproved stale policy.
4. Portal-template versions expose sync status and safe rollback; incomplete payment return never creates an entitlement before verified confirmation.
5. Disconnect, retry, timeout, failure, and recovery outcomes have bounded work, visible operator evidence, and no secret-bearing logs.

These are planning acceptance cases, not tests run in this task. A successful lab run supports only the exact device, software, path, and scenario exercised; it does not establish production availability or country/provider approval.

## Roles and closure

The project owner appoints the Network Operations owner, Platform/Database Operations owner, Security reviewer, Product/Portal owner, and Payments owner for affected return flows. Record each person's lab and approval scope in the [Phase 0 owner appointment register](PHASE-0-OWNER-APPOINTMENT-REGISTER.md). Keep implementation, production, and support-recovery authority separate from lab evidence.

Before `MN-DEC-017` can be closed, reconcile this worksheet with ADR-0005, technical §5.7 and §14.9, the route/action traceability catalog, country/device support expectations, and service-operation evidence. Preserve open choices until named owners provide evidence and accept them.

## Related artifacts

- [ADR-0005 — RADIUS and router boundary](adr/ADR-0005-radius-and-router-boundary.md)
- [Interface and sequence contracts](adr/INTERFACE-AND-SEQUENCE-CONTRACTS.md)
- [MN-DEC-017](DECISION-REGISTER.md)
- [Readiness decisions — Network and captive portal](READINESS-DECISIONS.md)
- [Technical Specification §5.7](TECHNICAL-SPECIFICATION.md#57-radius-and-router-operations)
- [Phase 0 owner appointment register](PHASE-0-OWNER-APPOINTMENT-REGISTER.md)
