# 🤖 AI Agents — User-facing Settings (Minimal but real)

Settings live in:
- `team_settings.settings_json` (team-wide)
- `user_agent_settings.settings_json` (per user)

## 🔎 Job Search Agent settings
Per user:
- platforms: `['upwork','linkedin']`
- `tightness`: 1–5
- `exploration`: `low|medium|high`
- `daily_run_schedule`: off | every 6h | every 12h | daily
- keywords include/exclude lists
- location constraints

Team-wide:
- max jobs stored per day (retention)

## ✍️ Cover Letter Agent settings
Per user:
- `style_preset`
- `temperature`
- `vocabulary_level`
- `length_words`
- `use_placeholders`
- “include portfolio link” toggle

## 💬 Dashboard Copilot settings
Per user:
- `brief_frequency`: `on_open | daily | weekly`
- `alert_threshold_score`: default 85
- “show explanations” toggle

## 🎯 Upwork Profile Optimizer settings
Per user:
- target niches (1–3)
- tone
- aggressiveness
- keyword strictness

Team-wide:
- run schedule (weekly default)

## 🎙️ Interview Prep settings
Per user:
- mode: `chat|voice|both`
- difficulty
- strictness
- question categories enabled


## 🧱 Agents SDK mapping (how settings actually apply)
All agent settings are applied to the OpenAI Agents SDK runner per execution:
- model + global tuning are configured on `Runner` (run config)
- agent-specific tuning (temperature, style) is passed into the agent’s instructions and/or tool inputs
- approval-based actions use Human-in-the-loop (`needsApproval`) and resume from saved run state

