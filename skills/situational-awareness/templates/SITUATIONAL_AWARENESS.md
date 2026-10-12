---
updated: {{now}}
owner: {{person.name}}
domains: {{json watch.world.domains}}
horizon: The model's training cutoff is around {{watch.world.model_cutoff}}. Everything here postdates or corrects that.
---

# Situational awareness: {{person.name}}'s fields

**What this is.** A running map of named things in {{person.name}}'s fields that happened *after* the model's training cutoff. These are the products, rulings, deals, incidents, papers, and coined terms that people in the field now use as shorthand without explaining them. It is maintained daily by the `situational-awareness` skill, from {{person.name}}'s own feeds plus gap-filling research.

**How agents use it.**
1. The newest entries, plus every older term by name, are injected at session start. Before you call any term obscure, undefined, or nonexistent, check here.
2. If a term is here, treat it as known in this field. Don't over-explain it.
3. If a term isn't here and could postdate your training, **say you're unsure and search**. Never assert absence from ignorance.
4. Detail on one term: `sa-lookup.ts <term>`. Don't read this whole file; it grows without bound by design.

---

## Glossary

Newest first. Format: `- **Name** *(a.k.a. …)* — YYYY-MM-DD — gloss. → [Primary](url) · [Secondary](url)`

<!-- SA:GLOSSARY-END -->

## Live storylines

Active threads (≤12). Each thread: one paragraph, dated movements, `[[Glossary Name]]` cross-links.

<!-- SA:STORYLINES-END -->

## Cooled

Threads with no movement in ~3 weeks. The summary is kept here; their glossary terms stay in the Glossary.

## Candidates

Seen but not yet promoted (≤20). Drop anything not promoted after ~5 runs.

## Maintenance

Budgets and rotation rules: see the skill's SKILL.md § Budgets. Health: `sa-health.ts`.
