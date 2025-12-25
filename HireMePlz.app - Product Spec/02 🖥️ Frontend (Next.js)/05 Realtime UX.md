# 🖥️ Frontend — Realtime UX

## ✅ What is realtime in this product
Realtime is used only for:
- agent run progress
- new jobs becoming available
- new messages/feedback ingestion

## 🔔 Subscriptions
The dashboard subscribes to:
- `agent_runs` (updates)
- `agent_run_steps` (inserts)
- `jobs` (inserts)
- `messages` (inserts)
- `feedback` (inserts)

## 🧭 UI patterns
- “Agent running” banner with step timeline
- Live counter updates (jobs ingested, shortlist size)
- Toast notifications for high-score jobs

## 🧯 Failure handling
- If realtime disconnects:
  - show “Live updates paused”
  - fall back to periodic refresh in the UI (every 60s)

