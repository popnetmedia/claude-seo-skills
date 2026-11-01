---
name: local-eeat
description: Use alongside non-commodity-content when writing or editing any page or post for a LOCAL service business: location pages, venue pages, "[service] in [city]" pages, local blog posts, service-area content. Adds the local-EEAT layer (named venues, geographic specificity, first-hand local proof, service-area signals) that the general non-commodity standard doesn't cover, plus a voice-capture and internal-linking workflow, and the nine-part City Page Anatomy with its programmatic gate that every city and service-area page (hand-written or generated) is built to. Skip for non-local content (general guides, national/global topics), use non-commodity-content alone there.
---

# Local E-E-A-T Content Standard

## Relationship to non-commodity-content

This skill is a LOCAL EXTENSION, not a replacement. The `non-commodity-content`
skill is the quality gate (unique + specific + authentic, the 5-question check).
This skill adds the local layer on top. Run BOTH for any local page or post.

**Where this skill and older SEO playbooks conflict, non-commodity-content wins:**

1. **Headlines: do NOT default to generic question H1s.** Older guides (and
   general blog-writing skills) push "phrase the H1 as the question the reader
   types" (e.g. "What Does a Wedding DJ Cost in Huntsville?"). Per the
   non-commodity standard, a standalone generic-question title is commodity.
   Favor a SPECIFIC LOCAL INSTANCE instead:
   - Commodity: "How Much Does a Wedding DJ Cost in Huntsville?"
   - Non-commodity: "What We Charged for a 180-Guest Wedding at The Ledges Last June, and Why"
   A question title is acceptable ONLY when the body delivers first-hand local proof.

2. **FAQ schema is not the AI-citation lever.** Keep FAQ sections where they
   genuinely help readers, and apply FAQPage schema for rich results, but do
   not treat FAQ blocks or "exactly 5 FAQs" as the thing that earns AI citations.
   First-hand local specificity earns citations. (Per Google's AI Optimization Guide.)

## The Local Non-Commodity Bar

A local page passes only if a competitor in the same city COULDN'T write it from
public sources. The proof that they can't: you were physically there.

Every local piece must contain at least one of these, original and not paraphrased:

- **A named venue or location you actually worked**, with a specific detail only
  someone on-site would know ("the ceremony lawn at Burritt on the Mountain loses
  the sun behind the tree line around 6:30 in May, so we moved the first dance up").
- **A named local event/date** ("a 180-guest reception at The Ledges, June 2025").
- **An original on-location photo**: the actual venue, your actual setup. No stock,
  no venue's own marketing shots.
- **A real local client moment or quote**, attributed and dated.
- **A logistics/geography detail specific to the area** (load-in at Stovehouse,
  parking at Stonehaven, noise ordinance timing in Madison, drive time from the
  shop to a Decatur venue).

## Geographic Specificity Without Going Commodity

- Name the SPECIFIC place, not just the city. "Stone Bridge Farms in Cullman"
  beats "a venue near Huntsville."
- Use real local entities: neighborhoods, landmarks, venues, seasons, local
  events. These are entity signals search engines and AI tie to your locality.
- Put the place in the headline when it's tied to a real instance, not as a
  bolt-on keyword. "...at The Roundhouse" not "...in Huntsville, AL | Best DJ".
- One primary city/area per page. Don't spin near-duplicate pages per town
  (that's scaled-content-abuse spam, see non-commodity-content). Genuinely
  distinct service-area pages need genuinely distinct first-hand content.

## City Page Anatomy (programmatic "[service] in [city]" pages, every site)

The shape for every city, town and service-area page, hand-written or generated. Nine parts, top
to bottom.

1. **Title: service + city in the keyword slot.** `RV Rentals in Tampa | <benefit> | <Brand>`, still
   the `Keyword | Benefit | Brand` title formula, 60 characters or fewer. Use the searcher's noun, not
   industry jargon.
2. **A bulleted summary at the top**: what the service is, where it is delivered, and what it costs.
   This is the block answer engines lift. Price follows `ai-search-readiness` Move 1: a range with
   both ends, what moves it and the date measured, read from the source at build time. Never a bare
   "starting at" figure. Where no price is published, the bullet says so and names where to get one.
3. **Write about the actual city.** The page proves both expertise in the service and knowledge of the
   place, using the Local Non-Commodity Bar above. It carries at least one fact computed from that
   city's own data that no other page states.
4. **Real driving directions** from a well-known landmark to where the service happens (the shop, the
   park, the venue), naming the roads and landmarks on the way. Every road and landmark is checked
   against a map, and none is invented ("near the Home Depot" only if a Home Depot is there). A
   service-area business with no public address gives directions to where the work happens, never
   to a home address. With nothing real to route to, the section is left out.
5. **Cut everything unrelated**: mission statements, founding stories and generic boilerplate. Every
   block is about this service in this city, or it goes.
6. **City FAQs in the searcher's words**, e.g. "How much does a monthly RV rental cost in Tampa?".
   Take them from real customer questions where they exist (`ai-search-readiness` Move 3), answer each
   from a source, and mark them up as FAQPage. The same five questions with the city swapped in is
   the templated pattern this skill forbids.
7. **Real local organizations, named**: the chamber, a supplier used, a team sponsored, the park or
   venue worked with. Only relationships that exist. Never imply a membership, sponsorship or
   partnership the business does not hold, and disclose an owned
   business wherever it is named.
8. **Contact form and social proof on the same page.** Only real reviews and testimonials, attributed,
   dated and linked to their source. No `aggregateRating` without a linked source. With no real
   reviews, there is no review block; never write a placeholder or fabricate one.
9. **Point backlinks at this page**, not only the homepage. Citations and earned links point
   at the city page they are about,
   with anchors mostly branded or bare URL and the exact-match phrase used sparingly.

**The programmatic gate.** A generated city page publishes only when its data clears a minimum set
by the site: real local entities, a fact computed from local data, and something no other city page on the site
says. A park directory, for example, might require three verified parks per city. Below the gate the URL is not
generated: `dynamicParams = false`, left out of the sitemap, and redirected to the nearest real
page. City-swapped near-duplicates are doorway pages under Google's spam policy. One of them risks
the whole set.

## Service-Area & Entity Consistency (site-level, check before publishing)

- **NAP consistency**: business name, address/service area, and phone match
  across the site, footer, schema, and Google Business Profile exactly.
- **Service-area honesty**: state the real area ("local area only" per project
  stats). Don't imply coverage of cities you don't serve.
- **LocalBusiness / Organization schema** carries the consistent NAP and links to
  the verified GBP and social profiles. One-time setup, reinforced per page.

## Voice Capture (before drafting a local post)

Local content must sound like the operator who was there, not generic AI.
Before drafting, capture voice from one of:
- **Samples**: study existing site copy / past posts: sentence rhythm, formality,
  first-person "I" vs. team "we," recurring phrases, what they avoid. Name what
  you found back in one line and confirm.
- **Quick interview** (if no samples): at most 4 questions: how they talk to a
  client, first-person vs. "we," anything they never want to sound like, one
  sentence in their own voice on why the topic matters.

Default to the established brand voice already in the site copy
unless told otherwise.

## Internal Linking Into Existing Clusters (workflow step, not an afterthought)

Most local sites already have clusters: venue or place pages, location pages
(`/locations/[slug]`), and service pages. Every local post must connect to them.

1. Before drafting, identify 3–5 existing related pages (the venue page for the
   venue named, the city's location page, the relevant service page).
2. Weave 3–5 contextual links into the body with descriptive anchor text
   (the venue's name, the service), never "click here," never repeated exact-match.
3. Add a short "Related" block linking the same cluster.
4. If a post names a venue with NO existing venue page, that gap is a creation
   signal: flag a new venue page (per the content-flywheel rule in
   non-commodity-content).

## Imagery (local pieces specifically)

Per non-commodity-content, every piece resolves imagery at plan time. For local:
- Prefer ORIGINAL on-location photos (the real venue, the real setup).
- File names and alt text include the venue + city naturally
  (`360-photo-booth-the-ledges-huntsville.jpg`, alt: "360 photo booth set up in
  the courtyard at The Ledges in Huntsville").
- If generating images programmatically, the venue/setting must be plausible and
  not misrepresent a real named venue you didn't shoot. If you can't show the
  real place, don't imply you did.

## Pre-Publish Local Check (in addition to the non-commodity 5-question check)

Every answer must be YES:

- [ ] Does the piece name a SPECIFIC local venue, event, or place tied to real work?
- [ ] Is there first-hand local proof a competitor in this city couldn't fake?
- [ ] Posts: does the headline name a specific local instance (not a generic city + keyword)?
  City pages: is the title service + city per the City Page Anatomy, with the specific local
  instance carried in the body?
- [ ] City pages: are all nine anatomy parts present, or honestly left out (no directions without a
  real route, no review block without real reviews), and does the page clear the programmatic gate?
- [ ] Does it link into the existing venue / location / service clusters?
- [ ] Is NAP / service-area consistent with the rest of the site and GBP?
- [ ] Are images original-or-honest, with venue+city in file name and alt text?

If any is NO, it's commodity local content. Rework before publishing.

## Publisher / Handoff Checklist (append to every local deliverable)

- Set SEO title (keyword-forward, includes locality) + meta description (130–150 chars).
- Confirm LocalBusiness/Organization schema NAP matches; FAQPage schema only if an
  FAQ section exists and genuinely helps readers.
- Add "Last Updated: [Month Year]".
- Source the on-location image(s); add provided alt text + file names.
- Confirm internal cluster links resolve.
- Set a 6–12 month refresh reminder: update venues/prices/stats, confirm links,
  refresh the date.
