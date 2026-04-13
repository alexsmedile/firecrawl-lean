---
name: firecrawl-lean
description: |
  Web scraping, search, crawling, and browser interaction via Firecrawl CLI. Use when user wants to search the web, fetch a URL, scrape content, crawl docs, download a site, extract structured data, or interact with JS-heavy/login-gated pages. Triggers on: "search for", "scrape", "fetch this URL", "get the page", "crawl", "download the site", "extract structured data", "click the button", "fill the form", "log in to". Do NOT use for local files, git, or code editing.
  For advanced use (bulk crawl, AI agent extraction, browser profiles, parallel jobs): install firecrawl-crawl, firecrawl-agent, or firecrawl-instruct skills.
allowed-tools:
  - Bash(firecrawl *)
  - Bash(npx firecrawl *)
---

# Firecrawl CLI — lean reference

Check status: `firecrawl --status` (shows auth + credits + concurrency limit).
Not installed? Run: `npm install -g firecrawl-cli && firecrawl auth`
Auth commands: `firecrawl login`, `firecrawl logout`, `firecrawl view-config`

## WebFetch vs Firecrawl

Try WebFetch first (free). Use Firecrawl when:
- Need web search (WebFetch can't search)
- JS-heavy SPA / marketing pages
- Full article content (WebFetch truncates long pages)
- WebFetch output < 1000 chars or looks incomplete

## Escalation pattern

```
search → scrape → map+scrape → crawl* → interact*
```
`*` = advanced, install dedicated skill if needed

## Commands

**Search** — no URL yet, find pages:
```bash
firecrawl search "query" --limit 5 -o .firecrawl/results.json --json
firecrawl search "query" --scrape -o .firecrawl/results.json --json        # include full content
firecrawl search "query" --sources news --tbs qdr:d --json                 # news, past day
firecrawl search "query" --country US --location "New York" --json         # geo-targeted
```

**Scrape** — have a URL:
```bash
firecrawl scrape "<url>" -o .firecrawl/page.md
firecrawl scrape "<url>" --only-main-content -o .firecrawl/page.md        # strip nav/footer
firecrawl scrape "<url>" --wait-for 3000 -o .firecrawl/page.md            # wait for JS
firecrawl scrape "<url>" --query "What is the enterprise price?"            # targeted Q (5 credits)
firecrawl scrape "<url>" --format markdown,html,screenshot -o .firecrawl/page.md  # multi-format
firecrawl scrape "<url>" --format json --schema '{"price":"string"}' -o .firecrawl/data.json  # structured
```

**Map** — find URLs within a site:
```bash
firecrawl map "<url>" --search "authentication" -o .firecrawl/urls.txt
firecrawl map "<url>" --limit 200 --json -o .firecrawl/urls.json
firecrawl map "<url>" --include-subdomains --sitemap -o .firecrawl/urls.json
```

**Crawl** — bulk extract site section (advanced — install `firecrawl-crawl` for full docs):
```bash
firecrawl crawl "<url>" --include-paths /docs --limit 50 --wait -o .firecrawl/crawl.json
firecrawl crawl "<url>" --max-depth 3 --delay 500 --exclude-paths /blog --wait -o .firecrawl/crawl.json
```

**Download** — save site as local files (advanced):
```bash
firecrawl download "<url>" --limit 20 -y -o .firecrawl/site/
```

**Interact** — click/fill forms after scrape (advanced — install `firecrawl-instruct` for full docs):
```bash
firecrawl scrape "<url>"
firecrawl interact --prompt "Click the login button"
firecrawl interact stop
```

**Agent** — AI-powered structured extraction (advanced — install `firecrawl-agent` for full docs):
```bash
firecrawl agent "extract all pricing tiers" --wait -o .firecrawl/data.json
firecrawl agent "extract pricing" --urls "<url>" --schema '{"tiers":[]}' --wait -o .firecrawl/data.json
```

**Browser** — CDP/Playwright-style automation (distinct from `interact`):
```bash
firecrawl browser launch-session "<url>"
firecrawl browser execute --code "document.querySelector('button').click()"
firecrawl browser list
firecrawl browser close <session-id>
```

## Output rules

- Always write to `.firecrawl/` with `-o` to avoid context bloat
- Add `.firecrawl/` to `.gitignore`
- Always quote URLs (`?` and `&` are shell special chars)
- Never read entire output at once: `head -50 file.md` or `grep "keyword" file.md`
- Naming: `.firecrawl/search-{query}.json`, `.firecrawl/{site}-{path}.md`

## Extract URLs from JSON results

```bash
jq -r '.data.web[].url' .firecrawl/search.json
```

## Credits

```bash
firecrawl credit-usage
```
