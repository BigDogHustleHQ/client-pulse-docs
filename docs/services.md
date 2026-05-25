# VENDRR — Service Decisions

Each row maps a functional need to the specific service or tool we've committed to.

---

## Frontend & API

| Need | Service | Notes |
|------|---------|-------|
| Frontend hosting | **Vercel** | Next.js deployment, CDN, edge middleware |
| API layer | **Railway** (NestJS + GraphQL) | NestJS 11, Apollo Server 5, code-first schema, port 4000 |
| AI / LLM | **Anthropic** (Claude via SDK) | Library import inside API routes — not a separate service |
| Auth | **Clerk** | Sign-in, sign-up, Google SSO — see `docs/features/auth.md` |

---

## Backend Services (Railway)

All three modules run as a single Node.js process on Railway to start. Split into separate services later if one scales beyond the others.

| Need | Service | Notes |
|------|---------|-------|
| Real-time / WebSocket | **Railway** (Socket.io 4) | Confirmed — upgrade to Pusher or Ably if operational overhead grows |
| Workflow Engine (cron, DAGs, queues) | **Railway** (BullMQ 5 + node-cron) | Confirmed — upgrade to Inngest or Trigger.dev if complexity warrants it |
| Integration Hub (OAuth, webhooks, sync) | **Railway** (Express 5 + adapters) | Confirmed — upgrade to Nango or Paragon if connector count grows |

---

## Data & Storage

| Need | Service | Notes |
|------|---------|-------|
| Primary database | **Supabase** | PostgreSQL — tenant data, audit logs |
| Blob / file storage | **Supabase** | Supabase Storage — media uploads, generated sites |
| Cache & pub-sub | **Upstash** | Redis — sessions, real-time channels |

---

## External APIs (third-party, you hold the keys)

| Integration | Provider |
|-------------|---------|
| Reservations | OpenTable, Resy |
| POS | Square, Toast |
| SMS / Voice | Twilio |
| Maps & Business data | Google |
| Reviews | Yelp |
| Social | Instagram, TikTok |
