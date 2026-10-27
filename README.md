# Investors Club Scraper

Extract structured data from [investors.club](https://investors.club) — investors.club businesses for sale with asking price, annual net profit and profit multiple per listing. Each row adds industry, established year and monetization; revenue and country where the seller lists them. Ready for spreadsheets or a CRM.

**[Investors Club Scraper on Apify →](https://apify.com/blackfalcondata/investors-club-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each company.

**Incremental mode** — Only get new or changed companies since your last run. Content hash per company — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how companies evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N companies (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every company returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured companies from investors.club on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor companies, track trends, and analyze market dynamics with structured, deduplicated data from investors.club.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per company is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "shopify",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | `""` | Keyword to filter listings by (matched against listing title and description), e.g. "shopify", "saas", "youtube channel". Use a JSON array for several searches in one run: ["shopify","newsletter"]. Leave empty to get every active listing (see Browse all listings). |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste raw listing-search URLs from investors.club (e.g. https://investors.club/listings/?keyword_search=shopify). Also accepted: the filter pages (e.g. https://investors.club/business-type/saas/, https://investors.club/budget/under-50000/, https://investors.club/monetization/affiliate/, https://investors.club/tech-stack/wordpress/, https://investors.club/country/united-states/, https://investors.club/industry/...) and direct listing pages (https://investors.club/listing/{id}-{slug}/). Each URL becomes its own search task; results are merged and deduped by listing ID across all URLs. When provided, startUrls REPLACE the search term (each supported URL becomes one task; unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `browseAll` | boolean | `true` | When no search term and no start URL is given, return every active listing on investors.club instead of stopping. Ignored when a search term or start URL is set. |
| `includeDetails` | boolean | `true` | Fetch full listing details. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from your search inputs (search terms, startUrls) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | When incremental, also emit records that haven't changed. |
| `emitExpired` | boolean | `false` | When incremental, also emit listings no longer found (sold or withdrawn). Scans the whole catalogue so nothing is marked expired by mistake. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |

---

## Output fields

Every company returns the same 46-field schema. Missing values are `null` — never omitted.

- `type`
- `listingId`
- `listingUrl`
- `portalUrl`
- `title`
- `businessType`
- `tagline`
- `description`
- `language`
- `market`
- `industry`
- `establishedYear`
- `teamSize`
- `technology`
- `monetization`
- `askingPrice`
- `annualGrossRevenue`
- `annualNetProfit`
- `revenueMultiple`
- `profitMultiple`
- `monthlyGrossRevenue`
- `monthlyNetProfit`
- `category`
- `categorySlug`
- `country`
- `countrySlugs`
- `techStackSlugs`
- `classifiedsTags`
- `classifiedsSlugs`
- `revenueFeatures`
- `trafficChannels`
- `financialsAvailable`
- `dueDiligenceReportAvailable`
- `screenshotsAvailable`
- `earlyAccess`
- `status`
- `imageUrl`
- `listedAt`
- `modifiedAt`
- `scrapedAt`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per company. Here is a real example from a production run:

```json
{
  "type": "listing",
  "listingId": "110793",
  "listingUrl": "https://investors.club/listing/110793-shopify-evening-gown-and-pageant-store/",
  "portalUrl": "https://investors.club/listing/110793-shopify-evening-gown-and-pageant-store/",
  "title": "Shopify Evening Gown And Pageant Store",
  "businessType": [
    "For Sale by Owner"
  ],
  "tagline": "Shopify Formalwear Store With Private-Label Branding And Pageant Industry Relationships",
  "description": "Founded in 2018, this Shopify store sells evening gowns, pageant dresses, and special-occasion apparel with private-label branding. Sales currently come primarily from existing cus…",
  "language": "English",
  "market": "United States",
  "industry": "Fashion, Beauty & Personal Care",
  "establishedYear": 2018
}
```

*Truncated — full records contain 46 fields. See Output fields for the complete schema.*

**[Try Investors Club Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/investors-club-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.0045 |

See the [actor on Apify](https://apify.com/blackfalcondata/investors-club-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape investors.club?**
Use this actor on Apify to extract structured data from investors.club. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get investors.club data as JSON, CSV, or Excel?**
The actor writes each company to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape investors.club?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check investors.club's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each company gets a content hash. On subsequent runs, only new or changed companies are emitted — saving time, compute, and storage. Expired companies can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Investors Club Scraper](https://apify.com/blackfalcondata/investors-club-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
