# 🤖 AI Agents — Upwork Profile Optimizer Agent (Inbound)

## 🎯 Why this exists
The one-pager claims “$0 spent on Upwork Connects”. In reality, Upwork Connects are rarely zero.

**MVP truth:** HireMePlz reduces Connects spend by increasing *inbound* opportunities and improving conversion.

## 🎯 Mission
Improve the user’s Upwork profile so:
- clients find them via Upwork search
- profile converts views → invitations
- outreach cost per win (Connects + time) decreases

## ✅ Inputs
- Upwork profile snapshot (`user_profile_snapshots.source='upwork'`)
- CV snapshot
- historical results:
  - feedback categories
  - win/loss outcomes
  - earnings by category
- user preferences (niches, rate range)

## 🧠 Core outputs
- “Profile score” (0–100) broken down into:
  - niche clarity
  - keyword coverage
  - proof (portfolio/case studies)
  - trust (social proof, reviews)
  - pricing clarity
  - readability
- Concrete rewrite suggestions:
  - title
  - overview
  - specialized profiles (if used)
  - skills/tags list
- A prioritized task list:
  - add 1 case study
  - add 2 portfolio items
  - rewrite headline

## ⚙️ User settings
- target niche(s)
- tone: `professional | friendly | direct`
- aggressiveness: `safe | assertive` (how bold the positioning is)
- “keyword strictness” (how strongly to optimize for search phrases)

## 🧩 Workflow behavior
- runs weekly by default (leader can change schedule)
- never auto-edits the Upwork profile
- provides:
  - copy-paste blocks
  - optional Chrome-extension injection into profile editor (manual user confirmation)

## 🧯 Guardrails
- no fake claims, no fabricated stats
- no policy violations (no impersonation, no misleading guarantees)
- uses only verified user data + explicit user-provided claims

