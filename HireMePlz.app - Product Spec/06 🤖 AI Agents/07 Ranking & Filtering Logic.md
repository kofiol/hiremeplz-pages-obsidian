# 🤖 AI Agents — Ranking & Filtering Logic (Magic, but defined)

## 🎯 Goal
Produce a shortlist that feels “obviously right” to the user.

This is achieved by:
- deterministic scoring
- explainable breakdown
- a user-controlled strictness knob (`tightness` 1–5)

## 🧾 Scoring model (0–100)
Final score is a weighted sum:
- `skill_match` (0–30)
- `budget_fit` (0–20)
- `seniority_fit` (0–10)
- `client_quality` (0–15)
- `recency` (0–10)
- `platform_confidence` (0–10)
- `fraud_penalty` (0 to -25)

`score = clamp( skill_match + budget_fit + seniority_fit + client_quality + recency + platform_confidence + fraud_penalty, 0, 100 )`

## 🧠 How each component is computed
### 1) Skill match (0–30)
Inputs:
- user skills (tags + experience)
- job skills (extracted)

Compute:
- `coverage = matched_required_skills / required_skills`
- `depth = avg(level_of_matched_skills)/5`

Score:
- `skill_match = 30 * (0.7*coverage + 0.3*depth)`

### 2) Budget fit (0–20)
Inputs:
- user `hourly_min/hourly_max`, `fixed_budget_min`
- job budget fields

Rules:
- hourly job: penalize if `hourly_max < hourly_min_pref`
- fixed job: penalize if `fixed_budget_max < fixed_budget_min_pref`

Score:
- 20 if fully inside preference
- 10 if slightly below but within tolerance
- 0 if far below

### 3) Seniority fit (0–10)
- Map user experience years to `junior|mid|senior`
- Compare to job’s extracted seniority

### 4) Client quality (0–15)
Upwork-specific:
- payment verified (+5)
- rating >= 4.8 (+5)
- hires >= 5 (+5)

LinkedIn:
- unknown client quality defaults to neutral (7)

### 5) Recency (0–10)
- 10 if posted < 24h
- 7 if < 3 days
- 4 if < 7 days
- 1 otherwise

### 6) Platform confidence (0–10)
- reflects extraction quality and field completeness
- prevents high scores when the job is poorly parsed

### 7) Fraud penalty (0 to -25)
Penalty applied if:
- suspicious patterns (crypto scams, “download this file”, etc.)
- mismatch between apply link and platform domain

## 🎚️ Tightness (filtering thresholds)
Tightness controls:
- minimum score required
- how many “exploration” jobs are allowed
- how strict budget matching is

| Tightness | Minimum score | Budget tolerance | Exploration jobs |
|---:|---:|---:|---:|
| 1 | 45 | 40% below preference allowed | 40% of shortlist |
| 2 | 55 | 25% below allowed | 25% |
| 3 (default) | 65 | 15% below allowed | 15% |
| 4 | 75 | 5% below allowed | 5% |
| 5 | 85 | 0% below allowed | 0% |

## 🧾 Explainability contract
For every job shown in the dashboard, store `job_rankings.breakdown`:
```json
{
  "skill_match": 24.2,
  "budget_fit": 16,
  "client_quality": 12,
  "recency": 7,
  "fraud_penalty": 0,
  "notes": ["Matched: Next.js, Supabase", "Budget fits hourly preference"]
}
```

This powers the “Why this job?” UI.
