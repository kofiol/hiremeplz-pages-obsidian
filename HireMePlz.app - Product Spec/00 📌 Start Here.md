# 📌 HireMePlz.app — Technical Product Spec (MVP)

**Project name:** HireMePlz / `hiremeplz.app`  
**Primary user:** high-volume solo freelancer applying to short gigs & medium projects (LinkedIn + Upwork)  
**Secondary user:** small freelancing team (2–5) with a team leader

## 🎯 Product promise (non-fluff)
HireMePlz turns job search + outreach into a repeatable pipeline:
- An AI job search agent pulls *hundreds* of roles and produces a **ranked shortlist** of high-fit opportunities.
- A cover letter agent generates tailored proposals on demand.
- A unified dashboard tracks jobs → applications → messages/feedback → earnings.
- A read-only dashboard copilot helps humans interpret the pipeline (never a data source for other agents).

## ✅ Non‑negotiables
- **Supabase is the only source of truth.**
- **Trigger.dev workflows are the only writers of agent-produced data** (jobs, cover letters, inbound messages, feedback, agent-run artifacts).
- The **Serverless REST API** (Next.js) is the only gateway for:
  - Business rules (tiers/limits)
  - AuthZ enforcement
  - Side effects (triggering workflows, sending notifications)
- Agents do **not** directly talk to each other; they coordinate via:
  - canonical tables in Supabase
  - events/logs
  - embeddings + vector search
- **Dashboard Copilot is read-only and human-facing only**:
  - ❌ no schema authority
  - ❌ no canonical reshaping
  - ❌ not used by other agents

## 🧱 Exact stack (defined)
**Frontend**
- Next.js (App Router) + TypeScript
- Tailwind CSS
- `@supabase/supabase-js` (browser client)
- Realtime via Supabase channels
- Analytics: PostHog
- Error monitoring: Sentry

**Backend**
- Next.js Route Handlers as REST API under `/api/v1/*`
- Supabase Auth JWT verification (server-side)
- Rate limits + usage caps via Vercel KV (Upstash Redis)

**Data**
- Supabase Postgres + RLS
- Supabase Storage (CVs + scraped snapshots)
- `pgvector` for embeddings

**Workflows**
- Trigger.dev (orchestrator)
- Job Source Router (provider adapters: Apify, BrightData, future)
- Mailgun inbound webhook → Trigger.dev

**LLM / Voice**
- OpenAI Agents SDK (TypeScript) `@openai/agents` for agent loop, tools, guardrails, handoffs, tracing
- OpenAI models via Agents SDK runner (`run()` / `Runner`)
- OpenAI Realtime (WebRTC) for interview prep (beta), via Realtime Agents

## 📚 Glossary (canonical)
- **Job**: normalized posting (platform-agnostic shape) stored in `public.jobs`.
- **Application**: user’s intent/outreach record (`public.applications`).
- **Cover letter**: generated text tied to `(job_id, user_id)` (`public.cover_letters`).
- **Agent run**: one execution of an agent workflow, with steps/logs (`public.agent_runs`).
- **Tightness**: user-controlled strictness of job filtering/ranking (1–5).

## 📂 Spec structure
This spec lives in the following sections:
1. 🧱 Architecture
2. 🖥️ Frontend (Next.js)
3. 🧩 Backend (REST API)
4. 🗄️ Supabase (DB, RLS, Storage, Realtime)
5. 🧵 Trigger.dev (workflows)
6. 🤖 AI Agents (incl. settings & ranking)
7. 💼 User & business model

## 🚦MVP scope boundaries
**In scope**
- LinkedIn + Upwork sourcing
- Ranked shortlist + cover letters
- DM/email ingest into dashboard
- Team support (2–5)
- Interview prep (beta)
- Upwork profile optimizer agent (inbound optimization)

**Out of scope (explicit)**
- Auto-applying to jobs
- Sending DMs automatically
- Managing payments/contracts inside HireMePlz
