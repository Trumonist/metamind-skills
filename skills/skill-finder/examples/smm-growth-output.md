# Skill Collection — SMM Growth Manager

**Role:** SMM Growth Manager (SMB/B2C)
**Date:** 2026-04-20
**Budget used:** 5/5 web_search, 4/6 web_fetch
**Status:** complete

---

## Role Brief (echo)

SMM Growth Manager for SMB/B2C. Focus: growth — turning social into lead-gen channel, not engagement-for-engagement. Works across TG channels, Instagram, X threads, LinkedIn. Writes copy, builds content strategy, designs lead magnets, measures growth. Russian-first, English secondary.

**Core tasks (from input):**
1. Write platform-specific posts (TG, IG, X, LinkedIn)
2. Craft hooks and CTAs
3. Plan content calendar and pillar topics
4. Build lead-gen funnels from social → email
5. Design DM / reply templates for community
6. Measure growth metrics (ENG, CTR, conversion)
7. Run A/B experiments

**Out of scope:** paid ads, scheduling tools, video editing, influencer outreach.

**Languages:** RU primary, EN secondary.
**Platforms:** TG, IG, X, LinkedIn, VK.

---

## Funnel Notes

- Marketing domain has **strong editorial hubs** (Corey Haines, Kim Barrett) — 2 searches with author names gave 4× the signal of generic queries.
- **4 repos cover 80% of useful skills** — coreyhaines31, OpenClaudia, alirezarezvani, realkimbarrett. Dedupe shows heavy overlap on fundamentals (copywriting, page-cro, email-sequence).
- **TG ecosystem almost absent** — all skills assume US/EN platforms. Russian-specific TG/VK are a clear gap.
- Aggregator blurbs hide `API key required` — `email-sequence` from OpenClaudia needs Resend token, not documented in one-liner.
- Hook-writing is scattered — no atomic `hook-writer` skill; every `copywriting` skill covers it partially.

---

## Gap Matrix

| Core task | Coverage | Best skill found | Rating |
|---|---|---|---|
| Write platform-specific posts (TG) | ❌ gap | — | — |
| Write platform-specific posts (IG carousel) | ✅ covered | `social-carousel-writer` (EvolutionAPI) | 7/10 |
| Write platform-specific posts (X threads) | ✅ covered | `thread-writer` (OpenClaudia) | 7/10 |
| Write platform-specific posts (LinkedIn) | ✅ covered | `linkedin-content` (OpenClaudia) | 8/10 |
| Craft hooks and CTAs | ⚠️ partial | `copywriting` (coreyhaines31) | 9/10 (but not atomic hook skill) |
| Content strategy / pillars | ✅ covered | `content-strategy` (coreyhaines31) | 9/10 |
| Content calendar | ✅ covered | `content-calendar` (OpenClaudia) | 8/10 |
| Repurposing one→many | ✅ covered | `content-repurposing` (OpenClaudia) | 8/10 |
| Lead magnets | ✅ covered | `lead-magnets` (coreyhaines31) | 9/10 |
| Landing / page-CRO | ✅ covered | `page-cro` (coreyhaines31) | 8/10 |
| Email capture / sequence | ⚠️ partial | `email-sequence` (coreyhaines31) | 7/10 (OpenClaudia variant needs API) |
| Community DM / reply | ❌ gap | — | — |
| Growth metrics framework | ❌ gap | `analytics-tracking` (coreyhaines31) | 6/10 (too generic) |
| A/B experimentation | ✅ covered | `ab-test-setup` (coreyhaines31) | 8/10 |

---

## A. Import-as-is

| Slug | Source | Cluster | Rating | Why |
|---|---|---|---|---|
| `copywriting` | coreyhaines31/marketingskills | copy | 9.8/10 | Consensus in 3 repos, MIT, minimal deps |
| `content-strategy` | coreyhaines31/marketingskills | strategy | 9.0/10 | Pillar+cluster framework by editorial hub |
| `lead-magnets` | coreyhaines31/marketingskills | lead-gen | 9.0/10 | Core growth move, stdlib-only |
| `ab-test-setup` | coreyhaines31/marketingskills | exp | 8.0/10 | Clean experiment design |
| `page-cro` | coreyhaines31/marketingskills | lead-gen | 8.0/10 | Landing optimization for social traffic |
| `content-repurposing` | OpenClaudia/openclaudia-skills | strategy | 8.0/10 | Atomization one→many |
| `content-calendar` | OpenClaudia/openclaudia-skills | strategy | 8.0/10 | Planning calendar |
| `linkedin-content` | OpenClaudia/openclaudia-skills | platform | 8.0/10 | LinkedIn specialized |
| `thread-writer` | OpenClaudia/openclaudia-skills | platform | 7.0/10 | X/Reddit thread format |
| `marketing-psychology` | coreyhaines31/marketingskills | copy | 7.0/10 | Behavioral frameworks for hooks |
| `copy-editing` | coreyhaines31/marketingskills | copy | 8.0/10 | Pair with copywriting for edit pass |
| `free-tool-strategy` | coreyhaines31/marketingskills | lead-gen | 8.0/10 | Free tool as lead-magnet tactic |
| `signup-flow-cro` | coreyhaines31/marketingskills | lead-gen | 7.0/10 | Post-click conversion |
| `analytics-tracking` | coreyhaines31/marketingskills | metrics | 7.0/10 | Generic, applicable foundation |

**Total: 14 skills from 2 repos.** All MIT-licensed, sandbox-friendly, no API requirements.

---

## B. Fork-adapt

| Slug | Source | What to change | Why |
|---|---|---|---|
| `social-carousel-writer` | EvolutionAPI/evo-nexus | Add RU examples, adapt CTAs to RU market | EN-only, carousel structure solid but copy templates not RU-idiomatic |
| `direct-response-copy` | boringmarketer gist | Add RU classic ad frameworks (Огилви + рус копирайтеры) | Schwartz/Hopkins only, missing RU heritage (Гари Бенсивенга, Каплан) |
| `newsletter` | OpenClaudia | Switch email-provider examples from Resend to Substack/Beehiiv generic | Current version tied to Resend API |

---

## C. Write-ourselves

| Proposed slug | Task it covers | Why nothing exists | Effort (days) |
|---|---|---|---|
| `tg-channel-writer` | TG channel posts with polls / voice / stickers / threads | Ecosystem is US-heavy; TG barely covered, VK absent | 1.0 |
| `smm-hook-writer` | 10 hook frameworks × brief → 3 variants | Hooks scattered inside copywriting; no atomic skill | 0.5 |
| `smm-engagement-toolkit` | DM sequences, reply templates, comment triggers | `community-marketing` is abstract; no tactical toolkit | 0.5 |
| `smm-growth-metrics` | What to measure per platform + simple sheets | `analytics-tracking` is generic; no SMM-specific framework | 0.5 |

**Total: 4 new skills, ~2.5 days effort.**

---

## Recommendation

**Total:** 14 imports + 3 forks + 4 new = **21 skills for complete SMM Growth role coverage.**

**Priority order:**
1. Import 14 as-is (today — just add to `skills` table)
2. Write `smm-hook-writer` + `tg-channel-writer` first (0.5 + 1 day) — highest leverage
3. Fork 3 adapted versions in parallel (days 2-3)
4. Finish `smm-engagement-toolkit` + `smm-growth-metrics` (0.5 + 0.5 day)

**Quick wins (import today):** `copywriting`, `content-strategy`, `lead-magnets`.

**Longest gap:** `tg-channel-writer` — no EN source to adapt from; must be written from scratch for Russian market. Highest unique value.
