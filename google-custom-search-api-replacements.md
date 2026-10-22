# Google Custom Search JSON API replacements (it shuts down January 1, 2027)

> Maintained by the CrawlerData team; CrawlerData is on this list.

Google's Custom Search JSON API (`customsearch/v1`) is closed to new customers. Google's overview page says existing customers "have until January 1, 2027 to transition to an alternative solution". Until then they keep the old terms: 100 free queries a day, then $5 per 1,000, up to 10,000 a day. New Google Cloud projects get `403 PERMISSION_DENIED: This project does not have the access to Custom Search JSON API`.

The usual fallback is gone too: Microsoft retired the Bing Search APIs on August 11, 2025.

Prices checked on each provider's pricing page on 2026-10-22.

## First: what did you use CSE for?

| You used CSE to… | Look at |
|---|---|
| Search **your own site(s)** | Vertex AI Search (#1) |
| Get **Google web results** as JSON (rank checks, lookups, copyvio and plagiarism checks, research) | CrawlerData (#2), SerpApi (#4), Serper (#5) |
| Get **any good web results**, not necessarily Google's | Brave (#3), SearXNG (#6) |

## Comparison

| # | Replacement | $ per 1,000 at entry | $ per 1,000 at volume | Free tier | Keeps CSE's request/response shape | MCP server |
|---|---|---|---|---|---|---|
| — | *CSE today (existing customers)* | *$5.00* | *$5.00, max 10k/day* | *100/day* | — | — |
| 1 | Vertex AI Search | Not comparable (indexes your content) | — | See Google Cloud pricing | No | — |
| 2 | CrawlerData | $1.00 | $0.30 | 50 credits (work email) | Yes (`/customsearch/v1`) | Yes |
| 3 | Brave Search API | $5.00 | $5.00 | $5 credit/month | No | Yes |
| 4 | SerpApi | $25.00 | ~$1.96 | 250/month | No | Yes |
| 5 | Serper | $1.00 | $0.30 | 2,500 once | No | No official server |
| 6 | SearXNG (self-hosted) | Server cost | Server cost | Free, open source | No | Community servers |

## The replacements

### 1. Vertex AI Search

Google's own recommendation: its page calls Vertex AI Search "a favorable alternative for searching up to 50 domains".

**Key features:** you create a data store, Google indexes your content, and you query that index, with relevance tuning, summaries and structured data.

**Drawbacks:** a different product. It doesn't return "what Google shows for this query on the public web", so it only replaces CSE if your engine was limited to your own domains. For full-web search Google points to an interest form, with no public pricing. Setup is heavier than an API key and a `cx`.

### 2. CrawlerData

A Google SERP API with a CSE-compatible endpoint, so existing CSE code can switch by changing the base URL and key.

    GET https://api.crawlerdata.com/customsearch/v1?q=…&key=YOUR_CRAWLERDATA_KEY

It returns CSE's shape: `items[]` with `kind`, `title`, `htmlTitle`, `link`, `displayLink`, `snippet`, `htmlSnippet`, `formattedUrl`, `htmlFormattedUrl`. `num` (1–10), `start` (1–100) and `siteSearch` work, and errors come back in Google's format (for example 401 `keyInvalid`).

**Key features**
- Real Google results, from any of the supported countries or a specific city
- No daily cap like CSE's 10,000 (the rate limit is 100 requests a minute per key)
- The same key also gets the full Google Search response (AI Overview with sources, People Also Ask, ads, local pack), plus Images, News, Local, Autocomplete and Shopping
- Prepaid credits, 1 per request; failed requests aren't charged

**Drawbacks (read these before switching)**
- **Slower than CSE.** Results are fetched live and take seconds, sometimes much longer. Set a timeout of 60 seconds or more, and run searches in parallel if you make many.
- **Not full parity.** `cx`, `safe` and `fields` are accepted but ignored. There's no `pagemap` or `cacheId`. `htmlTitle` and `htmlSnippet` are plain text, without bold tags.
- It searches all of Google, not a Programmable Search Engine you configured. Use `siteSearch` or `site:` to narrow it.
- A newer, smaller provider. The free trial is 50 credits and needs a work email.

**Pricing:** $50 for 50k credits ($1.00 per 1,000) to $3,750 for 12.5M ($0.30 per 1,000), valid 6 months. [Field-by-field mapping from CSE](https://crawlerdata.com/compare/google-custom-search-api?utm_source=github&utm_campaign=listicle-cse-replacements)

### 3. Brave Search API

Brave runs its own web index: real web results over a stable, documented API.

**Key features:** `web.results[]` with `title`, `url`, `description`; $5 free credit a month; clear terms; official MCP server.

**Drawbacks:** not Google's results or ranking. The same $5 per 1,000 as CSE, at any volume. Needs a card. Different field names, so you map `url` to `link` and `description` to `snippet`.

### 4. SerpApi

**Key features:** full Google results pages as JSON, many other engines, mature docs, hosted MCP server.

**Drawbacks:** $25 per 1,000 on the entry plan, five times CSE's price. Monthly subscription. Different response format (`organic_results[]`), so you rewrite the parsing.

### 5. Serper

**Key features:** Google results at $1.00 down to $0.30 per 1,000, 2,500 free queries, simple `POST /search`.

**Drawbacks:** different request and response format from CSE (`organic[]`, an API-key header). No official MCP server. Credits expire after 6 months.

### 6. SearXNG (self-hosted)

An open-source metasearch engine. Run it with Docker, enable the `json` format in `settings.yml`, and query `/search?q=…&format=json`.

**Drawbacks:** it queries upstream engines (Google among them) from your server's IP, and they rate-limit or CAPTCHA you at any real volume. Fine for a personal tool, fragile as a product's backend. You run and maintain it.

## Migration checklist

1. `grep -r "customsearch/v1"` across your repos. Also check no-code tools (n8n, Make, Zapier) for "Google Custom Search" nodes.
2. Decide per use case: own-site search, Google web results, or any web results.
3. List the fields you read. Most code reads only `items[].link`, `title` and `snippet`.
4. Check your timeouts and concurrency: most replacements are slower than CSE was.
5. Run old and new side by side for a week on your real queries.
6. Remove the CSE key once traffic is at zero, and before January 1, 2027.

## FAQ

**Can I still get a Custom Search JSON API key?** Not for new projects: the API is closed to new customers. Existing projects keep working until January 1, 2027.

**Does Programmable Search Engine (the search box) go away too?** This page covers the JSON API. Check Google's Programmable Search Engine pages for the embedded search element.

**Is there a free replacement?** Brave ($5 credit a month) and SearXNG (self-hosted) are the closest to free for light use. SerpApi has 250 free searches a month.

---

Last checked 2026-10-22. See [how we write these](README.md#how-we-write-these).
