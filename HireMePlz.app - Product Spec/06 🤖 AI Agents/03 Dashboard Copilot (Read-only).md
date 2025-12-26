# 🤖 AI Agents — Dashboard Copilot (Read-only, Human-facing)

## 🎯 Mission
Help humans understand:
- what changed
- what matters
- what to do next

## 🚫 Absolute constraints
- ❌ does not invent schemas
- ❌ does not reshape canonical data
- ❌ is not a data source for other agents
- ❌ does not write to canonical tables

## ✅ Allowed capabilities
- Decide which REST endpoints to call
- Aggregate results across endpoints
- Interpret patterns and trends
- Produce:
  - summaries
  - insights
  - alerts
  - recommendations
- Request side effects by calling backend endpoints that explicitly allow it (e.g. `POST /notifications`), but only as a *request*; backend decides.

## 🧰 Tools
The copilot only has access to:
- `GET /jobs`
- `GET /applications`
- `GET /messages`
- `GET /feedback`
- `GET /earnings`
- `GET /agent-runs`

## 🧾 Output format (UI)
- “Daily brief” block on Overview tab:
  - 3 highlights
  - 3 risks
  - 3 actions

## 🧯 Safety
- Copilot responses must include uncertainty when data is missing.
- Never suggest actions that break platform ToS (auto-messaging/auto-applying).


## 🧱 Implementation (OpenAI Agents SDK + Human-in-the-loop)
We implement the copilot using the OpenAI Agents SDK for TypeScript.

References:
- SDK: https://openai.github.io/openai-agents-js/
- Running agents: https://openai.github.io/openai-agents-js/guides/running-agents/
- Human-in-the-loop: https://openai.github.io/openai-agents-js/guides/human-in-the-loop/

**Tools = internal REST calls**
Copilot tools are thin wrappers over backend read endpoints:
- `get_jobs()` → `GET /api/v1/jobs`
- `get_applications()` → `GET /api/v1/applications`
- `get_messages()` → `GET /api/v1/messages`
- `get_feedback()` → `GET /api/v1/feedback`
- `get_earnings()` → `GET /api/v1/earnings`
- `get_agent_runs()` → `GET /api/v1/agent-runs`

**Guardrails**
- Input guardrail blocks prompts that ask for raw dumps / database access.
- Tool guardrail blocks any non-GET behavior.
- Output guardrail forces the UI shape: `highlights[]`, `risks[]`, `actions[]`.

**Human-in-the-loop approvals**
Any tool that would request a side effect is marked as requiring approval (`needsApproval`).
Example approved-only requests:
- `request_notification(type, payload)` → backend decides final delivery

When approval is required:
- the run returns an interruption
- UI shows an “Approve/Reject” action
- the run resumes from the saved state after user decision

