# Vendrr — Product Spec

> The org and repo names use the `client-pulse` prefix. The product is **Vendrr** everywhere else.

---

## What is Vendrr?

Vendrr is an AI-driven growth and operations platform for local businesses — restaurants, retail shops, service providers. It gives owners a single dashboard to manage reservations, customer relationships, marketing automations, and third-party integrations through a unified interface backed by an AI agent.

---

## Repository Map

| Repo | Purpose | Deployed to |
|------|---------|-------------|
| `client-pulse-frontend` | Next.js 16 UI — App Router, React 19, Tailwind v4 | Vercel |
| `client-pulse-api` | GraphQL API — NestJS 11, Apollo Server 5, code-first schema | Railway |
| `client-pulse-backend` | Backend service — Socket.io, BullMQ, Integration Hub (single Node.js process) | Railway |
| `client-pulse-docs` | Documentation only — Markdown + draw.io diagrams | — |

---

## Tech Stack

### Frontend (`client-pulse-frontend`)
- **Next.js 16** (App Router) + **React 19**
- **TypeScript 5** strict mode
- **Tailwind CSS v4** via `@tailwindcss/postcss`
- **Clerk** — auth provider (sign-in, sign-up, SSO/Google)
- **Zustand** — client-side auth state (`useAuthStore`)
- **TanStack Query** — server state / data fetching
- **Jest 30** + React Testing Library — co-located `.test.tsx` files
- Middleware file convention: `proxy.ts` (Next.js 16 rename from `middleware.ts`)

### API (`client-pulse-api`)
- **NestJS 11** + **TypeScript 5**
- **GraphQL** code-first via `@nestjs/graphql` + Apollo Server 5
- Schema auto-generated at `src/schema.gql` on startup — do not edit by hand
- Feature module pattern: `<feature>.module.ts` / `.resolver.ts` / `.service.ts`
- **Jest 30** + `ts-jest` — `.spec.ts` test files
- GraphQL endpoint: `POST /graphql` (port 4000 in dev)

### Backend (`client-pulse-backend`)
- **Node.js 24** + **TypeScript 6** — single Express process
- **Socket.io 4** — WebSocket / real-time push
- **BullMQ 5** + **node-cron** — job queue and scheduled triggers (Redis-backed)
- **Express 5** — Integration Hub webhook routes and OAuth adapters
- **Jest 30** + `ts-jest` — 100% coverage threshold enforced
- Port 3001 in dev

---

## Feature Status

### SID-60 — Monorepo PostgreSQL and Row-Level Security
**Status: Merged to main (api + backend)**

- NestJS + GraphQL scaffolding live in `client-pulse-api`
- Node.js backend service (WebSocket + Workflow Engine + Integration Hub) live in `client-pulse-backend`
- Database: Supabase (PostgreSQL) — Row-Level Security to be layered in as tenant models are added

### SID-61 — Auth, Sessions, and Tenant Scoping
**Status: In progress — frontend branch `feature/sid-61-auth-sessions-and-tenant-scoping`**

- Clerk auth fully implemented in frontend (login, registration, Google SSO, SSO callback)
- Zustand `useAuthStore` managing client-side session state
- `proxy.ts` Clerk middleware protecting all non-public routes
- API and backend tenant-scoping work not yet started
- See [`docs/features/auth.md`](features/auth.md) for implementation details

---

## Data & Services

See [`docs/services.md`](services.md) for the full service-to-provider mapping.

Key decisions:
- **Database**: Supabase (PostgreSQL)
- **Blob storage**: Supabase Storage
- **Cache / pub-sub**: Upstash (Redis)
- **Auth**: Clerk
- **Frontend hosting**: Vercel
- **API + backend hosting**: Railway

---

## Architecture Diagram

See [`docs/architecture/vendrr-system-architecture.drawio`](architecture/vendrr-system-architecture.drawio).
