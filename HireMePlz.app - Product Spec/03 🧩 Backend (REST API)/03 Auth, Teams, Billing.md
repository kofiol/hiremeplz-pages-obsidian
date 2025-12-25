# 🧩 Backend — Auth, Teams, Billing Enforcement

## 🔐 Auth flow (magic link)
- User logs in via Supabase Auth magic link.
- Frontend stores session and uses JWT for API calls.

## 👥 Teams model
- Every user belongs to exactly **one** team in MVP.
- Team sizes allowed: 1–5.
- Roles:
  - `leader`
  - `member`

## 💳 Plan model
Plans stored in `profiles.plan`:
- `trial`
- `solo_pro`
- `team_pro`

## 🚧 Limits (enforced in backend)
Limits are stored in `team_settings.settings_json` and interpreted by backend.

Example caps by plan (MVP defaults):
- `trial`:
  - 2 job search runs/day
  - 100 jobs ingested/day
  - 5 cover letters/day
- `solo_pro`:
  - 6 job search runs/day
  - 500 jobs ingested/day
  - 30 cover letters/day
- `team_pro`:
  - 8 job search runs/day
  - 1000 jobs ingested/day
  - 80 cover letters/day

Backend increments `usage_counters` for each billable action.

## 🧾 Billing provider
- Stripe is the billing provider.
- Stripe events are consumed by backend webhook:
  - `/api/v1/billing/stripe-webhook`
- Webhook updates:
  - `profiles.plan`
  - `profiles.plan_ends_at`

