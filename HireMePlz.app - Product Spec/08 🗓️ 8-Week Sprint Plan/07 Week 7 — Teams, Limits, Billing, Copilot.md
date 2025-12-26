# 🗓️ Week 7 — Teams, Limits, Billing, Copilot

## 🎯 Goals (measurable)
- [ ] Team invites work (leader invites member; member joins team).
- [ ] Daily limits enforced (job runs, cover letters, interviews).
- [ ] Stripe billing baseline exists and plan controls limits.
- [ ] Read-only dashboard copilot works with guardrails + approvals.

## ✅ Deliverables
- [ ] Team endpoints implemented (`/api/v1/team/*`).
- [ ] Team UI in `/app/team`.
- [ ] Usage counters implemented + enforced in API.
- [ ] Stripe webhook endpoint updates `profiles.plan`.
- [ ] Copilot agent implemented using OpenAI Agents SDK tools calling read-only REST.

## 🧪 Acceptance criteria
- [ ] Member cannot read data from other teams (RLS verified).
- [ ] Hitting a limit returns `429` with structured error.
- [ ] Upgrading plan increases limits (tested in Stripe test mode).
- [ ] Copilot returns a "Daily brief" without accessing DB directly.
- [ ] Any side-effect request requires explicit approval (Human-in-the-loop).

## 🧱 Checklist (tight)
### Day 1–2 — Teams
- [ ] Implement invite creation + magic link accept.
- [ ] Lock down leader-only actions.
- [ ] Build team UI (list members, invite, role management).

### Day 3 — Limits
- [ ] Implement `usage_counters` increments for billable actions.
- [ ] Enforce caps for job search + cover letters + interview sessions.
- [ ] Add UI banner when limits reached.

### Day 4 — Billing baseline
- [ ] Implement Stripe checkout + customer mapping.
- [ ] Implement Stripe webhook to update `profiles.plan`.
- [ ] Connect plan → limits mapping.

### Day 5 — Copilot v1
- [ ] Implement copilot agent with Agents SDK `Runner`.
- [ ] Tools call only `GET /api/v1/*` endpoints.
- [ ] Add output guardrail (highlights/risks/actions).

### Weekend — Hardening
- [ ] Add tracing metadata for copilot runs.
- [ ] Add abuse guardrails (no raw dumps, no ToS-violating suggestions).

## 🧯 Cut list (if time slips)
- [ ] Billing UI can be minimal; keep webhook + plan enforcement.
