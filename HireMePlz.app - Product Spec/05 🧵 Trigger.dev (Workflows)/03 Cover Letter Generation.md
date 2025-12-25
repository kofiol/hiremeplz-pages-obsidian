# 🧵 Trigger.dev — Cover Letter Workflow (`cover_letter.generate`)

## 🎯 Goal
Generate a professional, role-specific cover letter that feels human and matches the user’s style.

## ✅ Inputs
From backend trigger payload:
- `team_id`
- `user_id`
- `job_id`
- optional override:
  - `temperature`
  - `vocabulary_level`
  - `style_preset`

From Supabase:
- job canonical object
- user profile (parsed snapshot + structured fields)
- user agent settings

## 🧩 Steps
1. `load_job`
2. `load_user_profile`
3. `compose_prompt`
4. `generate_cover_letter`
5. `quality_check`
6. `write_cover_letter`

## ✍️ Output format rules (hard)
- 120–220 words by default (configurable)
- no markdown
- no placeholders like “{Name}” unless user explicitly enabled placeholders
- must include:
  - 1–2 lines proving understanding of job
  - 2–3 lines mapping skills to requirements
  - 1 clear call-to-action + availability

## 📤 Outputs
- Insert `cover_letters` row
- Emit event `cover_letter.generated`

