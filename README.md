# Willhaben Jobs Feed

Extract structured job listings from [willhaben.at](https://willhaben.at) — Austria's largest classifieds platform with 15,000+ active job postings. Full salary data (mandatory in Austria), direct contact info, company profiles, and 7 search filters.

**[Willhaben Job Scraper - Austria’s Largest Job Portal on Apify →](https://apify.com/blackfalcondata/willhaben-scraper?fpr=1h3gvi)**

---

## Key features





**Search with filters** — Search by keyword and location. Filter by region (bundesland), field / sector, employment type, and more.

**Detail enrichment** — Fetch full job descriptions, salary data, employer profiles, contact information for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per listing to control output size and cost.

**Result cap** — Stop after N listings (up to 5.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases





**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from willhaben.at on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from willhaben.at.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired listings. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Compensation benchmarking**
Aggregate salary ranges across roles, industries, and locations on willhaben.at to inform pricing decisions, hiring plans, or candidate negotiations.

**Lead generation**
Extract employer contact details alongside listings to build outreach lists for recruiters, staffing agencies, or B2B sales teams.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "developer",
  "maxResults": 50,
  "includeDetails": true
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Job search keywords (e.g. "developer", "Tischler"). Leave empty to browse all jobs. |
| `location` | string | — | City or district name. |
| `region` | enum | — | Austrian federal state (Wien, Tirol, Salzburg, etc.). |
| `operationArea` | enum | — | Professional field: IT, Finance, Healthcare, Logistics, etc. (17 sectors). |
| `employmentMode` | enum | — | Vollzeit, Teilzeit, Freiberuflich, or Geringfügig. |
| `position` | enum | — | Mitarbeiter:in, Management, Praktikum, Lehre, Traineeship, etc. |
| `companyType` | enum | — | Direct employer or staffing agency (Personaldienstleister). |
| `timeLimit` | enum | — | Last 24 hours, last 3 days, or last week. |
| `maxResults` | integer | `50` | Maximum total results (0 = unlimited). |
| `includeDetails` | boolean | `true` | Fetch full job details (contact info, company profile, language skills, remote work). |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only — for AI-agent/MCP workflows. |
| `incrementalMode` | boolean | `false` | Only output new/changed jobs since last run. |
| `stateKey` | string | — | Stable identifier for tracked universe (e.g. query + region combo). |

---

## Output fields (34 total)

Each result includes: `jobId`, `title`, `company`, `companyId`, `companyLogoUrl`, `companyType`, `companyIndustry`, `companyUrl`, `companyAddress`, `companyEmployeeCount`, `companyFoundingYear`, `location`, `federalState`, `country`, `salary`, `salaryTimeFrame`, `salaryText`, `overpay`, `employmentModes`, `positionLevel`, `employmentTime`, `remoteWorkPossible`, `description`, `languageSkills`, `contactName`, `contactEmail`, `contactSex`, `applyUrl`, `portalUrl`, `postedDate`, `lastModifiedDate`, `expiryDate`, `scrapedAt`, `source`.

---

## FAQ

**What data does willhaben.at expose?**
Willhaben.at is Austria's largest classifieds platform. The jobs section has 15,000+ active listings with rich structured data including salary (mandatory by Austrian law), full job descriptions, company profiles, and direct recruiter contact info.

**How does the salary data work?**
Austrian law requires employers to disclose the minimum salary for each job posting based on the applicable collective agreement (Kollektivvertrag). The `salary` field contains this amount, `salaryTimeFrame` tells you if it's monthly or yearly, and `overpay` indicates if the employer explicitly offers above the minimum.

**How fast is it?**
Search returns results in ~400ms, detail enrichment in ~170ms per job. A run with 50 results and full detail enrichment typically completes in under 15 seconds.

**Does it need a proxy?**
No. Willhaben.at has no WAF or anti-bot protection on its job pages. The actor makes direct HTTP requests — no proxy, no Scrape.do, no browser needed. This keeps costs minimal.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Use `stateKey` to scope state per query/region combination.

**What is compact mode?**
Compact mode outputs only the core fields (ID, title, company, location, salary, employment type, URL, posted date) — ideal for AI agents and MCP servers that need structured data without exceeding token budgets.

---

## Known limitations

- **Austria only** — willhaben.at operates exclusively in Austria. For German jobs, use our [Arbeitsagentur Jobs Feed](https://github.com/BlackFalconData-org/arbeitsagentur-jobs-feed).
- **No application tracking** — the actor extracts listings but does not submit applications or track application status.
- **Description in SERP** — only promoted/TopJob listings include full descriptions in search results. For all other jobs, enable `includeDetails: true` to get the full description from the detail page.
- **Contact info varies** — not all employers provide direct contact details. Staffing agencies typically include recruiter contact, while direct employers may use willhaben's built-in application form.
- **German language** — all job content is in German (occasionally English for international roles). Field names and filter labels are in German.


## Output fields

Every listing returns the same 34-field schema. Missing values are `null` — never omitted.

- `jobId`
- `title`
- `company`
- `companyId`
- `companyLogoUrl`
- `companyType`
- `companyIndustry`
- `companyUrl`
- `companyAddress`
- `companyEmployeeCount`
- `companyFoundingYear`
- `location`
- `federalState`
- `country`
- `salary`
- `salaryTimeFrame`
- `salaryText`
- `overpay`
- `employmentModes`
- `positionLevel`
- `employmentTime`
- `remoteWorkPossible`
- `description`
- `languageSkills`
- `contactName`
- `contactEmail`
- `contactSex`
- `applyUrl`
- `portalUrl`
- `postedDate`
- `lastModifiedDate`
- `expiryDate`
- `scrapedAt`
- `source`


## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "jobId": "1cbe0671c6cf4badbdc95e2abc748a90363d55abc387f6d10d9b1304491951d8",
  "title": "Lehrling Land- und Baumaschinentechnik - Schwerpunkt Landmaschinen (m/w/d)",
  "company": "Unser Lagerhaus Warenhandelsgesellschaft m.b.H.",
  "companyId": 42716,
  "companyLogoUrl": "https://www.willhaben.at/jobs/api/v1/images/public/18692650?resolution=480",
  "companyType": "Firma",
  "companyIndustry": null,
  "companyUrl": "https://karriere.lagerhaus.at/unser-lagerhaus/",
  "companyAddress": "Südring 240, 9020, Klagenfurt, Österreich",
  "companyEmployeeCount": null,
  "companyFoundingYear": null,
  "location": "Eberndorf"
}
```

*Truncated — full records contain 34 fields. See Output fields for the complete schema.*


**[Try Willhaben Job Scraper - Austria’s Largest Job Portal now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/willhaben-scraper?fpr=1h3gvi)**


## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.002 |

See the [actor on Apify](https://apify.com/blackfalcondata/willhaben-scraper?fpr=1h3gvi) for current pricing.

---

## Related products by Black Falcon Data





- [StepStone Scraper](https://apify.com/blackfalcondata/stepstone-scraper?fpr=1h3gvi) — Job listings from 18 European portals
- [Indeed Job Scraper](https://apify.com/blackfalcondata/indeed-job-scraper?fpr=1h3gvi) — Indeed job listings with salary data
- [Glassdoor Job Scraper](https://apify.com/blackfalcondata/glassdoor-job-scraper?fpr=1h3gvi) — Glassdoor listings with company ratings
- [Arbeitsagentur Scraper](https://apify.com/blackfalcondata/arbeitsagentur-scraper?fpr=1h3gvi) — Germany's official job portal (1M+ listings)
- [SEEK Scraper](https://apify.com/blackfalcondata/seek-scraper?fpr=1h3gvi) — Australia & NZ's largest job board
- [Naukri Scraper](https://apify.com/blackfalcondata/naukri-scraper?fpr=1h3gvi) — India's largest job portal


## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. [Sign up free](https://console.apify.com/sign-up?fpr=1h3gvi) — $5 credit included
2. Open the actor and paste your input
3. Click Start — results download as JSON, CSV, or Excel

Need more volume? [See pricing](https://apify.com/pricing?fpr=1h3gvi).

---


## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---
---

*Last updated: 2026-03*
