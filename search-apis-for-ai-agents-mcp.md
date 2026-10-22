# Search APIs for AI agents and MCP in 2026: 7 options compared

> Maintained by the CrawlerData team; CrawlerData is on this list.

An LLM agent needs fresh facts, and a search tool is how it gets them. There are two kinds of search API to choose from:

- **Search-for-agents APIs** (Tavily, Exa): they return ranked passages or page content, ready to drop into a prompt.
- **SERP APIs** (CrawlerData, SerpApi, Bright Data): they return what Google shows (positions, links, snippets, AI Overview, People Also Ask) as structured JSON.

Many agents use both: a SERP API when the question is "what does Google rank or say", and a search-for-agents API when it's "find me the facts".

Prices checked on each provider's pricing page on 2026-10-22, except where a different date is shown.

## Comparison

| # | API | Returns | $ per 1,000 at entry | $ per 1,000 at volume | Free tier | Official MCP server |
|---|---|---|---|---|---|---|
| 1 | Tavily | Results + content summaries, optional answer | $8.00 (pay as you go, basic search) | $7.50 (Project plan) | 1,000 credits/month | Yes (hosted) |
| 2 | Exa | Results from Exa's own index, optional page content | $4.00 (`lite`/`instant`) | Enterprise: custom | $10 credit/month | Yes (hosted) |
| 3 | CrawlerData | Google's results as JSON, incl. AI Overview + sources | $1.00 | $0.30 | 50 credits (work email) | Yes (hosted, 8 tools) |
| 4 | Brave Search API | Brave's own index; Answers plan for grounded answers | $5.00 | $5.00 | $5 credit/month | Yes (open source) |
| 5 | SerpApi | Google + many engines as JSON | $25.00 | ~$1.96 | 250/month | Yes (hosted) |
| 6 | Bright Data SERP API | Google, Bing, Yandex results | $1.50 | ~$1.31 | 5,000/month | Yes (open source + hosted) |
| 7 | Serper | Google results as JSON | $1.00 | $0.30 | 2,500 once | No (community servers) |

Tavily's advanced search costs 2 credits (checked 2026-10-13). Exa's `auto`/`fast` search is $7 per 1,000 and deep search $12–$15.

## The APIs

### 1. Tavily

Built for agents, and in most LangChain and LlamaIndex tutorials.

**Key features:** results with content summaries and relevance scores; optional cleaned page content and an LLM-written answer; an extract endpoint; hosted MCP server at mcp.tavily.com.

**Drawbacks:** its docs don't say which search engine the results come from, so it's not for Google rank or AI Overview data. $8 per 1,000 pay as you go. Monthly plan credits reset on the 1st.

### 2. Exa

A search engine built for AI, with its own index and neural ("meaning-based") search.

**Key features:** search types from `instant` to `deep-reasoning`; page contents and summaries; good at "find pages like this"; hosted MCP server at mcp.exa.ai.

**Drawbacks:** the results aren't Google's. The price depends on the search type ($4 to $15 per 1,000), and results beyond 10 cost extra.

### 3. CrawlerData

A Google SERP API with a hosted MCP server, for agents that need to know what Google actually shows.

**Key features**
- Remote MCP at `https://api.crawlerdata.com/mcp` with 8 tools: Google Search, Images, News, Local, Autocomplete, Shopping search, product details and sellers
- Connect with an API key, or OAuth 2.1 sign-in in clients that support it (no key in your config)
- Google Search results include the AI Overview with its cited sources, so the agent can check where Google's summary came from
- Organic positions with real destination links, People Also Ask, local pack, top stories; city-level location
- 1 credit per request on every tool; failed requests aren't charged
- Also REST (`POST /v1/jobs`), SerpApi-compatible `/search.json`, and an OpenAPI spec

**Drawbacks**
- It returns Google's results page, not page content: to read a page, the agent needs a fetch or extract tool too
- Google only
- Results take seconds, sometimes longer, which adds latency to every agent step that searches
- Newer, smaller provider; the free trial is 50 credits and needs a work email

**Pricing:** $50 for 50k credits ($1.00 per 1,000) to $3,750 for 12.5M ($0.30 per 1,000), valid 6 months. [MCP setup](https://crawlerdata.com/integrations/mcp?utm_source=github&utm_campaign=listicle-ai-agents-mcp)

### 4. Brave Search API

**Key features:** an independent index; the Search plan for results and the Answers plan for grounded answers ($4 per 1,000 plus tokens); official MCP server.

**Drawbacks:** not Google's results. $5 per 1,000 at any volume. A card is required.

### 5. SerpApi

**Key features:** Google and dozens of other engines (Maps, Scholar, YouTube, Amazon…) as JSON; hosted MCP server at mcp.serpapi.com.

**Drawbacks:** $25 per 1,000 on the entry plan; monthly subscription. Large JSON responses use a lot of context window if you pass them to the model unfiltered.

### 6. Bright Data SERP API

**Key features:** several engines; official MCP server with search and page-scraping tools; 5,000 free requests a month.

**Drawbacks:** a large platform to set up. $1.50 per 1,000 pay as you go.

### 7. Serper

**Key features:** Google results at $1.00 to $0.30 per 1,000, built into LangChain and CrewAI, 2,500 free queries.

**Drawbacks:** no official MCP server, so you use a community one or write your own tool.

## How to choose for an agent

1. **Which question does the agent ask?** "What does Google rank or say about X?" (SEO, brand monitoring, AI Overview tracking) needs a SERP API. "What are the facts about X?" works with Tavily or Exa.
2. **Does it need page content?** SERP APIs return links and snippets. Pair one with a fetch tool, or use Tavily or Exa, which can return content.
3. **MCP or function calling?** With Claude Desktop, Claude Code, Cursor or n8n, a hosted MCP server saves you writing tool code.
4. **Cost per agent run:** agents often search 3–10 times per task, so multiply.
5. **Latency:** every search adds to the time the agent takes to answer. Test with your real prompts.

## FAQ

**What is an MCP search server?** A server that speaks the Model Context Protocol and exposes search as tools an MCP client (Claude, Cursor, n8n and others) can call, without you writing integration code.

**Can an agent read Google's AI Overview?** With a SERP API that returns it, yes. CrawlerData returns the AI Overview text and its cited sources when Google shows one. SerpApi also returns AI Overviews.

---

Last checked 2026-10-22. See [how we write these](README.md#how-we-write-these).
