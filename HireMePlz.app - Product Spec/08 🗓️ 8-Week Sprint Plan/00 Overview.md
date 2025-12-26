# 🗓️ 8-Week Sprint Plan — Overview

## ⏱️ Capacity assumptions (yours)
- Solo builder
- ~5–6h focused work/day
- You code on weekends (use this to close gaps and ship)
- You use AI agents to accelerate implementation, but you still verify everything

## 🎯 MVP definition of done (Week 8)
- User can onboard (CV + skills + preferences), trigger job search, see ranked jobs, generate cover letters, and track applications.
- Inbound messages/feedback arrive in the dashboard (email ingest).
- Team mode (2–5) works with RLS.
- Daily limits are enforced.
- Copilot is read-only and guarded.
- Interview prep beta + Upwork profile optimizer exist (usable, not perfect).

## 🧱 Repo constraints (hard)
- Monorepo using `pnpm` + Turborepo.
- One Next.js app provides:
  - UI under `/app/*`
  - REST API under `/api/v1/*`
- Shared packages for:
  - DB types + Supabase clients
  - shared schemas/validation (Zod)
  - UI primitives

## ✅ Weekly rhythm (tight)
- Each week has:
  - **Deliverables** (what exists in repo)
  - **Acceptance criteria** (what must work)
  - **Checklist** (tasks with `[ ]`)
- You only start the next week when acceptance criteria pass.

## 🔥 Rule of execution
- If something is uncertain, implement the simplest version that satisfies canonical schemas and upgrade later.
- Always keep Supabase canonical data clean; avoid “temporary tables”.

