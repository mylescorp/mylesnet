# Product design reference evidence

**Status: planning only.** This document records observed patterns from the private, locally preserved ISP product and public-site reference galleries. They inform workflows and layout only; MylesNet's visual identity and tokens come from the approved MylesNet Design System v1.0.

The design-system archive and captured galleries are under local-only, ignored paths: `docs/reference/` and `docs/reference-images/`. Some captures contain live sample names, amounts, contact information, or network identifiers. Keep them private; do not commit them or copy their values into product fixtures.

The sanitized review notes are [tenant-console-review.md](tenant-console-review.md) and [public-site-review.md](public-site-review.md). The complete route/feature requirements and their evidence labels are in [TECHNICAL-SPECIFICATION.md](TECHNICAL-SPECIFICATION.md). The canonical visual contract is [DESIGN-SYSTEM-HANDOFF.md](DESIGN-SYSTEM-HANDOFF.md); do not define another token source here.

## Observed design patterns

- Tenant console: Light, Dark, and System themes; grouped primary navigation; a separate settings rail; top search and actions; centered bounded content; compact cards, tables, and form sections.
- Public marketing: charcoal-forward pages with accent color, Inter typography, generous hero regions, feature grids, and repeated calls to action. Registration has a theme switch and step-based onboarding.
- Tenant branding is distinct from platform tokens. Tenant brand values apply only to approved customer-facing surfaces; status, focus, security, shell geometry, and accessibility remain platform controlled.
- Domain setup uses tenant-specific DNS instructions, an explicit verification action, automatic HTTPS, and a safe previous-address migration period.
- Platform billing surfaces show invoice state, a separately enabled wallet auto-pay control, payment/retry state, and invoice actions.

These are rendered-browser observations, not source-CSS inspection or a complete accessibility audit. Values and counts are time-specific. Loading skeletons and gated pages are identified in the review notes and must not be treated as verified behavior.
