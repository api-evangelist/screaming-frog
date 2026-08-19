---
name: screaming-frog-technical-seo-audit
description: Run a full technical SEO crawl of a site with the Screaming Frog SEO Spider MCP server and produce a prioritised issue report without blowing the context window.
api: screaming-frog:seo-spider
surface: mcp
tools:
  - sf_crawl
  - sf_crawl_progress
  - sf_list_available_reports
  - sf_generate_report
  - sf_list_available_filters_for_seo_element
  - sf_export_seo_element_urls
  - sf_write_text_file
  - sf_list_allowed_base_directory
generated: '2026-08-13'
method: generated
source: https://www.screamingfrog.co.uk/guides/mcp-server/
---

# Technical SEO audit with the Screaming Frog MCP

Every tool name below is transcribed from Screaming Frog's published MCP API reference.
Do not invent tool names or category strings — discover them.

## Before you start

- The MCP server is a **paid licence feature**. It will not run on the free SEO Spider.
- It requires SEO Spider **v24+** in **database storage mode**, running on this machine.
  There is no hosted endpoint; if the app is not installed and licensed, stop and say so.
- Call `sf_list_allowed_base_directory` first. Every `file_path` and `path` you pass is
  relative to that directory and nothing outside it is writable.

## Steps

1. **Crawl.** Call `sf_crawl` with `crawl_url`. Supply `crawl_name` and `project_name` if
   the user will want to compare against this crawl later — the comparison flow keys off
   `project_name`. Pass `config_path` only if the user has a saved `.seospiderconfig`;
   anything not exposed as a tool parameter (exclude rules, JavaScript rendering) can
   only be set that way.
2. **Wait.** Poll `sf_crawl_progress` rather than guessing. Use `sf_pause_crawl` /
   `sf_resume_crawl` if the user asks; `sf_clear_crawl` discards a paused crawl.
3. **Discover before you export.** Call `sf_list_available_reports` and read the real
   category names. Report paths are colon-delimited menu paths
   (`Category:Subcategory`) and they change between versions — never hard-code one you
   have not seen in a list response.
4. **Get the issue overview.** `sf_generate_report` with the Issues category returned in
   step 3. Pass `file_path` so the report lands on disk instead of in the conversation;
   the provider explicitly warns that crawl data overflows an LLM context window. Use
   `export_type: CSV` if a human will open it, `NDJSON` (the default) if you will process
   it.
5. **Drill into a specific element.** For each issue worth chasing, call
   `sf_list_available_filters_for_seo_element` with the element name (for example
   `Response Codes`, `Page Titles`, `Canonicals`, `Accessibility`), then
   `sf_export_seo_element_urls` with that `seo_element_name` and `filter_name`.
   Narrow the payload with `data_fields`, and page with `start_index` / `max_rows`.
6. **Report.** Write the prioritised findings with `sf_write_text_file`. It **overwrites
   without warning** — pick a fresh filename or read the directory first with
   `sf_list_directories`.

## Rules that are easy to get wrong

- **Never fill in a null.** The provider's own tool description says: "Fields with value
  null mean the information is unavailable. Do not guess or infer missing values."
- **There is no idempotency.** Re-running an export over the same `file_path` destroys the
  previous file. Nothing is safe to blindly retry.
- **Prefer `file_path` to inline content** on every export tool. Returning a full crawl
  inline is the single most common way this integration fails.
- **Priority is editorial.** Screaming Frog's own guidance is that AI should not replace
  an experienced SEO; the tool gives you issue counts, not business impact. Say which
  ranking you applied and why.
