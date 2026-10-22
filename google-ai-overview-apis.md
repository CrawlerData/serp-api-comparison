# Google AI Overview APIs: which SERP APIs return the AI Overview and its sources (2026)

> Maintained by the CrawlerData team; CrawlerData is on this list.

Google's AI Overview sits above the organic results for many queries and cites a handful of pages. For SEO and brand teams the questions are: **does Google show an AI Overview for my keywords, what does it say, and is my site one of the cited sources?** Answering that at scale needs an API that returns the overview **with its source links**, not only the text.

Two details make providers differ more than you'd expect:

1. **Lazy loading.** Google often loads the AI Overview after the page. Some APIs need a second request, or a paid option, to get it.
2. **Sources.** Some return only the overview text. For citation tracking you need each cited page's title and link.

Prices and behavior checked on each provider's pricing pages and docs on 2026-10-22.

## Comparison

| # | API | AI Overview text | Cited sources | Lazy-loaded overviews | Cost per search incl. AI Overview | Free tier | MCP server |
|---|---|---|---|---|---|---|---|
| 1 | SerpApi | Yes | Yes (references) | Second call: `engine=google_ai_overview` + `page_token` (expires within ~1 min) | 1 search, or 2 when the second call is needed ($25.00 per 1,000 entry) | 250/month | Yes |
| 2 | CrawlerData | Yes | Yes (`sources[]` with title + link) | In the same request | 1 credit ($1.00 → $0.30 per 1,000) | 50 credits (work email) | Yes |
| 3 | DataForSEO | Yes | Yes | `load_async_ai_overview: true`, +1× base price (refunded if none) | $1.20 Standard / $4.00 Live per 1,000 | $1 trial credit | Yes |
| 4 | Bright Data SERP API | Yes (`aio_text`) | Per its product page | `brd_ai_overview` option; can add several seconds | $1.50 per 1,000 PAYG | 5,000/month | Yes |
| 5 | SearchApi | Yes | Check its docs | Separate `google_ai_overview` engine with a `page_token` | From $4.00 per 1,000 (monthly plans) | 100 requests | No official server |
| 6 | Serper | Not documented on serper.dev | Not documented | — | $1.00 → $0.30 per 1,000 | 2,500 once | No official server |

## The APIs

### 1. SerpApi

**Key features:** an `ai_overview` block in Google Search results with text blocks and references; a dedicated Google AI Overview API for lazy-loaded overviews; AI Mode and other AI engines; hosted MCP server.

**Drawbacks:** when Google lazy-loads the overview, the Search response returns a `page_token` that expires within about a minute. You make a second call, billed as a second search, and SerpApi's own guidance is to run those calls one after another. $25 per 1,000 on the entry plan.

### 2. CrawlerData

**Key features**
- `ai_overview` is part of every Google Search response: `null` when Google shows none, otherwise `text` and `sources[]` with each cited page's `title` and `link`
- Lazy-loaded overviews come back in the same request, at the same 1 credit, with no option to set and no second call
- Country, city-level (12,239 cities) and desktop/mobile targeting, since overviews differ by location and device
- Organic results, People Also Ask and ads in the same response, so you can compare "cited in the overview" with "ranks organically"
- Also via SerpApi-compatible `/search.json` and a hosted MCP server, so an agent can check AI Overview citations directly
- A free [AI Overview checker](https://crawlerdata.com/tools/ai-overview-checker?utm_source=github&utm_campaign=listicle-ai-overview-apis) to try a query without a key

**Drawbacks**
- Google AI Overview only: no AI Mode, Gemini, ChatGPT or Perplexity answers
- Results are fetched live and take seconds, sometimes longer, so plan batches with webhooks or concurrency
- Newer, smaller provider; 50 free credits with a work email

**Pricing:** $50 for 50k credits ($1.00 per 1,000) to $3,750 for 12.5M ($0.30 per 1,000), valid 6 months.

### 3. DataForSEO

**Key features:** an `ai_overview` element in Google Organic SERP results, with references; `load_async_ai_overview` to fetch lazy-loaded overviews; the charge is refunded when there's no async overview; a separate AI Mode SERP API; official MCP server.

**Drawbacks:** loading async overviews doubles the SERP price ($0.60 becomes $1.20 per 1,000 on Standard; $2.00 becomes $4.00 on Live). Task-based API. $50 minimum payment.

### 4. Bright Data SERP API

**Key features:** AI Overview captured when Google shows one (`aio_text` in parsed JSON); a `brd_ai_overview` option for overviews that load late; `collapse_aio` to control layout for pixel-position tracking; Google, Bing and Yandex; 5,000 free requests a month.

**Drawbacks:** the AI Overview option can add several seconds per request. The "light" JSON mode doesn't include it. Its docs don't show the full sources schema in one place, so test your queries.

### 5. SearchApi

**Key features:** a dedicated Google AI Overview engine plus AI Mode and Gemini engines; many other engines on one key.

**Drawbacks:** a two-step flow for overviews (token from a search, then the AI Overview engine), which costs more than one request. Monthly plans from $40. Check how it returns sources in its docs.

### 6. Serper

**Key features:** low-cost live Google results ($1.00 to $0.30 per 1,000), simple API, 2,500 free queries.

**Drawbacks:** serper.dev doesn't document an AI Overview field or its sources. Third-party tests disagree on whether one is returned, so run your own queries before relying on it for AI Overview tracking.

## How to choose

1. **Do you need sources, or only "is there an overview"?** For citation tracking, you need sources with links.
2. **Count the real cost per keyword.** Include second calls or AI Overview surcharges: one tracked keyword can cost 1× or 2× the listed price.
3. **Location and device matter.** AI Overviews differ by country, city and mobile vs desktop. Check that the API can target the ones you report on.
4. **Store the history.** AI Overviews change often. Store the text hash and sources per check to see changes over time.

## FAQ

**Is there an official Google AI Overview API?** No. Google doesn't offer an API for AI Overviews. SERP APIs read them from public results pages.

**Why does my API return no AI Overview when I see one in the browser?** Overviews depend on location, device, language and sign-in state, and many load late. Match the location and device, and use an API that handles lazy-loaded overviews.

**Can I track whether my site is cited?** Yes: request your keywords, read the source links in the AI Overview, and match their domain against yours.

---

Last checked 2026-10-22. See [how we write these](README.md#how-we-write-these).
