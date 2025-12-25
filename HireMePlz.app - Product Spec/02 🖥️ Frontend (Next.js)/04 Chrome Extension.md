# 🖥️ Frontend — Chrome Extension (Apply Flow + Text Injection)

## ✅ Purpose
- Make “Generate cover letter → Apply” frictionless.
- Inject generated cover letter text into application fields on Upwork and LinkedIn.

## 🧩 Architecture
- Chrome extension (Manifest V3)
- Content scripts for:
  - `upwork.com/*`
  - `linkedin.com/*`
- A lightweight “handoff” from dashboard:
  - dashboard calls backend to create a short-lived `apply_session`
  - extension reads `apply_session` via authenticated fetch

## 🔐 Security model
- No long-term secrets stored in the extension.
- The extension uses:
  - a short-lived `apply_session_token` (5–10 minutes)
  - fetched from backend after the user clicks “Send to extension”

## 🔁 User flow
1. User clicks `Generate cover letter` in dashboard.
2. User clicks `Send to extension`.
3. Dashboard creates `apply_session` and opens the apply link.
4. On the apply page, extension detects supported form fields.
5. Extension requests cover letter payload with `apply_session_token`.
6. Extension injects text and highlights inserted fields.

## 🧾 Stored data
- `apply_sessions` are stored in Supabase with expiration.
- Injection events are stored as `events`:
  - `cover_letter_injected`
  - `apply_link_opened`

