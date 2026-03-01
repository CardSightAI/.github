# 👋 Welcome to CardSight AI

<div align="center">

[![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://github.com/cardsightai/cardsightai-sdk-node)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://github.com/cardsightai/cardsightai-sdk-python)
[![Swift](https://img.shields.io/badge/Swift-FA7343?style=for-the-badge&logo=swift&logoColor=white)](https://github.com/cardsightai/cardsightai-sdk-swift)
[![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)](https://github.com/cardsightai/cardsightai-sdk-java)
[![.NET](https://img.shields.io/badge/.NET-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)](https://github.com/cardsightai/cardsightai-sdk-dotnet)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![API Status](https://img.shields.io/badge/API-Online-success)](https://status.cardsight.ai/)
[![Documentation](https://img.shields.io/badge/docs-latest-blue)](https://cardsight.ai/documentation)

**AI-native infrastructure for the trading card industry**

[Website](https://cardsight.ai) • [Documentation](https://cardsight.ai/documentation) • [API Reference](https://api.cardsight.ai/documentation) • [Get API Key](https://app.cardsight.ai/)

</div>

---

## 🚀 What We Build

CardSight AI provides **developer-first infrastructure** for trading cards through advanced computer vision, AI, and comprehensive data APIs. We're not building another card app—we're building the technology that powers the next generation of card apps, marketplaces, and tools.

### Core Capabilities

- **🔍 Visual Card Identification** - 99.5% accuracy across 1,750+ distinct sets with sub-second response times
- **📊 Comprehensive Catalog API** - 6M+ baseball cards from 1934 to present with complete metadata
- **💰 Market Pricing Data** - Market pricing updated daily with historical trends and analytics
- **🤖 AI-Native Integration** - Native MCP endpoints for Claude and ChatGPT plugin support
- **🖼️ Multi-Card Detection** - Identify multiple cards in a single image, including graded slabs
- **⚡ Production-Ready** - 99.9% uptime SLA, global CDN distribution, enterprise-grade infrastructure

## 💻 Technology Stack

We provide native SDKs and tools across all major platforms:

| Platform | Source | Package Manager | Status |
|----------|-----|-----------------|--------|
| **Node.js** | [Repository](https://github.com/cardsightai/cardsightai-sdk-node) | `npm install cardsightai` | ✅ Stable |
| **Python** | [Repository](https://github.com/cardsightai/cardsightai-sdk-python) | `pip install cardsight` | ✅ Stable |
| **Swift** | [Repository](https://github.com/cardsightai/cardsightai-sdk-swift) | Swift Package Manager | ✅ Stable |
| **Java** | [Repository](https://github.com/cardsightai/cardsightai-sdk-java) | Maven Central | ✅ Stable |
| **.NET** | [Repository](https://github.com/cardsightai/cardsightai-sdk-dotnet) | NuGet | ✅ Stable |
| **REST API** | [OpenAPI Spec](https://api.cardsight.ai/documentation) | Any HTTP Client | ✅ Stable |

All SDKs and example projects are released under the **MIT License**.

## 🎯 Built for Developers

### Quick Start

```bash
# Install the SDK
npm install cardsightai
```

```typescript
import { CardSightAI } from 'cardsightai';

// Initialize the client
const client = new CardSightAI({ apiKey: 'your_api_key' });

// Identify a card from an image (File, Blob, Buffer, or ArrayBuffer)
const result = await client.identify.card(imageFile);

if (result.data?.success) {
  const detection = result.data.detections?.[0];
  if (detection) {
    console.log(`Card: ${detection.card.name}`);
    console.log(`Set: ${detection.card.releaseName}`);
    console.log(`Confidence: ${detection.confidence}`);
    console.log(`Total cards detected: ${result.data.detections.length}`);
  }
}

// Search the catalog
const cards = await client.catalog.cards.list({
  year: 2023,
  manufacturer: 'Topps',
  player: 'Aaron Judge',
  take: 10
});

// Natural language AI search
const response = await client.ai.query({
  query: 'Show me Mike Trout rookie cards worth over $100'
});
```

### API Endpoints

- **Visual Identification** - `identify.card()`, `identify.cardBySegment()` - Identify cards from images
- **Card Detection** - `detect.card()` - Check if trading cards are present in an image
- **Catalog Search** - `catalog.cards.list()`, `catalog.sets.list()` - Search 6M+ card database
- **Collection Management** - `collections.create()`, `collections.cards.add()` - Full CRUD for collections
- **AI Search** - `ai.query()` - Natural language card queries
- **MCP Tools** - Native integration with Claude AI assistants
## 🌟 What Makes Us Different

### AI-First Architecture
Built from the ground up for AI integration. Our MCP (Model Context Protocol) endpoints enable Claude and ChatGPT to naturally understand and work with trading cards.

### Developer Experience
- Complete OpenAPI specification
- Auto-generated SDKs for type safety
- Comprehensive documentation with examples
- Sandbox environment for testing
- Webhook support for async operations

### Production Scale
- Sub-second response times (250-450ms for identification)
- 99.9% uptime SLA
- Global CDN for image optimization
- Rate limiting and quota management
- Enterprise support available

### Real-World Ready
Our computer vision handles the messy reality of card collecting: poor lighting, angles, reflections, graded slabs, multiple cards, and various conditions.

## 🎮 Try It Now

- **[Interactive Demo](https://play.cardsight.ai)** - Test identification in your browser in our API Playground
- **[Get Free API Key](https://app.cardsight.ai)** - 750 free calls/month, no credit card required
- **[View Documentation](https://cardsight.ai/documentation)** - Complete integration guides

## 🗺️ Current Coverage & Roadmap

**Available Now:**
- ⚾ **Baseball** - 6M+ cards (1934-present) across all major brands
- 🏀 **Basketball** - 1M+ cards, full NBA coverage including rookies and parallels
- 🏈 **Football** - 1M+ NFL cards from all major manufacturers

**Coming Soon:**
- 🏒 **Hockey** - NHL cards from all major manufacturers in the US and Canada
- 🎮 **Pokemon** - TCG cards with rarity detection

*All new sports and TCGs are automatically available to existing API users at no additional cost.*

## 💡 Use Cases

CardSight AI powers a wide range of applications:

- **Marketplaces** - Instant card identification reduces listing abandonment by 70%
- **Collection Management** - Apps for organizing, tracking, and valuing collections
- **Price Tracking** - Real-time market data for investment and analytics platforms
- **Grading Services** - Automated pre-screening and population reporting
- **Card Shops** - Point-of-sale integration and inventory management
- **AI Assistants** - Natural language card queries through Claude or ChatGPT

## 📈 Why CardSight AI?

### What Used to Cost $1M+ Now Takes Minutes

Building trading card infrastructure from scratch requires:
- 12-18 months for visual identification ($500k+)
- 6-12 months for catalog database ($250k+)
- 4-6 months for pricing integration ($150k+)
- 3-4 months for AI integration ($125k+)

**Total: $1M+ and 2+ years**

CardSight AI provides all of this starting at **$0/month** with our free tier.

## 🤝 Built by Collectors, for Collectors

We're a small but passionate team of collectors and developers who believe the future of collectibles is more open, more accessible, and powered by AI. Every feature starts with a real problem we've faced in our own collecting journeys.

## 🔗 Connect With Us

<div align="center">

[![Twitter](https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://x.com/CardSightAI)
[![Bluesky](https://img.shields.io/badge/Bluesky-0285FF?style=for-the-badge&logo=bluesky&logoColor=white)](https://bsky.app/profile/cardsight.ai)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/company/cardsight-ai/)
[![Discord](https://img.shields.io/badge/Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/UrYrv2SZm8)

</div>

## 📧 Get in Touch

- **General Inquiries:** hello@cardsight.ai
- **Sales & Enterprise:** sales@cardsight.ai
- **Support:** [cardsight.ai/support](https://cardsight.ai/support)
- **Documentation:** [cardsight.ai/documentation](https://cardsight.ai/documentation)

---

<div align="center">

**[Get Started](https://app.cardsight.ai) • [Documentation](https://cardsight.ai/documentation) • [API Reference](https://api.cardsight.ai/documentation)**

Built with ❤️ by collectors, for collectors

</div>
