# 🗄️ Supabase — RLS Policies (defined)

## ✅ Principle
- **Team membership** controls all access.
- **Agent-written tables** are read-only for clients.
- Only backend/workflows using **service role** can write agent outputs.

## 🧩 Helper functions
```sql
create or replace function public.is_team_member(t uuid)
returns boolean
language sql
stable
as $$
  select exists(
    select 1
    from public.team_members tm
    where tm.team_id = t
      and tm.user_id = auth.uid()
      and tm.status = 'active'
  );
$$;

create or replace function public.is_team_leader(t uuid)
returns boolean
language sql
stable
as $$
  select exists(
    select 1
    from public.team_members tm
    where tm.team_id = t
      and tm.user_id = auth.uid()
      and tm.status = 'active'
      and tm.role = 'leader'
  );
$$;
```

## 🔐 Enable RLS
```sql
alter table public.teams enable row level security;
alter table public.team_members enable row level security;
alter table public.profiles enable row level security;
alter table public.user_cv_files enable row level security;
alter table public.user_profile_snapshots enable row level security;
alter table public.user_skills enable row level security;
alter table public.user_experiences enable row level security;
alter table public.user_educations enable row level security;
alter table public.user_preferences enable row level security;

alter table public.job_sources enable row level security;
alter table public.jobs enable row level security;
alter table public.job_rankings enable row level security;

alter table public.applications enable row level security;
alter table public.cover_letters enable row level security;

alter table public.messages enable row level security;
alter table public.feedback enable row level security;

alter table public.earnings enable row level security;

alter table public.agent_runs enable row level security;
alter table public.agent_run_steps enable row level security;

alter table public.embeddings enable row level security;

alter table public.team_settings enable row level security;
alter table public.user_agent_settings enable row level security;

alter table public.usage_counters enable row level security;
alter table public.events enable row level security;
alter table public.notifications enable row level security;

alter table public.apply_sessions enable row level security;
```

## 👥 Teams
```sql
-- A user can read teams they belong to
create policy "teams_select_member" on public.teams
for select
using (public.is_team_member(id));

-- Only leader can update team name
create policy "teams_update_leader" on public.teams
for update
using (public.is_team_leader(id))
with check (public.is_team_leader(id));

-- Team creation is done by backend with service role (no client insert)
```

## 👥 Team members
```sql
create policy "team_members_select_member" on public.team_members
for select
using (public.is_team_member(team_id));

create policy "team_members_insert_leader" on public.team_members
for insert
with check (public.is_team_leader(team_id));

create policy "team_members_update_leader" on public.team_members
for update
using (public.is_team_leader(team_id))
with check (public.is_team_leader(team_id));

create policy "team_members_delete_leader" on public.team_members
for delete
using (public.is_team_leader(team_id));
```

## 🧑 Profiles & onboarding tables
```sql
create policy "profiles_select_member" on public.profiles
for select
using (public.is_team_member(team_id));

create policy "profiles_update_self" on public.profiles
for update
using (user_id = auth.uid() and public.is_team_member(team_id))
with check (user_id = auth.uid() and public.is_team_member(team_id));

-- structured fields: user can manage their own
create policy "user_skills_crud_self" on public.user_skills
for all
using (user_id = auth.uid() and public.is_team_member(team_id))
with check (user_id = auth.uid() and public.is_team_member(team_id));

create policy "user_experiences_crud_self" on public.user_experiences
for all
using (user_id = auth.uid() and public.is_team_member(team_id))
with check (user_id = auth.uid() and public.is_team_member(team_id));

create policy "user_educations_crud_self" on public.user_educations
for all
using (user_id = auth.uid() and public.is_team_member(team_id))
with check (user_id = auth.uid() and public.is_team_member(team_id));

create policy "user_preferences_crud_self" on public.user_preferences
for all
using (user_id = auth.uid() and public.is_team_member(team_id))
with check (user_id = auth.uid() and public.is_team_member(team_id));

-- CV files can be listed by the user; writes via Storage policies
create policy "user_cv_files_select_self" on public.user_cv_files
for select
using (user_id = auth.uid() and public.is_team_member(team_id));
```

## 🔎 Jobs & rankings (agent-written)
```sql
create policy "jobs_select_member" on public.jobs
for select
using (public.is_team_member(team_id));

create policy "job_rankings_select_member" on public.job_rankings
for select
using (public.is_team_member(team_id));

-- No client inserts/updates/deletes for jobs/rankings/job_sources
```

## 🚀 Applications (user-written)
```sql
create policy "applications_select_member" on public.applications
for select
using (public.is_team_member(team_id));

create policy "applications_insert_self" on public.applications
for insert
with check (user_id = auth.uid() and public.is_team_member(team_id));

create policy "applications_update_self" on public.applications
for update
using (user_id = auth.uid() and public.is_team_member(team_id))
with check (user_id = auth.uid() and public.is_team_member(team_id));

create policy "applications_delete_self" on public.applications
for delete
using (user_id = auth.uid() and public.is_team_member(team_id));
```

## ✍️ Cover letters (agent-written)
```sql
create policy "cover_letters_select_member" on public.cover_letters
for select
using (public.is_team_member(team_id));

-- no client insert/update/delete
```

## 💬 Messages + ⭐ feedback (agent-written)
```sql
create policy "messages_select_member" on public.messages
for select
using (public.is_team_member(team_id));

create policy "feedback_select_member" on public.feedback
for select
using (public.is_team_member(team_id));

-- allow user to mark feedback resolved (human action)
create policy "feedback_update_member" on public.feedback
for update
using (public.is_team_member(team_id))
with check (public.is_team_member(team_id));

-- no client insert/delete
```

## 💰 Earnings (user-written)
```sql
create policy "earnings_select_member" on public.earnings
for select
using (public.is_team_member(team_id));

create policy "earnings_crud_self" on public.earnings
for all
using (user_id = auth.uid() and public.is_team_member(team_id))
with check (user_id = auth.uid() and public.is_team_member(team_id));
```

## 🤖 Agent runs (agent-written but readable)
```sql
create policy "agent_runs_select_member" on public.agent_runs
for select
using (public.is_team_member(team_id));

create policy "agent_run_steps_select_member" on public.agent_run_steps
for select
using (public.is_team_member(team_id));

-- no client insert/update/delete
```

## 🧠 Embeddings (agent-written)
```sql
create policy "embeddings_select_member" on public.embeddings
for select
using (public.is_team_member(team_id));

-- no client insert/update/delete
```

## ⚙️ Settings
```sql
create policy "team_settings_select_member" on public.team_settings
for select
using (public.is_team_member(team_id));

create policy "team_settings_update_leader" on public.team_settings
for update
using (public.is_team_leader(team_id))
with check (public.is_team_leader(team_id));

create policy "user_agent_settings_crud_self" on public.user_agent_settings
for all
using (user_id = auth.uid() and public.is_team_member(team_id))
with check (user_id = auth.uid() and public.is_team_member(team_id));
```

## 🧾 Events + notifications
```sql
create policy "events_select_member" on public.events
for select
using (public.is_team_member(team_id));

create policy "notifications_select_member" on public.notifications
for select
using (public.is_team_member(team_id));

create policy "notifications_update_member" on public.notifications
for update
using (public.is_team_member(team_id))
with check (public.is_team_member(team_id));

-- no client insert/delete
```

## 🧷 Apply sessions
```sql
create policy "apply_sessions_select_self" on public.apply_sessions
for select
using (user_id = auth.uid() and public.is_team_member(team_id));

-- apply session rows are created by backend only (service role)
```
