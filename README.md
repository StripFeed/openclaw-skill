# StripFeed - OpenClaw Skill

OpenClaw skill for [StripFeed](https://www.stripfeed.dev), the API that converts any URL to clean, AI-ready Markdown.

## What it does

When installed, your OpenClaw agent can fetch and read any web page using the StripFeed API. Content is stripped of ads, navigation, scripts, and noise, returning clean Markdown with token counts.

## Install

```bash
clawhub install stripfeed
```

Or manually: copy the `stripfeed/` folder into your OpenClaw `skills/` directory.

## Setup

1. Get a free API key at [stripfeed.dev](https://www.stripfeed.dev) (200 requests/month, no credit card)
2. Add your key to OpenClaw config (`~/.openclaw/openclaw.json`):

```json
{
  "skills": {
    "entries": {
      "stripfeed": {
        "enabled": true,
        "env": {
          "STRIPFEED_API_KEY": "sf_live_your_key_here"
        }
      }
    }
  }
}
```

## Features

- **Single URL fetch** - `GET /api/v1/fetch?url=...` returns clean Markdown
- **Batch fetch** - `POST /api/v1/batch` processes up to 10 URLs in parallel (Pro)
- **Token counting** - know exactly how much context each page costs
- **Caching** - 1h default, configurable up to 24h
- **CSS selectors** - extract specific page sections (Pro)
- **Multiple formats** - Markdown, JSON, HTML, plain text (Pro)
- **Token truncation** - `max_tokens` parameter to fit your context budget

## Plans

| | Free | Pro ($19/mo) |
|---|---|---|
| Requests | 200/month | 100K/month |
| Formats | Markdown | All (JSON, HTML, text) |
| Selectors | - | CSS selectors |
| Batch | - | Up to 10 URLs |
| Cache TTL | 1h max | 24h max |

## Links

- [StripFeed](https://www.stripfeed.dev)
- [API Documentation](https://www.stripfeed.dev/docs)
- [ClawHub listing](https://clawhub.ai)
- [OpenClaw](https://openclaw.ai)

## License

MIT
