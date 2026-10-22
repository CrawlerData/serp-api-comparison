# Top 7 Google SERP APIs for web scraping in 2026 (free and paid)

> Maintained by the CrawlerData team; CrawlerData is on this list.

A SERP API runs a query on Google and gives you the results page as structured JSON: organic results, ads, People Also Ask, the local pack, news and, more and more often, the AI Overview. You get the data without running browsers, proxies or parsers yourself.

Prices checked on each provider's pricing page on 2026-10-22, except where a different date is shown.

## Why get Google results through an API

- **Rank tracking and SEO tools:** positions for your keywords by country, city and device.
- **AI Overview monitoring:** whether Google's AI Overview appears and which sites it cites.
- **Market and competitor research:** who ranks, who advertises, what People Also Ask shows.
- **AI agents and RAG:** fresh web results as a tool an LLM can call.
- **Local data:** businesses, ratings and phone numbers from the local pack.

## TL;DR

| # | API | $ per 1,000 at entry | $ per 1,000 at volume | Free tier | Results from | MCP server |
|---|---|---|---|---|---|---|
| 1 | SerpApi | $25.00 | ~$1.96 (Cloud 54M) | 250 searches/month | Google + many other engines | Yes (hosted) |
| 2 | CrawlerData | $1.00 | $0.30 | 50 credits (work email) | Google (8 endpoints) | Yes (hosted) |
| 3 | Serper | $1.00 | $0.30 | 2,500 queries once | Google | No official server |
| 4 | DataForSEO | $0.60 (Standard queue) to $2.00 (Live) | Same rates | $1 trial credit¹ | Google + other engines | Yes (open source) |
| 5 | SearchApi | $4.00 | $1.00 (Octo 5M) | 100 requests | Google + other engines | No official server |
| 6 | Bright Data SERP API | $1.50 (pay as you go) | ~$1.31 (Scale) | 5,000 requests/month | Google + other engines | Yes (open source + hosted) |
| 7 | Brave Search API | $5.00 | $5.00 | $5 credit/month | Brave's own index | Yes (open source) |

¹ DataForSEO figures checked 2026-10-13.

## The APIs

### 1. SerpApi

The best-known SERP API. Its `GET /search.json?engine=google&q=…` request format is so common that other providers copy it.

**Key features**
- Dozens of engines beyond Google Search: Google Maps, Shopping, Scholar, Bing, Baidu, YouTube, Amazon and more
- Mature docs, client libraries in most languages, a searches archive
- Hosted MCP server at mcp.serpapi.com
- Failed and cached searches are not charged

**Drawbacks**
- The most expensive at low volume: $25 for 1,000 searches a month on Starter
- Monthly subscription. You pay the plan whether or not you use it
- Faster "Ludicrous Speed" tiers cost 2× and 4× the base price

**Pricing:** Free (250/month), then $25/month for 1,000 searches up to $106,050/month for 54M (about $1.96 per 1,000).

### 2. CrawlerData

A Google-only SERP API with 8 endpoints, prepaid credits and two drop-in modes: SerpApi's `/search.json` format and Google Custom Search's `customsearch/v1` format.

**Key features**
- Google Search with organic results, ads, People Also Ask, local pack, top stories and the AI Overview with its cited sources
- City-level targeting (12,239 cities) as well as country
- Also Google Images, News, Local, Autocomplete, Shopping search, Shopping product details, and every seller for a product in one request
- 1 credit per request on every endpoint; failed requests aren't charged
- Live (synchronous) or webhook delivery at the same price
- Hosted MCP server with 8 tools, using an API key or OAuth sign-in

**Drawbacks**
- Newer and smaller than most providers here, so it has a shorter track record
- Google only: no Bing, YouTube, Amazon or other engines
- Small free trial: 50 credits, and it needs a work email (Gmail and other webmail addresses get none)
- Results are fetched live and take seconds, sometimes longer, so it's not built for search-as-you-type

**Pricing:** prepaid packs, $50 for 50k credits ($1.00 per 1,000) to $3,750 for 12.5M ($0.30 per 1,000). Credits are valid 6 months; no subscription. [crawlerdata.com](https://crawlerdata.com/?utm_source=github&utm_campaign=listicle-top-serp-apis)

### 3. Serper

A simple Google SERP API (its site quotes results in 1–2 seconds), popular in LangChain and agent stacks.

**Key features**
- `POST /search` with an `X-API-KEY` header: easy to integrate
- Google Search, Images, News, Places, Shopping and more
- Built into many LLM frameworks
- 2,500 free queries, no credit card

**Drawbacks**
- Google only
- No official MCP server (community-built servers exist)
- Credits expire after 6 months

**Pricing:** prepaid packs, $50 for 50k credits ($1.00 per 1,000) to $3,750 for 12.5M ($0.30 per 1,000), valid 6 months. That's the same pack structure as CrawlerData's.

### 4. DataForSEO

A large data platform for SEO tools: SERP, keywords, backlinks, on-page and more.

**Key features**
- Very low price on the queued Standard mode
- Many engines and SEO datasets under one account
- Task-based delivery with postback and pingback webhooks, or a Live mode
- Official open-source MCP server

**Drawbacks**
- Live results cost more than three times the Standard price ($2.00 vs $0.60 per 1,000)
- Pricing is per SERP of 10 results, and deeper results cost more
- More complex API (tasks, queues), with a learning curve
- $50 minimum payment

**Pricing:** pay as you go. Google Organic: $0.60 per 1,000 SERPs on Standard, $1.20 on Priority, $2.00 on Live (checked 2026-10-13).

### 5. SearchApi

A SerpApi-style API with a wide engine list and lower prices than SerpApi.

**Key features**
- Google Search and other Google products, plus engines such as Bing and YouTube
- Familiar request format
- Monthly plans that get down to $1.00 per 1,000

**Drawbacks**
- Monthly subscription, starting at $40 for 10,000 searches
- No official MCP server found (third-party ones exist)
- Small free allowance (100 requests)

**Pricing:** $40/month for 10,000 ($4.00 per 1,000) to $5,000/month for 5M ($1.00 per 1,000).

### 6. Bright Data SERP API

The SERP product of a large proxy and web-data company.

**Key features**
- Google, Bing, Yandex and other engines
- Unlimited concurrency, and you pay only for successful requests
- 5,000 free requests a month
- Official MCP server (open source, also hosted)

**Drawbacks**
- Account setup and dashboard are built around Bright Data's wider proxy platform, which is more to learn
- Pay-as-you-go price is above the prepaid-pack providers
- The best price needs a $499/month plan

**Pricing:** $1.50 per 1,000 pay as you go; Scale is $499/month for 380k requests (about $1.31 per 1,000).

### 7. Brave Search API

Not a Google SERP API: Brave runs its own web index. It's on the list because it's the main independent alternative.

**Key features**
- Brave's own index, so results don't depend on scraping anyone
- Simple REST API, clear terms
- $5 of free credit every month
- Official MCP server

**Drawbacks**
- The results are not Google's, so it's no good for Google rank tracking or AI Overview data
- $5 per 1,000 at any volume
- A card is required

**Pricing:** Search plan, $5 per 1,000 requests, with $5 free credit a month.

## How to choose

1. **Do you need Google's results specifically?** For rank tracking, SEO or AI Overviews, yes: use a SERP API (1–6). If any good web results will do, Brave is an option.
2. **Monthly subscription or prepaid?** At low or bursty volume, prepaid credits (CrawlerData, Serper) or pay as you go (DataForSEO, Bright Data) usually cost less than a plan you don't use up.
3. **Which data do you need?** Check for AI Overview with citations, city-level location, People Also Ask, local pack, Shopping.
4. **Other engines?** If you need Bing, YouTube or Amazon too, SerpApi, SearchApi, DataForSEO and Bright Data cover more.
5. **Agents?** If an LLM will call it, check for an MCP server (see the [AI agents page](search-apis-for-ai-agents-mcp.md)).
6. **Try two.** Run your real queries through two free tiers and compare the JSON before you buy.

## FAQ

**Is scraping Google legal?** SERP APIs collect public results pages. The rules depend on your country and use case. Read the provider's terms and get legal advice if you're unsure.

**Which one costs least per request?** It depends on the volume and the mode. DataForSEO's queued Standard mode has the lowest listed per-SERP price here. For live results at volume, Serper and CrawlerData list $0.30 per 1,000.

**Can I switch providers without rewriting code?** Often yes. Several providers accept SerpApi's `/search.json` request format, so switching is a base URL and key change. Check which fields your code reads.

---

Last checked 2026-10-22. See [how we write these](README.md#how-we-write-these).
