# 🧵 Trigger.dev — Email Ingest Workflow (`email_ingest.mailgun`)

## 🎯 Goal
Turn inbound recruiter/client emails into:
- `messages`
- extracted `feedback` items
- notifications

## ✅ Inputs
- Mailgun inbound payload (validated by backend)
- forwarded event from backend to Trigger.dev

## 🧩 Steps
1. `validate_payload`
2. `normalize_message`
3. `dedupe`
4. `write_message`
5. `extract_feedback`
6. `write_feedback`
7. `notify_user`

## 🧾 Normalization rules
- `thread_id`:
  - use `Message-Id` / `In-Reply-To` / `References` headers when present
- store:
  - `subject`, `from_email`, `to_email`
  - plaintext body
  - raw payload JSON

## 🧠 Feedback extraction
- Extract only actionable content:
  - “budget too high”
  - “need portfolio examples”
  - “timeline mismatch”
- Store:
  - `feedback.content`
  - `category`
  - `sentiment` (optional numeric)
  - `status = action_required`

