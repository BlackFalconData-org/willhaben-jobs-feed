# Willhaben Jobs Feed

Extract structured job listings from [willhaben.at](https://willhaben.at) — Austria's largest classifieds platform with 15,000+ active job postings. Full salary data (mandatory in Austria), direct contact info, company profiles, and 7 search filters.

**[Willhaben Scraper on Apify →](https://apify.com/blackfalcondata/willhaben-scraper)**

---

## Key features



**Search with filters** — Search by keyword and location. Filter by region (bundesland), field / sector, employment type, and more.

**Detail enrichment** — Fetch full job descriptions, salary data, employer profiles, contact information for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

---

## Use cases



**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from willhaben.at on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from willhaben.at.

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



- [StepStone Scraper](https://github.com/BlackFalconData-org/stepstone-scraper) — Job listings from 18 European portals
- [Indeed Job Scraper](https://github.com/BlackFalconData-org/indeed-job-scraper) — Indeed job listings with salary data
- [Glassdoor Job Scraper](https://github.com/BlackFalconData-org/glassdoor-job-scraper) — Glassdoor listings with company ratings

---

*Last updated: 2026-03*
