---
name: stripfeed
description: Fetch any URL as clean, token-efficient Markdown for AI agents using the StripFeed API. Use when fetching web pages, articles, documentation, or any URL to get clean Markdown output with token savings. Replaces raw web_fetch/curl for AI workflows. Supports single URL fetch, batch fetch (up to 10 URLs), CSS selectors, multiple output formats, and cache control.
---

# StripFeed

Convert any URL to clean Markdown using the StripFeed API (stripfeed.dev).
No LLM used internally — pure Mozilla Readability + Turndown. Deterministic, fast, cacheable.

## Setup

API key required. Set as env var or pass directly:

```bash
export STRIPFEED_API_KEY=sf_live_your_key
```

Get a key at: https://www.stripfeed.dev/dashboard/keys

## Fetch a URL

```bash
curl "https://www.stripfeed.dev/api/v1/fetch?url=ENCODED_URL" \
  -H "Authorization: Bearer $STRIPFEED_API_KEY"
```

**Key response headers:**
- `X-StripFeed-Tokens` — token count of clean output
- `X-StripFeed-Savings-Percent` — % tokens saved vs raw HTML
- `X-StripFeed-Cache` — HIT or MISS

## Parameters

| Param | Description |
|-------|-------------|
| `url` | Target URL (required, must URL-encode) |
| `format` | `markdown` (default), `json`, `text`, `html` |
| `selector` | CSS selector to extract specific elements |
| `model` | AI model for cost tracking (e.g. `claude-sonnet-4-6`) |
| `max_tokens` | Truncate output to N tokens (adds `X-StripFeed-Truncated` header) |
| `cache` | Set `false` to bypass cache |
| `ttl` | Cache TTL in seconds (default 3600, max 86400) |

## JSON response (format=json)

```bash
curl "https://www.stripfeed.dev/api/v1/fetch?url=ENCODED_URL&format=json&model=claude-sonnet-4-6" \
  -H "Authorization: Bearer $STRIPFEED_API_KEY"
```

Returns: `markdown`, `tokens`, `originalTokens`, `savingsPercent`, `cached`, `fetchMs`, `title`, `selector`, `truncated`

## Batch fetch (up to 10 URLs, Pro)

```bash
curl -X POST "https://www.stripfeed.dev/api/v1/batch" \
  -H "Authorization: Bearer $STRIPFEED_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"urls": ["https://a.com", {"url": "https://b.com", "selector": "article"}], "model": "claude-sonnet-4-6"}'
```

## Check usage

```bash
curl "https://www.stripfeed.dev/api/v1/usage" \
  -H "Authorization: Bearer $STRIPFEED_API_KEY"
```

Returns: `plan`, `usage`, `limit`, `remaining`, `resetsAt`

## TypeScript SDK

```typescript
import StripFeed from "stripfeed"; // npm install stripfeed
const sf = new StripFeed(process.env.STRIPFEED_API_KEY);
const result = await sf.fetch("https://example.com", { model: "claude-sonnet-4-6" });
console.log(result.markdown);
console.log(`Saved ${result.savingsPercent}%`);
```

## Python SDK

```python
from stripfeed import StripFeed  # pip install stripfeed
sf = StripFeed(os.environ["STRIPFEED_API_KEY"])
result = sf.fetch("https://example.com", model="claude-sonnet-4-6")
print(result["markdown"])
```

## MCP Server (Claude Code / Cursor)

```bash
claude mcp add stripfeed -- npx -y @stripfeed/mcp-server
export STRIPFEED_API_KEY=sf_live_your_key
```

Exposes tools: `fetch_url`, `batch_fetch`, `check_usage`

## Notes

- `format`, `selector`, `batch` require Pro plan
- `max_tokens` available on all plans
- Selector results cached per URL+selector combination
- Cloudflare Markdown-for-Agents passthrough: if site responds with `text/markdown`, returned directly
- Full API reference: https://www.stripfeed.dev/llms-full.txt
