# 🧱 Architecture — Security, Privacy, Limits

## 🔐 Authentication
- Supabase Auth (email magic link)
- Frontend obtains Supabase JWT
- Backend verifies JWT on every `/api/v1/*` request

## 🛡️ Authorization model
- All tables are multi-tenant by `team_id`.
- RLS policies allow access only if `auth.uid()` is an active member of the row’s `team_id`.
- “Leader-only” actions:
  - manage team membership
  - manage billing plan
  - set team-wide agent schedules

## 🔑 Secrets & service role
- Only Vercel serverless routes and Trigger.dev workers use **service role**.
- No secrets in browser.
- External APIs (Apify, BrightData, Mailgun, OpenAI) are only called from workflows.

## 🚧 Rate limits & usage caps
**Where enforced:** backend API (not the frontend)
- Per-team daily caps by plan:
  - `job_search_runs_per_day`
  - `jobs_ingested_per_day`
  - `cover_letters_per_day`
  - `interview_sessions_per_day`
- Hard failures return `429` with a structured error body.

## 🧯 Data retention
- Raw platform snapshots can be large.
- Retention policy:
  - store raw extraction JSON for 30 days (configurable)
  - keep canonical `jobs` indefinitely
  - keep messages/feedback 180 days by default

## ⚖️ Compliance posture (MVP)
- Only collect what’s needed for product function.
- Provide user controls to delete:
  - CV file
  - platform snapshots
  - interview recordings/transcripts
- Do not attempt to bypass platform ToS via auto-apply or auto-messaging.

## 🧪 Abuse prevention
- Reject job content that looks like malware/phishing with a simple ruleset + LLM safety classifier.
- Mark suspicious jobs with `fraud_risk` breakdown and hide by default when tightness ≥ 3.

