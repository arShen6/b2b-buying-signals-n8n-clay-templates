# Clay HTTP API Setup Guide (Copy-Paste Single-Row Recipes)

All four Apify Actors are engineered specifically for **Clay single-row HTTP API columns**:
- **Zero `$0.50` minimum spend** and ultra-low per-run start fees (`$0.00005`–`$0.0005`).
- **Safe blank-cell hygiene**: Passing an empty `companyName` or `domain` exits `SUCCEEDED` with `$0` item charges instead of failing your Clay waterfall or returning irrelevant demo rows.
- **Flat JSON output**: Ready to map directly into Clay columns without writing nested JavaScript parsers.

---

## Common Header Setup (All 4 Recipes)

In your Clay table, click **Add enrichment → HTTP API**:
- **Method**: `POST`
- **Headers**:
  - `Authorization`: `Bearer <YOUR_APIFY_API_TOKEN>`
  - `Content-Type`: `application/json`

---

## Recipe 1: Pre-Claygent B2B Domain Contact, Tech Stack & Impressum Enricher

Filter ~89% of domains at `$0.003/live site` (`$0.00005` start) before spending `$0.05–$0.20/row` on Claygent.

- **Endpoint**:
  ```text
  https://api.apify.com/v2/acts/vayneshen~b2b-domain-contact-tech-enricher/run-sync-get-dataset-items?memory=512&maxTotalChargeUsd=0.05
  ```
- **Body**:
  ```json
  {
    "domain": "{{Company Domain}}",
    "maxSubpagesPerDomain": 3,
    "includeDachImpressum": true,
    "includeSummary": true
  }
  ```
- **Recommended Clay Waterfall Condition (for your downstream Claygent column)**:
  - Run Claygent **only if** `needsBrowserFallback === true` (never waste browser credits on `parked_or_for_sale` or dead `unreachable` domains).
- **Best Columns to Map**: `bestEmail`, `bestEmailRole`, `linkedinUrl`, `emailProvider`, `dmarcPolicy`, `techStackNames`, `tagManagerIds`, `impressumLegalName`, `impressumManagingDirector`, `impressumVatId`, `companySummary`, `needsBrowserFallback`.

---

## Recipe 2: Single-Row Company News & B2B Buying Signals (Real Decoded URLs)

Check each account row for recent funding, M&A, executive hires, or expansion with real publisher URLs and ready-to-use cold-email icebreakers (~2.8s per row).

- **Endpoint**:
  ```text
  https://api.apify.com/v2/acts/vayneshen~google-news-scraper-b2b-signals/run-sync-get-dataset-items?memory=512&maxTotalChargeUsd=0.05
  ```
- **Body**:
  ```json
  {
    "companyName": "{{Company Name}}",
    "companyDomain": "{{Company Domain}}",
    "maxItemsPerCompany": 3,
    "timeRange": "30d",
    "onlyBuyingSignals": true,
    "decodeRealUrls": true,
    "extractFullText": false,
    "deduplicateAcrossRuns": true,
    "dedupKey": "clay-outbound-table-1"
  }
  ```
- **Best Columns to Map**: `[0].primaryTrigger`, `[0].signalConfidence`, `[0].title`, `[0].url` *(real decoded publisher URL)*, `[0].publisherName`, `[0].publishedAt`, `[0].icebreakerSnippet`, `[0].coverageCount`.

---

## Recipe 3: ABM Live Hiring & Salary Signals by Target Company Domain

Auto-discovers each company's live Ashby, Greenhouse, or Lever ATS board directly from its domain.

- **Endpoint**:
  ```text
  https://api.apify.com/v2/acts/vayneshen~tech-hiring-salary-radar/run-sync-get-dataset-items?memory=512&maxTotalChargeUsd=0.05
  ```
- **Body**:
  ```json
  {
    "onlyCustomBoards": true,
    "targetCompanyDomains": ["{{Company Domain}}"],
    "keywords": ["Sales", "AI", "Security", "VP", "Head of"],
    "postedWithinDays": 14,
    "maxResults": 10
  }
  ```
- **Best Columns to Map**: `[0].title`, `[0].seniority`, `[0].salarySummary`, `[0].hiringTriggerTags`, `[0].techStackSignals`, `[0].jobUrl`.

---

## Recipe 4: Pull Freshly Funded US Operating Startups (SEC Form D Source)

Best run on a **weekly/daily Clay Source Webhook** or **n8n → Clay Webhook** to feed brand-new funded startups into your Clay table (hedge funds, SPVs, and M&A swaps already stripped out).

- **Endpoint**:
  ```text
  https://api.apify.com/v2/acts/vayneshen~sec-form-d-funding-leads/run-sync-get-dataset-items?memory=256&maxTotalChargeUsd=0.50
  ```
- **Body**:
  ```json
  {
    "daysBack": 7,
    "excludeInvestmentFunds": true,
    "excludeRealEstateAndFinance": true,
    "excludeBusinessCombinations": true,
    "onlyCorporations": true,
    "minSoldUsd": 1000000,
    "enableDomainEnrichment": true,
    "deduplicateAcrossRuns": true,
    "dedupKey": "clay-weekly-form-d",
    "maxResults": 50
  }
  ```
