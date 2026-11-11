# Public ISP website reference review

Reviewed in the browser on 10 November 2026. This covers the public marketing site and ISP self-registration flow. It is separate from the signed-in tenant console review in `tenant-console-review.md`.

## Route coverage

- Captured the homepage and all 307 non-root URLs listed in the source sitemap: 308 public pages total.
- Also captured the cookie banner, Features/Solutions/Resources navigation menu states, and registration/recovery screens.
- Full-page captures and a searchable gallery remain in the local-only `docs/reference-images/public-site-screenshots/` directory. Its `route-manifest.csv` maps each captured route or state to an image; the gallery contains 325 screenshots.
- The route collection covers product and feature pages, pricing, solutions, country pages, integrations, calculators/tools, comparisons, blog and changelog pages, and legal/content pages.

## Visual and flow observations

- Public pages use a dark charcoal presentation with green accents, Inter typography, bold display headings, and a centered content grid. Marketing pages combine broad hero regions, rounded cards, feature grids, and repeated calls to action.
- Registration uses a step-based flow with a dark theme and a Light mode control. Country selection defaults to Uganda, Africa/Kampala, and UGX; a video platform appears among referral choices.
- The captured signup sequence covers contact verification, account-address selection, country/referral settings, password handoff, and provisioning status.
- Signup completed in the inspected environment, showing an admin account provisioned, a welcome message sent, and an account-ready state. Personal contact information and tenant-specific addresses are intentionally omitted from this report.

## Scope notes

This is rendered-browser evidence, not a source-code inspection or complete computed-token inventory. Captures reflect the visible content/loading state at the time. Signup screenshots contain personal account information and remain in the local-only ignored gallery; they must not be copied to public documentation or synthetic product data.
