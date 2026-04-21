# Skills Sources Catalog

Curated list of places to find Claude/Agent skills. Ordered by priority for use in `skill-finder`.

## Tier 1 — Official (Anthropic)

- **anthropics/skills** — github.com/anthropics/skills — 14+ built-in skills (pdf, docx, pptx, xlsx, canvas-design, brand-guidelines, doc-coauthoring, internal-comms, skill-creator, algorithmic-art, web-artifacts-builder, slack-gif-creator, mcp-builder, theme-factory). Always check first.
- **anthropics/claude-code** (plugins/) — github.com/anthropics/claude-code — CLI-oriented plugins. Skills here require Claude Code CLI runtime; many NOT compatible with managed-agents `/process_api` runtime.
- **Agent Skills spec** — agentskills.io — canonical SKILL.md format reference.

## Tier 2 — Large aggregators

- **VoltAgent/awesome-agent-skills** — github.com/VoltAgent/awesome-agent-skills — 1100+ skills from 33+ companies. Main source for vendor-official skills.
- **sickn33/antigravity-awesome-skills** — 1400+ cross-platform skills. High overlap with other aggregators.
- **ComposioHQ/awesome-claude-skills** — 50+ skills with workflow categorization.
- **travisvn/awesome-claude-skills** — curated list with quality signals.
- **BehiSecc/awesome-claude-skills** — supplementary curated list.

## Tier 3 — Marketplace sites (web UI)

- **claudemarketplaces.com** — 4200+ skills, 770+ MCP, 28 categories. No public API — scrape only.
- **awesome-skills.com** — 150+ tagged skills (dev/ai/content/security/3d/office/mcp).
- **claudeskills.info** — searchable marketplace.
- **mcpmarket.com/tools/skills** — Claude/ChatGPT/Codex directory.
- **lobehub.com/skills** — LobeHub marketplace.

## Tier 4 — Vendor collections (inside VoltAgent list)

Reference-quality skills from single vendors:
- Microsoft (133), OpenAI (44), Sentry (33), Trail of Bits (21), Google Workspace (17), fal.ai (15), Hugging Face (13), Netlify (12), Cloudflare (8), Vercel (7), Figma (7), Better Auth (7), ClickHouse (6), Firecrawl (5), Stripe (2), HashiCorp Terraform (10), Expo (11)

Most require auth / API keys — flag accordingly.

## Tier 5 — Marketing-domain editorial hubs

For the marketing / SMM / growth domain specifically:
- **coreyhaines31/marketingskills** — 36 skills, CRO/copywriting/SEO/analytics/growth. High consensus quality.
- **OpenClaudia/openclaudia-skills** — 60+ marketing skills. Some require Resend API for email.
- **realkimbarrett/advertising-skills** — ~30 skills, direct-response advertising focus.
- **alirezarezvani/claude-skills** — 232+ cross-domain; marketing-skill/ folder has 44.
- **boringmarketer** (gists) — Direct Response Copy single-file skills.

For OTHER domains — search for the equivalent editorial hub (pattern: person publishes multiple skills under one repo and has social presence in that domain).

## Tier 6 — MCP servers (adapter source)

MCP servers can be wrapped into skills via Anthropic's `mcp-builder` skill.
- **smithery.ai** — 1000+ MCP servers.
- **mcpservers.org** — directory.

## Tier 7 — Discovery (not for bulk, for long-tail)

- GitHub code search: `filename:SKILL.md` or `path:skills/` filter by stars >10.
- Reddit `r/ClaudeAI` pinned weekly threads.
- Twitter/X hashtags: `#ClaudeSkills`, `#AgentSkills`.
- Medium editorials: "All About Claude" collection.

---

## Fetch priority for skill-finder workflow

When executing step 4 (Collect), use this order. Stop after 4 sources.

1. Author-hub repo for the role's domain (from step 2) — **if found**
2. `anthropics/skills` — always
3. `VoltAgent/awesome-agent-skills` — always
4. One of: OpenClaudia / ComposioHQ / awesome-skills.com — depending on domain

Rationale: author hubs give consensus; anthropics sets format baseline; VoltAgent covers vendor-official; the fourth source fills long-tail domain-specific.
