# Output Schema

Template for the final deliverable markdown. Write to `/mnt/session/outputs/skill-finder-<role_slug>-<YYYY-MM-DD>.md`.

## Template

```markdown
# Skill Collection — <Role Name>

**Role:** <role_name>
**Date:** <YYYY-MM-DD>
**Budget used:** <X>/5 web_search, <Y>/6 web_fetch
**Status:** complete | [INCOMPLETE: clusters not researched]

---

## Role Brief (echo)

<one-paragraph summary of what the role does>

**Core tasks (from input):**
1. <task 1>
2. <task 2>
...

**Out of scope:**
- <item>
...

**Languages:** <ru | en | both>
**Platforms:** <if applicable>

---

## Funnel Notes

Brief observations (3-5 bullets):
- Surprising findings
- Weak evidence spots
- Sources that over/underperformed

---

## Gap Matrix

| Core task | Coverage | Best skill found | Rating |
|---|---|---|---|
| <task 1> | ✅ covered | `<slug>` | 9/10 |
| <task 2> | ⚠️ partial | `<slug>` (needs ru adaptation) | 6/10 |
| <task 3> | ❌ gap | — | — |

---

## A. Import-as-is

Skills rated ≥7, sandbox-friendly, permissive license — add directly to collection.

| Slug | Source | Cluster | Rating | Why |
|---|---|---|---|---|
| `copywriting` | coreyhaines31/marketingskills | copy | 9.8/10 | Consensus pick, MIT, no deps |
| `content-strategy` | coreyhaines31/marketingskills | strategy | 9.0/10 | Pillar+cluster planning |
| ... | | | | |

**Integration note:** these skills are added via `skills` table with `repo_url=<url>`, `source=community`, `visibility=private` (for user's own bot) or `visibility=public` (if curated into MetaMind collection — requires review gate, not in MVP).

---

## B. Fork-adapt

Rating 5-7, good direction but needs our customization.

| Slug | Source | What to change | Why |
|---|---|---|---|
| `linkedin-content` | OpenClaudia | Add TG/VK examples, translate tone to RU | Role is RU-first, LinkedIn is secondary |
| ... | | | |

**Integration note:** fork means copy SKILL.md into `users/<tg_id>/skills/<slug>/` on our monorepo, modify, attribute original author in SKILL.md frontmatter `based_on: <original-url>`.

---

## C. Write-ourselves

Every ❌ gap from matrix becomes a proposed skill here.

| Proposed slug | Task it covers | Why nothing exists | Effort (days) |
|---|---|---|---|
| `tg-channel-writer` | TG channel posts with polls/stickers/voice | Ecosystem is EN-heavy; TG barely covered | 1 |
| `smm-hook-writer` | 10-framework hook matrix | Scattered inside copywriting skills | 0.5 |
| ... | | | |

**Integration note:** these go into `skills/<slug>/` on metamind-skills repo (public) OR `users/<tg_id>/skills/<slug>/` (private, user-specific variant).

---

## Recommendation

**Total:** <N> imports + <M> forks + <K> new = <N+M+K> skills for complete role coverage.
**Priority order:** <which to build first>
**Quick wins (import today):** <top 3 by rating>
**Longest gap:** <which "write-ourselves" has highest leverage>
```

---

## Validation checklist

Before finalizing output, confirm:
- [ ] Budget line filled in (searches/fetches)
- [ ] Three sections (A/B/C) all present even if empty
- [ ] Every listed skill has source URL
- [ ] Every gap has effort estimate
- [ ] At least 5 rows in gap matrix
- [ ] Role brief echoed back (user must recognize their input)
- [ ] No skill appears in both A and B sections

If any check fails — fix before writing to disk.
