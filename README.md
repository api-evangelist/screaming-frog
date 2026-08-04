# Screaming Frog

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Screaming Frog is a UK-based SEO software company and search marketing agency, best known for the SEO Spider website crawler tool used by SEO professionals worldwide to perform technical SEO audits, crawl analysis, and integrations with third-party APIs. The SEO Spider integrates with Google Analytics (GA4), Google Search Console, PageSpeed Insights, Ahrefs, Majestic, Moz, OpenAI, Gemini, Ollama, and Anthropic.

**Website:** https://www.screamingfrog.co.uk/
**APIs.yml:** https://raw.githubusercontent.com/api-evangelist/screaming-frog/refs/heads/main/apis.yml

## Scope

- **Type:** Company
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags

- SEO
- Search Engine Optimization
- Website Crawler
- Technical Audit
- Marketing
- Analytics

## Tools

| Tool | Description |
|------|-------------|
| [SEO Spider](https://www.screamingfrog.co.uk/seo-spider/) | Desktop website crawler for technical SEO audits with third-party API integrations |
| [Log File Analyser](https://www.screamingfrog.co.uk/log-file-analyser/) | Free tool for analyzing server log files to understand search bot crawl behavior |

## Third-Party API Integrations

The SEO Spider integrates with the following external APIs:

| Integration | Purpose |
|-------------|---------|
| Google Analytics (GA4) | Session, user, and conversion metrics alongside crawl data |
| Google Search Console | Impressions, clicks, CTR, average position, and URL inspection |
| PageSpeed Insights | Core Web Vitals and Lighthouse performance scores |
| Ahrefs | Domain Rating, URL Rating, and backlink metrics |
| Majestic | Trust Flow, Citation Flow, and link data |
| Moz | Domain Authority and link metrics |
| OpenAI | AI-powered content analysis and categorization |
| Gemini | Google's AI integration for content analysis |
| Anthropic | Claude AI integration for content analysis |
| Ollama | Local LLM integration for offline AI processing |

## Artifacts

### JSON Schema

| File | Description |
|------|-------------|
| [json-schema/screaming-frog-crawl-result-schema.json](json-schema/screaming-frog-crawl-result-schema.json) | Schema for URL crawl results from the SEO Spider |

### JSON Structure

| File | Description |
|------|-------------|
| [json-structure/screaming-frog-crawl-result-structure.json](json-structure/screaming-frog-crawl-result-structure.json) | Field-level documentation for crawl result data |

### JSON-LD

| File | Description |
|------|-------------|
| [json-ld/screaming-frog-context.jsonld](json-ld/screaming-frog-context.jsonld) | JSON-LD context mapping SEO crawl data to schema.org vocabulary |

### Examples

| File | Description |
|------|-------------|
| [examples/screaming-frog-crawl-result-example.json](examples/screaming-frog-crawl-result-example.json) | Example crawl result record for a successfully crawled page |

### Vocabulary

| File | Description |
|------|-------------|
| [vocabulary/screaming-frog-vocabulary.yml](vocabulary/screaming-frog-vocabulary.yml) | SEO and crawl domain vocabulary |

## Links

- **Website:** https://www.screamingfrog.co.uk/
- **SEO Spider Documentation:** https://www.screamingfrog.co.uk/seo-spider/user-guide/
- **Pricing:** https://www.screamingfrog.co.uk/seo-spider/pricing/
- **Blog:** https://www.screamingfrog.co.uk/blog/
- **Release History:** https://www.screamingfrog.co.uk/seo-spider/release-history/

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
