# 🤖 AI Agents — Cover Letter Agent

## 🎯 Mission
Generate human-like, role-specific cover letters that increase response rate in high-volume applications.

## ✅ Inputs
- Canonical job (`jobs`)
- User profile (snapshots + structured fields)
- User settings (`user_agent_settings`)
- Optional: application context if exists (`applications`)

## ⚙️ User-controllable knobs
- `style_preset`: `professional | concise | friendly | technical | bold`
- `temperature`: `0.2..1.0`
- `vocabulary_level`: `1..5` (simple → advanced)
- `length_words`: default 160
- `use_placeholders`: `true|false`

## 🧪 Output QA (hard rules)
Reject/regenerate if:
- contains forbidden placeholders when `use_placeholders=false`
- exceeds word limit by >25%
- includes claims not supported by user profile (hallucination check)

## 📤 Outputs
- `cover_letters` row per generation
- `events`: `cover_letter.generated`


## 🧱 Implementation (OpenAI Agents SDK)
We implement this agent with the OpenAI Agents SDK for TypeScript: https://openai.github.io/openai-agents-js/

**How it runs**
- Triggered by backend: `POST /api/v1/cover-letters/generate`.
- Executed in Trigger.dev workflow `cover_letter.generate`.
- Uses the Agents SDK runner loop (`run()` / `Runner`) to:
  - call tools (load job/profile)
  - generate draft
  - run QA guardrails
  - persist a final output

**Tools (concrete)**
- `load_job(job_id)`
- `load_profile(user_id)`
- `draft_cover_letter(job, profile, settings)`
- `quality_check(text)` (rejects hallucinations + format violations)
- `write_cover_letter(text, metadata)`

**Guardrails**
- Output guardrail enforces the hard formatting rules in this page.
- Hallucination guardrail: disallow claims not grounded in `user_profile_snapshots` + structured profile tables.

**Tracing**
- Tracing is enabled for debugging prompt/tool behavior and regressions.

