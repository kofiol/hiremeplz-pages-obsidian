# 🗓️ Week 4 — Job Source Router + Job Search MVP

## 🎯 Goals (measurable)
- [ ] Job Search workflow ingests jobs from Upwork + LinkedIn.
- [ ] Provider router exists so sources can failover cleanly.
- [ ] Canonical `jobs` + `job_rankings` are written to Supabase.
- [ ] Jobs page shows ranked shortlist with filters.

## ✅ Deliverables
- [ ] Job Source Router module with provider adapters.
- [ ] Trigger.dev `job_search.run` workflow implemented end-to-end.
- [ ] `GET /api/v1/jobs` and `GET /api/v1/jobs/:id` implemented.
- [ ] `/app/jobs` list + `/app/jobs/[jobId]` details implemented.

## 🧪 Acceptance criteria
- [ ] Triggering job search creates an `agent_runs` row with step updates.
- [ ] At least 50 jobs ingest in a run (in dev/test mode) and appear in UI.
- [ ] Jobs show score (0–100) and breakdown highlights.
- [ ] Dedupe works: re-running does not create duplicates.

## 🧱 Checklist (tight)
### Day 1 — Router foundation
- [ ] Define provider-agnostic raw job contract (`RawJob`).
- [ ] Implement router interface: `search(platform, queryPlan)`.
- [ ] Implement health + fallback rules (timeouts, retries, provider priority).

### Day 2–3 — Providers v1
- [ ] Implement Apify adapter for Upwork search.
- [ ] Implement Apify adapter for LinkedIn search.
- [ ] Implement BrightData adapter as fallback (same `RawJob` output).

### Day 4 — Canonicalization + ranking v1
- [ ] Map `RawJob` → canonical `public.jobs` fields.
- [ ] Implement ranking according to `06 🤖 AI Agents/07 Ranking & Filtering Logic`.
- [ ] Write `jobs` + `job_rankings` in workflow.

### Day 5 — UI + API
- [ ] Build `GET /api/v1/jobs` with filters (platform, min_score, q).
- [ ] Render `/app/jobs` shortlist with filters.
- [ ] Render job detail view with breakdown explanation.

### Weekend — Quality pass
- [ ] Add fraud/suspicion rules + hide-by-default behavior (tightness ≥ 3).
- [ ] Add embeddings generation for top N jobs (optional if time).

## 🧯 Cut list (if time slips)
- [ ] Ship only Upwork first; add LinkedIn in Week 4 weekend.
