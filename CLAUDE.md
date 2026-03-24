# Web3Base Skills — Project Conventions

## What This Project Is

This repository contains AI skills for [Web3Base](https://www.web3base.ai/). Skills are structured prompts that teach AI assistants how to interact with Web3Base's public APIs.

## API Base URL

All Web3Base API calls MUST use: `https://www.web3base.ai`

## Skill Structure

Each skill lives in its own directory under the repo root:

```
<skill-name>/
├── SKILL.md              # Required. Skill definition with frontmatter.
└── references/           # Optional. Supporting docs, API specs, examples.
```

### SKILL.md Frontmatter

```yaml
---
name: <skill-name>        # Must match directory name
description: <one-liner>  # Used for skill routing/matching
when_to_use: <trigger>    # When AI should activate this skill
version: <semver>         # Semantic version
---
```

## Conventions

- Skill names use kebab-case: `web3base-ai`, not `web3baseAI`
- All API examples must include the full URL (base + path + params)
- References should document every endpoint parameter and response schema
- Keep SKILL.md focused on routing logic and usage flow
- Keep detailed API specs in `references/`
- Use bilingual (English + Chinese) descriptions in SKILL.md
