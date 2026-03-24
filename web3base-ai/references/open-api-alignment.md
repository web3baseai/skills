# Web3Base Open API Reference

## Base URL

```
https://www.web3base.ai
```

All endpoints below are relative to this base URL.

## Authentication

| Method | Rate Limit |
|--------|------------|
| No auth (public) | 60 req/min per IP |
| `X-API-Key` header | 300 req/min |

## Common Parameters

| Name | Type | Description |
|------|------|-------------|
| `locale` | string | Locale code, e.g. `en`, `zh-hans`. Used for i18n labels. |

## Common Error Responses

| Status | Description |
|--------|-------------|
| 400 | Invalid or missing required parameter |
| 404 | Resource not found |
| 429 | Rate limit exceeded |
| 500 | Server error |

---

## Endpoints

### 1. GET /api/open/chains

List blockchain chains with optional filtering and sorting.

**Full URL**: `https://www.web3base.ai/api/open/chains`

**Parameters**:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| locale | string | No | Locale code |
| q | string | No | Search query |
| tag | string | No | Comma-separated category tag slugs |
| sort | string | No | Sort field: `name`, `chainId`, `platforms`, `tvl` |
| sortDir | string | No | Sort direction: `asc`, `desc` |

**Response** (200):

```json
{
  "chains": [
    {
      "_id": "string",
      "slug": "string",
      "name": "string",
      "logo": "string (URL)",
      "chain_id": "number",
      "categorySlugs": ["string"],
      "llama_tvl": "number | null"
    }
  ]
}
```

**Example**:
```bash
curl "https://www.web3base.ai/api/open/chains?q=AI&sort=tvl&sortDir=desc&locale=en"
```

---

### 2. GET /api/open/twitter/users

List Twitter (X) users with optional filtering.

**Full URL**: `https://www.web3base.ai/api/open/twitter/users`

**Parameters**:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| locale | string | No | Locale code |
| q | string | No | Search query |
| tag | string | No | Comma-separated tag slugs |

**Response** (200):

```json
{
  "users": [
    {
      "handle": "string",
      "name": "string",
      "description": "string",
      "twitterCategory": ["string"],
      "categorySlugs": ["string"],
      "followers_count": "number"
    }
  ]
}
```

**Example**:
```bash
curl "https://www.web3base.ai/api/open/twitter/users?tag=defi&locale=en"
```

---

### 3. GET /api/open/youtube/channels

List YouTube channels with optional filtering.

**Full URL**: `https://www.web3base.ai/api/open/youtube/channels`

**Parameters**:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| locale | string | No | Locale code |
| q | string | No | Search query |
| tag | string | No | Comma-separated tag slugs |

**Response** (200):

```json
{
  "channels": [
    {
      "channelId": "string",
      "name": "string",
      "description": "string",
      "youtubeCategory": ["string"],
      "categorySlugs": ["string"]
    }
  ]
}
```

**Example**:
```bash
curl "https://www.web3base.ai/api/open/youtube/channels?q=ethereum&locale=en"
```

---

### 4. GET /api/open/nav/groups

List all navigation groups with their cards.

**Full URL**: `https://www.web3base.ai/api/open/nav/groups`

**Parameters**:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| locale | string | No | Locale code |

**Response** (200):

```json
{
  "groups": [
    {
      "key": "string",
      "slug": "string",
      "name": "string",
      "description": "string",
      "navs": []
    }
  ],
  "totalGroups": "number",
  "totalMenus": "number"
}
```

**Example**:
```bash
curl "https://www.web3base.ai/api/open/nav/groups?locale=en"
```

---

### 5. GET /api/open/nav/groups/{slug}

Retrieve a single navigation group by slug.

**Full URL**: `https://www.web3base.ai/api/open/nav/groups/{slug}`

**Path Parameters**:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| slug | string | Yes | Group slug identifier |

**Query Parameters**:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| locale | string | No | Locale code |

**Response** (200): Single `NavGroup` object.

**Example**:
```bash
curl "https://www.web3base.ai/api/open/nav/groups/web3-tools?locale=en"
```

---

### 6. GET /api/open/nav/menus/{pathSlug}

Retrieve complete navigation menu structure including groups, categories, and cards.

**Full URL**: `https://www.web3base.ai/api/open/nav/menus/{pathSlug}`

**Path Parameters**:

| Name | Type | Required | Example |
|------|------|----------|---------|
| pathSlug | string | Yes | `web3-core-tools` |

**Query Parameters**:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| locale | string | No | Locale code |

**Response** (200):

```json
{
  "menuSlug": "string",
  "title": "string",
  "description": "string",
  "ogImage": "string (URL)",
  "groups": [
    {
      "id": "string",
      "name": "string",
      "slug": "string",
      "items": []
    }
  ],
  "standalone": [
    {
      "id": "string",
      "name": "string",
      "slug": "string",
      "type": "category | link",
      "description": "string",
      "cards": [
        {
          "id": "string",
          "title": "string",
          "description": "string",
          "link": "string (URL)",
          "image": "string | null"
        }
      ]
    }
  ],
  "blogSlugs": ["string"]
}
```

**Example**:
```bash
curl "https://www.web3base.ai/api/open/nav/menus/web3-core-tools?locale=en"
```

---

### 7. GET /api/open/category-groups

List filter dimension groups per module. Used for building sidebar filters.

**Full URL**: `https://www.web3base.ai/api/open/category-groups`

**Parameters**:

| Name | Type | Required | Enum |
|------|------|----------|------|
| module | string | **Yes** | `chain`, `twitter`, `youtube` |

**Response** (200):

```json
{
  "module": "chain",
  "groups": [
    {
      "key": "string (e.g. chain_layer, ecosystem)",
      "i18n": { "en": "string", "zh-hans": "string" },
      "priority": "number"
    }
  ]
}
```

**Example**:
```bash
curl "https://www.web3base.ai/api/open/category-groups?module=chain"
```

---

### 8. GET /api/open/categories

List global categories with multilingual support.

**Full URL**: `https://www.web3base.ai/api/open/categories`

**Parameters**:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| locale | string | No | Used when `includeI18n` is absent |
| scope | string | No | Filter by scope: `chain`, `twitter`, `youtube`, `item` |
| includeI18n | string | No | Set to `1` or `true` for full multilingual map |

**Response** (200) — without `includeI18n`:

```json
{
  "categories": [
    {
      "key": "string",
      "name": "string (resolved for locale)",
      "parent": "string | null",
      "priority": "number",
      "scopes": ["chain", "twitter", "youtube", "item"]
    }
  ]
}
```

**Response** (200) — with `includeI18n=1`:

```json
{
  "categories": [
    {
      "key": "string",
      "name": "string",
      "parent": "string | null",
      "priority": "number",
      "scopes": ["string"],
      "i18n": { "en": "string", "zh-hans": "string" },
      "descriptionI18n": { "en": "string" },
      "groupKey": "string",
      "moduleGroupKeys": ["string"]
    }
  ]
}
```

**Example**:
```bash
curl "https://www.web3base.ai/api/open/categories?scope=chain&locale=en"
curl "https://www.web3base.ai/api/open/categories?scope=twitter&includeI18n=1"
```

---

### 9. GET /api/open/spec

Retrieve the OpenAPI specification document itself.

**Full URL**: `https://www.web3base.ai/api/open/spec`

**Response** (200): OpenAPI JSON document.

**Example**:
```bash
curl "https://www.web3base.ai/api/open/spec"
```
