# 🧩 Backend — REST API Rules

## ✅ Core responsibilities
- Verify Supabase JWT
- Enforce business rules:
  - subscription tier
  - daily limits
  - team permissions
  - profile completeness
- Expose strict canonical REST endpoints
- Emit events and trigger workflows

## 🧾 API style
- Base path: `/api/v1`
- JSON only
- Pagination: `limit`, `cursor`
- Sorting: `sort=created_at:desc`
- Errors:
  - `{ "error": { "code": "...", "message": "...", "details": {...} } }`

## 🔐 Auth
- `Authorization: Bearer <supabase_jwt>`
- Backend verifies JWT and resolves:
  - `user_id = auth.uid`
  - `team_id` from `profiles.team_id`
  - role from `team_members.role`

## 🧠 Business rule gates
- Job search trigger requires:
  - `profile_completeness_score >= 0.8`
  - daily limit available
- Cover letter trigger requires:
  - job exists and belongs to team
  - daily cover-letter limit available

## 🧨 Side effects policy
Only the backend can:
- trigger Trigger.dev workflows
- create notifications
- call external webhooks

Workflows can request side effects by calling backend endpoints that are explicitly designated for that.

