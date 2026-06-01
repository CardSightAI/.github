# CardSight AI — Trading Card Identification API, Pricing & Catalog Data
 
<div align="center">
  
[![Node.js SDK](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://github.com/cardsightai/cardsightai-sdk-node)
[![Python SDK](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://github.com/cardsightai/cardsightai-sdk-python)
[![Swift SDK](https://img.shields.io/badge/Swift-FA7343?style=for-the-badge&logo=swift&logoColor=white)](https://github.com/cardsightai/cardsightai-sdk-swift)
[![Java SDK](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)](https://github.com/cardsightai/cardsightai-sdk-java)
[![.NET SDK](https://img.shields.io/badge/.NET-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)](https://github.com/cardsightai/cardsightai-sdk-dotnet)
 
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![API Status](https://img.shields.io/badge/API-Online-success)](https://status.cardsight.ai/)
[![Documentation](https://img.shields.io/badge/docs-latest-blue)](https://cardsight.ai/documentation)
 
**AI-powered trading card identification, pricing, and catalog infrastructure — so you can bring your ideas to life.**
 
[Website](https://cardsight.ai) • [Documentation](https://cardsight.ai/documentation) • [API Reference](https://api.cardsight.ai/documentation) • [Get a Free API Key](https://app.cardsight.ai/)
 
</div>

---
 
## What is CardSight AI?
 
CardSight AI is an **AI-native trading card infrastructure platform** built around one core capability that makes every other card experience possible: **knowing what card you're looking at.**
 
We exist to help developers, marketplaces, and collectibles businesses **bring their ideas to life** without spending two years and a million dollars building the visual identification, catalog, and pricing layers from scratch. One REST API, typed SDKs in five languages, and native MCP endpoints for Claude and ChatGPT — and you're shipping.
 
**Three pillars:**
 
1. **🔍 Visual Identification** — our namesake. Point a camera at a card, know exactly what it is.
2. **📊 9M+ Trading Card Catalog** — the structured knowledge layer behind identification, spanning baseball, basketball, football, hockey, and Pokémon.
3. **💰 Pricing & Marketplace Data** — daily-updated completed sales and active listings, so "what is it?" immediately becomes "what's it worth?"
## 🔍 Visual Card Identification — The Platform's Core
 
Identification is the hardest problem in trading cards, and it's the one CardSight AI was built to solve. Every other feature — pricing lookups, collection tracking, marketplace integrations, AI agents that answer card questions — starts with a computer vision model knowing exactly what card is in the frame. Get that wrong and nothing downstream works.
 
Our identification engine is the reason the company is called CardSight.
 
**What it does:**
 
- **Industry-leading accuracy across 3,000+ sets** spanning baseball, basketball, football, hockey, and Pokémon
- **Sub-second response times** — built for real-time camera flows, not batch jobs
- **Multi-card detection** — identify multiple trading cards in a single image, including binder pages and showcase shots
- **Real-time video streaming identification** — continuous identification from live video streams over SRT, RTMP, RTSP, and WebSocket (enterprise tier)
- **Slab / grading awareness** — detects graded trading cards and reads the slab (PSA, Beckett, SGC, CGC, TAG) including grade value, condition, qualifiers, and autograph grades
- **Parallel variant recognition** — distinguishes Refractors, Prizms, numbered parallels, and inserts from base cards
- **Confidence tiers** — every detection returns `High`, `Medium`, or `Low` confidence plus match level (exact card, set-level, or unidentified) so your UI can respond intelligently
- **Works with messy reality** — poor lighting, angles, reflections, phone cameras, multiple trading cards in frame
**What you can build with it:**
 
- **Scan-to-list flows** for marketplaces that eliminate manual data entry
- **Collection apps** where users snap a photo and the trading card is fully catalogued
- **Grading and pre-screening tools** that recognize slabs and grade values automatically
- **AI assistants** that can "see" a trading card the user holds up and answer questions about it
- **Point-of-sale integration** for card shops that want instant identification at the counter
- **Live commerce and breaking platforms** with real-time identification of cards on stream
```typescript
import { CardSightAI } from 'cardsightai';
 
const client = new CardSightAI({ apiKey: 'your_api_key' });
 
// Identify a card from any image source — File, Blob, Buffer, or ArrayBuffer
const result = await client.identify.card(imageFile);
 
if (result.data?.success) {
  for (const detection of result.data.detections) {
    console.log(`${detection.card.name} (${detection.confidence} confidence)`);
    console.log(`  ${detection.card.year} ${detection.card.manufacturer} ${detection.card.releaseName}`);
 
    // Parallel variant?
    if (detection.card.parallel) {
      console.log(`  Parallel: ${detection.card.parallel.name}`);
    }
 
    // Graded slab?
    if (detection.grading) {
      console.log(`  Graded: ${detection.grading.company.name} ${detection.grading.grade?.value}`);
    }
  }
}
 
// Target a specific segment for faster, more accurate results
const pokemonResult = await client.identify.cardBySegment('pokemon', imageFile);
 
// Or just check if a card is present (lightweight, cheaper)
const presence = await client.detect.card(imageFile);
```
 
## 💰 Pricing & Marketplace — Data, Not Opinions
 
Identification tells you what the card is. Pricing tells you what the market thinks it's worth — and on that second question, we take a strong position: **data, not opinions.**
 
Most pricing APIs hand you a single number and ask you to trust it. We don't. CardSight AI gives you **the underlying market data itself** — every completed sale and every active listing — so you can build your own comps, charts, and valuations on a foundation you can audit. One aggregate number hides everything that matters; we surface all of it.
 
**Built for production:**
 
- **Millisecond-level response times** — pricing queries are blazing fast, built for real-time UIs and listing flows
- **Daily-refreshed data** — completed sales and active listings updated every day
- **Parallel-aware and grade-aware** — a 2023 Topps Chrome Refractor PSA 10 is priced separately from its raw base card
- **Filterable time periods** — `7d`, `2w`, `3m`, `1y`, `all`, or any combination
- **Bulk pricing** — pull data for up to 100 trading cards in a single call
- **Linked directly to the catalog** — no fuzzy joining between pricing feeds and card records
**Two complementary endpoints:**
 
- **Pricing** (`pricing.get`, `pricing.bulk`) — completed sales with price, date, source, and listing type
- **Marketplace** (`marketplace.get`) — currently-active listings with URLs, bid counts, and buy-it-now vs. auction distinction
```typescript
// Completed sales — the raw data, not a derived "price"
const pricing = await client.pricing.get('card_uuid', {
  parallel_id: 'parallel_uuid',   // optional
  grade_id: 'grade_uuid',         // optional — omit for all grades
  period: '90d',                  // freeform: "7d", "2w", "90d", "1y", "all"
  listing_type: 'both'            // "auction" | "fixed" | "both"
});
 
// Bulk pricing for up to 100 cards in one call
const bulk = await client.pricing.bulk({
  card_ids: ['card_uuid_1', 'card_uuid_2', 'card_uuid_3'],
  period: '90d'
});
 
// Active marketplace listings
const listings = await client.marketplace.get('card_uuid', {
  listing_type: 'fixed'
});
```
 
## 📊 The Catalog Behind Everything
 
Identification and pricing only work because of the catalog underneath. Over 9 million trading cards, fully structured, with parallels, variations, attributes, grading references, and card images.
 
- **9M+ trading cards** from 1934 through present
- **Baseball, basketball, football, hockey, and Pokémon** — full identification and catalog support across all five
- **Parallels and variations as first-class entities** — not an afterthought
- **Segments, manufacturers, releases, sets, parallels** — the full hierarchy developers actually need
- **Fuzzy search, autocomplete, and natural-language AI search** — pick whichever fits your UX
```typescript
// Structured search
const cards = await client.catalog.cards.list({
  year: '2023',
  manufacturer: 'Topps',
  name: 'Aaron Judge',
  take: 10
});
 
// Natural language search
const answer = await client.ai.query({
  query: 'Show me Mike Trout rookies that sold above $100 this week'
});
```
 
## Core Capabilities at a Glance
 
| Capability | What it does |
|---|---|
| **🔍 Visual Card Identification** | Industry-leading accuracy across 3,000+ sets, sub-second response, handles raw cards and graded slabs |
| **🎬 Real-Time Video Streaming Identification** | Continuous identification from live video streams over SRT, RTMP, RTSP, and WebSocket (enterprise tier) |
| **🏷️ Slab & Parallel Awareness** | Automatic grading company / grade detection and parallel variant recognition |
| **🖼️ Multi-Card Detection** | Identify multiple trading cards in a single image |
| **💰 Pricing & Marketplace API** | Daily-refreshed completed sales + active listings, grade- and parallel-aware, millisecond-level response times |
| **📊 9M+ Trading Card Catalog** | Comprehensive metadata for baseball, basketball, football, hockey, and Pokémon |
| **🤖 Native AI / MCP Integration** | First-class MCP endpoints for Claude and ChatGPT — no glue code required |
| **⚡ Production Infrastructure** | 99.9% uptime SLA, global CDN, typed SDKs, OpenAPI spec, webhook support |
 
## Why Developers Choose CardSight AI
 
### Identification is the hardest part — and we've already solved it
 
Computer vision for trading cards is deceptively hard. Lighting, angles, reflections, hologram interference, parallels that differ from base cards by a single foil pattern, graded slabs, multiple cards in frame, dramatic visual evolution across decades of releases. Most teams that try to build this themselves burn twelve to eighteen months and seven figures before they ship anything competitive. CardSight AI gives you production-grade identification from day one, so you can spend your engineering cycles on your product — not on your ML pipeline.
 
### Built as infrastructure, not an app
 
Most trading card data providers sell you a consumer product and grudgingly expose an API. CardSight AI is the opposite: the API *is* the product. Clean schemas, stable contracts, typed SDKs, and an OpenAPI spec you can actually build against.
 
### Data, not opinions
 
Third-party price feeds usually give you one number and ask you to trust it. We give you the underlying market data — every completed sale and every active listing — at millisecond-level response times. Build your own comps. Audit the data. Trust what you ship because you can see what's behind it.
 
### AI-native from day one
 
Our MCP endpoints let Claude and ChatGPT natively understand trading cards — no prompt engineering, no custom tool-calling glue. Ask an LLM "find me Mike Trout rookies under $500 graded PSA 9 or better" and it just works.
 
### Bring your ideas to life
 
Every feature we ship is in service of one goal: letting you turn a card idea into a running product. Free tier, typed SDKs, sandbox environment, comprehensive docs, MCP support, and a team that actually answers support emails.
 
## Quick Start
 
```bash
npm install cardsightai
```
 
```typescript
import { CardSightAI } from 'cardsightai';
 
const client = new CardSightAI({ apiKey: 'your_api_key' });
 
// 1. Identify a card from an image — the foundation
const result = await client.identify.card(imageFile);
const detection = result.data?.detections?.[0];

if (detection?.card?.id) {
  console.log(`${detection.card.name} — ${detection.card.releaseName}`);

  // 2. Now that you know what it is, see what it's worth
  const pricing = await client.pricing.get(detection.card.id, { period: '90d' });
  console.log(`${pricing.data?.meta.total_records} sales in the last 90 days`);

  // 3. And what's currently for sale
  const listings = await client.marketplace.get(detection.card.id);
}
 
// 4. Or search the catalog directly
const cards = await client.catalog.cards.list({
  year: '2023',
  manufacturer: 'Topps',
  name: 'Aaron Judge',
  take: 10
});
```
 
## SDKs & Platforms
 
All SDKs are open source under the MIT License.
 
| Platform | Install | Repository |
|---|---|---|
| **Node.js / TypeScript** | `npm install cardsightai` | [cardsightai-sdk-node](https://github.com/cardsightai/cardsightai-sdk-node) |
| **Python** | `pip install cardsight` | [cardsightai-sdk-python](https://github.com/cardsightai/cardsightai-sdk-python) |
| **Swift** | Swift Package Manager | [cardsightai-sdk-swift](https://github.com/cardsightai/cardsightai-sdk-swift) |
| **Java** | Maven Central | [cardsightai-sdk-java](https://github.com/cardsightai/cardsightai-sdk-java) |
| **.NET / C#** | NuGet | [cardsightai-sdk-dotnet](https://github.com/cardsightai/cardsightai-sdk-dotnet) |
| **REST API** | Any HTTP client | [OpenAPI Spec](https://api.cardsight.ai/documentation) |
| **MCP (Claude / ChatGPT)** | Connect endpoint | [mcp.cardsight.ai](https://mcp.cardsight.ai) |
 
## API Surface
 
- **Identification** — `identify.card()`, `identify.cardBySegment()`, `detect.card()`
- **Streaming Video Identification** — gRPC API with gRPC-Web support, session-based architecture (enterprise tier)
- **Pricing** — `pricing.get()`, `pricing.bulk()` (up to 100 cards per call, millisecond-level response)
- **Marketplace** — `marketplace.get()` (active listings with URLs, bid counts, listing type)
- **Catalog** — `catalog.search()`, `catalog.cards.*`, `catalog.sets.*`, `catalog.releases.*`, `catalog.parallels.*`, `catalog.random.*`
- **Collections** — `collections.*`, `collections.cards.*`, `collections.binders.*` (full CRUD + analytics)
- **Lists / Wishlists** — `lists.*`, `lists.cards.*`
- **AI Search** — `ai.query()` for natural language
- **Autocomplete** — `autocomplete.cards()`, `autocomplete.sets()`, and more
- **MCP Tools** — native integration for Claude and ChatGPT
Full reference at [api.cardsight.ai/documentation](https://api.cardsight.ai/documentation).
 
## Coverage & Roadmap
 
**Identification is live for:**
 
- ⚾ **Baseball** — full coverage, 1934 through present
- 🏀 **Basketball** — NBA rookies, base, inserts, and parallels
- 🏈 **Football** — all major NFL manufacturers
- 🏒 **Hockey** — NHL cards from US and Canadian manufacturers
- 🎮 **Pokémon TCG** — every era from the original 1999 Base Set through the current Scarlet & Violet series, including raw cards and graded slabs
That's **3,000+ sets** across the four major North American sports plus full Pokémon TCG coverage, all identifiable from a single photo or live video stream.
 
**Coming soon:**
 
- 🃏 **Magic: The Gathering** — visual ID for MTG cards is next on the roadmap
- ⚔️ **One Piece TCG** — rapidly growing in collector interest, in active development
- Additional TCGs to follow
*All new sports and TCGs are automatically available to existing API users at no additional cost.*
 
## What You Can Build
 
**Scan-to-list marketplaces** — users photograph trading cards, your listings populate automatically. Operators report listing abandonment drops of up to 70% when sellers don't have to hand-enter card details.
 
**Collection and portfolio apps** — users snap a photo, your app catalogues and values the trading card instantly, with daily-refreshed valuations across their entire collection.
 
**Card shop POS and inventory tools** — identify trading cards at the counter, price them against live market data, track inventory against the catalog.
 
**Live commerce and breaking platforms** — identify trading cards in real time as they appear on stream, with no capture step or upload required.
 
**Grading pre-screening services** — automatically recognize slabs, read grade values, cross-reference against the catalog, and help users decide whether to submit raw cards for grading.
 
**AI assistants and agents** — Claude or ChatGPT can natively answer identification, pricing, and catalog questions through our MCP endpoints, with zero custom plumbing.
 
**Investment and analytics platforms** — clean, structured sales history and active listings joined to a canonical card catalog. Build your own charts, comps, and valuation models on data you can audit.
 
## Pricing
 
| Tier | Price | Included API Calls | Best for |
|---|---|---|---|
| **Free** | $0/month | 750 calls | Prototyping, evaluation, side projects |
| **Pro** | $14.95/month | 5,000 calls | Production apps and growing businesses |
| **Premium** | $74.95/month | 30,000 calls | Growing Trading Card App, gaining market traction & users |
| **Ultra** | $199.95/month | 100,000 calls | High-volume marketplaces and platforms |
| **Enterprise** | Custom | Custom | Real-time video streaming, dedicated infrastructure, SLA-backed service |
 
No credit card required to start. [Get a free API key →](https://app.cardsight.ai)
 
## What Used to Cost Seven Figures
 
Building comparable infrastructure from scratch looks like this:
 
- Visual identification models and training pipeline — 12–18 months, $500K+
- 9M+ trading card catalog with parallels, variations, and attributes — 6–12 months, $250K+
- Pricing and marketplace aggregation + normalization — 4–6 months, $150K+
- AI and MCP integration — 3–4 months, $125K+
**Total: $1M+ and two years.** Or: `npm install cardsightai` and start building today.
 
## Try It
 
- **[Interactive Playground](https://play.cardsight.ai)** — upload a card image and see identification + pricing in your browser
- **[Free API Key](https://app.cardsight.ai)** — 750 calls/month, no credit card
- **[Documentation](https://cardsight.ai/documentation)** — integration guides, recipes, and reference
## Connect
 
<div align="center">
  
[![Twitter / X](https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://x.com/CardSightAI)
[![Bluesky](https://img.shields.io/badge/Bluesky-0285FF?style=for-the-badge&logo=bluesky&logoColor=white)](https://bsky.app/profile/cardsight.ai)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/company/cardsight-ai/)
[![Discord](https://img.shields.io/badge/Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/UrYrv2SZm8)
 
</div>

**Contact:**
- General: hello@cardsight.ai
- Sales & Enterprise: sales@cardsight.ai
- Support: [cardsight.ai/support](https://cardsight.ai/support)
---
 
<div align="center">
  
**[Get Started](https://app.cardsight.ai) • [Documentation](https://cardsight.ai/documentation) • [API Reference](https://api.cardsight.ai/documentation)**
 
Trading card infrastructure that helps you bring your ideas to life.
 
</div>
 




