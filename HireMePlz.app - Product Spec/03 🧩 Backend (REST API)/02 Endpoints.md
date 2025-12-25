# 🧩 Backend — REST Endpoints (MVP)

All endpoints are under `/api/v1/*`.

## 🔎 Jobs
- `GET /jobs`
  - query: `platform`, `min_score`, `status`, `q`, `limit`, `cursor`
  - returns canonical jobs + latest ranking
- `GET /jobs/:id`
  - returns job + ranking breakdown + linked application/cover letters

## 🚀 Applications
- `GET /applications`
- `POST /applications`
  - body: `job_id`, `status` (default `shortlisted`), optional notes
- `PATCH /applications/:id`
  - body: `status`, `notes`, `next_follow_up_at`

## ✍️ Cover letters
- `GET /cover-letters`
- `POST /cover-letters/generate`
  - body: `job_id`, optional settings override
  - side effect: triggers workflow

## 💬 Messages & feedback
- `GET /messages`
  - query: `thread_id`, `platform`, `needs_reply`
- `GET /feedback`
- `PATCH /feedback/:id`
  - body: `status` (`resolved|action_required`), `notes`

## 💰 Earnings
- `GET /earnings`
- `POST /earnings`
  - manual entry

## 🤖 Agents
- `POST /agents/job-search/trigger`
  - body: optional `platforms`, `tightness`, `exploration`
  - side effect: triggers workflow
- `POST /agents/upwork-profile/trigger`
  - side effect: triggers workflow
- `POST /agents/interview-prep/session`
  - returns session params for OpenAI Realtime

## 🧾 Agent runs
- `GET /agent-runs`
- `GET /agent-runs/:id`

## 👥 Team
- `GET /team`
- `POST /team/invite` (leader)
- `POST /team/accept-invite`
- `PATCH /team/members/:userId` (leader)

## ⚙️ Settings
- `GET /settings`
- `PATCH /settings`
  - agent settings payload stored in `user_agent_settings`

## 🧵 Extension handoff
- `POST /apply-sessions`
  - body: `job_id`, `cover_letter_id`
  - returns `apply_session_token`
- `GET /apply-sessions/:token`
  - returns cover letter payload for injection

