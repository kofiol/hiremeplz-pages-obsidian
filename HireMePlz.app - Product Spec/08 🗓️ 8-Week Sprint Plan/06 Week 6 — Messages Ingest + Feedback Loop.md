# 🗓️ Week 6 — Messages Ingest + Feedback Loop

## 🎯 Goals (measurable)
- [ ] Inbound emails arrive as `messages` in Supabase.
- [ ] Feedback extraction runs and produces actionable items.
- [ ] Dashboard has a unified inbox + feedback tab.

## ✅ Deliverables
- [ ] Mailgun inbound route → backend webhook → Trigger.dev event.
- [ ] `email_ingest.mailgun` workflow implemented.
- [ ] `/app/messages` inbox UI.
- [ ] `/app/feedback` list UI with resolve action.

## 🧪 Acceptance criteria
- [ ] Sending a test email produces a `messages` row.
- [ ] Workflow dedupes threads and does not double-ingest.
- [ ] At least 1 feedback item is extracted from a realistic email.
- [ ] User can mark feedback `resolved` and it persists.

## 🧱 Checklist (tight)
### Day 1–2 — Mailgun + webhook plumbing
- [ ] Configure Mailgun inbound routes.
- [ ] Implement `/api/v1/webhooks/mailgun/inbound` with signature validation.
- [ ] Forward payload into Trigger.dev event.

### Day 3 — Ingest workflow
- [ ] Normalize email → canonical message fields.
- [ ] Write message + dedupe logic.
- [ ] Emit notification for inbound message.

### Day 4 — Feedback extraction
- [ ] Extract feedback items (pricing, skills, timeline, communication).
- [ ] Persist into `feedback` with `action_required`.

### Day 5 — UI
- [ ] Build inbox with filters (needs reply, platform).
- [ ] Build feedback tab with resolve + notes.

### Weekend — Linkage
- [ ] Link messages to applications when possible.
- [ ] Add basic “needs reply” flagging.

## 🧯 Cut list (if time slips)
- [ ] Skip DM parsing; handle email only.
