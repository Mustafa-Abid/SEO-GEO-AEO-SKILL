# SEO / GEO / AEO Audit Skill for Claude

A production-grade Claude AI skill that audits any website for 
Search Engine Optimisation (SEO), AI Search visibility (GEO), 
and Answer Engine Optimisation (AEO).

Produces a single-file HTML dashboard with animated score gauges, 
a radar chart, priority action cards, and plain-language findings — 
in under 2 minutes.

---

## What it does

- Scores your site across three pillars: SEO, GEO (Perplexity / 
  ChatGPT Search / Google AI Overviews), and AEO (featured snippets / 
  voice search)
- Detects JavaScript-rendered (SPA) sites and runs a five-method 
  recovery protocol to find pages hidden behind JS rendering
- Distinguishes between pages that are missing vs. pages that exist 
  but are unreadable by search engines — a critical difference most 
  audit tools miss
- Generates a downloadable HTML dashboard with animated gauges, 
  radar chart, and prioritised action roadmap
- Sources evidence from sitemaps, search index, Companies House, 
  Google Business Profile, and third-party listings when the site 
  itself is not crawlable

## Quick Audit vs Extensive Audit

| | Quick | Extensive |
|---|---|---|
| Time | ~2 mins | ~5–10 mins |
| Pages crawled | Up to 5 | Up to 11 |
| Output | Top 5 priorities + scores | Full roadmap + technical deep-dive |
| Best for | First look / client prospecting | Full client deliverable |

---

## How to use it

This is a Claude skill — it runs inside Claude.ai or via the 
Claude API.

### Option 1 — Claude.ai (no code)

1. Copy the contents of `SKILL.md`
2. Paste it into a new Claude conversation as a system prompt, 
   or save it as a Project instruction in Claude.ai
3. Send a message: `audit [website URL]`
4. Claude will ask Quick or Extensive, then run the audit

### Option 2 — Claude API

Use the contents of `SKILL.md` as your system prompt. 
Send the target URL as the user message.

---

## Example output

See `examples/1drivemotors-quick-audit.html` for a real audit 
output. Open in any browser.

---

## Compatibility

Tested on Claude Sonnet. Works best with models that support 
web search and web fetch tools.

---

## Version history

| Version | What changed |
|---|---|
| v7.1 | Token-efficient escalating SPA recovery, fetch budget caps, 
         conditional third-party lookup |
| v7.0 | SPA Recovery Protocol, three-state page classification |
| v6.0 | Initial dashboard release |

---

## Built by

Mustafa — consultant and builder. 
[Website](https://mustafa-a.netlify.app/)

---

## License

MIT — free to use, modify, and share. Credit appreciated.
