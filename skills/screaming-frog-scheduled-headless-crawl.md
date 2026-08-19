---
name: screaming-frog-scheduled-headless-crawl
description: Set up an unattended, headless Screaming Frog SEO Spider crawl from the command line — the automation path for CI, cron and scheduled audits, including crawl-over-crawl comparison.
api: screaming-frog:seo-spider
surface: cli
operations:
  - --crawl
  - --headless
  - --save-crawl
  - --project-name
  - --task-name
  - --output-folder
  - --timestamped-output
  - --export-tabs
  - --bulk-export
  - --save-report
  - --export-format
  - --config
  - --auth-config
  - --project-crawl-comparison
  - --email-on-complete
generated: '2026-08-13'
method: generated
source: https://www.screamingfrog.co.uk/seo-spider/user-guide/general/#command-line
---

# Scheduled headless crawl

The SEO Spider has no REST API. The CLI **is** the integration surface, and it runs the
full product headless. Every flag below appears verbatim in Screaming Frog's command line
options documentation.

## One-time host setup

A headless machine has no UI to click through, so three things must exist on disk before
the first run:

1. `licence.txt` in the `.ScreamingFrogSEOSpider` directory — username on line one,
   licence key on line two.
2. `eula.accepted=15` in `spider.config` (the number tracks the EULA version and may need
   raising).
3. Memory allocation. On Linux, put `-Xmx8g` (or whatever the box can spare) in
   `~/.screamingfrogseospider`. Add `embeddedBrowser.enable=false` to `spider.config` for
   a truly headless box.

Storage mode defaults to `database`, which is what crawl comparison and the MCP server
both require. Only set `storage.mode=MEMORY` if you know you want it.

## The crawl

```
screamingfrogseospider \
  --crawl https://www.example.com \
  --headless \
  --save-crawl \
  --project-name "Example" \
  --task-name "weekly" \
  --output-folder /var/seo/example \
  --timestamped-output \
  --export-format csv \
  --export-tabs "Internal:All,Response Codes:Client Error (4xx)" \
  --bulk-export "Response Codes:Internal & External:Client Error (4xx) Inlinks" \
  --save-report "Redirects:All Redirects"
```

Binary names differ by platform: `screamingfrogseospider` on Linux,
`ScreamingFrogSEOSpiderCli.exe` on Windows (a **separate console build** — not
`ScreamingFrogSEOSpider.exe`), and the `ScreamingFrogSEOSpiderLauncher` script or
`open ... --args` on macOS.

## Week-over-week comparison

Set `--project-name` consistently, then on later runs add:

```
--project-crawl-comparison "true"
```

which auto-compares the last two crawls in that project. To compare two specific crawls,
use `--crawl-comparison <database Id> <database Id>`; list the IDs with `--list-crawls`.

## Anything the flags do not cover

Most configuration has **no** command line flag — exclude rules, JavaScript rendering,
crawl limits and so on. Set them once in the UI, save the profile, and pass it:

```
--config "/etc/seo/site.seospiderconfig"
--auth-config "/etc/seo/site.seospiderauthconfig"
```

## Failure modes worth pre-empting

- **Silent no-output.** If `--output-folder` does not exist, or exists with files and you
  passed neither `--overwrite` nor `--timestamped-output`, the crawl runs and exports
  nothing. Create the folder in the job, or always use `--timestamped-output`.
- **Escaped quotes.** Do not end a quoted argument with a backslash — it escapes the quote.
- **JavaScript rendering on Linux** needs a display. Run under a virtual frame buffer
  (Xvfb) if the crawl requires rendering.
- **Third-party API flags consume the customer's own quota.** `--use-pagespeed`,
  `--use-ahrefs`, `--use-majestic`, `--use-mozscape`, `--use-google-analytics-4`,
  `--use-google-search-console` call other companies' APIs with keys stored locally in
  `spider.config` or, for the Google OAuth ones, credential folders copied from a machine
  that has a UI. Screaming Frog neither meters nor bills these.
- `--email-on-complete <address>` is the only built-in notification.
