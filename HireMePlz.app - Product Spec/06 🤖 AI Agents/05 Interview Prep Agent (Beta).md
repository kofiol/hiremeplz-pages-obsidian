# 🤖 AI Agents — Interview Prep Agent (Beta)

## 🎯 Mission
Help freelancers prepare for interviews by:
- running mock interviews (text + voice)
- analyzing readiness (structure, clarity, confidence proxies)
- producing actionable coaching

## ✅ Modes
1. 💬 Chat mock interview
- question sets based on job + user profile
- evaluates answers for completeness and relevance

2. 🎙️ Voice mock interview (OpenAI Realtime)
- WebRTC session from the dashboard
- records transcript + timing

## ✅ Inputs
- `job_id` (optional but recommended)
- user profile
- past feedback items

## 📤 Outputs
- transcript summary
- strengths/weaknesses
- recommended improvements
- follow-up drills (5–10 questions)

## 🧾 Metrics stored (MVP)
Stored in `agent_runs.outputs`:
- speaking speed (words/min)
- filler word rate (approx.)
- average answer length
- confidence proxy score (heuristic)

## ⚙️ User settings
- interview type: `technical | behavioral | client_discovery`
- difficulty: `easy | medium | hard`
- strictness: how tough the evaluator is
- voice mode toggle

## 🧯 Disclaimer
These metrics are heuristics, not medical/psychological assessments.
