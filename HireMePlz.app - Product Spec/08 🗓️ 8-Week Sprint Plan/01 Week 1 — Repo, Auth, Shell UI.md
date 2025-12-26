# 🗓️ Week 1 — Repo, Auth, Shell UI

## 🎯 Goals (measurable)
- [x] Turborepo + `pnpm` workspace runs locally with one command.
- [x] Supabase magic link login works end-to-end in the UI.
- [x] `/api/v1/*` verifies JWT and rejects unauthenticated calls.
- [ ] Dashboard shell exists with tabs + protected routes. ==WORKING ON==

## ✅ Deliverables
- [x] Turborepo monorepo scaffold with `apps/web` (Next.js App Router).
- [x] ~~Shared packages (`packages/*`) created only where reused.~~
- [x] Auth screens: `/login`, `/app/*` guarded.
- [x] Backend auth middleware for `/api/v1/*`.
- [x] Minimal `GET /api/v1/health` and `GET /api/v1/me`.

## 🧪 Acceptance criteria
- [x] `pnpm dev` starts the app and hot reload works.
- [x] User can request magic link, click it, land in `/app/overview`.
- [x] Unauthed request to `GET /api/v1/me` returns `401`.
- [x] Authed request to `GET /api/v1/me` returns `user_id` + `team_id`.

## 🧱 Checklist (tight)
### Day 1–2 — Monorepo + Next.js baseline
- [x] Initialize Turborepo with `pnpm`.
- [x] Add Next.js app in `apps/web` with App Router.
- [x] Add env loading strategy for local + Vercel.

### Day 3–4 — Supabase Auth integration
- [x] Create Supabase project + configure Auth (email magic link).
- [x] Add Supabase client wiring in the app.
- [x] Implement `/login` flow + session persistence.
- [x] Add protected layout for `/app/*`.

### Day 5 — REST API auth gate
- [x] Create `/api/v1/*` route handlers.
- [x] Implement JWT verification + `team_id` resolution.
- [x] Implement `GET /api/v1/me`.

### Weekend — UI shell
- [ ] Build the sidebar tab layout and empty pages for each tab. ==WORKING ON==
- [ ] Add the first "Overview" skeleton with placeholders. ==WORKING ON==

## 🧯 Cut list (if time slips)
- [x] Skip shared UI package; keep UI local to `apps/web` for now.
