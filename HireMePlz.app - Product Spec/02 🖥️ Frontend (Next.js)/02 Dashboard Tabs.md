# 🖥️ Frontend — Dashboard Tabs (UX layout)

## 🧭 Top-level navigation (tabs)
Left sidebar (desktop) / bottom bar (mobile):
1. 🧾 Overview
2. 🔎 Jobs
3. 🚀 Applications
4. ✍️ Cover Letters
5. 💬 Messages
6. ⭐ Feedback
7. 💰 Earnings
8. 📈 Analytics
9. 👥 Team
10. ⚙️ Settings

## 🧾 Tab: Overview
**Purpose:** answer “What should I do today?”

**Sections**
- Today’s actions
  - `Generate cover letters (N)`
  - `Apply to top jobs (N)` (redirects to apply links)
  - `Reply needed (N)`
- Pipeline snapshot (counts by status)
- Recent agent runs
- Alerts
  - new high-score job
  - daily limit reached
  - suspicious job detected

## 🔎 Tab: Jobs
**Primary list view**
- Table cards with:
  - score badge (0–100)
  - platform + posted time
  - budget range
  - skill highlights (matched vs missing)
  - client quality badge (Upwork)

**Filters**
- platform: Upwork / LinkedIn
- score min
- budget type
- rate range
- keywords include/exclude
- “hide suspicious” toggle

**Job detail drawer/page**
- canonical description
- extracted requirements
- match explanation (from `job_rankings.breakdown`)
- actions:
  - `Generate cover letter`
  - `Open apply link`
  - `Archive`

## 🚀 Tab: Applications
Two sub-views:
- **Board**: drag between statuses
- **Table**: sortable, exportable

Fields:
- status
- job
- date applied
- last message
- next follow-up date

## ✍️ Tab: Cover Letters
- list grouped by job
- regenerated versions tracked
- actions:
  - copy
  - open apply link
  - “Send to extension” (stores payload for injection)

## 💬 Tab: Messages
- unified inbox
- grouping:
  - by platform thread (if available)
  - by email subject/thread
- actions:
  - mark “needs reply”
  - create feedback record
  - link to application

## ⭐ Tab: Feedback
- extracted feedback items
- statuses:
  - `action_required`
  - `resolved`
- categories:
  - pricing
  - communication
  - skills
  - timeline

## 💰 Tab: Earnings
- timeline view
- monthly totals
- per-platform breakdown

## 📈 Tab: Analytics
KPIs:
- jobs ingested/day
- shortlist size
- apply conversion
- reply rate
- win rate
- earnings/week
- time saved estimate (heuristic)

## 👥 Tab: Team
Leader:
- invite member (magic link)
- role management

Member:
- view shared jobs + applications
- personal cover letters

## ⚙️ Tab: Settings
Sub-sections:
- Profile completeness
- Agent settings (job search, ranking tightness, cover letter style)
- Integrations (Mailgun routing instructions)
- Chrome extension pairing
- Billing

