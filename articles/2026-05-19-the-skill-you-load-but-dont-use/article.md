---
title: "The skill you loaded but didn't use still cost you"
type: comparison
date: 2026-05-19
status: draft
---

# The skill you loaded but didn't use still cost you

A Claude Code skill isn't free to have installed. Every auto-loaded skill sits in
the context window on *every* turn — whether or not the current task touches it.
You feel it as a tool you reach for occasionally. The model feels it as text it
re-reads constantly.

That gap is easy to miss because the cost is invisible. The skill works, the task
gets done, nothing looks wrong. But the tokens a skill occupies are tokens your
actual task can't use, and they're spent on every turn of every conversation the
skill is installed in — most of which never call it.

This is a comparison of two ways to give Claude the Firecrawl CLI: the official
skill set, and a leaner alternative. The CLI is identical. What differs is what
it costs you to keep loaded.

## The two options

[Firecrawl](https://firecrawl.dev) ships its Claude Code integration as **five
separate skills** — `firecrawl`, `firecrawl-crawl`, `firecrawl-agent`,
`firecrawl-instruct`, `firecrawl-browser`. Each is a full skill file. Each loads
independently. Each repeats the shared context the others also need: the auth
commands, the output rules, the credits notes.

**firecrawl-lean** is one file. It merges all five — search, scrape, map, crawl,
download, interact, agent, browser — into a single reference, written once.

| | Official (5 skills) | firecrawl-lean |
|---|---|---|
| Files loaded into context | 5 | 1 |
| Estimated tokens | ~2,400 | ~900 |
| Token reduction | — | **~60%** |
| Commands covered | search, scrape, map, crawl, download, interact, agent, browser | same |
| Install | `firecrawl init`, per-agent | one file |

Same commands. Same CLI underneath. ~1,500 tokens of difference, paid on every
turn.

## Where the tokens go

~60% is a big number for "the same thing." It's worth being precise about where
it comes from, because it's not compression tricks — it's three deliberate
choices about how a skill file is written.

**Shared context, written once.** Five skills that each need the auth block, the
`.firecrawl/` output rule, and the credits note carry five copies of all three.
One file carries one copy. Most of the saving is just de-duplication.

**Examples instead of prose.** A skill file is read by a model, not a person. The
model does not need a paragraph explaining what `--limit` does — it needs to see
`firecrawl search "query" --limit 5`. firecrawl-lean shows commands and skips the
explanations. The official skills read like documentation; documentation is
written for humans and pays a human-readability tax the model doesn't need.

**Advanced commands stubbed, not padded.** Crawl, interact, agent, and browser
are all present — every command works — but they're not expanded into pages of
configuration detail. The skill notes that dedicated skills exist if you need the
depth, and stays lean for the 90% case that doesn't.

## What you give up

A comparison that only lists wins isn't a comparison. The official skills are
genuinely more thorough, and for some users that thoroughness is the right call.

If you run parallel agent extraction jobs, configure browser profiles, or need
deep crawl tuning — depth limits, path filters, delay strategies as a routine
thing — the official skills document all of it and firecrawl-lean does not. It
keeps those commands runnable but deliberately under-documented, on the bet that
most people invoke them rarely and don't want to pay for the docs on every turn.

If you *do* invoke them constantly, the official set's extra detail is worth its
extra weight. firecrawl-lean is a bet about the common case, not a claim that
detail is useless.

## Why it matters

The failure mode is specific and quiet: a research session where Claude runs out
of room to think halfway through, and you never connect it to the skills sitting
in context. Thirty fetches into the work, the context window is tighter than it
should be — and ~1,500 tokens of that is five Firecrawl skill files, four of
which the session never called.

You don't notice it because nothing errors. The skill that's loaded but unused
doesn't announce its cost. It just quietly takes the room. firecrawl-lean is the
same capability with that room handed back — one file, ~900 tokens, every command
still there.

For deep crawl configuration and parallel jobs, the [official
skills](https://www.firecrawl.dev/skills) are the better fit. For everyone who
runs Claude Code often and wants the context window back:

```bash
npx skills add https://github.com/alexsmedile/firecrawl-lean
```
