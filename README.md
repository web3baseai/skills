# Web3Base AI Skills

[![Web3Base](https://img.shields.io/badge/Web3Base-www.web3base.ai-blue)](https://www.web3base.ai/)

AI skill package for [Web3Base](https://www.web3base.ai/) — let AI assistants query Web3 chains, Twitter KOLs, YouTube channels, site navigation, and taxonomy through the Web3Base Open API.

## Quick Install

```bash
npx skills add https://github.com/web3baseai/skills --skill web3base-ai
```

Verify installation:

```bash
npx skills list
```

## Available Skills

### `web3base-ai`

A unified skill that routes natural-language requests to the correct [Web3Base Open API](https://www.web3base.ai/api/open/spec) endpoint.

| Domain | Endpoint | What it does |
|--------|----------|-------------|
| Chains | `GET /api/open/chains` | Search, filter, sort blockchains by TVL, name, ecosystem |
| Twitter | `GET /api/open/twitter/users` | Discover and filter Web3 KOLs and creators |
| YouTube | `GET /api/open/youtube/channels` | Find and compare Web3 educational/news channels |
| Navigation | `GET /api/open/nav/*` | Explore Web3Base site structure, menus, and tool cards |
| Taxonomy | `GET /api/open/categories` | Resolve category labels, i18n mappings, filter dimensions |

All requests go to **`https://www.web3base.ai`**.

### Example Queries

```
"Find top L2 chains related to AI"
 → GET https://www.web3base.ai/api/open/chains?q=AI&sort=tvl&sortDir=desc

"Show me DeFi Twitter KOLs"
 → GET https://www.web3base.ai/api/open/twitter/users?tag=defi

"List YouTube channels about Ethereum"
 → GET https://www.web3base.ai/api/open/youtube/channels?q=ethereum
```

## API Overview

| Item | Detail |
|------|--------|
| Base URL | `https://www.web3base.ai` |
| Auth | Optional `X-API-Key` header |
| Rate limit | 60 req/min (public) · 300 req/min (with API key) |
| Spec | `GET /api/open/spec` |

Full endpoint reference: [web3base-ai/references/open-api-alignment.md](web3base-ai/references/open-api-alignment.md)

## Project Structure

```
skills/
├── README.md                           # Project overview
├── CLAUDE.md                           # Conventions for AI assistants
├── skills-lock.json                    # Skill registry lock
└── web3base-ai/
    ├── SKILL.md                        # Skill definition & routing logic
    └── references/
        └── open-api-alignment.md       # Full API reference (params, schemas, examples)
```

## Contributing

1. Fork this repo
2. Create or edit a skill under its own directory
3. Each skill must have a `SKILL.md` with valid frontmatter (`name`, `description`, `when_to_use`, `version`)
4. Include API examples with full URLs (`https://www.web3base.ai/...`)
5. Submit a PR

## License

MIT
