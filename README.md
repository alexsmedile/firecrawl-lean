<p align="center">
  <strong>firecrawl-lean</strong>
</p>

<p align="center">
  <img src="./docs/assets/firecrawl-lean-mark.svg" alt="firecrawl-lean mark" width="120" height="120">
</p>

<h1 align="center">Token-efficient Firecrawl skill for Coding Agents.</h1>

<p align="center">
  Web search, scrape, crawl, and extract — ~60% fewer tokens than the official skill.
</p>

<p align="center">
  <img alt="claude-code" src="https://img.shields.io/badge/claude--code-skill-111111">
  <img alt="token-efficient" src="https://img.shields.io/badge/tokens-~60%25_fewer-ff4d00">
  <img alt="firecrawl" src="https://img.shields.io/badge/powered_by-firecrawl-222222">
  <img alt="license" src="https://img.shields.io/badge/license-MIT-444444">
</p>

---

## What this is

A single-file [Claude Code skill](https://code.claude.com/docs/en/skills) that gives Claude the full Firecrawl CLI — web search, scraping, crawling, structured extraction, browser automation — in a form optimized for token efficiency.

The [official Firecrawl skill set](https://www.firecrawl.dev/skills) ships as **5 separate skills** (`firecrawl`, `firecrawl-crawl`, `firecrawl-agent`, `firecrawl-instruct`, `firecrawl-browser`). Each loads independently, repeats shared context, and costs tokens every time Claude reads them. This skill merges all five into one lean reference.

## Why lean matters

Every skill file loads into Claude's context window on each turn. Bloated skills = wasted tokens = higher cost + slower responses.

| | Official (5 skills) | firecrawl-lean |
|---|---|---|
| **Files loaded** | 5 | 1 |
| **Estimated tokens** | ~2,400 | ~900 |
| **Token reduction** | — | **~60%** |
| **Commands covered** | search, scrape, map, crawl, download, interact, agent, browser | same |
| **Install** | `firecrawl init` (per-agent) | one file |

> Fewer tokens in context = more room for your actual task.

## Install

```bash
npx skills add https://github.com/alexsmedile/firecrawl-lean
```

Requires [Firecrawl CLI](https://github.com/firecrawl/cli):

```bash
npm install -g firecrawl-cli && firecrawl login
```

## What's covered

| Command | What it does |
|---------|-------------|
| `search` | Web search — no URL needed. News, geo-targeted, full-content modes. |
| `scrape` | Fetch a URL. Markdown, HTML, screenshot, or JSON schema output. |
| `map` | Discover all URLs within a site. Sitemap + subdomain support. |
| `crawl` | Bulk-extract a site section. Depth, delay, path filters. |
| `download` | Save a site as local files. |
| `interact` | NL-prompted browser actions — click, fill, login. |
| `agent` | AI-powered structured extraction with schema + seed URLs. |
| `browser` | CDP/Playwright code execution on a live session. |

## Escalation pattern

```
search → scrape → map + scrape → crawl → interact / browser / agent
```

Start cheap. Escalate only when needed. The skill tells Claude when and why to step up.

## Token efficiency techniques

- **One file, no repetition** — shared context (output rules, auth, credits) written once
- **Examples over prose** — commands show usage; no paragraphs explaining what `--limit` does
- **Advanced commands stubbed** — crawl/interact/agent/browser are present but not padded; install dedicated skills only if you need them
- **Output rules baked in** — Claude always writes to `.firecrawl/` and never dumps raw output to context

## Comparison with official skills

The official Firecrawl skills are excellent and full-featured. This skill is for users who:

- Run Claude Code frequently and care about context window cost
- Want a single install, not five
- Don't need the extended documentation — just the commands

If you need deep crawl configuration, browser profiles, or parallel agent jobs, the [official skills](https://www.firecrawl.dev/skills) have more detail.

## Credits

Built on top of [Firecrawl](https://firecrawl.dev) and the [Firecrawl CLI](https://github.com/firecrawl/cli). This skill is an independent, community-maintained efficiency wrapper — not affiliated with Firecrawl.
