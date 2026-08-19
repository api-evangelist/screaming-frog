---
name: screaming-frog-broken-link-report
description: Export every broken internal and external link from a Screaming Frog crawl together with the pages that link to them, so the output is a fix list rather than a URL list.
api: screaming-frog:seo-spider
surface: mcp
tools:
  - sf_list_crawls
  - sf_load_crawl
  - sf_list_available_bulk_exports
  - sf_generate_bulk_export
  - sf_list_available_filters_for_seo_element
  - sf_export_seo_element_urls
  - sf_url_links
generated: '2026-08-13'
method: generated
source: https://www.screamingfrog.co.uk/guides/mcp-server/
---

# Broken link report

A list of 404s is not actionable. A list of 404s **with their inlinks** is. That
distinction is why this flow uses a bulk export rather than a tab export.

## Steps

1. **Find the crawl.** `sf_list_crawls` returns the most recent crawls, newest first
   (`limit` defaults to 10). Take the `crawl_id` the user means and call `sf_load_crawl`.
   If no crawl exists yet, run the audit skill first — do not start a crawl silently.
2. **Discover the export.** `sf_list_available_bulk_exports`. You are looking for the
   Response Codes client-error inlinks export; the documented example of the path shape is
   `Response Codes:Internal & External:Client Error (4xx) Inlinks`. Use the string the
   list tool actually returned, not the example.
3. **Export.** `sf_generate_bulk_export` with that `category` and a `file_path`. Each row
   pairs a broken destination with a source page that links to it, which is the shape a
   developer can work from.
4. **Add the response-code context.** `sf_list_available_filters_for_seo_element` for
   `Response Codes`, then `sf_export_seo_element_urls` for the client-error and
   server-error filters. Restrict `data_fields` to what the fix needs — typically address,
   status code, status, and inlink count.
5. **Spot-check one URL.** `sf_url_links` with `links_direction: inlinks` and
   `links_category: Hyperlink` shows exactly where a single broken URL is referenced.
   Use this to sanity-check the bulk export before handing it over.

## Notes

- `links_category` is an enum with 29 values. `Hyperlink` is what people mean by "link";
  `Image`, `CSS`, `JavaScript`, `Redirect`, `XML Sitemap`, `Iframe`, `PDF` and the rest are
  separate categories and will not appear if you only ask for hyperlinks. If the user asks
  about broken images, ask for `Image`.
- External 4xx and internal 4xx are different filters. Report them separately: internal
  breakage is the site owner's to fix, external breakage is a link to someone else's site
  that has rotted.
- The same job runs headless from the CLI with
  `--bulk-export "Response Codes:Internal & External:Client Error (4xx) Inlinks"`;
  see `cli/screaming-frog-cli.yml` if the user wants this on a schedule instead.
