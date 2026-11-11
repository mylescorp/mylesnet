# Shared design system package

This workspace package follows the repository target in the approved design-system handoff. The directory boundaries are being prepared during planning/setup; no tokens or UI components have been implemented here.

Before implementation, use `docs/DESIGN-SYSTEM-HANDOFF.md` as the integration contract; it incorporates the repository structure requested from the approved v1.0 source. Keep the dependency direction one-way: UI components import generated output, and applications import UI components. Applications must not import source tokens directly. Generated `dist/` output is ignored and must never be edited by hand.
