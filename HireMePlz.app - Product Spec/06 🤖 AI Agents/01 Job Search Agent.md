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
- Apify actors (primary)
- BrightData (fallback)

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

