# 🗄️ Supabase — Realtime & Notifications

## ✅ Realtime channels used (MVP)
- `agent_runs` updates
- `agent_run_steps` inserts
- `jobs` inserts
- `messages` inserts
- `feedback` inserts
- `notifications` inserts/updates

## 🔔 Realtime usage rules
- Realtime is *UI convenience*, not a source of truth.
- UI always has a REST fallback.

## 🔕 Notification types
Defined `notifications.type` values:
- `job.high_score_added`
- `agent.run_failed`
- `message.inbound_received`
- `feedback.action_required`
- `limit.daily_reached`

## 🧾 Payload contract
`notifications.payload` must include:
- `title`
- `body`
- `deep_link` (e.g. `/app/jobs/<id>`)
- `severity` (`info|warning|critical`)

