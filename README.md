# Willhaben Jobs Feed

Extract structured job listings from [willhaben.at](https://willhaben.at) — Austria's largest classifieds platform with 15,000+ active job postings. Full salary data (mandatory in Austria), direct contact info, company profiles, and 7 search filters.

**[Run on Apify →](https://apify.com/blackfalcondata/willhaben-scraper)**

---

## Key features

🔍 **Smart search with 7 filters**

Search by keyword, location, Bundesland, professional field, employment type, position level, company type, and posting recency. Browse all 15,000+ jobs without a keyword.

💰 **Austrian salary transparency**

Austria requires employers to disclose minimum salary. Every listing includes salary amount, time frame (monthly/yearly), and an overpay indicator when the company pays above the collective agreement.

📄 **Full detail enrichment**

Fetch contact name and email, company industry, address, employee count, founding year, language requirements, remote work availability, and contract expiry date.

🔄 **Incremental mode**

Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

⚡ **Compact output for AI agents**

Core-fields-only mode optimized for MCP and AI agent workflows. Description truncation to control output size.

---

## Use cases

**Recruiting and talent intelligence**
Monitor Austrian job market trends by role, region, or sector. Track which companies are hiring, what salaries they offer, and how quickly positions get filled.

**Salary benchmarking**
Austria's mandatory salary disclosure makes willhaben.at a goldmine for compensation research. Compare salaries by region, sector, position level, and company size.

**Lead generation for HR tech**
Identify companies with open positions and extract direct contact details. Filter by company type to focus on direct employers vs. staffing agencies.

**Market research for Austria**
Track employment trends across Austria's 9 Bundesländer. Analyze which sectors are growing, which regions have labor shortages, and how remote work adoption is progressing.

**AI and LLM workflows**
Use compact mode and description truncation to feed structured Austrian job data into AI agents, MCP servers, and LLM pipelines without exceeding token budgets.

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

---

## Related products by Black Falcon Data

| Product | Description |
|:--------|:------------|
| [StepStone Jobs API](https://github.com/BlackFalconData-org/stepstone-jobs-api) | Job listings from 18 European portals |
| [Company Jobs Tracker](https://github.com/BlackFalconData-org/company-jobs-tracker-api) | Track new/removed jobs per company |
| [Indeed Jobs Feed](https://github.com/BlackFalconData-org/indeed-jobs-feed) | Indeed job listings with salary data |
| [Glassdoor Jobs Feed](https://github.com/BlackFalconData-org/glassdoor-jobs-feed) | Glassdoor listings with company ratings |
| [Arbeitsagentur Jobs Feed](https://github.com/BlackFalconData-org/arbeitsagentur-jobs-feed) | Germany's federal job portal (1M+ listings) |
| [Naukri Jobs Feed](https://github.com/BlackFalconData-org/naukri-jobs-feed) | India's largest job portal |
| [Bilbasen Scraper](https://github.com/BlackFalconData-org/bilbasen-scraper) | Denmark's largest car marketplace |

---

*Last updated: 2026-03*
