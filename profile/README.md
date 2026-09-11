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

We exist to help developers, marketplaces, collectors, and collectibles businesses **bring their ideas to life** without spending two years and a million dollars building the visual identification, catalog, and pricing layers from scratch. One REST API, typed SDKs in five languages, and a native MCP endpoint for Claude and ChatGPT — and you're shipping.

**Three pillars:**

1. **🔍 Visual Identification** — our namesake. Point a camera at a card, know exactly what it is.
2. **📊 14M+ Trading Card Catalog** — the structured knowledge layer behind identification, spanning baseball, basketball, football, hockey, MMA, Pokémon TCG, Magic: The Gathering, and One Piece TCG.
3. **💰 Pricing & Marketplace Data** — completed sales and active listings from real public marketplaces, so "what is it?" immediately becomes "what's it worth?"

## 🔍 Visual Card Identification — The Platform's Core

Identification is the hardest problem in trading cards, and it's the one CardSight AI was built to solve. Every other feature — pricing lookups, collection tracking, marketplace integrations, AI agents that answer card questions — starts with the AI pipeline knowing exactly what card is in the frame. Get that wrong and nothing downstream works.

Our identification pipeline is the reason the company is called CardSight.

**What it does:**

- **99.5% accuracy across 8,250+ sets** spanning baseball, basketball, football, hockey, MMA, Pokémon TCG, Magic: The Gathering, and One Piece TCG
- **Sub-300ms median response time** — built for real-time camera flows, not batch jobs
- **Multi-card detection** — identify multiple trading cards in a single image, including binder pages and showcase shots
- **Card language detection** — TCG results report the language a card was printed in across eight languages (English, French, German, Italian, Spanish, Japanese, Korean, Chinese), returned as an ISO 639-1 code in a `CARD_LANGUAGE` field, so a Japanese print is never silently returned as its US counterpart. Pokémon and Magic in all eight; One Piece in the five languages Bandai publishes it in.
- **Parallel identification (beta)** — every result names the exact card first. For baseball, results now also include **ranked parallel suggestions** drawn from **17,000+ parallel patterns, colors, and designs** across every supported baseball set. They come back as suggestions rather than a single verdict because optical parallels can look different under different lighting — so your app can confirm the top match or show a shortlist instead of shipping a wrong parallel.
- **Slab / grading awareness** — detects graded trading cards and reads the slab (PSA, Beckett/BGS, SGC, CGC, TAG) including grade value, condition, and qualifiers. Autograph grade identification is coming soon.
- **Real-time video streaming identification** — continuous identification from live video over SRT, WebSocket, RTMP, and RTSP, with results delivered over gRPC (gRPC-Web for browsers). Enterprise tier.
- **Confidence tiers** — every detection returns `High`, `Medium`, or `Low` confidence plus a match level (exact card, set-level match, or detected-but-unmatched) so your UI can respond intelligently
- **Works with messy reality** — poor lighting, angles, reflections, phone cameras, multiple trading cards in frame
- **Any common image** — JPEG, PNG, WebP, HEIF, or HEIC up to 20MB, as multipart form data or a direct binary upload

**What you can build with it:**

- **Scan-to-list flows** for marketplaces that eliminate manual data entry
- **Collection apps** where users snap a photo and the trading card is fully catalogued
- **Grading and pre-screening tools** that recognize slabs and grade values automatically
- **AI assistants** that can "see" a trading card the user holds up and answer questions about it
- **Point-of-sale integration** for card shops that want instant identification at the counter
- **Live commerce and breaking platforms** with real-time identification of cards on stream

```typescript
import { CardSightAI, getFieldValue } from 'cardsightai';

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

    // TCG cards also report the printed language as an ISO 639-1 code
    console.log(`  Language: ${getFieldValue(detection, 'CARD_LANGUAGE')}`); // e.g. "ja"
  }
}

// Target a specific segment for faster, more accurate results
const pokemonResult = await client.identify.cardBySegment('pokemon', imageFile);

// Or just check if a card is present (lightweight, cheaper)
const presence = await client.detect.card(imageFile);
```

## 💰 Pricing & Marketplace — Data, Not Opinions

Identification tells you what the card is. Pricing tells you what the market thinks it's worth — and on that second question, we take a strong position: **data, not opinions.**

Most pricing APIs hand you a single number and ask you to trust it. We don't. CardSight AI gives you **the underlying market data itself** — every completed sale and every active listing, sourced from eBay, Fanatics Collect, COMC, and other public marketplaces — so you can build your own comps, charts, and valuations on a foundation you can audit. One aggregate number hides everything that matters; we surface all of it.

**Built for production:**

- **Source-traceable records** — every data point carries its price, date, source, listing URL, and listing image
- **Refreshed continuously** — results update as new sales close, not on a publishing schedule
- **Bid/ask framing** — completed auction sales (bid) alongside current Buy It Now asking prices (ask)
- **Parallel-aware and grade-aware** — a 2023 Topps Chrome Refractor PSA 10 is priced separately from its raw base card, with raw and graded results split by grading company and grade
- **Filterable lookback periods** — `7d`, `14d`, `2w`, `3m`, `1y`, `all`, and more
- **Bulk pricing** — pull data for up to 100 trading cards in a single call
- **Listing-title search** — free-text search over historical and active listings, including listings never matched to a canonical card
- **Linked directly to the catalog** — no fuzzy joining between pricing feeds and card records

**Market data is live for** baseball, football, basketball, hockey, Pokémon TCG, and One Piece TCG. Magic: The Gathering market data is coming soon.

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

Identification and pricing only work because of the catalog underneath. Over fourteen million trading cards, fully structured, with parallels, variations, attributes, grading references, and card images.

- **14M+ trading cards** from 1933 through present
- **Baseball, basketball, football, hockey, MMA, Pokémon TCG, Magic: The Gathering, and One Piece TCG** — full identification and catalog support across all eight
- **Parallels and variations as first-class entities** — not an afterthought
- **Segments, manufacturers, releases, sets, parallels** — the full hierarchy developers actually need
- **Updated continuously** — new products land in the catalog as they release, often before most collectors have the cards in hand
- **Fuzzy search, autocomplete, and natural-language AI search** — pick whichever fits your UX. Full-text search returns in under 200 milliseconds.

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

## 🏅 Population Reports — Free on Every Plan

Graded population counts at the card, set, and release level, sourced directly from each grading company's published census — never synthesized, estimated, or interpolated.

- **Live today:** PSA on baseball
- **Next:** TAG, Beckett, SGC, and CGC on baseball, then basketball, hockey, football, and Pokémon across all graders
- **Free on every plan, including Free** — card-level and set-level population endpoints never count toward monthly usage

## Core Capabilities at a Glance

| Capability | What it does |
|---|---|
| **🔍 Visual Card Identification** | 99.5% accuracy across 8,250+ sets, sub-300ms response, handles raw cards and graded slabs |
| **🌐 Card Language Detection** | Reports the printed language of Pokémon, Magic, and One Piece cards across eight languages as an ISO 639-1 code |
| **🎨 Parallel Identification (Beta)** | The exact card first, then ranked parallel suggestions across 17,000+ baseball parallel patterns, colors, and designs |
| **🎬 Real-Time Video Streaming Identification** | Continuous identification from live video streams over SRT, WebSocket, RTMP, and RTSP (Enterprise tier) |
| **🏷️ Slab Identification** | Grading company, grade, condition, and qualifier detection for PSA, Beckett/BGS, SGC, CGC, and TAG |
| **🖼️ Multi-Card Detection** | Identify multiple trading cards in a single image |
| **💰 Pricing & Marketplace API** | Completed sales + active listings, source-traceable, grade- and parallel-aware |
| **📊 14M+ Trading Card Catalog** | Comprehensive metadata for baseball, basketball, football, hockey, MMA, Pokémon TCG, Magic: The Gathering, and One Piece TCG |
| **🏅 Population Reports** | Graded population counts by card, set, and release — free on every plan |
| **🗂️ Collection Management** | Collections, binders, want lists, and portfolio analytics through the API |
| **🤖 Native AI / MCP Integration** | Hosted MCP endpoint for Claude, ChatGPT, and any MCP-compatible assistant — no glue code required |
| **⚡ Production Infrastructure** | 99.9% uptime SLA, global CDN, typed SDKs, OpenAPI spec |

## Why Developers Choose CardSight AI

### Identification is the hardest part — and we've already solved it

Computer vision for trading cards is deceptively hard. Lighting, angles, reflections, hologram interference, parallels that differ from base cards by a single foil pattern, graded slabs, multiple cards in frame, dramatic visual evolution across decades of releases. Most teams that try to build this themselves burn twelve to eighteen months and seven figures before they ship anything competitive. CardSight AI gives you production-grade identification from day one, so you can spend your engineering cycles on your product — not on an identification pipeline.

### Built as infrastructure, not an app

Most trading card data providers sell you a consumer product and grudgingly expose an API. CardSight AI is the opposite: the API *is* the product. No approvals, no questions about what you're building, every feature on every tier including Free. Clean schemas, stable contracts, typed SDKs, and an OpenAPI spec you can actually build against.

### Data, not opinions

Third-party price feeds usually give you one number and ask you to trust it. We give you the underlying market data — every completed sale and every active listing, each traceable to its source. Build your own comps. Audit the data. Trust what you ship because you can see what's behind it.

### Accurate first, honest about the rest

Parallels are the hardest problem in card identification, so we don't pretend otherwise. Identification names the exact card first, and the parallel identification beta returns ranked suggestions alongside it instead of a single guess that might be wrong. A wrong parallel on a listing is worse than a shortlist your user can confirm.

### AI-native from day one

Our hosted MCP endpoint lets Claude and ChatGPT natively understand trading cards — no prompt engineering, no custom tool-calling glue. Ask an LLM "find me Mike Trout rookies under $500 graded PSA 9 or better" and it just works.

### Bring your ideas to life

Every feature we ship is in service of one goal: letting you turn a card idea into a running product. Free tier with every feature, typed SDKs, comprehensive docs, MCP support, and a team that actually answers support emails. Every sport and TCG we add comes to existing plans at no extra charge.

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

All SDKs are open source under the MIT License, with types generated from the OpenAPI spec, built-in error handling, and IDE autocomplete.

| Platform | Install | Repository |
|---|---|---|
| **Node.js / TypeScript** | `npm install cardsightai` (server-side and browser) | [cardsightai-sdk-node](https://github.com/cardsightai/cardsightai-sdk-node) |
| **Python** | `pip install cardsightai` | [cardsightai-sdk-python](https://github.com/cardsightai/cardsightai-sdk-python) |
| **Swift** | Swift Package Manager | [cardsightai-sdk-swift](https://github.com/cardsightai/cardsightai-sdk-swift) |
| **Java** | Maven Central | [cardsightai-sdk-java](https://github.com/cardsightai/cardsightai-sdk-java) |
| **.NET / C#** | NuGet `CardSightAI` | [cardsightai-sdk-dotnet](https://github.com/cardsightai/cardsightai-sdk-dotnet) |
| **REST API** | Any HTTP client — base URL `https://api.cardsight.ai`, key in the `X-API-Key` header | [OpenAPI Spec](https://api.cardsight.ai/documentation/json) · [Swagger UI](https://api.cardsight.ai/documentation) |
| **MCP (Claude / ChatGPT)** | Connect `https://mcp.cardsight.ai/?k=YOUR_API_KEY` (Streamable HTTP) | [MCP docs](https://cardsight.ai/documentation/mcp) |

## API Surface

- **Identification** — `identify.card()`, `identify.cardBySegment()`, `detect.card()`, plus free pre-flight endpoints that list every identifiable set and check a single set before you spend an identify call
- **Streaming Video Identification** — gRPC API with gRPC-Web support, session-based architecture (Enterprise tier)
- **Pricing** — `pricing.get()`, `pricing.bulk()` (up to 100 cards per call)
- **Marketplace** — `marketplace.get()` (active listings with URLs, bid counts, listing type) and free-text listing-title search
- **Catalog** — `catalog.search()`, `catalog.cards.*`, `catalog.sets.*`, `catalog.releases.*`, `catalog.parallels.*`, `catalog.random.*`, catalog statistics, and a release calendar
- **Population** — graded counts by card, set, or release (free on every plan)
- **Collections** — `collections.*`, `collections.cards.*`, `collections.binders.*` (full CRUD, per-card buy/sell tracking, analytics and breakdowns)
- **Lists / Wishlists** — `lists.*`, `lists.cards.*`
- **Grading data** — grading companies, types, and grade scales (PSA, Beckett/BGS, SGC, CGC, TAG)
- **AI Search** — `ai.query()` for natural language
- **Autocomplete** — `autocomplete.cards()`, `autocomplete.sets()`, and more (free on every plan)
- **Images** — catalog card images and collection card images / thumbnails (free on every plan)
- **Feedback, health, usage** — submit corrections, run health and auth checks, read current billing-period usage
- **MCP Tools** — native integration for Claude and ChatGPT

Full reference at [api.cardsight.ai/documentation](https://api.cardsight.ai/documentation).

## Coverage & Roadmap

**Identification is live for:**

- ⚾ **Baseball** — 4.5M+ cards, 1933 through present, plus the parallel identification beta across 17,000+ parallel patterns, colors, and designs
- 🏀 **Basketball** — 2M+ cards across 750+ sets: NBA and WNBA rookies, base, inserts, and parallels
- 🏈 **Football** — 2.5M+ cards from all major NFL manufacturers
- 🏒 **Hockey** — 1M+ cards: NHL cards from Upper Deck, O-Pee-Chee, Parkhurst, and more
- 🥊 **MMA** — new: nearly 300 releases spanning 35+ years from Topps, Leaf, Panini, and other publishers
- 🎮 **Pokémon TCG** — 172 sets, full US retail coverage since the 1999 Base Set, with language detection in all eight supported languages
- 🧙 **Magic: The Gathering** — 250+ releases from 1993 Alpha to the latest 2026 release, with language detection in all eight supported languages. Identification and catalog are live; market data is coming soon.
- ⚔️ **One Piece TCG** — every US retail release since the 2022 launch: 4,500+ cards across 56 releases, with identification, catalog, and market data all live and language detection across the five languages Bandai publishes

That's **8,250+ sets** across the four major North American sports, MMA, and three trading card games, all identifiable from a single photo. Streaming video identification currently covers baseball, football, basketball, hockey, Pokémon TCG, and One Piece TCG.

**Coming soon:**

- 🃏 **Yu-Gi-Oh! TCG** — planned for 2026
- ⚽ **Soccer** — planned for 2026
- Also on the roadmap: Golf, Tennis, NASCAR, Wrestling (WWE), Disney, VeeFriends, and Star Trek

*All new sports and TCGs are automatically available to existing API users at no additional cost.*

## What You Can Build

**Scan-to-list marketplaces** — users photograph trading cards, your listings populate automatically. Cut listing abandonment by up to 70% when sellers don't have to hand-enter card details.

**Collection and portfolio apps** — users snap a photo, your app catalogues and values the trading card instantly, with valuations that refresh as new sales close across their entire collection.

**Card shop POS and inventory tools** — identify trading cards at the counter, price them against live market data, track inventory against the catalog.

**Live commerce and breaking platforms** — identify trading cards in real time as they appear on stream, with no capture step or upload required.

**Grading pre-screening services** — automatically recognize slabs, read grade values, cross-reference against the catalog and free population reports, and help users decide whether to submit raw cards for grading.

**AI assistants and agents** — Claude or ChatGPT can natively answer identification, pricing, and catalog questions through our MCP endpoint, with zero custom plumbing.

**Investment and analytics platforms** — clean, structured sales history and active listings joined to a canonical card catalog. Build your own charts, comps, and valuation models on data you can audit.

## Pricing

Every plan includes every feature; tiers differ only in monthly call volume, rate limit, and the per-call discount on additional calls.

| Tier | Price | Included API Calls | Rate limit | Best for |
|---|---|---|---|---|
| **Free** | $0/month | 750 calls | 4 req/s | Prototyping, evaluation, side projects |
| **Pro** | $14.95/month | 5,000 calls | 6 req/s | Production apps and growing businesses |
| **Premium** | $74.95/month | 30,000 calls | 8 req/s | Growing trading card apps gaining market traction and users |
| **Ultra** | $199.95/month | 100,000 calls | 10 req/s | High-volume marketplaces and platforms |
| **Enterprise** | Custom | Unlimited | No rate limit | Real-time video streaming, dedicated or white-label infrastructure, custom endpoints, SLA guarantees, 24/7 phone support, on-premise options |

Additional calls are bought as 10,000-call packs on any paid tier — $0.0030 per call on Pro, $0.0025 on Premium (20% tier discount), and $0.0020 on Ultra (35% tier discount). Packs stack on top of your monthly allotment, never expire, and nothing is billed automatically. Card image fetches, collection image uploads, autocomplete, population reports, and the identifiable-set list and check are free on every plan and never count toward usage.

No credit card required to start. [Get a free API key →](https://app.cardsight.ai)

## What Used to Cost Seven Figures

Building comparable infrastructure from scratch looks like this:

- Visual identification pipeline — 12–18 months, $500K+
- 14M+ trading card catalog with parallels, variations, and attributes — 6–12 months, $250K+
- Pricing and marketplace aggregation + normalization — 4–6 months, $150K+
- AI and MCP integration — 3–4 months, $125K+

**Total: $1M+ and two years.** Or: `npm install cardsightai` and start building today.

## Try It

- **[Interactive Playground](https://play.cardsight.ai)** — upload a card image and see identification + pricing in your browser
- **[Free API Key](https://app.cardsight.ai)** — 750 calls/month, no credit card
- **[Documentation](https://cardsight.ai/documentation)** — integration guides, recipes, and reference
- **[Discord](https://discord.gg/UrYrv2SZm8)** — talk to the team and other builders

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
- Support: support@cardsight.ai · [cardsight.ai/support](https://cardsight.ai/support)
- Phone: (207) 699-4565
- Mail: PO Box 442, Biddeford, ME 04005

CardSight AI was founded in 2025 in Biddeford, Maine — built by collectors, for collectors. We're part of the Founder Residency at Northeastern University's Roux Institute, the AWS Startups and NVIDIA Inception programs, supported by the Maine Technology Institute, and a member of the Certified Trading Card Association (CTCA).

---

<div align="center">

**[Get Started](https://app.cardsight.ai) • [Documentation](https://cardsight.ai/documentation) • [API Reference](https://api.cardsight.ai/documentation)**

Trading card infrastructure that helps you bring your ideas to life.

</div>
