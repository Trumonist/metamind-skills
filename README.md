# MetaMind Public Skills

Curated SKILL.md bundles for Claude Managed Agents via Telegram (MetaMind bridge).

## How these load

Each managed bot in MetaMind bridge can subscribe to skills via `/skills add <slug>`.
On next session create, subscribed skills are mounted into the sandbox via
`session.resources[{"type":"github_repository","url":"https://github.com/Trumonist/metamind-skills",...}]`
at `/workspace/metamind-skills/`.

The agent reads `SKILL.md` files and triggers them based on their `description`
frontmatter when the user's request matches.

## Skills catalog

| Folder | Trigger |
|--------|---------|
| `skills/russian-poetry-styler/` | Write/rewrite text in the style of classic Russian poets |

## Contributing

Each skill = one folder under `skills/` with:
- `SKILL.md` — required, YAML frontmatter (`name`, `description`) + instructions body
- `examples/` — optional reference material the agent can read on demand
- `scripts/` — optional Python/bash helpers

Keep skills self-contained. No API keys, no network access assumptions beyond
what the sandbox provides by default (python, node, bash, web_fetch).
