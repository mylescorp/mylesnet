# ADR-0008: Theme source, compiler, and schema boundary

- **Date:** 11 November 2026
- **State:** Token-source and tenant-brand boundaries accepted; runtime compiler/schema remain open
- **Owner:** Design-system owner not named; assign in MN-PLAN-008
- **Evidence:** `AGENTS.md`; `DESIGN-SYSTEM-HANDOFF.md` §§1–4, 8–12; `MYLESNET-DESIGN-SYSTEM-V1.0-AUDIT.md`; `DECISION-REGISTER.md` MN-DEC-007/021

## Context

The approved MylesNet Design System v1.0 is the shared visual source across public, platform, tenant, subscriber, captive, email, and document surfaces. Its local source package generates semantic tokens but does not include the runtime tenant theme compiler/validator. Several schema/output/a11y gaps are explicitly open.

## Decision

- Use one DTCG token source and generated semantic CSS/Tailwind mappings. Components consume semantic/component output; applications and UI do not import token source files directly. Generated `dist/` is a build output and is never edited by hand.
- Use one shared component contract and preserve Light, Dark, and System modes. Separate appearance mode from tenant branding.
- Resolve tenant brand only from a verified tenant host or authenticated membership. Allow only approved brand inputs. Tenant-provided data cannot override status meaning, focus, security, density/layout-control, or accessibility behavior. Platform routes stay platform-branded.
- Email and PDF tokens are renderer adapters, not alternative token systems; issued historical documents do not change when a tenant theme changes.
- Do not claim the static package includes runtime per-tenant compilation. Schema IDs, secondary-accent derivation, shape preset, complete dark output, contrast/chart coverage, attribution, logo/font assets, and compiler/validator remain open before the design-system implementation phase.

## Alternatives considered

- **Per-app or per-panel token palettes:** rejected; a single authored source and generated outputs are required.
- **Let tenant branding override all semantic colors:** rejected for status, focus, security, and layout-control tokens.
- **Treat the static token generator as a complete runtime theme system:** rejected because the package audit found that runtime compiler, validation, storage/cache/SSR wiring, and other pieces are absent.

## Security and operational tradeoffs

Generated mappings prevent visual drift but require schema/version discipline and parity checks across renderers. Tenant-provided theme inputs are untrusted and must be strictly validated, sanitized, bounded, and isolated in caches/SSR. Incorrect dark values or contrast can create accessibility/security-signaling failures. The package's existing tests do not independently prove the audit's requested guarantees.

## Interfaces and sequences

See `IC-THEME-01` and `IC-APP-02` in [`INTERFACE-AND-SEQUENCE-CONTRACTS.md`](INTERFACE-AND-SEQUENCE-CONTRACTS.md). Detailed compiler and theme publish flows belong to the design-system phase after authorization.

## Open items and revisit trigger

Close all gaps in `DESIGN-SYSTEM-HANDOFF.md` §12 and the v1.0 audit, including approved assets and attribution, before implementation. Select runtime compile/cache/SSR strategy only after schema and performance/security review. Revisit token decisions only through an approved design-system change.
