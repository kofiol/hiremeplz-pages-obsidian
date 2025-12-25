# 🗄️ Supabase — SQL Schema (Canonical, MVP)

This is the **defined** MVP schema. All IDs are UUIDs. All canonical rows are scoped by `team_id`.

## ✅ Extensions
```sql
create extension if not exists pgcrypto;
create extension if not exists vector;
```

## ✅ Enums
```sql
do $$ begin
  create type public.team_role as enum ('leader', 'member');
exception when duplicate_object then null; end $$;

do $$ begin
  create type public.platform as enum ('upwork', 'linkedin');
exception when duplicate_object then null; end $$;

do $$ begin
  create type public.application_status as enum (
    'shortlisted',
    'ready_to_apply',
    'applied',
    'in_conversation',
    'interviewing',
    'won',
    'lost',
    'archived'
  );
exception when duplicate_object then null; end $$;

do $$ begin
  create type public.agent_type as enum (
    'job_search',
    'cover_letter',
    'dashboard_copilot',
    'upwork_profile_optimizer',
    'interview_prep',
    'profile_parser',
    'email_ingest'
  );
exception when duplicate_object then null; end $$;

do $$ begin
  create type public.run_status as enum ('queued', 'running', 'succeeded', 'failed', 'canceled');
exception when duplicate_object then null; end $$;

do $$ begin
  create type public.message_direction as enum ('inbound', 'outbound');
exception when duplicate_object then null; end $$;

do $$ begin
  create type public.feedback_status as enum ('action_required', 'resolved');
exception when duplicate_object then null; end $$;
```

## 🧑‍🤝‍🧑 Teams & membership
```sql
create table if not exists public.teams (
  id uuid primary key default gen_random_uuid(),
  name text not null,
  owner_user_id uuid not null,
  created_at timestamptz not null default now()
);

create table if not exists public.team_members (
  team_id uuid not null references public.teams(id) on delete cascade,
  user_id uuid not null,
  role public.team_role not null,
  status text not null default 'active',
  invited_email text,
  created_at timestamptz not null default now(),
  primary key (team_id, user_id)
);

create index if not exists idx_team_members_user_id on public.team_members(user_id);
```

## 🧑 Profiles & onboarding data
```sql
create table if not exists public.profiles (
  user_id uuid primary key,
  team_id uuid not null references public.teams(id) on delete cascade,
  email text,
  display_name text,
  timezone text not null default 'UTC',
  plan text not null default 'trial',
  plan_ends_at timestamptz,
  profile_completeness_score numeric not null default 0,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
);

create index if not exists idx_profiles_team_id on public.profiles(team_id);
```

### CV files and profile snapshots
```sql
create table if not exists public.user_cv_files (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  user_id uuid not null,
  storage_path text not null,
  filename text,
  uploaded_at timestamptz not null default now()
);

create index if not exists idx_user_cv_files_team_user on public.user_cv_files(team_id, user_id);

create table if not exists public.user_profile_snapshots (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  user_id uuid not null,
  source text not null, -- 'cv' | 'linkedin' | 'upwork'
  source_url text,
  raw_json jsonb,
  parsed_json jsonb,
  parsed_at timestamptz,
  created_at timestamptz not null default now()
);

create index if not exists idx_user_profile_snapshots_team_user on public.user_profile_snapshots(team_id, user_id);
```

### Structured profile fields
```sql
create table if not exists public.user_skills (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  user_id uuid not null,
  name text not null,
  level int not null default 3, -- 1..5
  years numeric,
  created_at timestamptz not null default now()
);

create index if not exists idx_user_skills_team_user on public.user_skills(team_id, user_id);

create table if not exists public.user_experiences (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  user_id uuid not null,
  title text not null,
  company text,
  start_date date,
  end_date date,
  highlights text,
  created_at timestamptz not null default now()
);

create table if not exists public.user_educations (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  user_id uuid not null,
  school text,
  degree text,
  field text,
  start_year int,
  end_year int,
  created_at timestamptz not null default now()
);

create table if not exists public.user_preferences (
  user_id uuid primary key,
  team_id uuid not null references public.teams(id) on delete cascade,
  platforms text[] not null default array['upwork','linkedin'],
  currency text not null default 'USD',
  hourly_min numeric,
  hourly_max numeric,
  fixed_budget_min numeric,
  project_types text[] not null default array['short_gig','medium_project'],
  tightness int not null default 3, -- 1..5
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
);
```

## 🔎 Jobs (canonical + raw)
```sql
create table if not exists public.job_sources (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  platform public.platform not null,
  platform_job_id text not null,
  url text,
  fetched_at timestamptz not null default now(),
  raw_json jsonb,
  unique(team_id, platform, platform_job_id)
);

create index if not exists idx_job_sources_team_platform on public.job_sources(team_id, platform);

create table if not exists public.jobs (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  platform public.platform not null,
  platform_job_id text not null,
  title text not null,
  description text not null,
  apply_url text not null,
  posted_at timestamptz,
  fetched_at timestamptz not null default now(),

  budget_type text not null default 'unknown',
  fixed_budget_min numeric,
  fixed_budget_max numeric,
  hourly_min numeric,
  hourly_max numeric,
  currency text not null default 'USD',

  client_country text,
  client_rating numeric,
  client_hires int,
  client_payment_verified boolean,

  skills text[] not null default array[]::text[],
  seniority text,
  category text,

  canonical_hash text not null,
  source_raw jsonb,

  created_at timestamptz not null default now(),
  unique(team_id, canonical_hash)
);

create index if not exists idx_jobs_team_platform on public.jobs(team_id, platform);
create index if not exists idx_jobs_team_created_at on public.jobs(team_id, created_at desc);
create index if not exists idx_jobs_skills_gin on public.jobs using gin (skills);
```

### Job rankings
```sql
create table if not exists public.job_rankings (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  job_id uuid not null references public.jobs(id) on delete cascade,
  agent_run_id uuid,
  tightness int not null,
  score numeric not null,
  breakdown jsonb not null,
  created_at timestamptz not null default now()
);

create index if not exists idx_job_rankings_job_id_created_at on public.job_rankings(job_id, created_at desc);
```

## 🚀 Applications
```sql
create table if not exists public.applications (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  job_id uuid not null references public.jobs(id) on delete cascade,
  user_id uuid not null,
  status public.application_status not null default 'shortlisted',
  notes text,
  applied_at timestamptz,
  next_follow_up_at timestamptz,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now(),
  unique(team_id, job_id, user_id)
);

create index if not exists idx_applications_team_status on public.applications(team_id, status);
```

## ✍️ Cover letters (agent-written)
```sql
create table if not exists public.cover_letters (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  job_id uuid not null references public.jobs(id) on delete cascade,
  user_id uuid not null,
  style_preset text not null default 'professional',
  temperature numeric not null default 0.7,
  vocabulary_level int not null default 3, -- 1..5
  content text not null,
  model text,
  tokens_used int,
  created_at timestamptz not null default now()
);

create index if not exists idx_cover_letters_team_user_created_at on public.cover_letters(team_id, user_id, created_at desc);
```

## 💬 Messages & ⭐ feedback (agent-written)
```sql
create table if not exists public.messages (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  platform public.platform,
  thread_id text,
  direction public.message_direction not null,
  subject text,
  from_email text,
  to_email text,
  body_text text,
  body_html text,
  received_at timestamptz,
  raw jsonb,
  created_at timestamptz not null default now()
);

create index if not exists idx_messages_team_received_at on public.messages(team_id, received_at desc);

create table if not exists public.feedback (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  application_id uuid references public.applications(id) on delete set null,
  message_id uuid references public.messages(id) on delete set null,
  status public.feedback_status not null default 'action_required',
  category text,
  sentiment numeric,
  content text not null,
  created_at timestamptz not null default now(),
  resolved_at timestamptz
);

create index if not exists idx_feedback_team_status on public.feedback(team_id, status);
```

## 💰 Earnings
```sql
create table if not exists public.earnings (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  user_id uuid not null,
  platform public.platform,
  amount numeric not null,
  currency text not null default 'USD',
  occurred_at timestamptz not null,
  source_json jsonb,
  created_at timestamptz not null default now()
);

create index if not exists idx_earnings_team_occurred_at on public.earnings(team_id, occurred_at desc);
```

## 🤖 Agent runs & steps
```sql
create table if not exists public.agent_runs (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  user_id uuid,
  agent_type public.agent_type not null,
  trigger text not null, -- 'manual' | 'schedule' | 'email'
  status public.run_status not null default 'queued',
  inputs jsonb,
  outputs jsonb,
  error_text text,
  started_at timestamptz,
  finished_at timestamptz,
  created_at timestamptz not null default now()
);

create index if not exists idx_agent_runs_team_created_at on public.agent_runs(team_id, created_at desc);

create table if not exists public.agent_run_steps (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  agent_run_id uuid not null references public.agent_runs(id) on delete cascade,
  step_name text not null,
  status public.run_status not null default 'running',
  meta jsonb,
  started_at timestamptz,
  finished_at timestamptz,
  created_at timestamptz not null default now()
);

create index if not exists idx_agent_run_steps_run_id on public.agent_run_steps(agent_run_id, created_at asc);
```

## 🧠 Embeddings (vector memory)
```sql
create table if not exists public.embeddings (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  entity_type text not null, -- 'job' | 'profile'
  entity_id uuid not null,
  model text not null,
  dims int not null,
  embedding vector(1536) not null,
  meta jsonb,
  created_at timestamptz not null default now()
);

create index if not exists idx_embeddings_team_entity on public.embeddings(team_id, entity_type, entity_id);
create index if not exists idx_embeddings_ivfflat on public.embeddings using ivfflat (embedding vector_cosine_ops) with (lists = 100);
```

## ⚙️ Settings
```sql
create table if not exists public.team_settings (
  team_id uuid primary key references public.teams(id) on delete cascade,
  settings_json jsonb not null default '{}'::jsonb,
  updated_at timestamptz not null default now()
);

create table if not exists public.user_agent_settings (
  team_id uuid not null references public.teams(id) on delete cascade,
  user_id uuid not null,
  agent_type public.agent_type not null,
  settings_json jsonb not null default '{}'::jsonb,
  updated_at timestamptz not null default now(),
  primary key (team_id, user_id, agent_type)
);
```

## 📈 Usage counters
```sql
create table if not exists public.usage_counters (
  team_id uuid not null references public.teams(id) on delete cascade,
  day date not null,
  metric text not null,
  count int not null default 0,
  updated_at timestamptz not null default now(),
  primary key (team_id, day, metric)
);
```

## 🧾 Events + notifications
```sql
create table if not exists public.events (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  user_id uuid,
  event_type text not null,
  payload jsonb not null default '{}'::jsonb,
  created_at timestamptz not null default now()
);

create index if not exists idx_events_team_created_at on public.events(team_id, created_at desc);

create table if not exists public.notifications (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  user_id uuid,
  type text not null,
  payload jsonb not null default '{}'::jsonb,
  created_at timestamptz not null default now(),
  read_at timestamptz
);

create index if not exists idx_notifications_team_created_at on public.notifications(team_id, created_at desc);
```

## 🧷 Apply sessions (extension handoff)
```sql
create table if not exists public.apply_sessions (
  id uuid primary key default gen_random_uuid(),
  team_id uuid not null references public.teams(id) on delete cascade,
  user_id uuid not null,
  job_id uuid not null references public.jobs(id) on delete cascade,
  cover_letter_id uuid not null references public.cover_letters(id) on delete cascade,
  token_hash text not null,
  expires_at timestamptz not null,
  created_at timestamptz not null default now()
);

create unique index if not exists idx_apply_sessions_token_hash on public.apply_sessions(token_hash);
create index if not exists idx_apply_sessions_team_user_created_at on public.apply_sessions(team_id, user_id, created_at desc);
```
