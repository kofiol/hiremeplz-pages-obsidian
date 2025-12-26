# 🧵 Trigger.dev — Job Search Agent Workflow (`job_search.run`)

## 🎯 Goal
Produce a **high-quality ranked shortlist** of jobs that match the user’s profile and constraints.

## ✅ Inputs
From backend trigger payload:
- `team_id`
- `user_id`
- `platforms` (default from `user_preferences.platforms`)
- `tightness` (1–5)
- `exploration` (`low|medium|high`)

From Supabase:
- profile snapshot(s)
- structured skills/experience/education
- wage preferences + constraints

## 🧩 Steps (persisted in `agent_run_steps`)
1. `load_profile`
2. `plan_queries`
3. `fetch_jobs_upwork` (if enabled)
4. `fetch_jobs_linkedin` (if enabled)
5. `normalize_and_dedupe`
6. `score_and_filter`
7. `write_jobs_and_rankings`
8. `emit_notifications`

## 🔧 Tools
- Job Source Router (single integration surface)
  - Apify adapters:
    - Upwork jobs search + parsing
    - LinkedIn jobs search + parsing
  - BrightData adapters (fallback)

The router returns provider-agnostic raw results, then the workflow runs normalization + ranking and writes canonical `jobs` + `job_rankings`.

## 🧹 Normalization rules
- Always set canonical fields (see Architecture → Canonical Data Contracts).
- `canonical_hash`:
  - `sha256(team_id + platform + platform_job_id)`
  - fallback when missing external ID: hash of normalized title + normalized description + apply_url

## 🧮 Scoring + tightness
Scoring is defined in `06 🤖 AI Agents/07 Ranking & Filtering Logic.md`.

Workflow must:
- compute `job_rankings` for every candidate job
- filter jobs below the tightness threshold
- write top K jobs (default K=50) into the shortlist for dashboard

## 📤 Outputs
- `jobs` inserts/upserts
- `job_rankings` inserts
- optional `embeddings` generation for top N jobs
- notifications for:
  - new job with score ≥ 85
  - run failed

