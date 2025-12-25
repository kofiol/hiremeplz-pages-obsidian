# 🧵 Trigger.dev — Scheduled Pollers (every 6 hours)

## 🔄 `profile_poll.linkedin`
**Goal:** keep LinkedIn profile snapshot fresh for search and cover letters.

Steps:
- fetch profile page / exported data via Apify/BrightData
- store `user_profile_snapshots` with `source='linkedin'`
- run lightweight parsing to update structured fields if new

## 🔄 `profile_poll.upwork`
**Goal:** keep Upwork profile snapshot fresh for profile optimizer + matching.

Steps:
- fetch profile page via Apify/BrightData
- store `user_profile_snapshots` with `source='upwork'`
- compute keyword coverage + completeness metrics

## 🧾 `profile_parser.cv`
Triggered on CV upload.

Steps:
- extract text
- parse into structured JSON
- update:
  - `user_profile_snapshots(source='cv')`
  - `user_skills`, `user_experiences`, `user_educations`
  - `profiles.profile_completeness_score`

