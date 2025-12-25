# 🧵 Trigger.dev — Workflow Catalog (MVP)

## ✅ Workflows (defined)
1. 🔎 `job_search.run`
2. ✍️ `cover_letter.generate`
3. 📥 `email_ingest.mailgun`
4. 🔄 `profile_poll.linkedin`
5. 🔄 `profile_poll.upwork`
6. 🧾 `profile_parser.cv`
7. 🎯 `upwork_profile_optimizer.run`
8. 🎙️ `interview_prep.analyze_session` (beta)

## 🧾 Shared workflow conventions
Every workflow must:
- create `agent_runs` row at start
- create `agent_run_steps` rows per major step
- write all outputs into canonical tables
- update `agent_runs.status` to `succeeded|failed`

## 🔑 Credentials
Stored as Trigger.dev environment variables:
- `SUPABASE_URL`
- `SUPABASE_SERVICE_ROLE_KEY`
- `OPENAI_API_KEY`
- `APIFY_TOKEN`
- `BRIGHTDATA_API_KEY`
- `MAILGUN_SIGNING_KEY`

