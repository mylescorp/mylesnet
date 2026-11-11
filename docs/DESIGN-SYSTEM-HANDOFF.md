# MylesNet Design System Handoff

**Status:** Approved design-system baseline v1.0; planning only<br>
**Date:** 11 November 2026<br>
**Applies to:** MylesNet marketing, signup/authentication, platform administration, tenant console, subscriber portal, captive portal, email, invoices, receipts and PDFs<br>
**Implementation status:** Not started

The approved source package is preserved in the local-only extracted tree `docs/reference/mylesnet-design-system-v1.0/mylesnet-design-system/`. The exhaustive package review and reconciliation is in [MYLESNET-DESIGN-SYSTEM-V1.0-AUDIT.md](MYLESNET-DESIGN-SYSTEM-V1.0-AUDIT.md).

This handoff translates that package into the MylesNet product contract and follows the user-directed workspace boundaries in `docs/08-repo-structure.md`. MylesNet’s tenant custom-domain workflow is governed by the product decision in `READINESS-DECISIONS.md`, not the package’s conflicting CNAME/paid-tier copy.

## 1. Design intent and rules of precedence

One shared system should make every MylesNet surface recognizable and accessible while allowing each audience a fitting shell. Consistency comes from shared tokens, component behavior, typography, spacing, interaction states and accessibility. It does not require one navigation layout for the marketing site, platform console, operator workspace and captive portal.

### Source precedence

1. Explicit MylesNet product decisions already recorded in project planning documents govern product behavior.
2. Within the approved design-system package, DTCG token JSON and theme schemas are the machine-readable v1.0 source.
3. Numbered package specifications clarify application and component behavior.
4. Supplied JPGs are visual references.
5. HTML references are design artifacts only. The file named SUPERSEDED is historical and must not be used to reintroduce old values.

If package files conflict internally, follow the explicit resolution or gap recorded in the v1.0 audit. Do not silently infer a new token or tenant override.

### Visual principles

- Keep the operational UI compact, calm and readable, with strong hierarchy and generous page-level structure.
- Use MylesNet’s orange accent for primary actions and identity. Keep neutral surfaces and status meaning platform-controlled.
- Show freshness for live data. Never communicate payment, network, security or permission state with color alone.
- Give every state a clear textual label, icon or shape in addition to its color.
- Make keyboard, touch, narrow-screen, zoom and translation behavior part of component design.
- Keep third-party saved product screenshots as layout/workflow evidence only. Do not copy its colors, logo, customer data or literal wording.

## 2. Shared foundation across product surfaces

All surfaces consume one semantic token contract and shared components. A surface may have a distinct shell, navigation and allowed brand context.

| Surface | Shell and brand context |
|---|---|
| Public marketing site | MylesNet-owned public header, navigation, content grid, calls to action and footer. No tenant identity or tenant colors. The 308 captured marketing routes use static/structured content unless editing requires a CMS. |
| Signup, sign-in and recovery | Focused form shell, progress where needed, security/help copy and clear retry/error states. A tenant sign-in host may resolve that tenant’s approved logo and brand; platform identity and security copy stay protected. |
| Platform admin | Separate platform navigation, global health, tenant lifecycle and operator identity. Always platform-branded. Never inherits a tenant theme, including during approved support access. |
| Tenant operator console | Persistent grouped sidebar, topbar, global search, page header, bounded content and settings sub-navigation. Tenant branding is permitted within the override contract. |
| Subscriber account portal | Mobile-first customer tasks and service information. Tenant brand is permitted. Do not show staff controls. |
| Captive portal | Minimal mobile-first tenant-branded access page for package purchase or voucher redemption. No staff navigation or operator chrome. |
| Email, invoices, receipts and PDFs | Static renderer-specific output using approved tenant logo/contact details and locked financial/status wording. A theme edit must not rewrite an issued historical document. |

### Common application theme context

Theme mode and tenant brand are separate inputs:

- Appearance mode is Light, Dark or System.
- System follows the operating-system preference. Forced-colors support is an accessibility behavior, not a separately selectable product theme.
- Authenticated users may persist their appearance preference to their profile. Anonymous captive/signup flows can follow the OS preference and may persist a local preference where shown.
- Tenant branding is resolved from a verified tenant host or authenticated tenant membership. A browser-supplied tenant ID is never sufficient.
- Platform routes ignore tenant branding.
- HTML surfaces expose a controlled theme context such as data-theme, data-surface and data-density. These are presentation selectors, never authorization data.

## 3. Token architecture and use

### Three token tiers

Resolution proceeds from raw values to meaning to component-specific aliases:

1. **Primitive:** raw values such as neutral/accent ramps, spacing, radii, font families, motion and layout dimensions. Application components must not consume primitives directly.
2. **Semantic:** meaning-based values such as surface, text, action, border, focus, feedback, density and typography roles. Components consume this layer.
3. **Component:** component aliases such as button/input/card/table/navigation values, defaulting to semantic roles. Only the owning component consumes its private component token.

The email and PDF token files are renderer adapters, not additional product token tiers.

### Authored source and generated output

DTCG JSON is the only authored token source. Keep the source split by responsibility:

- tokens/primitive/: color, layout, motion, radius, space and type
- tokens/semantic/: common, light, dark and comfortable/compact/touch density values
- tokens/component/: button, input, badge, card, table and navigation
- tokens/platform/: email and PDF adapter inputs

The package generator produces CSS variables, resolved JSON, a Tailwind preset, and literal email/PDF token outputs. Generated files are build artifacts. Never hand-edit them or create a panel-specific palette copy. The static package does not include a runtime per-tenant theme compiler.

Product CSS and components use semantic and component aliases in the --mn-* namespace, such as --mn-color-*, --mn-radius-*, --mn-density-*, --mn-font-* and --mn-button-*. Primitive variables such as --mn-neutral-* and --mn-accent-* are restricted to the token package and derivation pipeline. Do not put raw hex/rgb/hsl values or arbitrary pixel values in product components for properties with token scales.

## 4. Approved token values

The archive is the detailed machine-readable source. These compact tables record the primary values needed for product planning; use token names in implementation rather than copying literal values into components.

### Neutral and brand palette

| Ramp | Values |
|---|---|
| Neutral | 0 #ffffff; 50 #f3f3f1; 100 #efefec; 200 #e8e8e5; 300 #d2d2ce; 350 #a0a0a0; 400 #8f8f8b; 450 #767676; 500 #6b6b68; 600 #444444; 700 #2d2d2d; 750 #2a2a2a; 800 #202020; 900 #1b1b1b; 950 #151515 |
| Accent | 50 #fef5ec; 100 #fcead9; 200 #fbdbbc; 300 #f7c18c; 400 #f3a253; 500 #ee7e10; 600 #d16f0e; 700 #b5600c; 800 #864709; 900 #562e06 |
| Accent UI alternative | #e2780f |

The default orange fill #ee7e10 uses dark ink #1b1b1b for normal-size button text, not white. The documented ratio is 6.26:1 for ink; white on that fill does not meet 4.5:1. Accent-colored text uses darker derived shades, rather than the bright fill, to meet normal-text contrast.

### Light and Dark semantic map

| Role | Light | Dark |
|---|---|---|
| Canvas | #f3f3f1 | #151515 |
| Default surface | #ffffff | #202020 |
| Inset surface | #efefec | #2a2a2a |
| Primary text | #1b1b1b | #efefec |
| Muted text | #6b6b68 | #a0a0a0 |
| Control border | #8f8f8b | #767676 |
| Accent text | #b5600c | #ee7e10 |
| Strong accent text | #864709 | derived for dark mode |
| Focus ring | #b5600c | #ee7e10 |
| Accent tint | #fdf0e2 | #412f1d |

Light and Dark are separately specified; Dark is not created by inverting Light.

### Locked status semantics

| Status | Light foreground / tint | Dark foreground / tint |
|---|---|---|
| Success | #287250 / #ddf3ea | #4fd1a1 / #123b2e |
| Warning | #8a5a00 / #fbf0d9 | #e0a93b / #3a2f14 |
| Danger | #b3362b / #fbe4e1 | #f07a6e / #3a1f1c |
| Info | #2a66a8 / #e4eefa | #6aa6e8 / #17283d |

Status hues, focus, security and sensitive-data meaning are platform-owned. The package’s chart series 2–6 are fixed values, but color-vision differentiation is not proven by the included checks; chart accessibility review is an implementation gate.

### Spacing, radius and motion

- Spacing scale: 0, 4, 8, 12, 16, 20, 24, 32, 40, 48 and 64 px.
- Radius scale: 5, 8, 10, 12 and 16 px, plus fully rounded 999 px pills.
- Shape preset values in the archive: sharp uses medium/large 6/10 px; default uses 10/16 px; soft uses 14/20 px. The archive still labels these presets proposed, so default is the only runtime-safe choice until that discrepancy is resolved.
- Motion: 120 ms fast, 180 ms base, 280 ms slow, with the documented ease curve. Respect reduced-motion preferences.
- Elevation is primarily expressed by surface steps. Use the overlay shadow for menus/popovers, not every card.
- Layer values: sticky 10, dropdown 20, modal 30, toast 40.

### Layout measurements

| Element | v1 value |
|---|---:|
| Expanded operator sidebar | 238 px |
| Collapsed rail | 64 px |
| Topbar | 56 px |
| Centered content maximum | 1136 px |
| Medium / expanded breakpoints | 640 px / 1024 px |
| Minimum interactive target | 44 px |
| Preferred touch target | 48 px |

At widths below 1024 px the operator sidebar uses the rail. Below 640 px it becomes a mobile drawer and the primary content stacks. Keep all layout CSS logical so the shell can be mirrored if a future locale requires RTL.

## 5. Density, typography, localization and assets

### Density

- Comfortable is the default and measures 36 px controls, 52 px rows and 36 px navigation items.
- Compact is selectable for operator work and measures 32 px controls, 44 px rows and 32 px navigation items.
- Touch measurements are 48 px controls, 60 px rows and 44 px navigation items. Touch density follows coarse-pointer/device capability and is not a tenant theme input.
- Visual control height does not reduce the hit target below 44 px. Prefer 48 px on touch devices.
- Density may change spacing and row rhythm only; it never changes access, state meaning, text contrast or required control affordances.

### Typography

- Geist is the default operational UI family.
- Geist Mono is for identifiers, codes, numeric references and shortcut keycaps, not prose.
- Instrument Serif is a display/marketing face only, at 24 px and above; do not use it for tables, labels or warnings.
- Body default is 13/20 px; body-small 12/18 px; caption/eyebrow minimum 11 px. Nothing renders below 11 px.
- Tenant UI font choices are restricted to Geist, Inter, Plus Jakarta Sans, DM Sans, Hanken Grotesk and IBM Plex Sans. Display choices are Instrument Serif or Geist. No arbitrary font string, uploaded font or remote tenant font URL is accepted.
- Load approved font assets with an explicit fallback and non-blocking font-display behavior. The ZIP includes no font binaries.

### Language and assets

- Launch languages are English and Kiswahili, with tenant default and a customer-facing language switch where applicable.
- Format currency, date and number by explicit branch currency, user locale and timezone settings. Never infer currency from a language setting.
- Use logical CSS properties and root direction support; English and Kiswahili launch layouts are LTR. Full RTL content/QA scope remains a separate launch decision.
- The ZIP contains no official MylesNet logo. Use an approved MylesNet asset, not a screenshot logo. Asset handling must check real file type, size, dimensions, transparency and safe rendering. Sanitize SVG; reject scripts, external references and event handlers. Provide light/dark logo slots and a text fallback.

## 6. Shared shell contracts

### Operator shells

The tenant shell follows the reference geometry and information architecture: persistent grouped navigation, topbar, bounded content and separate settings navigation. Groups include Overview, Customers, Network, Finance, Outreach and Insights. Use the lower rail for approved secondary links such as Academy/What’s new.

The platform shell has its own navigation for overview, tenants, platform billing, operators/access, support grants, integrations, incidents and audit. Keep platform identity/environment visible at all times. Entering a tenant through a support grant must show a prominent tenant, reason, scope and expiry banner with a clear exit action. Platform shell colors never inherit tenant theme.

### Marketing and identity shells

Marketing pages use a public navigation and footer, structured content widths, responsive menus and clear CTA hierarchy. Article reading width and marketing landing grids may differ without introducing another token palette.

Signup and recovery use focused forms with clear headings, progress where needed, labels, help/errors, support routes and recoverable provisioning states. Do not copy names, contacts or values from references.

### Subscriber and captive shells

Subscriber pages put connection status, active plan, invoices/payments and support tasks before account administration. Captive pages are phone-first, lightweight and focused on the currently valid plan purchase or voucher redemption. Payment confirmation and activation states must be explicit and safe to retry.

Tenant logo and approved brand choices appear only where a tenant context is verified. Never trust a portal URL parameter as proof of tenant identity.

### Documents and messages

Email and PDF use renderer-specific token output, not browser CSS variable assumptions. Resolve the theme/version when the artifact is created and preserve that snapshot for historical records. Tenant content must not change authoritative totals, tax/legal identity, payment state, security statements or consent requirements. Escape text and URLs; use only the sanitized, constrained email header/footer subset.

## 7. Tenant theme and white-label contract

### Approved tenant-facing inputs

The v1.0 schema places tenant-writable values under brand, auth, email and plan. Unknown properties are rejected. Asset references are tenant-owned IDs, never arbitrary URLs or file paths.

| Input | v1.0 intent | Guardrail |
|---|---|---|
| Brand name | Tenant’s displayed name, 1–80 characters | Escape as text; do not replace platform identity/security/legal disclosures. |
| Light logo and square mark | Tenant-owned assets; dark logo optional | Validate and preview on both surfaces/modes; use a readable fallback. |
| Favicon | Optional tenant asset | Validate and generate required sizes server-side. |
| Primary color | Required six-digit sRGB hex | Server derives mode-specific action/text/tint/chart roles; reject invalid or inaccessible output. |
| Secondary accent | Optional six-digit sRGB hex in schema/docs | **Not runtime-ready:** the supplied derive function does not consume it. Do not expose it until behavior and checks are implemented. |
| UI/display font | Approved enums only | Font choices are bounded; no arbitrary file or CSS. |
| Shape | sharp, default or soft | Preset may change only specified medium/large radii. The archive still labels the preset set proposed; keep default until values are signed off for runtime. |
| Default density | comfortable or compact | Touch remains automatic; never saved as a brand override. |
| Login image | Optional asset | Apply a text-contrast scrim and mobile crop preview. |
| Email header/footer | Optional sanitized HTML subset, max 8,000 characters each | Allow only safe approved markup/content; no scripts, arbitrary CSS, legal address or unsubscribe overrides. |
| White-label flag | Optional setting available on every tier to preserve the same feature set | When enabled, remove only MylesNet product attribution from allowed tenant-facing footer/email areas. Keep legal and security notices. |

Schema syntax is strict: brand colors are six-digit sRGB hex values; tenant assets use asset IDs matching the package schema. Unknown keys and arbitrary URLs are rejected.

Tenant themes cannot set status colors, focus, security/sensitive-data meaning, spacing scale, type sizes, layout grid, touch target size, custom CSS, arbitrary fonts or shell/navigation structure.

### Custom domain is a separate product setting

The archive schema includes plan.customDomain and its documentation says CNAME plus paid-tier access. Both conflict with MylesNet’s already approved product decision. The MylesNet contract is:

- Every tier can use a tenant-owned captive portal domain.
- Offer MylesNet subdomain, own domain and buy-domain paths.
- For an own domain, issue a tenant-specific DNS A record, show “Awaiting DNS,” and provide an explicit Verify DNS action.
- Provision HTTPS after verification.
- Keep prior addresses serving until the Owner confirms payment callbacks/integrations have moved; then provide an explicit Stop serving action.
- Buy a domain opens a supported external registrar, then returns the tenant to DNS setup.
- Domain ownership, DNS/TLS readiness and host-to-tenant mapping live in the domain control plane, not in CSS variables. Do not let the schema’s plan.customDomain field reintroduce a tier gate.

The archive field’s persistence/normalization must be resolved before theme schema implementation. Domain is not a visual token and does not affect palette compilation.

### Safe publish lifecycle

1. Resolve authenticated membership or verified hostname server-side.
2. Validate tenant input against the versioned allowlist and asset ownership.
3. Derive Light and Dark theme values on the server and run the required contrast/asset checks.
4. Show a preview across affected tenant surfaces and both modes. Keep the last published theme live if validation fails.
5. Publish an immutable theme version and preserve rollback.
6. Cache by tenant ID, theme version, surface, resolved mode, schema/compiler version; invalidate on publish, rollback, host change and asset removal.
7. Render the resolved theme before first paint. Avoid flashing platform defaults before tenant theme loads.
8. On failed/stale resolution, show the last valid theme only if the policy permits; otherwise use a safe platform default and a recoverable error.

## 8. Component and interaction contract

The package includes component-token files for button, input, badge, card, table and navigation. Its React UI library is not built. Package doc03 is the component behavior specification to implement and maintain alongside shared tokens.

Every component contract records purpose, allowed surfaces, variants, sizes, token inputs, responsive and RTL rules, keyboard/focus behavior, accessible name/role/state, data states, localization, permissions/entitlements and visual acceptance criteria.

Core patterns include:

- Buttons, icon buttons and links: primary, secondary, outline, quiet and destructive; default, hover, focus-visible, pressed, disabled and busy.
- Inputs and selection: text, numeric, phone, password, currency, date, search, select, checkbox, radio and switch with associated label/help/error and documented keyboard behavior.
- Tables/data grids: sorted, filtered, selected, empty, loading, error, stale and permission-limited; accessible headings, bounded pagination and separate export permission.
- Metrics/charts: units, currency, period, source and freshness; text summaries and labels independent of color.
- Status badges and notices: platform-owned meanings, icon/text support and known/unknown/stale states.
- Dialogs, drawers, menus, tabs, pagination, file/logo upload, permission gates, live regions and captive checkout.
- Each realtime view distinguishes connecting, live, delayed, stale, reconnecting and offline.

All interactive and data components support the relevant states: default, hover, focus, pressed, selected, disabled, loading, success, warning, error, empty, stale/offline, read-only, permission-denied, feature-disabled, over-quota, trial/grace and suspended. Do not show actions to an unauthorized user when their presence itself leaks sensitive capability; where a disabled action is useful, explain why. Server-side authorization remains authoritative.

## 9. Accessibility and responsive behavior

- Target WCAG 2.2 AA unless product/legal review approves another target.
- Normal text contrast: at least 4.5:1; large text: 3:1; essential non-text controls/boundaries: 3:1.
- Validate all foreground/background combinations and states, including hover, pressed, selected, disabled, focus and error. The archive’s supplied tests do not prove exhaustive semantic contrast coverage.
- Brand primary action text is derived as dark ink or white based on contrast. Add a platform border where a fill fails the required visible-boundary contrast. Never hardcode white foreground text for every tenant brand.
- Focus is visible, platform-owned and cannot be overridden by tenant accent.
- Do not rely on color alone for status, quota, billing or security information.
- Support keyboard-only use, semantic landmarks, correct control names, screen-reader status announcements, 200% zoom, 320 px viewport, reduced motion and OS forced-colors.
- Minimum interactive target is 44 px; prefer 48 px for touch. Density cannot reduce target sizes.
- Use responsive reflow or intentional horizontal scroll; preserve readable identifiers and accessible full values when truncating.

The audit records specific coverage gaps: chart color-vision validation, exhaustive semantic contrast declarations, runtime asset checks and current test coverage must be completed before design-system release.

## 10. Repository application contract

The MylesNet workspace has a setup-only application shell; product features and theme runtime are still planning work. On 11 November 2026, the user explicitly directed this project to follow the repository target in the approved design-system package's `docs/08-repo-structure.md`. That direction supersedes the earlier single-app recommendation from the previous draft. The structure is reserved during planning; do not implement product routes or design-system behavior until authorized.

Use these monorepo boundaries:

~~~text
apps/
  web/                         # public marketing, sign-in handoff, subscriber and captive surfaces
  admin/                       # authenticated platform and ISP operator consoles, including theme editor/preview
  storybook/                   # design documentation and visual review with theme switcher
packages/
  design-system/
    tokens/
      primitive/
      semantic/
      component/
      platform/
    theme/
      schema/
      derive/
      fixtures/
      compile/
      validate/
      migrations/
    build/
    dist/                      # generated, ignored, never hand-edited
    docs/
    tests/
      visual/
    ui/                        # nested @mylesnet/ui package
    icons/
    fonts/
    lint/
convex/                        # tenant theme records, versions and assets
.changeset/                    # package semver and deprecation notes
~~~

- Keep source tokens, schemas, derivation, migrations and shared components in one design-system package; do not create one theme folder per panel.
- The `ui/` package imports generated output from `dist/`; apps import `@mylesnet/ui`; nothing imports source `tokens/` directly. Build tokens before consumers in local/CI builds.
- Keep generated output reproducible, ignored, and never hand-edited. Edit source tokens and theme schema/inputs, then run the package's token build command once the pipeline is installed.
- Each component lives in its own folder with its component source, CSS module, story, test, and behavior specification. Add the visual test harness to `apps/storybook`/`tests/visual` only in the authorized design-system implementation phase.
- A token rename deprecates the old token, names a replacement, and supplies a codemod; removal requires a major release. Schema changes add a new schema and a migration. Existing tenant themes remain pinned to `schemaVersion` until migrated.
- Convex stores tenant theme documents and immutable theme versions; publish compiled CSS to `/themes/{tenant}/{version}.css`. The admin app resolves tenant context and previews an approved theme; the web/app layout resolves tenant from its domain and loads the permitted critical theme block.
- Add lint rules for raw hex colors and direct primitive-token use inside components. Keep these controls inside shared tooling, not copied per application.
- Map Tailwind utilities to the generated CSS variables. A utility class must resolve through the same semantic/component API; it must not embed a second palette.
- Runtime tenant theme validation, compiler, immutable version storage, SSR resolution, cache invalidation and theme administration remain implementation work. They are not present in the setup shell.
- The application split is a repository boundary, not authorization to begin building panels. Keep visual primitives and tokens shared across all apps.

## 11. Governance, versioning and QA

- Design-system owners approve primitive/semantic token changes. Accessibility owner reviews contrast/focus/status changes. Product owner approves shell and interaction changes. Security/operations reviews tenant uploads, domain and support-context behavior.
- Version tokens/components and persisted theme schema deliberately. Additive compatible changes may be minor; corrective compatible changes patch; removal/renaming/incompatible behavior requires a major migration note and consumer inventory.
- Persist published themes as immutable, versioned records with schema/compiler version, hash and rollback pointer. Never silently drop an unknown tenant setting during migration.
- Review token changes with a before/after gallery across surfaces, modes, densities and key states; include contrast report, RTL/localization review, email/PDF output and migration notes.
- Required future QA: schema validation, token integrity, all semantic contrast pairs, theme derivation fixtures, asset sanitization, keyboard and screen-reader review, accessibility automation, screenshot regression, responsive widths, reduced motion, forced-colors and documentation consistency.
- Automated checks supplement human accessibility review. Pin tool versions and CI policies at implementation kickoff.

## 12. Readiness gaps recorded by the v1.0 audit

These are concrete source-package or implementation gaps, not requests to redesign the approved baseline:

1. Replace tokens.example.com schema IDs with stable MylesNet-owned identifiers before persistence.
2. Resolve the optional secondary accent mismatch: schema/docs accept and describe it, but derive.mjs ignores it.
3. Resolve the shape preset mismatch: schema allows three values, while presets.json still labels them proposed. Default remains safe until approved values are fixed.
4. Fix the generated dark JSON omission of common radius/density values; prove light/dark output parity.
5. Add exhaustive contrast declarations/checks and validate chart color-vision distinction. The current tests do not prove those claims.
6. Normalize the package’s plan.customDomain field against the separate MylesNet domain model and all-tier availability; retain the A-record/Verify DNS/TLS lifecycle above.
7. Confirm attribution behavior for white-label settings without introducing tier-specific theme features.
8. Source approved logo and font assets. Neither is included in the ZIP.
9. Build the React component library, runtime theme compiler/validator, theme editor, tenant storage/cache/SSR wiring, lint rule, component catalog and visual/accessibility CI.
10. Align docs/04's Noto Sans Arabic guidance with the tenant font enum before adding Arabic/RTL; English and Kiswahili remain the v1 language scope.
11. Treat shell-overview.dc.html as a non-runnable composition reference; do not ship its remote font or sub-11px styles.

No dependency install, package build or test run was performed during this documentation pass. The package’s README test claim is not independent validation.

## 13. Related project documents

- [Exhaustive ZIP audit](MYLESNET-DESIGN-SYSTEM-V1.0-AUDIT.md)
- [Product and domain decisions](READINESS-DECISIONS.md)
- [Technical specification, §7](TECHNICAL-SPECIFICATION.md)
- [Observed reference platform reference evidence](DESIGN-REFERENCE.md)
- [Product scope](PRODUCT-SCOPE.md)
