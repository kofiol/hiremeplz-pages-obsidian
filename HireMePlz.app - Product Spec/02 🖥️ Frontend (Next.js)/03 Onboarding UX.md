# 🖥️ Frontend — Onboarding UX (15 minutes)

## ✅ Onboarding steps (required)
Progress bar with 5 steps. Users cannot trigger job search until step 4 is done.

### Step 1 — Account
- email magic link login
- choose: Solo vs Team leader

### Step 2 — CV upload
- upload CV to Supabase Storage
- show “Parsing in progress” when workflow runs

### Step 3 — Manual profile completion
Structured forms:
- skills (tag + level)
- experience (role, years, highlights)
- education

### Step 4 — Preferences
- platforms: LinkedIn, Upwork (toggle)
- wage preferences:
  - hourly range
  - fixed budget minimum
  - currency
- constraints:
  - project types (short gigs / medium projects)
  - time zones
  - remote only
- job search tightness (1–5)

### Step 5 — Integrations (optional)
- Mailgun inbound email forwarding setup
- Chrome extension pairing
- add LinkedIn/Upwork profile URLs

## ✅ Profile completeness gate
Backend enforces a `profile_completeness_score` threshold.
- If < 0.8 → job search trigger returns 400 with missing fields.

