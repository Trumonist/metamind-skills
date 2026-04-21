---
name: skill-finder
description: Given a role brief (what the user's agent must do), find existing Claude/Agent skills that cover the role, rate them, cluster by task, and produce a shortlist with gap analysis (what to import vs fork vs write from scratch). Use when the user asks to "find skills for <role>", "research what skills exist for <domain>", "build a skill collection for <profession>", or wants to extend an agent with a curated set of skills.
---

# skill-finder

Research pipeline for building a role-specific skill collection. Input — role brief. Output — shortlist markdown with 3 sections: `import-as-is`, `fork-adapt`, `write-ourselves`.

## When to use this skill

- User wants to build or extend an agent for a specific role ("SMM manager", "legal assistant", "data analyst")
- User asks "what skills exist for <domain>" or "find me the best <X> skills"
- User wants gap analysis — what's already public vs what we need to write

## When NOT to use

- User asks to CREATE a new skill — use `skill-creator` instead
- User asks to LIST currently installed skills — use `/skills list` Telegram command
- User wants to install a specific already-known skill — use `/skills add <slug>` directly

## Budget (hard cap)

Do not exceed on a single run:
- **5 web_search calls**
- **6 web_fetch calls**
- **1 shortlist markdown** (< 500 lines)

If budget runs out before all clusters covered — output partial shortlist + explicit `[INCOMPLETE: clusters X,Y not researched]` marker.

## Workflow — 10 steps

### 1. Parse role brief

Read the user's role description. Extract:
- `role_name` — one phrase
- `core_tasks` — 5-8 concrete things the role does (verbs, not categories)
- `out_of_scope` — things explicitly NOT the role's job
- `languages` — working languages (ru, en, …)
- `platforms` — if role is platform-specific (TG, IG, LinkedIn…)

If brief is vague or under 3 sentences — ask ONE clarifying question before proceeding. Do not research on thin briefs.

### 2. Author hubs

For the role's domain, identify 1-3 editorial persons who publish skill collections. These are consensus-builders and save query budget. Examples from marketing domain: Corey Haines, Kim Barrett.

Query pattern: `<domain> github skills library personal` or `best <domain> claude skills author`.

If no clear editorial persons exist in the domain — skip and rely on aggregators.

### 3. Query expansion

For each core_task, write ONE narrow search query. Narrow = includes platform + format + skill word.

Good: `instagram carousel hook copywriting SKILL.md github`
Bad: `social media skills` (too broad, wastes budget)

Cap: max 3 queries in this step (plus 1-2 from step 2 = total ≤5 web_search).

### 4. Collect (aggregators)

Fetch the following PRIORITY sources in order (stop after 4):
1. Author hub repos found in step 2
2. `anthropics/skills` — official built-ins (always check)
3. `VoltAgent/awesome-agent-skills` — 1100+ curated
4. `OpenClaudia/openclaudia-skills` — 60+ marketing-focused
5. `ComposioHQ/awesome-claude-skills` — 50+ with categories
6. `awesome-skills.com` — 150+ tagged

Use `reference/sources.md` for full URL catalog and context. Do not fetch more than 4 aggregators total.

### 5. Dedupe

List all skills found. Mark `consensus_count = how many sources list it`. Skills with consensus ≥2 are safer picks.

### 6. Deep-read (top 20)

For the top 20 by relevance to role tasks, fetch the actual `SKILL.md` file (not the aggregator description). Extract:
- `dependencies`: none / api-key-required / mcp-required / file-upload-required
- `sandbox_ok`: yes / partial / no
- `trigger_clarity`: is description specific enough for auto-invocation?
- `has_examples`: yes / no

Skip skills with `SKILL.md` under 20 lines (too thin) — mark `excluded_too_thin`.

### 7. License check

For each top-20 skill, note the repo license. Accept: MIT, Apache-2.0, CC0, BSD, Unlicense.
Flag for review: GPL, AGPL, proprietary, no LICENSE file.
Exclude: explicit "all rights reserved" without usage permission.

### 8. Rating (0-5 each, 8 criteria = 40 max)

Score each surviving top-20 skill:
1. **Sandbox-friendly** (5) — works without external API/auth
2. **No-secrets** (5) — no credential setup needed
3. **Clarity** (4) — SKILL.md readable, no gaps
4. **Reusability** (4) — applies to >1 concrete use case
5. **Maintenance** (3) — commit in last 3 months OR from trusted vendor
6. **License** (3) — permissive open-source
7. **Docs quality** (2) — has README + working examples
8. **Dependencies** (2) — minimal (stdlib only preferred)

Convert to 0-10 scale (divide by 4) for readability in output.

### 9. Gap matrix

Build a table: `core_tasks × found_skills`. Explicitly mark cells:
- `✅ covered` — ≥1 skill rated 7+
- `⚠️ partial` — skill exists but rated 5-7 or needs adaptation
- `❌ gap` — no adequate skill found

Every `❌ gap` row is a candidate for "write-ourselves".

### 10. Output markdown (three sections)

Write to `/mnt/session/outputs/skill-finder-<role_slug>-<YYYY-MM-DD>.md` using the template in `reference/output_schema.md`. Three mandatory sections:

**A. Import-as-is** — rating ≥7, sandbox-friendly, permissive license. Format: table with `slug | source_repo | cluster | rating | why`.

**B. Fork-adapt** — rating 5-7, good direction but needs our customization (e.g. Russian adaptation, de-duping, de-vendor-lock). Format: table with `slug | what_to_change | why`.

**C. Write-ourselves** — every `❌ gap` row from step 9. Format: table with `proposed_slug | task_it_covers | why_nothing_exists | estimated_effort_days`.

Include at top:
- Role brief summary (echo back user input)
- Budget used (queries / fetches used out of cap)
- Funnel notes (what surprised you, where evidence was weak)

## Reference files

- `reference/sources.md` — URL catalog (Tier 1-7)
- `reference/funnel.md` — expanded step-by-step details with examples
- `reference/rating_criteria.md` — scoring rubric with examples
- `reference/output_schema.md` — exact markdown template for step 10

## Examples

- `examples/smm-growth-brief.md` — canonical role brief (SMM Growth Manager)
- `examples/smm-growth-output.md` — reference output produced by a v1 manual run

## Anti-patterns (do NOT do this)

- Fetch more than 4 aggregators (marginal utility drops fast)
- Use broad queries like `social media skills`
- Output only skills without gap analysis — the "write-ourselves" section is the most valuable part
- Skip deep-read of SKILL.md — aggregator blurbs often hide dependencies
- Include paid/API-locked skills without marking `⚠️ needs-API-key`
- Rate without reading — "looks good" is not a rating

## Output contract

The final markdown file MUST have:
- Three explicit sections (A/B/C) even if some are empty
- Every listed skill has: slug, source URL, rating 0-10, cluster
- Every gap has: proposed slug, effort estimate in days
- Budget line at top: `Budget used: X/5 searches, Y/6 fetches`
