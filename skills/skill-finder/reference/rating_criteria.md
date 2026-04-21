# Rating Criteria

Each skill gets scored on 8 dimensions (0-5 each, total 40 max, normalize to /10 for output).

## 1. Sandbox-friendly (weight 5) — the critical filter

- **5** — runs entirely inside sandbox with no external calls
- **4** — uses only built-in agent_toolset (web_fetch/web_search/bash/edit/glob/grep/read/write)
- **3** — requires user-provided file as input, nothing else
- **2** — requires internet to a single public API without auth
- **1** — requires auth but has free tier / demo mode
- **0** — requires paid API key / OAuth / private MCP

Managed-agents runtime is `/process_api`, NOT Claude Code CLI. Skills that depend on CLI-specific features (e.g., `.claude/commands`, hooks, IDE integration) score ≤2.

## 2. No-secrets (weight 5)

- **5** — zero credentials
- **4** — optional credentials for enhanced features
- **2** — requires setup but instructions clear
- **0** — requires secrets that we cannot safely distribute (per-user auth, vendor locks)

## 3. Clarity (weight 4)

- **5** — SKILL.md reads like a spec: clear triggers, input/output, examples
- **3** — readable but some implicit steps
- **1** — vague, reader must guess

## 4. Reusability (weight 4)

- **5** — applies to many concrete use cases within the role
- **3** — narrow but useful for multiple triggers
- **1** — one-shot, hard-coded to a specific workflow

## 5. Maintenance (weight 3)

- **5** — commit in last 30 days
- **4** — commit in last 90 days OR from a trusted vendor (Anthropic / Microsoft / OpenAI / major SaaS)
- **2** — commit in last 12 months
- **0** — abandoned / archived / >12 months stale

## 6. License (weight 3)

- **5** — MIT, Apache-2.0, CC0, Unlicense, BSD
- **4** — Mozilla Public License, LGPL
- **2** — GPL (viral — may restrict commercial bundling)
- **1** — custom permissive
- **0** — no LICENSE file OR "all rights reserved" OR explicit non-commercial

## 7. Docs quality (weight 2)

- **5** — README + reference/ folder + examples/ folder
- **3** — README with at least 1 worked example
- **1** — bare SKILL.md with no supporting content

## 8. Dependencies (weight 2)

- **5** — pure SKILL.md, no scripts, no libs
- **4** — Python stdlib only
- **3** — widely-installed libs (requests, pydantic, pandas)
- **1** — heavy ML libs or custom binaries
- **0** — requires compilation or GPU

## Score bands → verdict

| Total (/40) | /10 | Verdict |
|---|---|---|
| 32-40 | 8-10 | **import-as-is** — add to collection directly |
| 22-31 | 5.5-7.9 | **fork-adapt** — useful but needs our customization |
| 0-21 | 0-5.4 | **write-ourselves** (better to start fresh) OR **reject** |

## Auto-rejects (before scoring)

- SKILL.md < 20 lines → `excluded_too_thin`
- Last commit > 18 months AND not from trusted vendor → `excluded_abandoned`
- No LICENSE + no permission signal → `excluded_unclear_rights`
- Requires Claude Code CLI features only → `excluded_cli_only`

## Examples

**High score (9/10):**
> `copywriting` from coreyhaines31 — SKILL.md 150 lines, clear triggers, no deps, MIT, commit 2 weeks ago, 3 worked examples, consensus in 3 aggregators. Scores: 5+5+5+4+5+5+5+5 = 39/40 = **9.75/10**.

**Fork-adapt (6/10):**
> `linkedin-content` from OpenClaudia — good skill but LinkedIn-specific and in English only. For Russian SMM we'd need to add TG/VK examples and translate tone guidance. Scores: 4+4+4+3+4+5+2+2 = 28/40 = **7/10** → fork-adapt (localize).

**Reject:**
> `instagram-automation` uses Rube MCP (Composio) + requires IG OAuth. Score: 0 (sandbox) + 0 (secrets) + rest moot → `excluded_requires_paid_auth`.
