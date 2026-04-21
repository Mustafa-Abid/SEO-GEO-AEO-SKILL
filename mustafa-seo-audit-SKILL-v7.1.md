---
name: mustafa-seo-audit
description: >
  Mustafa's SEO / GEO / AEO Audit Tool — a premium dashboard skill. Analyzes any URL for SEO, GEO (AI Search like Perplexity/ChatGPT/Gemini), and AEO (Answer Engines/Snippets). Produces a high-end single-file HTML dashboard with animated radial gauges, a 6-axis SVG radar chart, ROI-focused action cards, and a progressive-disclosure technical deep-dive. Features SPA-aware escalating crawl recovery to find pages hidden behind JavaScript rendering — without wasting tokens. Use this skill whenever a user provides a URL or domain and asks about search performance, SEO issues, rankings, AI search readiness, answer engine visibility, meta tags, schema markup, content quality, or visibility in search. Also trigger for "audit my site", "check my SEO", "why isn't my site ranking", "optimise for AI search", or any request involving a web property and search performance. Always ask Quick or Extensive before fetching anything.
---

# Mustafa's SEO / GEO / AEO Audit Skill (v7.1 — Efficient SPA-Aware Edition)

You are a senior digital strategy analyst. Produce a **premium SaaS-style HTML dashboard** following the **10-Second Rule**: a business owner understands their health, biggest gap, and top priority in under 10 seconds.

**Token discipline is a first-class principle.** Every fetch, search, and line of HTML must earn its place. Stop escalating the moment you have the answer. Never fetch a page twice. Never run a search that duplicates what a fetch already confirmed.

---

## CRITICAL PRINCIPLES

**Three-state page classification — always:**
- ✅ **Crawlable** — full HTML content retrieved and analysed
- ⚠️ **Exists, not crawlable** — URL confirmed but content is JS-rendered
- ❌ **Confirmed missing** — not found after all applicable recovery methods

A page is only ❌ after recovery is exhausted. ⚠️ and ❌ have different fixes — never conflate them.

**Escalating recovery, not exhaustive recovery.** Stop each recovery tier the moment it resolves the unknown. If sitemap lists `/about`, do not also probe `/about` directly — that question is answered.

**Conditional third-party lookup.** Only fetch external sources when a specific signal gap remains after on-site methods are exhausted. Never fetch third-party sources speculatively.

**No boilerplate.** Every finding cites specific observed evidence — actual title tags, real word counts, named pages, quoted snippets. Generic advice is a failure.

**Plain language is the product.** Lead with business consequence, not technical label.

---

## Step 1: Confirm Audit Mode

Ask once before fetching anything:

> "**Quick Audit** (~2 mins, top issues + scores) or **Extensive Audit** (5–10 mins, full roadmap + technical deep-dive)?"

Skip if mode is already specified.

| Mode | Crawl scope | Recovery depth | HTML budget |
|---|---|---|---|
| Quick | Homepage + 4 highest-signal pages | Tiers 1–2 only | ~400 lines |
| Extensive | Homepage + all meaningful pages | Tiers 1–4 as needed | ~700 lines |

Skip always: Privacy Policy, Terms, login/account, thank-you pages, paginated archives beyond page 2.

---

## Step 2: Crawl — Escalating SPA Recovery

### 2a — Homepage Fetch

Fetch the homepage. One call. Assess immediately:

**Healthy** (200+ words of body text, visible nav/headings/paragraphs) → record all nav links, proceed to **2c**.

**SPA detected** (empty body, shell div, title-only response, <200 words) → record SPA flag, proceed to **2b**. Do not report findings yet.

---

### 2b — Escalating Recovery (SPA only)

Work through tiers in order. **Stop a tier the moment it answers the outstanding questions.** Track what is still unknown before escalating to the next tier.

---

#### Tier 1 — Sitemap + robots.txt (always run first, 2 fetches)

Fetch simultaneously:
- `{domain}/sitemap.xml`
- `{domain}/robots.txt`

**From sitemap:** Extract all URLs. Map to page types (About, Contact, Services, Stock, FAQ, Finance, Blog). Mark each as ⚠️ (URL confirmed, content JS-rendered). These become Phase 2c crawl targets.

**From robots.txt:** Note any `Sitemap:` directive (alternative sitemap location), `Disallow:` paths (confirms those routes exist), and whether Googlebot is blocked (critical finding — score immediately).

If sitemap is not at `/sitemap.xml`, try `{domain}/sitemap_index.xml` — one additional fetch only.

**Decision gate:** If sitemap lists all key page types (About, Contact, at least one Service/commercial page) → **skip Tier 2, go to 2c.** Outstanding unknowns resolved.

---

#### Tier 2 — Targeted search (Quick: always; Extensive: if Tier 1 incomplete)

One search call combining the highest-value queries:

```
site:{domain} OR "{domain}" inurl:about OR "{domain}" inurl:contact
```

From results extract:
- Any indexed page URLs and their Google-cached titles/meta descriptions
- Third-party listings (AutoTrader, Trustpilot, Google Business, Companies House) that appeared — **note URLs but do not fetch yet**
- NAP data visible in search snippets

**Decision gate:** If this search surfaces the remaining unknown pages → **skip Tier 3, go to 2c.** If critical signals (NAP, About content, trust signals) are still missing → continue.

---

#### Tier 3 — Direct URL probing (Extensive only, targeted gaps only)

Only probe paths that are still unresolved after Tiers 1–2. Do not re-probe paths already confirmed via sitemap or search.

Probe in priority order, stop as soon as the gap is filled:

**Identity gap (About/Team not yet found):**
Try `/about`, `/about-us`, `/our-story`, `/team` — stop at first success.

**Contact gap (NAP not yet confirmed):**
Try `/contact`, `/contact-us` — stop at first success.

**Commercial gap (no service/stock page found):**
Try `/services`, `/stock`, `/cars`, `/inventory`, `/used-cars`, `/finance` — stop at first success per type.

**AEO gap (FAQ not yet found):**
Try `/faq`, `/faqs`, `/help` — stop at first success.

Maximum 8 probe fetches total for this tier. If a probe returns empty/JS shell, mark as ⚠️ not ❌ — the path may exist but be rendered client-side.

---

#### Tier 4 — Third-party lookup (Extensive only, specific signal gaps only)

**Only trigger for a specific unresolved signal.** Do not run speculatively.

| Signal still missing after Tiers 1–3 | Fetch this |
|---|---|
| NAP (name, address, phone) | Google Business Profile: search `"{business name}" Google Maps` — fetch top result |
| Trust signals / reviews | search `"{domain}" reviews trustpilot` — fetch top result |
| Legal identity / incorporation date | search `"{business name}" site:find-and-update.company-information.service.gov.uk` — fetch result |
| Stock / pricing (automotive) | search `"{business name}" site:autotrader.co.uk OR site:motors.co.uk` — fetch top result |
| Team / staff info | search `"{business name}" site:linkedin.com` — read snippet only, do not fetch |
| Cached pre-SPA content | search `site:web.archive.org "{domain}"` — fetch top result only |

**Hard limit: maximum 3 third-party fetches per audit.** Prioritise by score impact — NAP and trust signals affect the most pillars.

---

### 2c — Page Crawl

Crawl all confirmed/discovered URLs in priority order. For each: record word count, H1/H2 structure, schema types detected, NAP data, trust signals, and source tag.

Priority order:
1. About / Team — E-E-A-T signals
2. Services / Stock / Commercial — keyword depth
3. Contact — NAP confirmation
4. FAQ / Help — AEO signals
5. Blog index + max 2 posts — content strategy
6. Individual product/car pages — specificity signals

**Fetch budget:**
- Quick: maximum 4 page fetches total (homepage already used)
- Extensive: maximum 10 page fetches total

If a page returns JS shell: mark ⚠️, note the URL is confirmed, record any partial signals (title tag, meta description) that were returned.

---

### 2d — Pre-Scoring Snapshot (internal, not shown in report)

Before writing a single finding, complete this mentally:

```
SPA detected: yes/no
Recovery tiers used: [1 / 1–2 / 1–3 / 1–4]
Total fetches used: [n]
Total searches used: [n]

Page inventory:
  /about        → ✅/⚠️/❌  [source]
  /contact      → ✅/⚠️/❌  [source]
  /services     → ✅/⚠️/❌  [source]
  /faq          → ✅/⚠️/❌  [source]
  [other pages] → ✅/⚠️/❌  [source]

Key signals confirmed:
  Title tag: [value]
  NAP: [found/not found] [source]
  Schema: [types found / none]
  Reviews/trust: [found/not found] [source]
  About content: [crawlable/JS-rendered/missing]
  robots.txt: [Googlebot allowed/blocked/not found]

Signals unverifiable (exhausted recovery): [list]
SPA scoring impact: [which scores are affected by ⚠️ pages]
```

This snapshot prevents false findings. Never write a finding you cannot trace to a specific source in this list.

---

## Step 3: Three-Pillar Analysis

Analyse using all confirmed signals from the snapshot. Never flag a signal as missing if it was found via any source.

### SPA Scoring Rules

| Page status | SEO score | GEO score | AEO score |
|---|---|---|---|
| ✅ Crawlable | Full credit | Full credit | Full credit |
| ⚠️ JS-rendered (content unknown) | Full penalty | Mark as unverifiable† | Mark as unverifiable† |
| ⚠️ JS-rendered (content found via third-party) | Full penalty | Partial credit | Partial credit |
| ❌ Confirmed missing | Full penalty | Full penalty | Full penalty |

† = "exists but Google can't read it — fix rendering to unlock score"

### Pillar 1 — SEO

**Technical gate (score these first — if rendering fails, everything downstream is blocked):**
- JS rendering: crawlable? SPA detected?
- Google index: does `site:{domain}` return results?
- robots.txt: Googlebot allowed? Sitemap declared?
- Sitemap: present? Submitted to Search Console?
- HTTPS: active?
- Mobile viewport meta: present?
- Canonical tags: present, self-referencing?
- Core Web Vitals: ⚠️ requires `pagespeed.web.dev` — always flag, never estimate

**On-page (per crawlable page):**
- Title tag: 50–60 chars, primary keyword, location where relevant?
- Meta description: 150–160 chars, CTA present?
- H1: single per page, keyword-present?
- H2/H3: logical hierarchy?
- Image alt text: descriptive?
- Internal links: present, descriptive anchors?
- Open Graph: og:title, og:description, og:image?

**Local SEO:**
- LocalBusiness / AutoDealer / relevant schema type present?
- NAP visible in HTML and consistent across all sources found?
- Google Business Profile: claimed and verified?
- Location mentioned naturally in body copy?

**Content:**
- Word count per page (500+ standard, 1500+ pillar)?
- Primary topic clear? Semantic terms present?
- Scannable: subheadings, short paragraphs, bullets?

**Structured data:**
- JSON-LD types present: Organization, LocalBusiness, Article, Product, FAQ, HowTo, BreadcrumbList?
- Syntactically complete?

### Pillar 2 — GEO

Optimises for Perplexity, ChatGPT Search, Google AI Overviews, Gemini. Award partial GEO credit for signals found via third-party sources — AI engines can access some of this even when a site is JS-rendered.

**E-E-A-T:**
- Named owner/author with verifiable identity — anywhere?
- About page: who runs it, background, trading history — any source?
- Contact info consistent across all sources?
- Trust signals: reviews, star ratings, awards, certifications — any source?
- Organization schema with name, logo, URL, social profiles?
- Companies House / legal registration confirms legitimacy?

**AI synthesis readiness:**
- Core value prop in first 100 words — any crawlable source?
- Factual density: specific numbers, named locations, verifiable claims?
- Brand named consistently across all sources?
- Publish/update dates on content?

**AI Overview compatibility:**
- Content sections open with a direct answer?
- No long wind-up paragraphs before the actual information?

### Pillar 3 — AEO

Optimises for featured snippets, People Also Ask, voice search, AI assistants.

**Featured snippet eligibility:**
- 40–60 word answer paragraphs under question-phrased headings?
- "X is..." definitional sentence near top of relevant pages?
- Numbered lists or bullet lists present?

**Structured answer formats:**
- FAQPage JSON-LD?
- Question-phrased H2/H3 headings (Who/What/Where/When/Why/How)?
- HowTo schema for step-by-step content?

**Voice search:**
- Conversational phrasing throughout?
- Local intent questions answered: where are you, what are your hours, do you offer finance?
- NAP consistent, LocalBusiness schema, location in body copy?

---

## Step 4: Score + Chat Summary

### Scoring

Score each pillar 1–10:
- **8–10 → 🟢 Strong**
- **5–7 → 🟡 On Track**
- **1–4 → 🔴 Needs Work**

**Overall Health %** = (SEO + GEO + AEO) ÷ 3 × 10

Mark any score affected by JS rendering with **†** and note: *"Content exists but is currently invisible to Google. Fix JS rendering to unlock full potential."*

**6 Radar metrics** (scored 1–10 independently):

| Metric | Covers |
|---|---|
| Technical | Rendering, crawlability, sitemap, robots, HTTPS, mobile, canonicals |
| Content | Depth, keywords, freshness, readability — any source |
| Trust | E-E-A-T, reviews, About depth, contact — any source |
| AI-Readiness | Factual density, entity clarity, answer formatting, third-party presence |
| Schema | Types present, coverage, completeness |
| Local Signal | NAP, GBP, LocalBusiness schema, location mentions, directory consistency |

### Chat Summary (output before HTML, keep tight)

```
## 🔍 [Site Name] — [Quick/Extensive] Audit · [date]
Pages reviewed: [n] | Fetches used: [n] | SPA: [detected/clear]
Overall Health: [X]%

| Pillar | Score | Status |
|---|---|---|
| SEO   | X/10 | 🟢/🟡/🔴 |
| GEO   | X/10 | 🟢/🟡/🔴 |
| AEO   | X/10 | 🟢/🟡/🔴 |

Top 3 priorities: [one specific sentence each]
Biggest strength: [one specific sentence]

Generating dashboard...
```

---

## Step 5: HTML Dashboard

Single self-contained file. No external dependencies except Google Fonts. Write directly to `/home/claude/mustafa-seo-audit-[domain]-[date].html`.

**HTML efficiency rules:**
- Card body copy: 1–2 sentences maximum. Business consequence first, technical detail in hidden `.tech` div only.
- No inline comments in HTML output.
- CSS: use shorthand properties. Combine selectors. No redundant rules.
- JS: minimal — gauges, radar, toggle, effort bars only. No libraries.
- Quick audit: target ~400 lines. Extensive: target ~700 lines. If over budget, cut card copy first, never cut data.

### Design System

**Fonts:** `Syne` (600/700/800, headings + scores) · `Inter` (400/500/600, body)

**CSS variables:**
```css
--navy:#1B2A4A; --navy-mid:#243659; --accent:#2563EB; --accent-lt:#EFF6FF;
--emerald:#10B981; --emerald-lt:#ECFDF5; --rose:#EF4444; --rose-lt:#FEF2F2;
--amber:#F59E0B; --amber-lt:#FFFBEB; --border:#E2E8F0;
--dark:#1E293B; --muted:#64748B; --bg:#EEF2F7; --white:#FFFFFF;
```

**Components:**
- Cards: `border-radius:12px; box-shadow:0 1px 3px rgba(0,0,0,.06),0 4px 16px rgba(0,0,0,.04); padding:20px`
- Body background: `var(--bg)` — light blue-grey makes white cards pop
- Priority roadmap and strategic summary: cards only, no tables
- Tables: Technical Deep Dive accordions only

### Dashboard Sections

**1. Fixed header bar** — `height:54px; background:var(--navy); position:fixed; top:0; z-index:200`
- Left: domain (Syne 700, 15px, white)
- Centre: "SEO / GEO / AEO Audit" (Inter 500, 10px, uppercase, `#93C5FD`)
- Right: `[📖 Plain Language]` toggle · `[⬇ Download PDF]` button

**2. Hero** — dark gradient, `padding-top:72px`
- Audit badge pill (+ amber `⚡ SPA Recovery Active` pill if SPA was detected)
- Site name (Syne 800, 38px)
- Sub-line: pages crawled · sources · date
- Gauge trio (SVG radial, spec below)
- Overall health bar (animated)
- Radar chart (SVG spider, spec below)

**Gauge spec:** viewBox `0 0 120 120`. Background circle r=48, stroke `rgba(255,255,255,.08)`, stroke-width 11. Arc: same r, stroke-dasharray 301.6, stroke-linecap round, rotate(-90 60 60). Animate dashoffset from 301.6 → `301.6-(score/10*301.6)` on load. Colour: `#10B981` ≥8 / `#F59E0B` ≥5 / `#EF4444` <5. Score text: Syne 800 22px white at x=60 y=56. "/10": Inter 9px muted at x=60 y=72.

**Radar spec:** viewBox `0 0 400 400`, max-width 420px. cx=cy=200, R=130. 6 axes: Technical, Content, Trust, AI-Readiness, Schema, Local Signal. Angle: `(2π×i/6)−π/2`. Grid: 5 concentric hexagons at 20–100% R, stroke `rgba(255,255,255,.08)`. Axis lines stroke `rgba(255,255,255,.12)`. Data polygon: fill `rgba(37,99,235,.2)`, stroke `#2563EB` sw=2. Dots: r=4, fill `#2563EB`, stroke white sw=1.5. Animate polygon + dots from centre on load, 1.2s ease.

**3. SPA Recovery Banner** (only if SPA detected) — amber-tinted strip below hero.
One sentence: what was detected, what recovery methods ran, what scores are marked †. Frames it as methodology transparency, not a disclaimer.

**4. Strategic Summary** — 3-column card grid:
- 🏆 Biggest Win (`--emerald-lt` bg)
- ⚠️ Biggest Risk (`--rose-lt` bg)
- 🚀 ROI Opportunity (`--accent-lt` bg) — frame as traffic/revenue/conversions

**5. Priority Roadmap** — action cards, sorted Critical → High → Medium → Quick Win.
Medium + Quick Win wrapped in `<details>` "Show further improvements".

Card layout: `grid: auto 1fr auto; gap:16px`. Left border: rose (Critical) / orange (High) / amber (Medium) / emerald (Quick Win).
Column 1: priority pill. Column 2: title (Syne 700 14.5px) + 1-sentence business impact + hidden `.tech` div. Column 3: Effort/Impact bars (Low=33%, Medium=66%, High=100%).

**Business impact writing rule — always lead with consequence:**

| ❌ | ✅ |
|---|---|
| "H1 missing" | "Google uses the H1 to categorise your business — without one you're invisible for your primary keyword" |
| "About page JS-rendered" | "Your About page exists but Google can't read it — the trust signals that get you recommended by AI search are locked behind JavaScript" |
| "No FAQ schema" | "Your FAQ answers are locked out of Google's quick-answer boxes — the most-clicked result format" |
| "No About page" | Only if About is ❌ confirmed missing after all recovery |

**6. What's Working Well** — emerald-tinted section, 3-col card grid. Min 3, max 6 cards. One sentence each citing specific observed evidence only.

**7. Technical Deep Dive** — Extensive audit only. Three `<details>` accordions (SEO / GEO / AEO), collapsed by default. Each contains:
- Score table: Check | Finding | Source | Status pill
- Insight box (2 sentences, plain language)
- `.tech` div (hidden by default): raw signal evidence

**Source tag in every finding row:**
`[site]` · `[sitemap]` · `[search]` · `[3P:Trustpilot]` · `[3P:GBP]` · `[3P:CH]` · `[unverifiable]`

**8. Pages Audited** — `<details>`, collapsed. Table: URL | Status (✅/⚠️/❌) | Source | Key observation (one specific sentence).

**9. Glossary** — Extensive only. `<details>`, collapsed. Terms: SEO, GEO, AEO, E-E-A-T, Schema, Featured Snippet, AI Overview, Core Web Vitals, SPA Rendering. One sentence each, written for a first-time business reader.

**10. Footer** — centred, muted, 11.5px. `Audited [date] · [domain] · [mode] · Powered by Claude AI · Mustafa's SEO Audit Tool v7.1`

### JS (minimal, inline at bottom of body)

```javascript
// Gauges — animate on load
const C = 301.6;
document.querySelectorAll('.gauge-arc').forEach(el => {
  const score = parseFloat(el.dataset.score);
  el.style.stroke = score >= 8 ? '#10B981' : score >= 5 ? '#F59E0B' : '#EF4444';
  setTimeout(() => {
    el.style.transition = 'stroke-dashoffset 1.4s cubic-bezier(.4,0,.2,1)';
    el.style.strokeDashoffset = C - (score / 10 * C);
  }, 100);
});

// Health bar
const pct = Math.round((seo + geo + aeo) / 3 * 10); // replace with real values
document.getElementById('h-fill').style.cssText = `width:${pct}%;transition:width 1.4s ease`;
let c = 0; const iv = setInterval(() => {
  document.getElementById('h-pct').textContent = (c = Math.min(c+1, pct)) + '%';
  if (c >= pct) clearInterval(iv);
}, 20);

// Effort/Impact bars
document.querySelectorAll('.ei[data-w]').forEach(el => {
  setTimeout(() => el.style.width = el.dataset.w + '%', 300);
});

// Radar
(function() {
  const cx=200,cy=200,R=130,n=6;
  const labels=['Technical','Content','Trust','AI-Ready','Schema','Local'];
  const scores=[/* inject real scores */];
  function pt(i,r){const a=(2*Math.PI*i/n)-Math.PI/2;return[cx+r*Math.cos(a),cy+r*Math.sin(a)];}
  // Grid
  const grid=document.getElementById('r-grid');
  [.2,.4,.6,.8,1].forEach(f=>{
    const p=Array.from({length:n},(_,i)=>pt(i,R*f).join(',')).join(' ');
    grid.innerHTML+=`<polygon points="${p}" fill="none" stroke="rgba(255,255,255,.07)" stroke-width="1"/>`;
  });
  // Axes + labels
  const ax=document.getElementById('r-ax'),lb=document.getElementById('r-lb');
  for(let i=0;i<n;i++){
    const[x,y]=pt(i,R);
    ax.innerHTML+=`<line x1="${cx}" y1="${cy}" x2="${x}" y2="${y}" stroke="rgba(255,255,255,.1)" stroke-width="1"/>`;
    const[lx,ly]=pt(i,R+26);
    lb.innerHTML+=`<text x="${lx}" y="${ly}" text-anchor="middle" font-family="Inter,sans-serif" font-size="10" fill="rgba(255,255,255,.6)">${labels[i]}</text>
    <text x="${lx}" y="${parseFloat(ly)+13}" text-anchor="middle" font-family="Syne,sans-serif" font-size="9.5" font-weight="700" fill="#2563EB">${scores[i]}/10</text>`;
  }
  // Animate polygon
  const poly=document.getElementById('r-poly'),dots=document.getElementById('r-dots');
  poly.setAttribute('points',Array(n).fill(`${cx},${cy}`).join(' '));
  const start=performance.now();
  (function animate(now){
    const p=Math.min((now-start)/1200,1),e=1-Math.pow(1-p,3);
    poly.setAttribute('points',scores.map((_,i)=>{const[x,y]=pt(i,(scores[i]/10)*R*e);return`${x},${y}`;}).join(' '));
    dots.innerHTML=scores.map((_,i)=>{const[x,y]=pt(i,(scores[i]/10)*R*e);return`<circle cx="${x}" cy="${y}" r="4" fill="#2563EB" stroke="white" stroke-width="1.5"/>`;}).join('');
    if(p<1)requestAnimationFrame(animate);
  })(start);
})();

// Audience toggle
const tb=document.getElementById('tb');
tb.addEventListener('click',()=>{
  const on=tb.classList.toggle('active');
  document.querySelectorAll('.tech').forEach(el=>el.style.display=on?'block':'none');
  tb.textContent=on?'🔧 Technical Detail':'📖 Plain Language';
});
```

### Print stylesheet

```css
@media print {
  .topbar{display:none}
  body{background:white}
  .hero{margin-top:0;-webkit-print-color-adjust:exact;print-color-adjust:exact}
  .tech{display:block!important}
  details{display:block}
  details summary{display:none}
  .card,.action-card{page-break-inside:avoid;box-shadow:none}
  .sec{page-break-before:always}
}
```

### Deliver

```bash
cp /home/claude/mustafa-seo-audit-[domain]-[date].html /mnt/user-data/outputs/mustafa-seo-audit-[domain]-[date].html
```

`present_files` then one sentence: *"Dashboard ready — open in any browser, hit ⬇ Download PDF top-right to export."*

---

## Step 6: Offer Next Steps

> "Want a **30-day action plan**, a **competitor audit** to benchmark against, or a **re-audit** after changes to track progress?"

---

## Reference Tables

### Scoring

| Score | Band | Colour |
|---|---|---|
| 8–10 | 🟢 Strong | `#10B981` |
| 5–7 | 🟡 On Track | `#F59E0B` |
| 1–4 | 🔴 Needs Work | `#EF4444` |

### Page Status

| Symbol | Meaning | SEO | GEO/AEO |
|---|---|---|---|
| ✅ | Crawlable | Full credit | Full credit |
| ⚠️ | Exists, JS-rendered, content unknown | Full penalty | Unverifiable† |
| ⚠️ | Exists, JS-rendered, found via 3P | Full penalty | Partial credit |
| ❌ | Confirmed missing | Full penalty | Full penalty |

### Fetch Budget

| Mode | Max page fetches | Max searches | Max 3P fetches | HTML target |
|---|---|---|---|---|
| Quick | 5 (incl. homepage) | 1 | 0 | ~400 lines |
| Extensive | 11 (incl. homepage) | 2 | 3 | ~700 lines |

Sitemap + robots.txt do not count against the page fetch budget.
