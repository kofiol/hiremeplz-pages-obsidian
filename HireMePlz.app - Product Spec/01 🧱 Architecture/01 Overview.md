# 🧱 Architecture — Overview

## 🎯 What this architecture optimizes for
- **High-volume throughput**: hundreds of jobs ingested every run, every day.
- **Canonical data discipline**: one schema, one truth, no agent-specific formats.
- **Strict boundaries**: UI reads; API enforces; workflows write; copilot interprets.
- **Auditability**: every agent run, input, output, and step is persisted.

## 🧩 Main components
### 1) Frontend (Next.js on Vercel)
- Dashboard UI + onboarding
- Manual triggers for workflows (job search, cover letters, interview prep, profile optimizer)
- Supabase Realtime subscriptions for run progress + new jobs/messages

### 2) Serverless REST API (Next.js Route Handlers)
- Verifies Supabase JWT
- Enforces: subscription tier, daily limits, team permissions
- Exposes **strict** REST endpoints for canonical data access
- Triggers Trigger.dev workflows by emitting events + payload
- The only layer allowed to cause side effects

### 3) Supabase (Postgres + RLS + Storage + Realtime)
- Canonical tables: jobs, applications, cover_letters, feedback, messages, earnings
- Agent observability: agent_runs, agent_run_steps, logs
- Vector memory: embeddings
- Storage: CV uploads, platform snapshot JSON/HTML

### 4) Trigger.dev (workflows)
- Job Search Agent (manual + scheduled)
- Cover Letter Agent (manual)
- Email ingest (Mailgun webhook)
- Pollers: LinkedIn/Upwork profile snapshots
- Interview prep analysis
- Upwork profile optimizer

### 5) External tools
- Apify actors for scraping/search
- BrightData where needed for anti-bot / stable extraction
- Mailgun inbound email routes
- OpenAI (text + realtime voice)

## 🔐 Trust & data boundaries
- **Browser**: can only read/write user-created artifacts (applications, notes) through API.
- **API**: authenticates and authorizes; does not run heavy scraping.
- **Workflows**: do scraping + LLM calls; write results to Supabase with service role.
- **Copilot**: read-only; receives summarized API outputs only.

## 📦 Canonical object philosophy
- All entities have:
  - `id` (UUID)
  - `team_id`
  - `created_at`
  - stable fields used by all agents
- Platform-specific data goes into:
  - `source_*` JSON columns or `raw_*` tables
- Canonical tables are designed for:
  - dashboard usage
  - consistent ranking
  - analytics/KPIs

## 🧭 Key invariants
- Jobs are written only by workflows.
- Cover letters are written only by workflows.
- Messages/feedback from DM/email are written only by workflows.
- Applications are user-driven records (manual “I applied” toggles).

