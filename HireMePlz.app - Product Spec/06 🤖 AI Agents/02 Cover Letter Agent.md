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

