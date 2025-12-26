# 🗓️ Week 8 — Interview Prep Beta, Upwork Optimizer, Launch

## 🎯 Goals (measurable)
- [ ] Interview prep beta works end-to-end (chat + voice session + analysis).
- [ ] Upwork profile optimizer produces copy-paste improvements and a score.
- [ ] MVP is stable enough for first paying users.

## ✅ Deliverables
- [ ] Interview prep UI in `/app/interview-prep`.
- [ ] Backend endpoint returns voice session parameters.
- [ ] Trigger.dev post-session analysis workflow writes results.
- [ ] Upwork profile optimizer workflow implemented.
- [ ] Settings wired for both agents.
- [ ] Launch checklist completed.

## 🧪 Acceptance criteria
- [ ] User can run a chat mock interview and receive a structured critique.
- [ ] User can start a voice session, finish, and see analysis results stored in `agent_runs.outputs`.
- [ ] Upwork optimizer produces:
  - a profile score breakdown
  - headline + overview rewrite blocks
  - a prioritized task list
- [ ] No P0 security issues (secrets, RLS leaks, unauth side effects).

## 🧱 Checklist (tight)
### Day 1–2 — Interview prep chat + analysis
- [ ] Implement interview prep chat mode with Agents SDK.
- [ ] Persist run outputs in `agent_runs.outputs`.
- [ ] Add job-context selection (optional but recommended).

### Day 3 — Voice session plumbing
- [ ] Implement `POST /api/v1/agents/interview-prep/session`.
- [ ] Add WebRTC UI with start/stop and transcript capture.

### Day 4 — Voice post-processing
- [ ] Implement Trigger.dev analysis workflow:
  - transcript summary
  - strengths/weaknesses
  - drills
  - heuristic metrics

### Day 5 — Upwork optimizer
- [ ] Implement `upwork_profile_optimizer.run` workflow.
- [ ] Produce score + rewrite blocks + tasks.
- [ ] Add UI view to render suggestions and copy.

### Weekend — Launch hardening
- [ ] Run a full end-to-end QA pass of onboarding → jobs → cover letters → inbox.
- [ ] Tighten rate limits + caps.
- [ ] Add Sentry + PostHog events for key funnels.
- [ ] Prepare waitlist/checkout flow and invite the first users.

## 🧯 Cut list (if time slips)
- [ ] Ship interview prep as chat-only; keep voice as “private beta”.
