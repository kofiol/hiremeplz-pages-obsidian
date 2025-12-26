# 🗓️ Week 2 — Supabase Schema, RLS, Onboarding

## 🎯 Goals (measurable)
- [ ] Canonical Supabase schema is applied (tables + enums + indexes).
- [ ] RLS is enabled and enforced for all tables.
- [ ] Onboarding writes real data: profile, skills, experience, preferences.
- [ ] Profile completeness gate blocks agent triggers.

## ✅ Deliverables
- [x] SQL migration(s) applied in Supabase for the canonical schema.
- [x] RLS helper functions + policies applied.
- [x] Storage buckets created (CV uploads + snapshots).
- [ ] `/app/onboarding` flow implemented.
- [ ] `GET/POST/PATCH /api/v1/settings` (reads/writes `user_preferences` and `user_agent_settings`).

## 🧪 Acceptance criteria
- [ ] A user sees only their team’s rows (verified with 2 test users).
- [ ] User completes onboarding and `profiles.profile_completeness_score >= 0.8`.
- [ ] API returns a structured error when completeness is insufficient.

## 🧱 Checklist (tight)
### Day 1–2 — Schema + RLS
- [x] Apply [[01 SQL Schema (Canonical)]] into Supabase.
- [x] Apply [[02 RLS Policies]] and verify RLS works.
- [x] Create Storage buckets and storage policies for CV uploads.

### Day 3–4 — Onboarding UI
- [ ] Build `/app/onboarding` steps (CV upload, skills/experience, preferences).
- [ ] Implement backend endpoints to persist onboarding data.
- [ ] Compute `profile_completeness_score` server-side on save.

### Day 5 — Gate enforcement
- [ ] Add backend completeness gate on agent trigger endpoints.
- [ ] Add onboarding progress UX + "missing fields" list.

### Weekend — Polish + QA
- [ ] Validate RLS with multiple team scenarios (leader/member).
- [ ] Fix schema/typing friction.

## 🧯 Cut list (if time slips)
- [ ] Defer education table UI; keep schema but skip UI editing.

579454ea-8864-4c7d-9ce1-9127bd09e117

eyJhbGciOiJFUzI1NiIsImtpZCI6ImQyMzhlMTE4LTkzMjYtNDdmYi04N2U0LThiOTQxODdkMGQ4YiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJodHRwczovL3BldXJid2NyaWxvYm1qbHBia3JwLnN1cGFiYXNlLmNvL2F1dGgvdjEiLCJzdWIiOiJkN2Q5MTI3My03NmQ4LTQyZjItYWU1ZS0wZWIzZjQ5NjJmMzMiLCJhdWQiOiJhdXRoZW50aWNhdGVkIiwiZXhwIjoxNzY2NzYxNDcwLCJpYXQiOjE3NjY3NTc4NzAsImVtYWlsIjoicHN5aGlrMTdAZ21haWwuY29tIiwicGhvbmUiOiIiLCJhcHBfbWV0YWRhdGEiOnsicHJvdmlkZXIiOiJlbWFpbCIsInByb3ZpZGVycyI6WyJlbWFpbCJdfSwidXNlcl9tZXRhZGF0YSI6eyJlbWFpbCI6InBzeWhpazE3QGdtYWlsLmNvbSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJwaG9uZV92ZXJpZmllZCI6ZmFsc2UsInN1YiI6ImQ3ZDkxMjczLTc2ZDgtNDJmMi1hZTVlLTBlYjNmNDk2MmYzMyJ9LCJyb2xlIjoiYXV0aGVudGljYXRlZCIsImFhbCI6ImFhbDEiLCJhbXIiOlt7Im1ldGhvZCI6Im90cCIsInRpbWVzdGFtcCI6MTc2Njc1Nzg3MH1dLCJzZXNzaW9uX2lkIjoiYWU5NmFhNzYtMzc5MC00NjdmLWFmZjEtYjI5ZGI2YzZkYmZmIiwiaXNfYW5vbnltb3VzIjpmYWxzZX0.WV6501huZJH9VnHlBNpWCnwLeuXWllFcezWED8HA51jmhNqT4756o1eRurcfsKz41rOa2jTjXoKLYPI6-FMy0Q

curl.exe ^ -H "Authorization: Bearer eyJhbGciOiJFUzI1NiIsImtpZCI6ImQyMzhlMTE4LTkzMjYtNDdmYi04N2U0LThiOTQxODdkMGQ4YiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJodHRwczovL3BldXJid2NyaWxvYm1qbHBia3JwLnN1cGFiYXNlLmNvL2F1dGgvdjEiLCJzdWIiOiJkN2Q5MTI3My03NmQ4LTQyZjItYWU1ZS0wZWIzZjQ5NjJmMzMiLCJhdWQiOiJhdXRoZW50aWNhdGVkIiwiZXhwIjoxNzY2NzYxNjU4LCJpYXQiOjE3NjY3NTgwNTgsImVtYWlsIjoicHN5aGlrMTdAZ21haWwuY29tIiwicGhvbmUiOiIiLCJhcHBfbWV0YWRhdGEiOnsicHJvdmlkZXIiOiJlbWFpbCIsInByb3ZpZGVycyI6WyJlbWFpbCJdfSwidXNlcl9tZXRhZGF0YSI6eyJlbWFpbCI6InBzeWhpazE3QGdtYWlsLmNvbSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJwaG9uZV92ZXJpZmllZCI6ZmFsc2UsInN1YiI6ImQ3ZDkxMjczLTc2ZDgtNDJmMi1hZTVlLTBlYjNmNDk2MmYzMyJ9LCJyb2xlIjoiYXV0aGVudGljYXRlZCIsImFhbCI6ImFhbDEiLCJhbXIiOlt7Im1ldGhvZCI6Im90cCIsInRpbWVzdGFtcCI6MTc2Njc1ODA1OH1dLCJzZXNzaW9uX2lkIjoiOWViZTA3Y2YtMmU4Ny00NGIyLWJhM2ItZGIwN2RmYjZjMDhjIiwiaXNfYW5vbnltb3VzIjpmYWxzZX0.Yf0Om0f-93wDO69Afy1mtZpciM3dgVXOY8g9F89yWCxhTl44lASxnyiHe1xlBUx1y_a9UlTq58vuyOH36GE18g" ^ http://localhost:3000/api/v1/me

bc29d99b-d4e6-497b-8087-73f0ebdb3353

curl.exe -H "Authorization: Bearer eyJhbGciOiJFUzI1NiIsImtpZCI6ImQyMzhlMTE4LTkzMjYtNDdmYi04N2U0LThiOTQxODdkMGQ4YiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJodHRwczovL3BldXJid2NyaWxvYm1qbHBia3JwLnN1cGFiYXNlLmNvL2F1dGgvdjEiLCJzdWIiOiJkN2Q5MTI3My03NmQ4LTQyZjItYWU1ZS0wZWIzZjQ5NjJmMzMiLCJhdWQiOiJhdXRoZW50aWNhdGVkIiwiZXhwIjoxNzY2NzYxNjU4LCJpYXQiOjE3NjY3NTgwNTgsImVtYWlsIjoicHN5aGlrMTdAZ21haWwuY29tIiwicGhvbmUiOiIiLCJhcHBfbWV0YWRhdGEiOnsicHJvdmlkZXIiOiJlbWFpbCIsInByb3ZpZGVycyI6WyJlbWFpbCJdfSwidXNlcl9tZXRhZGF0YSI6eyJlbWFpbCI6InBzeWhpazE3QGdtYWlsLmNvbSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJwaG9uZV92ZXJpZmllZCI6ZmFsc2UsInN1YiI6ImQ3ZDkxMjczLTc2ZDgtNDJmMi1hZTVlLTBlYjNmNDk2MmYzMyJ9LCJyb2xlIjoiYXV0aGVudGljYXRlZCIsImFhbCI6ImFhbDEiLCJhbXIiOlt7Im1ldGhvZCI6Im90cCIsInRpbWVzdGFtcCI6MTc2Njc1ODA1OH1dLCJzZXNzaW9uX2lkIjoiOWViZTA3Y2YtMmU4Ny00NGIyLWJhM2ItZGIwN2RmYjZjMDhjIiwiaXNfYW5vbnltb3VzIjpmYWxzZX0.Yf0Om0f-93wDO69Afy1mtZpciM3dgVXOY8g9F89yWCxhTl44lASxnyiHe1xlBUx1y_a9UlTq58vuyOH36GE18g" http://localhost:3000/api/v1/me