# MylesNet Design System v1.0 — archive audit and reconciliation

**Review date:** 11 November 2026<br>
**Review scope:** every entry in the supplied ZIP, including documentation, source tokens, generated output, schemas, fixtures, tests, scripts, screenshots, and HTML references.<br>
**Disposition:** v1.0 is the approved design-system baseline for MylesNet planning documents. This is a specification review only; no app implementation, dependency installation, package build, or test run was performed.

## Source archive and integrity

- Supplied archive reviewed and preserved locally as the extracted tree `docs/reference/mylesnet-design-system-v1.0/mylesnet-design-system/`.
- SHA-256: 23eb3f0da166d28d43c6ddab3f7a402446f39584d38876ef6a51b8c2eefbc121
- ZIP inventory: 84 entries, comprising 63 files and 21 directory entries.
- All ZIP paths were checked for absolute paths, parent traversal, and symlink entries. None were found.
- The original download was not modified. The preserved copy has the same SHA-256.

## Review approach

Text and source files were read; JSON/schema/token and generated files were inspected; all three JPG references were opened visually; both HTML references were inspected. The HTML files are reference artifacts, not the source of truth. The one named SUPERSEDED is explicitly historical.

The package README reports that npm run verify passed 12 tests when the package was assembled. That result is reported as package-provided information only. It was not independently verified here; dependencies were not installed and no package command was run.

## Complete archive inventory

The following is the complete extracted package tree. Every listed file was reviewed or visually inspected according to its type.

~~~text
mylesnet-design-system/
├── .gitignore
├── AGENT-PROMPT.md
├── CHANGELOG.md
├── README.md
├── package.json
├── package-lock.json
├── build/
│   └── build.mjs
├── dist/
│   ├── css/
│   │   ├── components.css
│   │   ├── density.compact.css
│   │   ├── density.touch.css
│   │   ├── primitives.css
│   │   ├── semantic.dark.css
│   │   ├── semantic.light.css
│   │   ├── semantic.system.css
│   │   └── tokens.css
│   ├── email/
│   │   └── tokens.json
│   ├── json/
│   │   ├── tokens.dark.json
│   │   └── tokens.light.json
│   ├── pdf/
│   │   └── tokens.json
│   └── tailwind/
│       └── preset.cjs
├── docs/
│   ├── 01-architecture-and-primitives.md
│   ├── 02-semantic-tokens-tenant-contract-accessibility.md
│   ├── 03-component-library.md
│   ├── 04-type-locale-brand-email-pdf.md
│   ├── 05-delivery-governance-qa-handoff.md
│   ├── 06-shell-and-screens.md
│   ├── 07-open-items.md
│   └── 08-repo-structure.md
├── reference/
│   ├── dashboard-dark.jpg
│   ├── dashboard-light.jpg
│   ├── shell-overview.dc.html
│   ├── subscribers-list.jpg
│   └── token-explorer-v1.SUPERSEDED.html
├── tests/
│   └── contrast/
│       ├── derive.test.mjs
│       ├── schema.test.mjs
│       └── tokens.test.mjs
├── theme/
│   ├── derive/
│   │   └── derive.mjs
│   ├── fixtures/
│   │   ├── default.json
│   │   ├── high-saturation.json
│   │   └── low-contrast.json
│   ├── presets.json
│   └── schema/
│       ├── theme.input.schema.json
│       └── theme.schema.json
└── tokens/
    ├── component/
    │   ├── badge.json
    │   ├── button.json
    │   ├── card.json
    │   ├── input.json
    │   ├── nav.json
    │   └── table.json
    ├── platform/
    │   ├── email.json
    │   └── pdf.json
    ├── primitive/
    │   ├── color.json
    │   ├── layout.json
    │   ├── motion.json
    │   ├── radius.json
    │   ├── space.json
    │   └── type.json
    └── semantic/
        ├── common.json
        ├── dark.json
        ├── density.comfortable.json
        ├── density.compact.json
        ├── density.touch.json
        └── light.json
~~~

Directory entries (21): mylesnet-design-system/, build/, dist/, dist/css/, dist/email/, dist/json/, dist/pdf/, dist/tailwind/, docs/, reference/, tests/, tests/contrast/, theme/, theme/derive/, theme/fixtures/, theme/schema/, tokens/, tokens/component/, tokens/platform/, tokens/primitive/, tokens/semantic/.

## File-by-file review ledger

### Package root and build

- README.md — package scope, precedence, commands, built versus specified boundary, and package-time test claim reviewed. Treat its test result as unverified in this review.
- AGENT-PROMPT.md — planning prompt and package assumptions reviewed as archive content, not authority to override this project’s already approved product decisions.
- CHANGELOG.md — v1.0.0 contents and D1–D7 approval note reviewed; the package’s “pending owner approval” wording is superseded by the user’s explicit approval of this ZIP as the baseline.
- .gitignore — excludes node_modules and .DS_Store only; future project integration must also ignore generated dist output.
- package.json — package name/version, Node >=20, Style Dictionary/AJV dependencies, and build/test scripts reviewed; not installed or executed.
- package-lock.json — lockfile v3 and resolved dependency graph reviewed structurally; packages were not installed.
- build/build.mjs — generation paths and source-to-output behavior reviewed. It exposes a dark JSON output gap described below.

### Numbered design specification

- docs/01-architecture-and-primitives.md — token tiers, naming, primitive ramps, layout, sizing, motion, elevation and shell measurements reviewed.
- docs/02-semantic-tokens-tenant-contract-accessibility.md — light/dark semantic maps, brand derivation, tenant override matrix and accessibility rules reviewed.
- docs/03-component-library.md — shared component and interaction contracts reviewed; the React library itself is not included.
- docs/04-type-locale-brand-email-pdf.md — type, locale, RTL, branding assets, email and PDF constraints reviewed.
- docs/05-delivery-governance-qa-handoff.md — generation, tenant theme compilation, lifecycle, QA, governance and handoff requirements reviewed.
- docs/06-shell-and-screens.md — dashboard, subscriber screen, shell and navigation reference reviewed. HTML is not a standalone runnable app.
- docs/07-open-items.md — stated package conflicts and open items reviewed and reconciled against the user's approval and earlier MylesNet decisions.
- docs/08-repo-structure.md — proposed package/app structure reviewed. The user directed this project to follow its workspace boundaries on 11 November 2026; see the updated repository application contract in [DESIGN-SYSTEM-HANDOFF.md](DESIGN-SYSTEM-HANDOFF.md).

### Authored token sources

- tokens/primitive/color.json — raw neutral, accent and status ramps reviewed.
- tokens/primitive/layout.json — shell dimensions, breakpoints and row sizing reviewed.
- tokens/primitive/motion.json — durations and easing reviewed.
- tokens/primitive/radius.json — radius scale reviewed.
- tokens/primitive/space.json — spacing scale reviewed.
- tokens/primitive/type.json — font families and type roles reviewed.
- tokens/semantic/common.json — shared radius/density semantic source reviewed.
- tokens/semantic/light.json — light surface, text, border, action, focus and feedback mapping reviewed.
- tokens/semantic/dark.json — corresponding dark mapping reviewed.
- tokens/semantic/density.comfortable.json — comfortable sizing reviewed.
- tokens/semantic/density.compact.json — compact sizing reviewed.
- tokens/semantic/density.touch.json — touch sizing reviewed; v1 interaction policy makes touch sizing device-driven.
- tokens/component/button.json — button token aliases reviewed.
- tokens/component/input.json — input token aliases reviewed.
- tokens/component/badge.json — badge token aliases reviewed.
- tokens/component/card.json — card token aliases reviewed.
- tokens/component/table.json — table token aliases reviewed.
- tokens/component/nav.json — navigation token aliases reviewed.
- tokens/platform/email.json — static email renderer adapter reviewed.
- tokens/platform/pdf.json — static PDF renderer adapter reviewed.

### Tenant theme engine and validation fixtures

- theme/schema/theme.input.schema.json — tenant-writable schema reviewed; schema URI currently uses tokens.example.com.
- theme/schema/theme.schema.json — stored theme schema reviewed; derived values are read-only; schema URI currently uses tokens.example.com.
- theme/derive/derive.mjs — primary-brand derivation and contrast logic reviewed. Optional secondary accent is accepted by schema/docs but is not consumed by the current derivation code.
- theme/presets.json — shape radius presets reviewed; its description still labels these presets proposed and awaiting sign-off.
- theme/fixtures/default.json — baseline fixture reviewed.
- theme/fixtures/high-saturation.json — stress fixture reviewed.
- theme/fixtures/low-contrast.json — stress fixture reviewed.

### Build outputs

All 13 supplied output files were reviewed as generated examples, not authored sources:

- dist/css/tokens.css
- dist/css/primitives.css
- dist/css/semantic.light.css
- dist/css/semantic.dark.css
- dist/css/semantic.system.css
- dist/css/density.compact.css
- dist/css/density.touch.css
- dist/css/components.css
- dist/tailwind/preset.cjs
- dist/json/tokens.light.json
- dist/json/tokens.dark.json
- dist/email/tokens.json
- dist/pdf/tokens.json

The dark resolved JSON omits common radius and density values that are present in the light output. This is a build-output consistency gap to fix and verify before integration.

### Tests

All three supplied test files were read; they were not executed:

- tests/contrast/derive.test.mjs
- tests/contrast/schema.test.mjs
- tests/contrast/tokens.test.mjs

The source docs describe 12 test cases, but the current suite does not establish every claim in the prose. In particular, chart color-vision distinction and every semantic contrast pair need broader explicit validation before release.

### Visual references

- reference/dashboard-light.jpg — visually inspected as a light shell/layout example.
- reference/dashboard-dark.jpg — visually inspected as a dark shell/layout example.
- reference/subscribers-list.jpg — visually inspected as a data-table/list example.
- reference/shell-overview.dc.html — read as a design-canvas artifact; it references absent support code and external Google fonts, and includes styling that does not meet the final token contract. Use for composition only.
- reference/token-explorer-v1.SUPERSEDED.html — read as historical material; explicitly superseded and not a source for v1.0 values.

## Approved v1.0 baseline

- Three token tiers: primitive, semantic and component. Email/PDF platform files are output adapters, not a fourth product token tier.
- DTCG JSON under tokens/ is authored source. Generated CSS, resolved JSON, Tailwind, email and PDF output is never hand-edited.
- Appearance choices: Light, Dark and System. System follows operating-system preference. Forced-colors is an accessibility mode, not a fourth product theme.
- Comfortable is the default density; Compact is a tenant UI preference. Touch sizing is selected by device input capability and cannot be configured as tenant branding.
- Shared product foundations and component behaviors apply across marketing, authentication, platform admin, tenant console, subscriber portal, captive portal, email and PDF. Each surface retains its own shell and allowed brand context.
- Platform admin and marketing remain MylesNet-branded. Tenant brand applies only to approved tenant-facing surfaces and documents.
- The saved product screenshots remain layout/workflow evidence only. No third-party logo, customer details, text, or palette is copied as MylesNet product identity.

## Reconciliation findings

| Finding | Existing package evidence | Project decision / handoff treatment |
|---|---|---|
| Custom-domain offer gate and DNS method conflict | theme.input.schema.json has plan.customDomain; docs/02 says paid tiers and CNAME. | Earlier explicit product decision wins: custom portal domains are included across MylesNet offers, use the tenant-specific A-record and Verify DNS flow, issue HTTPS after verification, and keep old callback hosts until owner confirmation. Domain provisioning is documented separately from visual token overrides. |
| White-label offer gate conflicts with branding availability | docs/02 labels removal of “Powered by MylesNet” as a full white-label tier; the theme schema includes a whiteLabel boolean. | Standard Hotspot/PPPoE modes share their core features, while Enterprise may include tailored services. Tenant branding is available across offers; if the optional whiteLabel setting is enabled, it removes only product attribution from allowed tenant-facing footer/email areas. Legal and security notices remain. |
| Three tiers versus old four-layer handoff | tokens and docs/01 define primitive, semantic and component. | Replace old four-tier wording. Platform email/PDF files are output adapters. |
| High Contrast versus old selectable theme | Package defines Light, Dark and System; OS forced-colors support is an accessibility requirement. | Remove the separate High Contrast theme option. Retain forced-colors testing and support. |
| Touch density versus user-selected density | Schema permits comfortable/compact; touch is a separate token output. | Touch output is applied from device capability; do not save it in tenant theme settings. |
| Secondary accent implementation mismatch | Schema/docs allow optional brand.color.accent; derive.mjs only derives primary. | Keep the schema field visible in the contract, but mark its application behavior as a source gap. Do not silently discard or assign it new semantics. Resolve before enabling the field in runtime. |
| Shape preset status mismatch | Schema enumerates sharp/default/soft; presets.json still says proposed. | Keep default as safe baseline. Confirm the preset values against design owner before runtime exposes sharp/soft. |
| Schema IDs | Both schema $id values use tokens.example.com. | Replace with stable MylesNet-owned schema identifiers before implementation or persistence. |
| Contrast metadata/test coverage | Docs promise declared contrast pairs and validated chart differentiation; token sources/tests do not prove full coverage. | Add explicit exhaustive source/build checks and chart color-vision review before release. No claim of complete validation is made here. |
| Generated dark JSON | Build outputs show common tokens missing from resolved dark JSON. | Correct generator and compare light/dark output completeness before adoption. |
| Runtime/library scope | Package does not include a React component library, runtime theme compiler/storage, tenant editor, lint plugin, Storybook, visual regression or axe CI. | Treat these as implementation work, not as shipped capabilities. |
| Font/logo assets | Package contains no font binaries and no official MylesNet logo files. | Source approved/self-hosted font and logo assets before implementation; do not use screenshot sample identities. |
| Locale/font mismatch | docs/04 names Noto Sans for Arabic, but the tenant font enum does not include it; launch languages are English and Kiswahili. | Keep v1 LTR and the approved font enum; align documentation and schema before adding Arabic/RTL support. |

The audit is complete for this archive. The open rows above are package inconsistencies or implementation prerequisites; they do not invalidate the user's approval of the v1.0 design direction.
