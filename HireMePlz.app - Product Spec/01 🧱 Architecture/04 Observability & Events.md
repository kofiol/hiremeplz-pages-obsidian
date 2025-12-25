# 🧱 Architecture — Observability & Events

## 🧾 What gets logged (canonical)
All significant actions create events in Supabase.

### User events
- onboarding step completion
- agent trigger clicks
- job opened / applied link clicked
- cover letter copied / injected

### Agent events
- each workflow run creates `agent_runs`
- each step creates `agent_run_steps`
- key metrics are stored in `agent_runs.outputs`

## 📊 Minimal event schema
Store events in `public.events` (see Supabase schema page) with:
- `team_id`, `user_id`
- `event_type`
- `payload` (jsonb)
- `created_at`

## 🔔 Notifications
- Notifications are requested by workflows via backend API
- Stored in `public.notifications`
- Delivered to UI via Supabase Realtime

## 🧭 Run progress
The dashboard subscribes to:
- `agent_runs` updates
- `agent_run_steps` inserts

This gives live progress without polling.

## 🚨 Error reporting
- Sentry in frontend + backend
- Workflow errors are persisted in `agent_runs.error_text`

