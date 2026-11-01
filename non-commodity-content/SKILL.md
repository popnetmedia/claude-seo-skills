---
name: non-commodity-content
description: Use when writing or substantially editing prose meant for publication: articles, blog posts, reviews, guides, long-form marketing copy, landing pages, email campaigns, social posts, product descriptions, case studies, newsletters. Establishes Google's April 2026 "non-commodity content" standard (unique + specific + authentic) with a pre-publish 5-question check. Skip for code comments, commit messages, internal-only docs, README files, slack messages, or any writing not intended for public/SEO consumption.
---

# Non-Commodity Content Standard

## Why this exists

At Google Search Central Live Toronto (April 21, 2026), Danny Sullivan codified what Google now actively rewards in the AI-search era. The keynote framed it as the operational version of E-E-A-T:

> Tips for AI Search success:
> 1. Follow SEO fundamentals
> 2. Make use of structured data
> 3. Have a great page experience
> 4. **More than anything else: unique, authentic, *non-commodity* content**

Items 1–3 are table stakes. Item 4 is the dominant ranking signal. This skill ensures every published piece passes the non-commodity bar before it ships.

## The Three Pillars

Every piece must demonstrate all three:

1. **Unique**: Brings a viewpoint, information, or content others lack or can't easily replicate. If a competitor could write the same article from public sources alone, it's commodity.
2. **Specific**: Talks about a specific instance, situation, person, or thing, not general rules, generic steps, or "X tips for..." advice.
3. **Authentic**: Demonstrates first-hand knowledge: you tested it, visited it, opened the box, photographed the wear pattern, talked to the person, ran the experiment, signed the contract.

## Commodity vs. Non-Commodity (Examples Across Domains)

These are Sullivan's examples and a few extensions. Note the pattern: non-commodity titles name a specific instance and signal a non-replicable action ("I refused," "we waived," "I tested," "what happened when…").

| Domain | Commodity (avoid as standalone) | Non-Commodity (publish) |
|---|---|---|
| Running store | "Top 10 Things to Consider When Buying Running Shoes" | "Why This Customer's Shoes Collapsed After 400 Miles: A Wear Pattern Analysis" |
| Real estate | "7 Tips for First-Time Homebuyers" | "Why We Waived the Inspection (And Saved $15k): A Look Inside the Sewer Line" |
| Interior design | "2024 Kitchen Trends You Need to See" | "Marble vs. Grape Juice: Why I Refused to Install Stone for a Family of Five" |
| Software / SaaS | "5 Tips for Better API Design" | "We Rewrote Our Pagination Three Times in Six Months: Here's the Final Schema and Why It Won" |
| Product review | "iPhone 17 Pro Review: Everything You Need to Know" | "I Used the iPhone 17 Pro as My Only Camera on a 12-Day Iceland Trip: Here Are the Three Failures That Surprised Me" |
| Restaurant | "Best Italian Restaurants in Brooklyn 2026" | "I Ate the Entire Carbonara Menu at Misi Last Saturday: Here's the One That's Worth the Wait" |
| Fitness | "How to Build Muscle After 40" | "I Followed 5/3/1 for 18 Months at Age 43: Here Are My Lifts, Photos, and the Two Mistakes That Cost Me a Year" |
| B2B / consulting | "Why CRM Migration Matters" | "We Migrated 14 Clients Off Salesforce to HubSpot in 2025: Here's the Cost Spreadsheet and the One That Failed" |

The pattern across every domain: **specific instance + first-hand evidence + non-replicable proof**.

## Operational Rules

Apply to every published piece:

1. **Anchor every article in a specific event, person, or moment.** Open with "Last Tuesday…" / "When I…" / "On our March trip…", not "Most marketers know that…" or "In today's fast-paced world…".
2. **Lead with non-replicable detail in the first 2 paragraphs.** A photo of the actual thing, a date, a price you paid, a person you spoke to, a side-by-side you ran. If the opening could appear on any site, rewrite it.
3. **Include at least one piece of evidence a competitor can't fake:** an original photo (no stock), a tasting/test date, a quote from someone you interviewed, an order confirmation reference, a measurement you took, a result you logged.
4. **Title patterns to favor:**
   - "Why I [refused / waived / kept / chose / abandoned]…"
   - "What Happened When I…"
   - "[Specific subject] After [Specific Event/Time]"
   - "I Tested X Against Y for Z [Days/Months/Miles]"
   - "[Number] Things I Got Wrong About [Topic] in [Time Period]"
5. **Title patterns to avoid as standalone pieces** (acceptable only when each section contains non-commodity evidence inside):
   - "Top 10…" / "X Best…"
   - "X Tips for…"
   - "Best of [Year]…"
   - "How to [Generic Verb]…"
   - "What Is [Category]?"
   - "The Ultimate Guide to…"
6. **For roundups specifically:** Every entry must contain at least one first-hand sentence: what arrived, what you tested, what surprised you, who sent it. Generic descriptions sourced from the brand site are commodity.
7. **For reviews / how-tos / case studies:** State the date, the setup, the conditions. The non-commodity signal is "this happened, here's the receipt."
8. **For interviews:** A real conversation occurred. Quote, attribute, and date it. If it didn't, it's not an interview.
9. **For listicles and "best of" content:** Apply the standard per-entry. A roundup of 10 products is commodity if all 10 entries are press-release paraphrases. It's non-commodity if each entry has a dated first-hand observation.
10. **Educational / explainer content** is the highest commodity risk. To pass: open with a specific incident or experiment that motivates the explanation, anchor each section with a first-person observation, close with your own preference or recommendation backed by experience.
11. **Cover the query fan-out, not just the title query.** AI search no longer returns one result for one query; it runs 5–20 adjacent queries in parallel (the *fan-out*), retrieves the best answers across the bundle, and synthesizes one cited response. Before publishing, list 5–8 adjacent questions a fan-out would generate from your title (the *why*, the *how*, the *what-about*, the *vs.*, the *after-how-long*, the *what-if*). Answer the obvious ones inline. For the rest, link to a highly relevant internal page, ideally a pillar page in the same topic cluster. **If no relevant internal target exists, that gap is a creation signal: trigger a plan for a new piece to fill it.** This is the content flywheel, fan-out audits surface gaps → gaps drive new pieces → new pieces deepen topical coverage → coverage wins more citations.
12. **Every creation plan includes imagery.** Original or task-specific visuals are part of the non-commodity bar. Stock photography and brand-pulled product shots are commodity. When planning a new piece, the plan must specify what images will be produced and how:
    - If the project has Replicate API integration (check the project CLAUDE.md), generate images programmatically as part of the build.
    - If the project does not, the plan must include the image prompts themselves, one per intended image, with subject, style, framing, and mood, so the user can paste them into ChatGPT / DALL·E / Midjourney and produce them manually.

    No piece publishes without imagery decisions resolved at plan time.

## Site-Level Prerequisite: Content Map

Per-piece rules only work if the surrounding library is healthy. Before publishing into an existing site:

- **The project must have a content map**: an inventory of every published piece with its title, primary topic, target query cluster, and pillar/cluster role. If one doesn't exist, build it first; this is non-negotiable for any site of meaningful size.
- **Run a cannibalization check before planning every new piece.** Search the inventory for pieces already targeting overlapping queries. If overlap exists, the default move is to *expand and consolidate* the existing piece, not publish a competing one that splits ranking signals.
- **Run a coverage check.** Where the inventory has gaps in a topic cluster, those gaps are creation candidates (per rule 11).

If the project lacks a content map, that's the first task, not the new piece.

## Pre-Publish 5-Question Check

Before any piece publishes, every answer must be YES:

- [ ] **Could a competitor write this same article from public sources alone?** (Must be NO.)
- [ ] **Does the opening contain a specific date, person, place, or event from my own direct experience?**
- [ ] **Is there at least one piece of evidence (photo, quote, measurement, receipt, test result, conversation) that proves I actually engaged with the subject?**
- [ ] **Does the headline name a specific instance rather than a generic category?**
- [ ] **Does my piece either answer the obvious fan-out questions inline, or link to internal pages that do?** (No relevant internal target = create one before or alongside this piece.)

If any answer is NO, the piece is commodity content. Rework before publishing.

## What this skill does NOT change

Sullivan's slide explicitly noted: *"These align with traditional SEO success, too."* Don't abandon the basics:

- Structured data (Article, Review, Product, ItemList, FAQPage JSON-LD): still required
- Internal linking, semantic headings, meta tags: still required
- Page experience (Core Web Vitals, mobile, no intrusive interstitials): still required
- Accuracy, fact-checking, citing sources: still required
- **Agent-readable rendering**: AI agents now access pages via screenshots, the DOM, and the accessibility tree. Semantic HTML, proper headings, alt text on every image, and critical content rendered server-side (not trapped in client-only JS): still required, now for AI consumption as well as humans.
- **Published prices and a bookable action**: on any site that sells a priced service, the price range and a machine-callable booking action are prerequisites for being quoted and transacted with at all. See the `ai-search-readiness` skill.

Non-commodity content is the ranking *multiplier*. The basics are the *floor*.

## What Google Explicitly Says NOT to Do

Per Google's *AI Optimization Guide* (Search Central docs), the following tactics are explicitly debunked. Do not do these regardless of what other guides claim:

- **No AI-specific files or markup.** No `llms.txt`, no special "AI markdown," no machine-readable bundles built just for LLMs. Google does not consume these.
- **Don't pre-chunk content for AI.** Write normal sections for humans; the retrieval system handles segmentation.
- **Don't rewrite for an "AI voice."** There is no special phrasing or structure that earns AI favor over normal good writing.
- **Don't chase inauthentic "mentions" across the web.** Trying to manufacture brand mentions on third-party sites for AI visibility is wasted effort.
- **Don't spin per-keyword variant pages.** Generating near-duplicate pages for every query variation is scaled-content-abuse spam under Google's policies.
- **Structured data isn't strictly required for AI search.** Keep using it for rich results and classic SEO (see "What this skill does NOT change"), but don't treat it as the lever that earns AI citations.

## How to apply this skill in a session

1. When invoked, run the 5-question pre-publish check on any draft before suggesting it's done.
2. When asked to write something new, anchor the structure in a specific real event the user can confirm or describe; ask for it if you don't have one.
3. When asked to edit existing copy, identify the commodity-vs-non-commodity score per pillar and propose specific, surgical edits rather than full rewrites unless the entire frame is commodity.
4. When in doubt about whether a piece is commodity, look at the opening sentence: if it could be the opening of any article on this topic, it's commodity.

## Sources

1. Danny Sullivan keynote, Google Search Central Live Toronto, April 21, 2026: origin of the three pillars and the non-commodity framing. Reported by [Search Engine Roundtable, Apr 23, 2026](https://www.seroundtable.com/google-commodity-content-41200.html).
2. *AI Optimization Guide*, Google Search Central, https://developers.google.com/search/docs/fundamentals/ai-optimization-guide: origin of the debunked-tactics list, query fan-out mechanics, and agent-readability guidance.
