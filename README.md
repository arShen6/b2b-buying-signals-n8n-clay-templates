# B2B Buying Signals & Lead Enrichment Templates for n8n & Clay

Production-ready, zero-proxy **n8n workflow JSONs** and **Clay HTTP API column recipes** to automate B2B buying-signal monitoring and clean contact/tech-stack enrichment — with **no `$0.50` minimum spend**, **cross-run delta deduplication (`$0` on repeats)**, and **single-row micro-start pricing** built specifically for Clay & n8n webhooks.

---

## Included Templates (Import in 1 Click)

| Workflow | Use Case | Apify Actor Used | Template File |
| :--- | :--- | :--- | :--- |
| **1. Google News Buying Signals → Auto-Enrich Contacts** | Daily ABM monitor decoding **real publisher URLs** + EN/DE buying triggers (`funding`, `acquisition`, `exec_hire`, `expansion`, `partnership`) + cross-run delta dedup, automatically piped into domain contact & tech-stack enrichment → Google Sheets & Slack | [Google News Scraper & B2B Buying Signals](https://apify.com/vayneshen/google-news-scraper-b2b-signals) + [B2B Domain Enricher](https://apify.com/vayneshen/b2b-domain-contact-tech-enricher) | [`templates/n8n-google-news-b2b-signals.json`](./templates/n8n-google-news-b2b-signals.json) |
| **2. Pre-Claygent B2B Domain Contact, Tech Stack & DACH Impressum Enricher** | Enrich 1 to 10,000 domains with **3-bucket email hygiene** (blocks `privacy@`, DPO, and law-firm emails from polluting `bestEmail`), 65+ tech/Shopify/GTM fingerprints, WAF-immune DNS/SPF/DMARC, and DACH/EU Impressum — routing only `needsBrowserFallback === true` (~11%) to Claygent | [B2B Website Contact, Tech Stack & Impressum Enricher](https://apify.com/vayneshen/b2b-domain-contact-tech-enricher) | [`templates/n8n-b2b-domain-enricher.json`](./templates/n8n-b2b-domain-enricher.json) |
| **3. Daily Newly Funded US Startups (SEC Form D)** | Monitors official SEC EDGAR Form D private placements every weekday, stripping out hedge funds, VC/PE funds, SPVs, and M&A swaps to surface freshly funded operating startups with verified domains & executives | [SEC Form D Startup Funding Leads](https://apify.com/vayneshen/sec-form-d-funding-leads) | [`templates/n8n-sec-form-d-leads.json`](./templates/n8n-sec-form-d-leads.json) |
| **4. Tech Hiring & Salary Radar (Greenhouse, Ashby & Lever)** | Scans 85+ top AI/DevTools/Fintech/SaaS companies (or auto-discovers ATS boards from your own ABM `targetCompanyDomains`) for `$180K+` roles, verified salary bands, and hiring buying triggers | [Tech Hiring & Salary Radar](https://apify.com/vayneshen/tech-hiring-salary-radar) | [`templates/n8n-tech-hiring-radar.json`](./templates/n8n-tech-hiring-radar.json) |

👉 **Using Clay instead of n8n?** See [`clay/CLAY_SETUP_GUIDE.md`](./clay/CLAY_SETUP_GUIDE.md) for copy-paste single-row **Clay HTTP API** configurations.

---

## Why These Workflows Cost 5x–10x Less in n8n & Clay

Most Apify scrapers charge `$0.005–$0.033` per Actor start or enforce a `$0.50` minimum spend per run, which destroys unit economics on daily scheduled n8n monitors or single-row Clay webhooks:

| Actor | Per-Run Start Fee | Unit Price (Standard / Gold) | Free / `$0` Built-In Guardrails |
| :--- | :---: | :---: | :--- |
| **[Google News B2B Signals](https://apify.com/vayneshen/google-news-scraper-b2b-signals)** | **`$0.0005`** *(10x cheaper)* | `$1.50/1K` RSS · `$2.30/1K` Real URLs *(Gold `$1.60/1K`)* | Cross-run delta dedup (`$0` seen news), `$0` failed decodes, `$0` paywalled full-text, blank Clay rows exit `$0` |
| **[B2B Domain Enricher](https://apify.com/vayneshen/b2b-domain-contact-tech-enricher)** | **`$0.00005`** *(660x cheaper)* | `$3.00/1K` live sites *(Gold `$2.50/1K`)* | Blocked/WAF/parked/dead domains = `$0` (still includes free DNS MX/SPF/DMARC!) |
| **[SEC Form D Funding Leads](https://apify.com/vayneshen/sec-form-d-funding-leads)** | **`$0.0005`** | Pay per qualified operating startup only | Hedge funds, SPVs, REITs, M&A swaps & previously seen filings = `$0` |
| **[Tech Hiring & Salary Radar](https://apify.com/vayneshen/tech-hiring-salary-radar)** | **`$0.0005`** | Pay per brand-new matching job opening | Previously seen `jobKey`s & filtered-out openings = `$0` |

---

## Quick Start (n8n — 2 Minutes)

1. Download any JSON from [`templates/`](./templates/).
2. In n8n, open a new workflow → click **`...` → Import from File** and select the JSON.
3. Open the Apify HTTP Request node(s) and create an **HTTP Header Auth** credential:
   - **Name**: `Authorization`
   - **Value**: `Bearer <YOUR_APIFY_API_TOKEN>` *(get a free `$5/mo` token from [Apify Console → Integrations](https://console.apify.com/settings/integrations))*
4. Connect your **Google Sheets** and/or **Slack** credentials, adjust your target companies/filters, and hit **Test Workflow**.

---

## Architecture: Full Automated B2B Outbound Signal Stack

```text
  [Daily Trigger]
        │
        ├──► Actor 4: Google News Buying Signals (Real URLs + Delta Dedup)
        ├──► Actor 2: SEC Form D Newly Funded Operating Startups
        └──► Actor 1: Tech Hiring & Salary Radar (Ashby / Greenhouse / Lever)
                                │
                                ▼ (Deduplicate unique company domains)
        ┌──────────────────────────────────────────────────────────────────┐
        │ Actor 3: B2B Domain Contact, Tech Stack & DACH Impressum Enricher│
        │  • 3-bucket email hygiene (zero privacy@/DPO pollution)          │
        │  • 65+ tech stack & Shopify/GTM fingerprints + DNS/DMARC         │
        │  • Routes ONLY ~11% WAF/JS-shell sites to Claygent fallback      │
        └──────────────────────────────────────────────────────────────────┘
                                │
                                ▼
              [Google Sheets / HubSpot / Clay / Slack Alerts]
```

## License

MIT — free to import, modify, and deploy in personal or agency workflows.
