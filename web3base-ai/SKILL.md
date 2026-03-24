---
name: web3base-ai
description: Unified Web3Base content skill for chains, Twitter, YouTube, navigation, and taxonomy. Use whenever the user asks to discover, compare, filter, rank, map, or explain Web3Base public data, especially when intent is mixed or the user does not know which endpoint to call.
when_to_use: Any broad or cross-domain content request about chains/twitter/youtube/nav/categories, or whenever one skill should handle end-to-end data lookup and synthesis.
version: 1.1.0
---

# web3base-ai

## Summary

English: One skill to query and synthesize Web3Base public content across chains, Twitter users, YouTube channels, navigation, and taxonomy.
中文：一个技能统一处理 Web3Base 公开内容查询与整合，覆盖链数据、Twitter 用户、YouTube 频道、站点导航与分类体系。

## API Connection / API 连接

All requests MUST be sent to the Web3Base public API:

- **Base URL**: `https://www.web3base.ai`
- **Protocol**: HTTPS only
- **Authentication**: Optional. Include `X-API-Key` header for higher rate limits.
- **Rate Limits**:
  - Without API key: 60 requests/min per IP
  - With `X-API-Key` header: 300 requests/min

**How to call**: Use HTTP GET requests via `curl`, `fetch`, or any HTTP client. Example:

```bash
# List chains
curl "https://www.web3base.ai/api/open/chains?locale=en&q=AI"

# List Twitter users filtered by tag
curl "https://www.web3base.ai/api/open/twitter/users?tag=defi&locale=en"

# List YouTube channels
curl "https://www.web3base.ai/api/open/youtube/channels?q=ethereum&locale=en"

# Get navigation groups
curl "https://www.web3base.ai/api/open/nav/groups?locale=en"

# Get categories for chains
curl "https://www.web3base.ai/api/open/category-groups?module=chain"

# Get global categories scoped to twitter
curl "https://www.web3base.ai/api/open/categories?scope=twitter&locale=en"
```

## Pre-flight Checks / 预检

Before routing any request, always do these checks:

1. Identify whether the user intent is single-domain or cross-domain.
2. Extract hard constraints first: `q` (query), `tag`, `limit`, `locale`, `module`, `scope`, `sort`, `sortDir`, explicit slugs/ids.
3. Validate constraints before call execution:
   - `module` for category groups must be one of `chain|twitter|youtube`.
   - `scope` for categories should be `chain|twitter|youtube|item`.
   - `sort` for chains must be one of `name|chainId|platforms|tvl`.
   - `sortDir` must be `asc` or `desc`.
4. If constraints are missing but required for precision, ask one focused follow-up question.
5. Never invent records when upstream data is missing.

## Skill Routing / 路由规则

Each intent maps to a specific endpoint on `https://www.web3base.ai`:

| Intent | Endpoint | Key Params |
|--------|----------|------------|
| Chains search/compare | `GET /api/open/chains` | `q`, `tag`, `sort`, `sortDir`, `locale` |
| Twitter user discovery | `GET /api/open/twitter/users` | `q`, `tag`, `locale` |
| YouTube channel search | `GET /api/open/youtube/channels` | `q`, `tag`, `locale` |
| Navigation groups | `GET /api/open/nav/groups` | `locale` |
| Navigation group detail | `GET /api/open/nav/groups/{slug}` | `locale` |
| Navigation menu | `GET /api/open/nav/menus/{pathSlug}` | `locale` |
| Category groups (filters) | `GET /api/open/category-groups` | `module` (required) |
| Global categories | `GET /api/open/categories` | `scope`, `locale`, `includeI18n` |

If one prompt spans multiple intents, split into sub-tasks, execute per route, then merge.

## Quickstart / 快速使用

Typical tasks this skill should handle directly:

1. "Find top L2 chains related to AI" -> `GET https://www.web3base.ai/api/open/chains?q=AI&sort=tvl&sortDir=desc`
2. "Compare 5 Twitter creators in DeFi" -> `GET https://www.web3base.ai/api/open/twitter/users?tag=defi`
3. "Evaluate educational YouTube channels" -> `GET https://www.web3base.ai/api/open/youtube/channels?q=education`
4. "Where is X in web3base navigation?" -> `GET https://www.web3base.ai/api/open/nav/groups?locale=en`
5. "Map category slug to readable labels" -> `GET https://www.web3base.ai/api/open/categories?scope=chain&includeI18n=1`

## Operation Flow / 操作流程

### Step 1: Identify intent / 识别意图

- Chains -> ranking/filter/comparison tasks for chain records.
- Twitter -> creator quality, account filtering, KOL discovery.
- YouTube -> channel cadence, focus, comparison.
- Nav -> site location/path discovery.
- Taxonomy -> dimension/category translation and label mapping.

### Step 2: Collect parameters / 收集参数

- Must collect user-critical constraints first (q, tag, locale, sort, category).
- If user gives vague goal, use safe defaults then continue:
  - `locale=en`
  - taxonomy context inferred by route domain
- Map user language to the correct parameter names:
  - "search for X" -> `q=X`
  - "filter by tag Y" -> `tag=Y`
  - "sort by TVL descending" -> `sort=tvl&sortDir=desc`

### Step 3: Execute API calls / 执行 API 调用

- Build the full URL: `https://www.web3base.ai` + endpoint path + query string.
- Execute via HTTP GET (use `curl`, `fetch`, or equivalent).
- Execute the minimal set of routes needed for the asked outcome.
- For cross-domain requests, execute calls in parallel when possible.

### Step 4: Synthesize results / 整合结果

- Return user-oriented summaries, not raw endpoint dumps.
- Include route-level notes only when they affect interpretation.
- After each result, suggest 1-2 concrete follow-up actions.

## Cross-workflow Patterns / 跨流程范式

### Pattern A: Discovery + Explain

1. Query a content list: `GET https://www.web3base.ai/api/open/chains?q=...`
2. Resolve category labels: `GET https://www.web3base.ai/api/open/categories?scope=chain&includeI18n=1`
3. Return shortlist with brief rationale and next filter option.

### Pattern B: Navigation + Content

1. Map user intent to nav group: `GET https://www.web3base.ai/api/open/nav/groups`
2. Drill into specific menu: `GET https://www.web3base.ai/api/open/nav/menus/{pathSlug}`
3. Pull related domain records if requested.
4. Return "where to go" + "what to inspect" in one answer.

### Pattern C: Filter Discovery

1. Get available filter dimensions: `GET https://www.web3base.ai/api/open/category-groups?module=chain`
2. Get category options: `GET https://www.web3base.ai/api/open/categories?scope=chain`
3. Apply discovered tags to content endpoint: `GET https://www.web3base.ai/api/open/chains?tag=...`

## Edge Cases / 边界场景

- Ambiguous cross-domain prompts with no ranking signal -> ask user to clarify domain.
- Empty results under valid filters -> inform user and suggest broadening criteria.
- Invalid `module/scope` in taxonomy mapping -> return validation error, suggest valid values.
- Locale provided but localized labels unavailable -> fall back to `en`.
- Conflicting constraints -> explain conflict and ask user to resolve.
- Rate limit (429) -> wait and retry, or inform user.

## Output Rules / 输出规范

- Keep output compact and decision-ready.
- Always include the full API URL that was called for transparency.
- Prefer readable identifiers and names over raw ids only.
- Include uncertainty notes when assumptions are used.

Recommended output shape:

```json
{
  "intent": "normalized-goal",
  "api_calls": ["https://www.web3base.ai/api/open/chains?q=AI"],
  "result": {},
  "notes": []
}
```

## Global Notes / 全局说明

- All API calls go to `https://www.web3base.ai` — no other base URL.
- Do not fabricate data. Only return what the API provides.
- Do not ignore user constraints silently.
- Prefer one precise follow-up question over broad clarification blocks.
- Keep responses concise unless user asks for full raw payload.

## Reference / 参考

For endpoint-level details (parameters, response schemas, error codes), see `references/open-api-alignment.md`.
