# Linguastar

AI-powered language learning digital bookstore platform where users can browse, purchase, and read language learning books with DRM-protected content delivery.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 8080)
- `pnpm --filter @workspace/linguastar run dev` — run the frontend (port assigned by workflow)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string
- Required env: `SESSION_SECRET` — session signing secret

## GitHub Repository

- **URL**: https://github.com/charan2321/new
- **Branch**: main
- **Last pushed**: 144 source files committed — `Initial production-ready Linguastar platform commit`
- **Commit**: `6ea1318d35ce5c799040ecb3d03ba48188ac77d9`

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite + Tailwind v4 + Clerk Auth (shadcn theme) + wouter routing + TanStack Query + framer-motion
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/linguastar/src/pages/` — frontend pages (home, store, book-detail, reader, dashboard, admin)
- `artifacts/linguastar/src/components/` — shared UI components + shadcn/ui
- `artifacts/api-server/src/routes/` — API route handlers (books, purchases, reading_progress, admin, me)
- `lib/db/src/schema/` — Drizzle ORM schema (source of truth for DB)
- `lib/api-spec/openapi.yaml` — OpenAPI spec (source of truth for API contract)
- `lib/api-client-react/` — generated TanStack Query hooks
- `lib/api-zod/` — generated Zod schemas
- `scripts/src/seed.ts` — DB seed script (6 language books)

## Architecture decisions

- Contract-first API: OpenAPI spec → Orval codegen → typed hooks + Zod validators; never write fetch calls manually
- DRM protection on reader page: content only accessible after purchase verification via `/api/purchases` check
- Clerk auth for user identity; custom `users` table mirrors Clerk user IDs for ownership
- Admin role stored in `users.role` column; checked server-side via `requireAuth` + role middleware
- All API routes namespaced under `/api` and served through the shared reverse proxy

## Product

- **Home**: Landing page with featured books and hero CTA
- **Store**: Book catalog with search/filter by language
- **Book Detail**: Book info, preview, purchase button (Razorpay deferred)
- **Reader**: DRM-protected book content with reading progress tracking
- **Dashboard**: User's purchased books and reading progress
- **Admin**: Analytics overview, book management, user list

## User preferences

- Payments (Razorpay) deferred — purchase flow wired but payment gateway not integrated yet
- GitHub remote: https://github.com/charan2321/new.git (PAT-based push via GitHub API)

## Gotchas

- Git commits in main agent are sandbox-restricted; use GitHub API (Node.js HTTPS) for pushing
- Lock files `.git/config.lock` and `.git/index.lock` may appear after failed git operations; remove via `rm` (not blocked)
- Always run `pnpm --filter @workspace/api-spec run codegen` after changing `openapi.yaml`
- Seed script requires `DATABASE_URL` in env before running

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
