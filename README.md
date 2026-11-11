# MylesNet

MylesNet is a planned multi-tenant ISP operations SaaS. This repository currently contains the planning documentation and a setup-only pnpm/Turborepo foundation. Product features, live integrations, and production deployments have not started.

## Repository layout

```text
apps/
  web/                    public site and shared Next.js shell
  admin/                  reserved admin app boundary
  storybook/              reserved design documentation/visual review app
packages/
  design-system/          design tokens, theme tooling, and shared UI boundary
  typescript-config/      Shared TypeScript settings
convex/                   reserved backend workspace
docs/                     Product, architecture, access, and design specifications
```

## Local development

Requirements: Node.js 20.9 or later and pnpm 11.

1. Copy `.env.example` to `.env` only if you need to rebuild your local configuration; an ignored local `.env` is already created for this workspace.
2. Fill in development-only WorkOS credentials. The checked-in example contains placeholders, never secrets.
3. Install dependencies with `pnpm install`.
4. Start the web shell with `pnpm dev`.

The scaffold uses the Convex development deployment recorded in `.env`. It does not connect to production and does not yet initialize Convex or WorkOS in application code.

## Planning references

Start with `AGENTS.md`, then use the specifications in `docs/`. The approved design-system source and captured reference galleries are preserved locally under ignored `docs/reference/` and `docs/reference-images/` paths because they include source material and live sample data. Do not commit these private reference files.

The end-to-end phased roadmap is [Implementation Plan](docs/IMPLEMENTATION-PLAN.md). It is a planning handoff, not authorization to begin product implementation.

## Project stage

This is a planning/setup repository. Do not build product workflows until the user explicitly starts the development phase. See `AGENTS.md` for the end-to-end project charter and delivery gates.
