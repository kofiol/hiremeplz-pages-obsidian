# 🤖 AI Agents — Job Search Agent

## 🎯 Mission
Continuously discover and prioritize freelance opportunities that match the user, **without** forcing them to sift through hundreds of irrelevant listings.

## 🔎 Inputs (authoritative)
- `user_profile_snapshots` (CV + LinkedIn + Upwork)
- `user_skills`, `user_experiences`, `user_educations`
- `user_preferences` (rates, project types, platforms, tightness)
- historical outcomes:
  - `applications` statuses
  - `feedback` categories
  - `earnings`

## 🧰 Tools
- Job Source Router (primary interface)
  - Apify adapter (one provider)
  - BrightData adapter (one provider)
  - Future adapters (drop-in): new APIs/scrapers without changing canonical logic

**Router contract (hard)**
- Input: `QueryPlan` (platform, keywords, filters, paging strategy)
- Output: array of *raw* job results in a provider-agnostic shape
- Responsibility: retries, provider fallback, rate limits, and provider health
- Canonicalization is separate: raw → `public.jobs` happens in `normalize_job`

## 🧠 Core behavior (deterministic + AI)
### 1) Query planning
The agent generates a **query plan** containing:
- which platforms to query (`upwork`, `linkedin`)
- which search terms (3–10)
- which filters (budget, location/remote, posted time)
- exploration strategy

### 2) Fetch + normalize
- fetch raw results
- normalize into canonical job objects
- dedupe by `canonical_hash`

### 3) Score + filter
- compute `score` and `breakdown`
- filter by tightness threshold
- store results in `jobs` + `job_rankings`

## 🧯 Hard constraints
The agent must not:
- auto-apply
- send DMs
- write canonical schemas other than defined tables

## 📤 Outputs
- canonical jobs persisted to Supabase
- ranking breakdown explainable in UI
- notifications for high-score jobs


## 🧱 Implementation (OpenAI Agents SDK)
We implement this agent with the OpenAI Agents SDK for TypeScript: https://openai.github.io/openai-agents-js/

**Primitives used**
- `Agent`: holds instructions + tool access.
- Tools: TypeScript functions wrapped as function-tools with Zod validation.
- `Runner` / `run()`: runs the built-in agent loop until completion or `maxTurns`.
- Guardrails: validate inputs/outputs during the run.
- Tracing: enabled for debugging and evaluation.

**Where it runs**
- Runs inside `job_search.run` (Trigger.dev). The workflow creates an `agent_runs` row, then executes the SDK runner, then persists results.

**Tools (concrete)**
- `job_source_router.search(platform, queryPlan)` → calls provider adapters (Apify/BrightData/others)
- `normalize_job(raw)` → maps to canonical `public.jobs` fields
- `dedupe_jobs(canonical[])` → uses `canonical_hash`
- `score_job(job, profile, prefs)` → produces `job_rankings.breakdown`
- `write_jobs(canonical[])` / `write_rankings(scores[])` → Supabase write (service role)

**Guardrails (hard)**
- Input guardrail rejects runs if required profile fields are missing (mirrors backend completeness gate).
- Output guardrail enforces:
  - `score` in `[0,100]`
  - `breakdown` keys exist (see ranking spec)
  - no schema invention outside canonical tables

**Runner settings**
- `maxTurns` is capped (default 10) to avoid infinite loops.
- Runner metadata for traces includes `team_id`, `user_id`, `agent_run_id`.

