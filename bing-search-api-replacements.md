# Bing Search API replacements: 7 options after the August 2025 retirement

> Maintained by the CrawlerData team; CrawlerData is on this list.

Microsoft retired the Bing Search APIs on August 11, 2025. In Microsoft's words: "Any existing instances of Bing Search APIs will be decommissioned completely, and the product will no longer be available to be used or new customer signup." Calls to `api.bing.microsoft.com/v7.0/search` no longer work.

What you replace it with depends on what you need: **Bing's results specifically**, **any web results** as a list your code can process, or **grounding for an LLM**.

Prices checked on each provider's pricing page on 2026-10-22, except where a different date is shown.

## Comparison

| # | Replacement | Results from | Raw results list in your code? | $ per 1,000 at entry | $ per 1,000 at volume | Free tier | MCP server |
|---|---|---|---|---|---|---|---|
| 1 | Grounding with Bing Search (Azure) | Bing | No: model answers with citations | See Azure pricing | See Azure pricing | — | — |
| 2 | SerpApi (Bing engine) | Bing (and Google, others) | Yes | $25.00 | ~$1.96 | 250/month | Yes |
| 3 | Brave Search API | Brave's own index | Yes | $5.00 | $5.00 | $5 credit/month | Yes |
| 4 | CrawlerData | Google | Yes | $1.00 | $0.30 | 50 credits (work email) | Yes |
| 5 | DataForSEO (Bing SERP) | Bing (and Google, others) | Yes | from $0.60 (queued)¹ | Same | $1 credit¹ | Yes |
| 6 | Bright Data SERP API | Bing, Google, Yandex | Yes | $1.50 | ~$1.31 | 5,000/month | Yes |
| 7 | Tavily | Not stated | Results + summaries | $8.00 | $7.50 | 1,000 credits/month | Yes |

¹ DataForSEO's Google Organic rates (checked 2026-10-22). Check its Bing SERP pricing page for the Bing rate.

## The replacements

### 1. Grounding with Bing Search (Microsoft's recommendation)

Microsoft points Bing Search API customers to Grounding with Bing Search, a tool in Azure AI Foundry Agent Service.

**Key features:** real-time Bing data for an Azure AI agent; the model's answer includes citations with links; it stays in the Microsoft stack.

**Drawbacks:** it isn't a search endpoint. Microsoft's docs say developers and end users don't get the raw content it returns, so you can't get a list of titles, links and snippets to process yourself. It needs a Grounding with Bing Search resource connected to an Azure AI project, on a paid or pay-as-you-go Azure subscription.

### 2. SerpApi (Bing engine)

**Key features:** `engine=bing` returns Bing's results page as JSON, which is the closest thing to "Bing results as a list" today. The same key covers Google and many other engines. Hosted MCP server.

**Drawbacks:** $25 per 1,000 on the entry plan, as a monthly subscription. It's a different response format from Bing's `webPages.value[]`, so you rewrite the parsing.

### 3. Brave Search API

**Key features:** an independent index, like Bing's was, with a documented, stable API and clear terms. `web.results[]` with `title`, `url`, `description`. $5 free credit a month. Official MCP server.

**Drawbacks:** a different index, so results and ordering differ from Bing's. $5 per 1,000 at any volume. A card is required.

### 4. CrawlerData

A Google SERP API. If you used Bing only because it was "the search API you could buy", Google results are the usual next step.

**Migration in one line:** `webPages.value[]` becomes `organic_results[]`, `name` becomes `title`, `url` becomes `link`, and `snippet` keeps its name. The `Ocp-Apim-Subscription-Key` header becomes a Bearer token, and `mkt=en-US` becomes `country: "us"` and `language: "en"`.

**Key features**
- Google's results as JSON: organic results with real destination links, People Also Ask, ads, local pack, top stories, and the AI Overview with its cited sources
- Google News, Images, Local and Autocomplete from the same key (Bing's `responseFilter` becomes the endpoint you call)
- Live or webhook delivery at the same price; failed requests aren't charged
- Hosted MCP server, 8 tools

**Drawbacks**
- Not Bing: positions and snippets differ, so re-baseline stored rankings or evaluation sets
- One Google results page (up to 10 results) per request: Bing's `count=50` becomes 5 requests
- No `freshness` or `safeSearch` parameters
- Newer, smaller provider; 50 free credits with a work email
- Live fetching takes seconds, so it's slower than Bing's API was

**Pricing:** $50 for 50k credits ($1.00 per 1,000) to $3,750 for 12.5M ($0.30 per 1,000), valid 6 months. [Bing → CrawlerData field mapping](https://crawlerdata.com/compare/bing-search-api?utm_source=github&utm_campaign=listicle-bing-replacements)

### 5. DataForSEO (Bing SERP API)

**Key features:** Bing organic results as structured data, plus Google and other engines; queued tasks with webhooks, or a Live mode; official MCP server.

**Drawbacks:** task-based API with Basic auth and a learning curve. Live mode costs more than three times the queued rate. $50 minimum payment.

### 6. Bright Data SERP API

**Key features:** Bing, Google and Yandex; you pay only for successful requests; unlimited concurrency; 5,000 free requests a month; official MCP server.

**Drawbacks:** a large proxy platform to set up. The best rate needs the $499/month Scale plan.

### 7. Tavily

**Key features:** if you used Bing to ground an LLM, Tavily is built for that: results with content summaries, optional answers, a hosted MCP server.

**Drawbacks:** its docs don't say which engine the results come from. $8 per 1,000 pay as you go. Not a SERP, so it has no positions to track.

## Migration checklist

1. Find the calls: `grep -r "api.bing.microsoft.com"` and `Ocp-Apim-Subscription-Key`.
2. Decide what you need: Bing's results (SerpApi, DataForSEO, Bright Data), any good web index (Brave), Google's results (CrawlerData and other Google SERP APIs), or LLM grounding (Grounding with Bing, Tavily).
3. Map the fields you read: `webPages.value[].name / url / snippet`, `relatedSearches`, `news`, `images`.
4. Plan for paging: Bing allowed `count` up to 50. Most SERP APIs return 10 per request.
5. Re-baseline anything that stored positions or compared results over time.

## FAQ

**Is the Bing Search API completely gone?** Yes. Microsoft's lifecycle page says it was retired on August 11, 2025, existing instances were decommissioned, and sign-up ended.

**Can I get raw results from Grounding with Bing Search?** No. Microsoft's docs say developers and end users don't have access to the raw content it returns. The model's response includes citations with links.

**Which replacement returns Bing's own results?** SerpApi (`engine=bing`), DataForSEO's Bing SERP API and Bright Data's SERP API all return Bing results pages as data.

---

Last checked 2026-10-22. See [how we write these](README.md#how-we-write-these).
