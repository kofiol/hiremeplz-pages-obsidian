# 🖥️ Frontend — Information Architecture (IA) & Routes

## ✅ Framework decisions
- Next.js App Router
- TypeScript everywhere
- Tailwind CSS for UI
- Supabase browser client for:
  - auth session
  - Realtime subscriptions

## 🧭 Route map (App Router)
**Public**
- `/` — landing
- `/login` — magic link
- `/waitlist` — optional

**App (authenticated)**
- `/app` — redirect to `/app/overview`
- `/app/onboarding` — multi-step profile completion
- `/app/overview` — pipeline summary
- `/app/jobs` — shortlist + filters
- `/app/jobs/[jobId]` — job details + generate cover letter
- `/app/applications` — pipeline board/table
- `/app/cover-letters` — generated library
- `/app/messages` — inbox (email + DM)
- `/app/feedback` — extracted feedback + statuses
- `/app/earnings` — earnings + timeline
- `/app/analytics` — KPIs and trends
- `/app/team` — members, invites (leader)
- `/app/settings` — agent settings + integrations
- `/app/interview-prep` — beta

## 🧩 Shared UI patterns
- Global search: job title/company/keywords
- Saved views: “Top matches”, “High budget”, “Fast response likelihood”
- Bulk actions:
  - generate cover letters for selected jobs
  - archive jobs

## 🔄 Data fetching pattern
- UI reads canonical data from `/api/v1/*` endpoints.
- UI does not call Supabase tables directly for canonical reads (keeps business rules centralized).
- Realtime is used for:
  - agent run progress
  - new jobs inserted
  - new messages/feedback ingested

