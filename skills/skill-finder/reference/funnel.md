# Funnel — expanded walkthrough

Full step-by-step with examples. Use this as reference if SKILL.md steps feel abstract.

## Step 1 — Parse role brief

Extract `core_tasks` as **verbs with objects**, not categories.

Bad: `marketing`, `content`, `growth`
Good: `write IG carousels`, `design lead magnets`, `plan content calendar`

5-8 tasks is the right granularity. Fewer = too abstract to find specific skills. More = voronka gets too narrow per task.

**Example (SMM Growth):**
```
core_tasks:
  1. write platform-specific posts (TG, IG, X, LinkedIn)
  2. craft hooks and CTAs
  3. plan content calendar and pillar topics
  4. build lead-gen funnels from social → email
  5. design DM / reply templates for community
  6. measure growth metrics (ENG, CTR, conversion)
  7. run A/B experiments on hooks and formats
```

## Step 2 — Author hubs

Human editors curate consensus. Finding them saves 2-3 web_search queries.

How to find:
- `<domain> github skills library personal` — finds single-author repos
- Cross-reference: if a person's repo is linked from 2+ aggregators, they're a hub
- Check X/Twitter for announcement threads ("I built a skills pack")

**Found hubs by domain:**
- Marketing / growth → Corey Haines (`coreyhaines31`)
- Direct response / ads → Kim Barrett (`realkimbarrett`)
- ML education → Forrest Chang (forrestchang)
- Data viz → Safi Shamsi (safishamsi)

For new domain — spend 1 search on hub discovery. If no hub found in 1 query, skip and rely on aggregators.

## Step 3 — Query expansion

Cap at 3 queries. Each query must have THREE dimensions:
- Platform or format (instagram, threads, PDF, xlsx…)
- Skill word (skill, SKILL.md, plugin)
- Role verb (carousel, copywriting, analysis…)

**Good queries:**
- `instagram carousel copywriting SKILL.md github`
- `B2B cold email sequence skill claude`
- `xlsx financial model skill anthropic`

**Bad queries (waste budget):**
- `claude skills` — 50M results, none specific
- `marketing skills` — too broad
- `best skills for SMM` — editorial content, not actual skills

## Step 4 — Collect

Do NOT fetch 10 sources. Marginal utility after 4 is <10%.

Priority order (take 4):
1. Author hub (if found in step 2)
2. `anthropics/skills` (always — built-ins must be known)
3. `VoltAgent/awesome-agent-skills` (main vendor list)
4. One domain-relevant aggregator:
   - Marketing/growth → `OpenClaudia/openclaudia-skills`
   - Dev tools → `ComposioHQ/awesome-claude-skills`
   - General → `awesome-skills.com`

## Step 5 — Dedupe

Produce a list with `consensus_count` per skill (how many sources list it).

Skills with `consensus_count ≥ 2` that are NOT obviously identical deserve a closer look — community convergence is a signal.

But consensus is NOT the same as quality. Some dubious skills appear in multiple aggregators due to auto-scraping.

## Step 6 — Deep-read (top 20)

This is the step most likely to be skipped, but **the one that saves our users from broken skills**.

For each top-20 candidate, fetch the raw SKILL.md. Do NOT trust aggregator descriptions — they're often copied without verification.

Red flags to look for in SKILL.md body:
- `export ANTHROPIC_API_KEY` in setup → API needed at runtime (expected)
- `export STRIPE_SECRET_KEY` / `export FIGMA_TOKEN` → vendor lock
- `pip install proprietary-sdk` → cannot run in clean sandbox
- References to `.claude/commands` → CLI-only, won't work in managed-agents
- "This skill requires Claude Code Desktop" → CLI-only

## Step 7 — License

One web_fetch of the repo root is enough to see LICENSE file. If no LICENSE: mark for review, don't auto-accept.

Default interpretation:
- No LICENSE = all rights reserved → cannot redistribute. Can use as inspiration only.

## Step 8 — Rating

Use `rating_criteria.md`. Be honest — scores below 5 are normal and valuable (they drive write-ourselves).

Do not inflate scores to "save" skills. If a skill needs a paid API, score 0 on sandbox-friendly. That IS the correct rating.

## Step 9 — Gap matrix

This is the most valuable deliverable. `✅` / `⚠️` / `❌` per task.

If all tasks are `✅` — question the brief (probably too generic — real roles always have unique gaps).

## Step 10 — Output

Follow `output_schema.md` template exactly. Three sections A/B/C are the product.

Validation checklist at end prevents ship with missing sections.
