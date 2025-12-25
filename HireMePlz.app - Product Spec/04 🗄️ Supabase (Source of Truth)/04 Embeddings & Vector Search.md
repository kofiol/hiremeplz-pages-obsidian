# 🗄️ Supabase — Embeddings & Vector Search

## ✅ What embeddings are used for
- Improve job ranking beyond keyword matching.
- Match job descriptions to the user’s profile highlights.
- Power “similar jobs” and “why this matches” explanations.

## 🧠 Entities embedded
- `jobs` (description + requirements)
- `profiles` (parsed CV summary + skills + experience highlights)

## 🧾 Embedding generation
- Generated only in workflows.
- Stored in `public.embeddings`.

**Model**
- Use a single embedding model consistently (store in `embeddings.model`).
- Keep `dims` explicit.

## 🔎 Similarity query (cosine)
Example query to retrieve similar jobs:
```sql
select j.id, j.title, 1 - (e.embedding <=> q.embedding) as similarity
from public.jobs j
join public.embeddings e
  on e.entity_type = 'job'
 and e.entity_id = j.id
join public.embeddings q
  on q.entity_type = 'profile'
 and q.entity_id = :profile_entity_id
where j.team_id = :team_id
order by e.embedding <=> q.embedding
limit 50;
```

## 🧯 Safety + cost control
- Only embed the top N jobs per run (e.g. N=200) based on preliminary score.
- Reuse embeddings for unchanged jobs using `canonical_hash`.

