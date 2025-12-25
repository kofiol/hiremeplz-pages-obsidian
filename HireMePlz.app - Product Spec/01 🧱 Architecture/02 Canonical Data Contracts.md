# 🧱 Architecture — Canonical Data Contracts

## ✅ Canonical entities (MVP)
- `profiles`: user identity + plan + timezone
- `teams`, `team_members`: 2–5 team access
- `jobs`: normalized postings across LinkedIn + Upwork
- `job_rankings`: scoring results per search run
- `applications`: pipeline record
- `cover_letters`: generated outputs per job
- `messages`: inbound/outbound communication capture (email/DM ingest)
- `feedback`: derived from messages + manual input
- `earnings`: manual or imported records
- `agent_runs`, `agent_run_steps`: audit trail of workflows
- `embeddings`: vector memory for jobs + user profile

## 🧱 Canonical job object (normalized)
All jobs must map into this minimal canonical shape.

**Required fields**
- `platform`: `upwork | linkedin`
- `platform_job_id`: platform external ID (string)
- `title`
- `description`
- `apply_url`
- `posted_at` (if unknown, use `fetched_at`)
- `currency` (default `USD` if platform omits)

**Budget fields**
- `budget_type`: `fixed | hourly | unknown`
- `fixed_budget_min`, `fixed_budget_max`
- `hourly_min`, `hourly_max`

**Client fields (when available)**
- `client_country`
- `client_rating`
- `client_hires`
- `client_payment_verified` (bool)

**Tags/metadata**
- `skills` (array of strings)
- `seniority` (enum-ish: `junior|mid|senior|unknown`)
- `category` (string)
- `source_raw` (jsonb) — original extraction snapshot

## 🧠 Canonical ranking output
Ranking is stored separately so jobs remain stable.

- `job_rankings.score` is a number in `[0, 100]`.
- `job_rankings.breakdown` is JSON:
  - `skill_match`
  - `budget_fit`
  - `seniority_fit`
  - `client_quality`
  - `recency`
  - `fraud_risk`
  - `platform_confidence`
- `job_rankings.tightness` is the user-selected strictness (1–5).

## 🧾 Application pipeline states (MVP)
`applications.status`:
- `shortlisted`
- `ready_to_apply`
- `applied`
- `in_conversation`
- `interviewing`
- `won`
- `lost`
- `archived`

## 🧠 Copilot data rule
Copilot summaries are derived views only:
- stored as `copilot_summaries` (optional) or computed on demand
- never referenced as a canonical source by other agents

