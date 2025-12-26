# 🗓️ Week 3 — Trigger.dev, CV Parse, Agent Runs

## 🎯 Goals (measurable)
- [ ] Trigger.dev is wired: backend can trigger workflows.
- [ ] Every workflow run writes `agent_runs` + `agent_run_steps`.
- [ ] CV upload triggers parsing workflow and updates structured profile tables.
- [ ] UI shows live agent progress via Realtime.

## ✅ Deliverables
- [ ] Trigger.dev project created with environment secrets.
- [ ] Backend event trigger endpoints implemented.
- [ ] `profile_parser.cv` workflow implemented.
- [ ] Agent run UI component shows steps timeline.

## 🧪 Acceptance criteria
- [ ] Uploading a CV creates a `user_cv_files` row and Storage object.
- [ ] CV parse workflow writes `user_profile_snapshots(source='cv')` and updates skills/experience.
- [ ] `profiles.profile_completeness_score` updates automatically.
- [ ] Dashboard shows workflow progress without refresh.

## 🧱 Checklist (tight)
### Day 1–2 — Trigger.dev plumbing
- [ ] Create workflows skeletons and shared Supabase service client.
- [ ] Implement backend trigger call to Trigger.dev for `profile_parser.cv`.
- [ ] Write `agent_runs` + `agent_run_steps` helper in workflows.

### Day 3–4 — CV parsing v1
- [ ] Extract CV text (simple and robust; support PDF/DOCX).
- [ ] Parse into `parsed_json` with deterministic fields.
- [ ] Upsert `user_skills` + `user_experiences` from parsed output.

### Day 5 — Realtime progress
- [ ] Subscribe UI to `agent_runs` and `agent_run_steps`.
- [ ] Add progress panel to `/app/overview`.

### Weekend — Hardening
- [ ] Add retries + failure states that land in `agent_runs.error_text`.
- [ ] Add basic analytics events for onboarding completion.

## 🧯 Cut list (if time slips)
- [ ] Skip DOCX support; accept PDF only for MVP.
