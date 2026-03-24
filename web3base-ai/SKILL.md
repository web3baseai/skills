---
name: web3base-ai
description: Unified Web3Base content skill for chains, Twitter, YouTube, navigation, and taxonomy. Use whenever the user asks to discover, compare, filter, rank, map, or explain Web3Base public data, especially when intent is mixed or the user does not know which endpoint to call.
when_to_use: Any broad or cross-domain content request about chains/twitter/youtube/nav/categories, or whenever one skill should handle end-to-end data lookup and synthesis.
version: 1.0.0
---

# web3base-ai

## Summary / 概述

English: One skill to query and synthesize Web3Base public content across chains, Twitter users, YouTube channels, navigation, and taxonomy.
中文：一个技能统一处理 Web3Base 公开内容查询与整合，覆盖链数据、Twitter 用户、YouTube 频道、站点导航与分类体系。

## Pre-flight Checks / 预检

Before routing any request, always do these checks:

1. Identify whether the user intent is single-domain or cross-domain.
2. Extract hard constraints first: `query`, `limit`, `locale`, `module`, `scope`, explicit slugs/ids.
3. Validate constraints before call execution:
   - `module` for category groups must be one of `chain|twitter|youtube`.
   - `scope` for categories should be `chain|twitter|youtube|item`.
4. If constraints are missing but required for precision, ask one focused follow-up question.
5. Never invent records when upstream data is missing.

## Skill Routing / 路由规则

- **Chains intent / 链检索与对比**  
  Route to `GET /api/open/chains` semantics.
- **Twitter intent / Twitter 账号分析**  
  Route to `GET /api/open/twitter/users` semantics.
- **YouTube intent / YouTube 频道评估**  
  Route to `GET /api/open/youtube/channels` semantics.
- **Navigation intent / 导航定位**  
  Route to nav endpoints semantics: groups -> group by slug -> menu by path slug.
- **Taxonomy intent / 分类映射**  
  Route to taxonomy semantics: `GET /api/open/category-groups` and `GET /api/open/categories`.

If one prompt spans multiple intents, split into sub-tasks, execute per route, then merge.

## Quickstart / 快速使用

Typical tasks this skill should handle directly:

1. "Find top L2 chains related to AI"
2. "Compare 5 Twitter creators in one category"
3. "Evaluate educational YouTube channels"
4. "Where is X in web3base navigation?"
5. "Map category slug to readable labels"

## Operation Flow / 操作流程

### Step 1: Identify intent / 识别意图

- Chains -> ranking/filter/comparison tasks for chain records.
- Twitter -> creator quality, account filtering, KOL discovery.
- YouTube -> channel cadence, focus, comparison.
- Nav -> site location/path discovery.
- Taxonomy -> dimension/category translation and label mapping.

### Step 2: Collect parameters / 收集参数

- Must collect user-critical constraints first (query, locale, limits, category).
- If user gives vague goal, use safe defaults then continue:
  - `limit=20`
  - `locale=en`
  - taxonomy context inferred by route domain

### Step 3: Execute and synthesize / 执行与整合

- Execute the minimal set of routes needed for the asked outcome.
- Return user-oriented summaries, not raw endpoint dumps.
- Include route-level notes only when they affect interpretation.

### Step 4: Suggest next actions / 建议下一步

After each result, suggest 1-2 concrete follow-up actions (for example: narrow by category, compare with another domain, drill into nav path).

## Cross-workflow Patterns / 跨流程范式

### Pattern A: Discovery + Explain

1. Query a content list (chain/twitter/youtube).
2. Resolve category labels through taxonomy context.
3. Return shortlist with brief rationale and next filter option.

### Pattern B: Navigation + Content

1. Map user intent to nav group/menu path.
2. Pull related domain records (if requested).
3. Return "where to go" + "what to inspect" in one answer.

## Edge Cases / 边界场景

- Ambiguous cross-domain prompts with no ranking signal.
- Empty results under valid filters.
- Invalid `module/scope` in taxonomy mapping.
- Locale provided but localized labels unavailable.
- Conflicting constraints (for example, incompatible slug + domain).

## Output Rules / 输出规范

- Keep output compact and decision-ready.
- Always include route trace: `routes`.
- Prefer readable identifiers and names over raw ids only.
- Include uncertainty notes when assumptions are used.

Recommended output shape:

```json
{
  "intent": "normalized-goal",
  "routes": ["chain", "taxonomy"],
  "result": {},
  "notes": []
}
```

## Global Notes / 全局说明

- Do not fabricate data.
- Do not ignore user constraints silently.
- Prefer one precise follow-up question over broad clarification blocks.
- Keep responses concise unless user asks for full raw payload.

## Reference / 参考

For endpoint-level mapping details, see `references/open-api-alignment.md`.
