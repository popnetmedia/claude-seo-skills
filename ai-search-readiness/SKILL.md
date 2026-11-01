---
name: ai-search-readiness
description: What a commercial site must publish to be usable by AI search and by booking agents. Use on any client site that sells a priced service or takes bookings, alongside non-commodity-content. Covers the three moves: publish the price range, make scheduling agent-ready with real schema, and mine real customer questions for content. Triggers on pricing pages, booking pages, service pages, schema work, and any question about whether AI can find or transact with the site.
---

# AI Search Readiness

Three moves. They are not content quality, which `non-commodity-content`
governs. They are whether the site can be **found, quoted and transacted
with** by a machine acting for a customer.

A site can pass every content rule and still be invisible to an agent, because it never
published a number and never exposed a booking endpoint a machine can read.

---

## Move 1: publish the price range

**The rule.** Every commercial site publishes what its service costs. Not a price book. A **range**
with the two ends and what moves the number between them.

People ask AI what things cost, constantly. An AI with nothing to quote sends them to a competitor
who published something. "Call for pricing" is a dead end to a machine, and increasingly to a
person.

**What to publish, minimum:**

- the **low end** and the **high end**, as real figures
- **what moves it**: the two or three variables that decide where in the range a job lands
- the **date the range was measured**, beside the figures
- **every fixed fee**, in full: delivery, setup, deposit, cleaning, minimums, penalties
- a link to the **live source** for an exact quote

**The objection, and the answer.** Operators resist because prices move and they fear being held
to a stale figure. That objection is about *a single fixed price presented as the price*. It is
not an argument against a range. A dated range with its drivers named is more accurate than
silence, and it is the thing the operator is uniquely able to state.

**The rule that resolves it.** Never publish a bare rate. Publish:

> Nightly rates run $106 to $175 across the fleet, measured 18 September 2026. Which camper and
> which dates decide where you land. Check your dates for the exact rate.

Not:

> Campers from $106/night.

**Never type a price twice.** Read it from the source the business already maintains, at build
time: the booking platform's feed, the POS, the published fee table. A price typed into a page is
a price that will disagree with checkout within a month. Where the number is parsed from the
operator's own page, the two can never drift.

**Where a rate genuinely cannot be published** (it is quoted per job, or set by a third party), the
page still owes the reader everything adjacent that is fixed: the fee table, the minimums, the
deposit, the penalty for changing a booking. Then say plainly that the rate itself is set at
quote, and point at where. A page that says "we do not publish X, here is why, here is where to get
it, and here is everything that does not move" beats a page that says nothing.

**Derived pricing facts are the differentiator.** A range anyone can read off a listing is table
stakes. The consequence comes from arithmetic on it:

- a fee charged once per job costs a tenth as much per day on a month as on a three-day booking
- a platform's "weekly rate" is often a fixed percentage of seven nights, set once and never
  touched, which means it is not a negotiated discount and a real one has to be asked for
- the cheapest headline rate plus the dearest delivery zone beats the dearest rate plus the nearest

Run the arithmetic and state what the reader should now do differently. That arithmetic is what a competitor
cannot copy by rewriting the same listing.

**Pre-publish check for any pricing page:**

- [ ] Both ends of the range are real figures, not "from"
- [ ] The drivers that move the price are named
- [ ] The measurement date sits beside the figures
- [ ] Every fixed fee is published in full
- [ ] The figures are read from a source at build time, not typed
- [ ] Structured data carries the same numbers as the prose (see Move 2)
- [ ] Where something cannot be priced, the page says so and points at the quote

---

## Move 2: agent-ready scheduling

**The rule.** If a customer's agent cannot see how to transact with the calendar, the business is
invisible to it. Agentic booking is live: an assistant will book a table, a service call, a rental.
It reaches for structured data first and the DOM second.

**Three layers, all required.**

**1. The offer, in structured data.** Every bookable thing carries an `Offer` with a real price or
a `priceSpecification` range, a currency, an availability state, and a `priceValidUntil` matching
the measurement date. A `priceRange` on the business is not enough; the machine wants the offer on
the item it is being asked to book.

```jsonc
"offers": {
  "@type": "Offer",
  "priceCurrency": "USD",
  "priceSpecification": {
    "@type": "UnitPriceSpecification",
    "priceCurrency": "USD",
    "minPrice": 106,
    "maxPrice": 175,
    "unitCode": "DAY",
    "referenceQuantity": { "@type": "QuantitativeValue", "value": 1, "unitCode": "DAY" }
  },
  "availability": "https://schema.org/InStock",
  "priceValidUntil": "2026-12-31",
  "url": "https://example.com/book"
}
```

**2. The action.** The entity carries a `potentialAction` naming the booking endpoint, the HTTP
target and the inputs it requires. This is the part almost nobody publishes and the part an agent
actually needs.

```jsonc
"potentialAction": {
  "@type": "ReserveAction",
  "name": "Book this camper",
  "target": {
    "@type": "EntryPoint",
    "urlTemplate": "https://example.com/book?rental_id={rental_id}&from={checkin}&to={checkout}",
    "actionPlatform": [
      "https://schema.org/DesktopWebPlatform",
      "https://schema.org/MobileWebPlatform"
    ],
    "httpMethod": "GET"
  },
  "result": { "@type": "Reservation", "name": "Camper rental reservation" },
  "object": { "@type": "Product", "name": "2023 Jayco Jay Flight" }
}
```

Pick the action type that matches the verb: `ReserveAction` for a booking or reservation,
`ScheduleAction` for an appointment, `OrderAction` for a purchase, `RsvpAction` for an event.

**3. The constraints, stated as data, not prose.** An agent that books a stay the business cannot
honour has wasted everyone's time. Publish the rules that would make a booking fail:

- minimum and maximum duration
- lead time and cut-off (`availabilityStarts`, `advanceBookingRequirement`)
- eligibility (minimum age, licence, deposit)
- service area (`areaServed`), and what it costs to reach each part of it
- opening or delivery windows, where arrival time is constrained by someone else

Put each of these in schema where a property exists, and in plain sentences on the page where it
does not. Both, always: the agent reads one, the person reads the other, and they must agree.

**Agent-readable rendering.** Critical content server-rendered, semantic HTML, real headings, alt
text, tables as `<table>`. An agent reads the DOM and the accessibility tree. Content that appears
only after a client-side fetch is content the agent did not see.

**Do not fake capability.** Only publish a `ReserveAction` against an endpoint that actually
accepts a booking. An action pointing at a contact form is a lie an agent will act on, and the
customer finds out at the worst moment. Where booking is genuinely by phone, publish the phone
number in the schema and no reserve action.

### The protocol layers, and which one to build

Three layers exist. They are not alternatives, they stack, and the order matters because the
lower ones are read today and the upper ones mostly are not yet.

**Layer 1: structured data (build this first, always).** `Offer` plus `potentialAction`, as above.
This is what today's assistants actually consume when they answer a question about a business and
when they follow a link to act. It costs a page of JSON-LD and works immediately.

**Layer 2: a machine-readable feed.** One stable JSON URL holding the catalogue: what is for
rent or for sale, the price of each, the constraints, the booking URL, and the date it was
measured. No protocol commitment, no endpoint to defend, readable by any agent, crawler or
sibling site. Cheap, and the thing that makes layer 3 a morning's work later.

**Layer 3: A2A (Agent2Agent).** The open protocol for agent-to-agent task delegation: an **agent
card** at a well-known URL describing the agent's identity, skills and auth, and an endpoint
speaking JSON-RPC over HTTPS that accepts tasks and returns results. It is the right answer when
a customer's assistant should be able to *ask questions and get answers*, rather than read a page.

Be honest about where it stands. As of late 2026 there is no registry that consumer assistants
routinely crawl for agent cards, so publishing one is a bet on discovery arriving, not a source of
bookings this quarter. Build it when the business has something an agent genuinely needs to ask
that a static feed cannot answer, typically **live availability**.

**The honesty rule applies hardest here.** An agent card declares skills, and another agent will
call them. Declare only what the endpoint truly does. Most small operators cannot complete a
booking agent-to-agent at all, because payment, insurance and the rental contract sit inside a
third-party platform. That is fine, and the card should say it:

```jsonc
{
  "name": "Example Rentals booking assistant",
  "description": "Answers availability and price questions for one fleet. Cannot take payment.",
  "skills": [
    { "id": "check-availability", "description": "Which units are free between two dates" },
    { "id": "quote-stay", "description": "Total for a unit and dates, including delivery by town" }
  ],
  "capabilities": { "streaming": false, "pushNotifications": false }
}
```

A card claiming a `book-stay` skill that ends in a redirect is the same failure as a
`ReserveAction` pointing at a contact form, with a machine on the other end instead of a person.

**Payments.** Agentic payment rails (AP2 as an A2A extension, and the competing agentic-commerce
specs) are moving fast and are not supported by most booking platforms. Do not design a checkout
around them. Hand off to the platform's own checkout and say so in the card.

**Pre-publish check for any bookable page:**

- [ ] `Offer` with price or price range, currency, availability, `priceValidUntil`
- [ ] `potentialAction` with a real `EntryPoint` an agent can call
- [ ] The action's URL template's parameters are the ones the booking system truly takes
- [ ] Minimums, lead times, eligibility and service area published as data and as prose
- [ ] Every figure in the schema matches the figure in the copy
- [ ] The page renders its booking facts server-side
- [ ] No action is published for a capability that does not exist

---

## Move 3: content from real questions

**The rule.** The questions customers actually ask are the content. Every phone call, every email,
every "before you book" question is a page or a section, answered in public, in the customer's own
words.

Google's own guidance says generic advice content ("7 tips for summer") does not win. What wins is
hyper-specific and real. The question a customer asked this morning is both, by construction,
because a competitor writing from public sources does not know it was asked.

**The operational habit:**

1. **Capture the question verbatim.** The customer's wording, not industry jargon. "Can you
   drop it at my house while they fix the roof" is the query. "Residential placement logistics" is
   not. This is the same rule as writing the searcher's noun.
2. **Answer it where the reader has it.** On the page where the question arises, not in a blog
   post nobody reaches.
3. **Answer it with a source.** An answer with no source is an opinion; it is the fixed fee, the
   published rule, the measured figure that makes it worth citing.
4. **Say when the answer is not yours to give.** Where the answer belongs to a third party, a
   council, an insurer, a park, say so and name who to ask. That sentence is an asset.
5. **Keep a list of what only the operator knows.** Every project keeps an open questions file.
   Those answers are the content nobody can copy, and the file is the interview agenda.

**FAQ blocks are for readers, with `FAQPage` markup because it states plainly what the page
answers.** They are not the lever that earns citations. First-hand specificity is.

---

## How the three fit together

A reader arrives from an AI answer that quoted the range. The page confirms the figure, names what
moves it and answers the question they actually had. Their agent then books it, because the offer
and the action were published as data.

Any one missing breaks the chain. A site with great content and no published price is not quoted.
A site with a price and no action cannot be transacted with. A site with both and generic content
is not worth quoting in the first place.

## Sources

1. Danny Sullivan keynote, Google Search Central Live Toronto, April 2026, on non-commodity content.
2. *AI Optimization Guide*, Google Search Central, on agent readability and query fan-out.
3. schema.org `Offer`, `UnitPriceSpecification`, `ReserveAction`, `EntryPoint`.
4. A2A (Agent2Agent) protocol, Linux Foundation, for the agent card and JSON-RPC task interface.
