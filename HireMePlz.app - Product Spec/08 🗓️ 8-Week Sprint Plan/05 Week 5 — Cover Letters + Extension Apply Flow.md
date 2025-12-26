# 🗓️ Week 5 — Cover Letters + Extension Apply Flow

## 🎯 Goals (measurable)
- [ ] Cover Letter workflow generates on-demand and persists outputs.
- [ ] User can copy or send cover letter to extension.
- [ ] Apply sessions are secure, short-lived, and usable.

## ✅ Deliverables
- [ ] Trigger.dev `cover_letter.generate` workflow implemented.
- [ ] `POST /api/v1/cover-letters/generate` triggers workflow.
- [ ] `/app/jobs/[jobId]` has “Generate cover letter” and shows versions.
- [ ] `apply_sessions` endpoints implemented.
- [ ] Minimal Chrome extension MVP that injects into Upwork + LinkedIn textareas.

## 🧪 Acceptance criteria
- [ ] 1 click generates a cover letter and it appears in `/app/cover-letters`.
- [ ] Regenerations create new rows (version history).
- [ ] Apply session token expires and cannot be reused.
- [ ] On supported pages, extension injects the latest cover letter text.

## 🧱 Checklist (tight)
### Day 1–2 — Cover letter workflow
- [ ] Implement Agents SDK agent + tools for loading job/profile.
- [ ] Add output QA guardrails (length, no hallucinations, no placeholders).
- [ ] Persist `cover_letters` + events.

### Day 3 — UI integration
- [ ] Build cover letter viewer with copy action.
- [ ] Add generation settings UI (temperature, preset, vocab level).

### Day 4 — Apply sessions API
- [ ] Implement `POST /api/v1/apply-sessions`.
- [ ] Implement `GET /api/v1/apply-sessions/:token`.
- [ ] Hash token storage + expiration enforcement.

### Day 5 — Extension MVP
- [ ] Content script for Upwork apply text field.
- [ ] Content script for LinkedIn easy apply textarea.
- [ ] Inject with clear visual confirmation.

### Weekend — Hardening
- [ ] Add injection event logging.
- [ ] Add fallback UI when extension not installed.

## 🧯 Cut list (if time slips)
- [ ] Support Upwork only; add LinkedIn injection next week.
