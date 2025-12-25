# 🧩 Backend — Webhooks & Side Effects

## 🧵 Triggering workflows
The backend triggers workflows by calling Trigger.dev with:
- `event_name`
- `payload` containing `team_id`, `user_id`, and any parameters

Examples:
- `agent.job_search.triggered`
- `agent.cover_letter.triggered`
- `agent.upwork_profile_optimizer.triggered`
- `mail.inbound.received`

## ✉️ Mailgun inbound
- Mailgun routes inbound emails to a backend webhook:
  - `/api/v1/webhooks/mailgun/inbound`
- Backend validates Mailgun signature.
- Backend forwards the email payload to Trigger.dev as an event.

## 🔔 Notifications (side effects)
Workflows never send notifications directly.
They call backend:
- `POST /api/v1/notifications`

Backend validates:
- workflow auth (service token)
- team/user existence

## 🧾 Audit
All side effects create `events` rows.

