# VENDRR — Service Decisions

Each row maps a functional need to the specific service or tool we've committed to.

---

## Frontend & API

| Need | Service | Notes |
|------|---------|-------|
| Frontend hosting | **Vercel** | Next.js deployment, CDN, edge middleware |
| API layer | **Vercel** (Next.js API Routes) | Serverless functions, same deployment as frontend |
| AI / LLM | **Anthropic** (Claude via SDK) | Library import inside API routes — not a separate service |

---

## Backend Services (Railway)

All three modules run as a single Node.js process on Railway to start. Split into separate services later if one scales beyond the others.

| Need | Service | Notes |
|------|---------|-------|
| Real-time / WebSocket | **Railway** (Socket.io or ws) | Self-hosted; upgrade to Pusher or Ably if operational overhead grows |
| Workflow Engine (cron, DAGs, queues) | **Railway** (BullMQ + node-cron) | Self-hosted; upgrade to Inngest or Trigger.dev if complexity warrants it |
| Integration Hub (OAuth, webhooks, sync) | **Railway** (Express + adapters) | Self-hosted; upgrade to Nango or Paragon if connector count grows |

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
