# Substack Scraper MCP Server

[![Smithery](https://smithery.ai/badge/mambabuilt/mcp-substack-scraper)](https://smithery.ai/servers/mambabuilt/mcp-substack-scraper) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-substack-scraper/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-substack-scraper) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-substack-scraper%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-substack-scraper&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-substack-scraper)](https://www.npmjs.com/package/@mambalabsdev/mcp-substack-scraper) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-substack-scraper)](https://www.npmjs.com/package/@mambalabsdev/mcp-substack-scraper) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-substack-scraper)](https://github.com/mambalabsdev/mcp-substack-scraper/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-substack-scraper)

An MCP server for the Mamba Labs Substack Scraper actor on Apify. Give it Substack publications or keywords. It returns publication profiles and posts as flat rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Your keys](#your-keys)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

- **Publications**: name, author, tagline, subscriber count as Substack shows it (`subscriber_count_text`, and `subscriber_count_min` as a number), subscriber tier text, and monthly, yearly, and founding plan prices.
- **Posts**: title, URL, date, `audience` (free, paid, or founding), likes, comments, restacks, word count, bylines, and tags. Optional post text with `include_body`.
- **Search**: keyword search across Substack posts, returned as posts or as the publications behind them.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-substack-scraper": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-substack-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `scrape_substack` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "How many subscribers does Lenny's Newsletter have, and what does a paid plan cost?"
- "Find Substack newsletters that write about cold email and list their subscriber counts."
- "What did notboring.co publish in the last 30 days, and which posts were paid only?"

## Tool

`scrape_substack`. One tool. Example arguments:

```json
{
  "publications": [
    "lenny.substack.com"
  ],
  "search_queries": [
    "cold email"
  ],
  "max_posts_per_publication": "5",
  "max_search_results": "5"
}
```

## Inputs

Every buyer facing input of the actor, unchanged. Omitted inputs take the actor's own defaults. Numbers and booleans are sent as strings.

| Input | Type | Description |
|---|---|---|
| `publications` | array | Substack publications, one per line, up to 100: a URL (https://www.lennysnewsletter.com), a domain (lenny.substack.com), a profile (substack.com/@lenny), or a bare Substack name (lenny). Each one returns a publication row and its newest posts |
| `search_queries` | array | Keywords to search across Substack posts, one per line, up to 20. Each query returns matching posts, or the publications behind them when Search output is publications |
| `search_output` | string | What a search query returns Default posts |
| `max_posts_per_publication` | string | Newest posts returned per publication, 0 to 1000. 0 returns only the publication row Default 20 |
| `max_search_results` | string | Rows returned per search query, 1 to 1000 Default 50 |
| `since_days` | string | Optional. Skip posts older than this many days. 0 or empty keeps every post up to the max Default 0 |
| `include_publication_row` | string | true to return one publication row per listed publication (name, author, subscribers, paid plan prices), billed as a publication row. false returns only post rows Default true |
| `include_body` | string | true to add the post text (up to 20,000 characters) to each post row of a listed publication. A paid post gives its free preview. Reads one more page per post; no extra charge Default false |
| `max_charge_usd` | string | Optional. The run stops before it would charge more than this |
| `proxy_mode` | string | How pages are fetched Default auto |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `post-row`, one Substack post returned as a row: from a listed publication or from a search | $0.00115 | $0.001093 | $0.001035 | $0.000977 |
| `publication-row`, one Substack publication profile returned as a row: name, author, subscriber count as shown, and paid plan prices | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `apify-actor-start`, per run start | $0.001 | $0.001 | $0.001 | $0.001 |

`no_results` and `error` rows are free, including the one row a refused input returns.

## Your keys

The actor needs no key. It reads Substack's public pages and public JSON routes and holds no key of any kind. Proxy traffic adds nothing to your price: Mamba Labs pays the platform costs of the run.

## Full actor documentation

[apify.com/mambalabs/substack-scraper](https://apify.com/mambalabs/substack-scraper)

## License

MIT. See [LICENSE](LICENSE).
