# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

I'm the new PM for **Rook Dispatch**, taking over from Priya Raghunathan, who left
on 21 Aug 2026 with no overlap. Sources are in `00-rook/company/`; these notes
reflect them as of 2 Sep 2026.

### The company
- Rook sells coordination and provisioning software to independently operating
  masked responders, their **handlers** and **quartermasters**. Publicly it is a
  "logistics and workforce-coordination vendor". Rook does not employ responders.
- Revenue is a subscription priced per active responder. Releases ship on a monthly
  train numbered 4.x, and Dispatch and Supply release on the same numbers.
- **Confidentiality is a hard constraint.** No cover identity is stored, and Rook has no
  link to anyone's legal identity. Never propose features, analyses or data joins
  that assume or try to work out who a responder is. Security Policy 4.1 governs this.

### The products
- **Dispatch** (mine): an incident arrives in the web console, Dispatch ranks the
  available responders, and it offers the callout to the top one on mobile. If they
  accept, the incident is assigned. If they decline or time out, it goes to the next.
  Handlers use the console (they can override routing); responders use mobile.
  Routing config ships with the release and is not a setting handlers can change.
- **Supply**: requisitions, quartermaster approval, maintenance schedules and
  field failure reports. Handlers and quartermasters use it.
- **Shared dependency:** Dispatch writes the **Responder Availability Record** and
  Supply reads it to schedule maintenance into low-callout windows. Any Dispatch
  change to availability lands in Supply with no action on their side. Check this
  for blast radius whenever we touch availability.

### Metrics
- **Acceptance rate** (headline): accepted ÷ offers, where declines and timeouts both
  count against it. Reported weekly in aggregate.
- **Time-to-accept**: median seconds from offer to accept.
- **Coverage gap**: no available responder had the required tags. "Nobody *could*
  go" is different from low acceptance ("nobody *would*"). Keep the two separate.

### Vocabulary
- **Callout**: a request for a responder to attend an incident. **Callout offer**:
  one callout offered to one responder. **Timeout**: how long an offer stays live
  (the same for everyone). **Decline**: an active refusal, recorded separately from
  a timeout in the data.
- **Routing priority**: the score that ranks responders. Inputs are proximity
  (travel time since 4.1), availability, capability match and *recent acceptance
  history*. Declines and timeouts lower the acceptance component, which lowers
  priority until it recovers.
- **Capability tags**: flight, structural-entry, hazmat-tolerant, cold-weather,
  aquatic, crowd-management, de-escalation.
- **Mutual aid / shared cover**: responders covering for each other across
  regions. Not supported; on the Q4 exploration list.
- Supply terms: **requisition**, **field failure report**, **service interval**.
- Colloquially, "who gets pinged" = routing and "ping timeout" = callout timeout.

### People
| Who | Role | Notes |
|---|---|---|
| Helen Achebe | Director of Product (my manager) | Owns roadmap and commitments. Chicago. |
| Marcus Oyelaran | EM, Dispatch | Candid. Go-to for anything unclear. Chicago. |
| Wen Li | Staff Eng, Dispatch | Built routing, and there is no doc of how it works, so ask her. Berlin. |
| Sofia Marino | Product Designer | Console and mobile app. Chicago. |
| Nadia Hoffmann | Support Lead (both surfaces) | Sees complaints first. Is tracking the 4.2 ticket split. Berlin. |
| Ravi Menon | Data Analyst (both surfaces) | **Owns the official weekly acceptance numbers.** Requests go through #data. Singapore. |

Priya's handover sends you to Marcus for numbers, but the real weekly series is
Ravi's. Marcus has said his numbers would be "rough".

### Where things stand: 4.2 (shipped 12 Aug 2026)
4.2 bundled three changes:
1. **Routing rebalance**: proximity weighted up relative to recent acceptance
   history. Responders across wide geographies had asked for it for a long time.
2. **Timeout cut from 90s to 60s.**
3. Console filter persistence (cosmetic; Priya expects noise tickets), plus 3 bug fixes.

Since the release, acceptance is reportedly down and callout tickets are about
3× normal. The split is roughly ⅔ "my phone never goes off" and ⅓ "it was gone
before I could answer".

- The 60s timeout plausibly explains the ⅓. Nobody has explained the ⅔.
- **Open question nobody answered:** on 14 Aug Marcus asked whether the routing
  change was *meant* to apply to responders who have been declining, or whether that
  "just fell out" of the config. Wen was on PTO and never replied.
- **Priya's view** is that the drop is mostly seasonal ("August is always soft") and
  should recover in September, and she warns against relitigating or reverting
  4.2. This was **a hypothesis, not a finding** — and the one data file in the repo
  doesn't support it (see below). September/October data still isn't in the repo.
- Marcus deliberately held off on conclusions so the new PM could look first.
  A team regroup on 4.2 is pending.

### First look at the data (14 Oct 2026)
`00-rook/data/callout-history.csv` — weekly `pings_sent`/`pings_taken` per
responder, 29 Jun–31 Aug 2026. Not yet confirmed as Ravi's official export;
that's the first thing to ask him (see `notes/ravi-1-1-agenda.md`).
- **The "September rebound" in the pooled rate is an artifact.** Pooled weekly
  acceptance goes 0.77 → 0.54 → 0.73, which looks like recovery. But 4 of 16
  responders (Farlight/Linda Pruitt, Meteor Mite/Kip, The Undertow/Desmond Okafor,
  Vesper/Aunt Dot) collapsed from ~12 offers/week to 0–1 and dropped out of the
  denominator. The other 12 took on more volume and still sit below their pre-4.2
  rate. The unweighted per-responder average is 0.59, not 0.73 — ask Ravi which
  one is "the" headline.
- **Tickets and the data disagree on who's affected.** 11 responders were
  reported quiet in tickets; only the 2 above (Farlight, The Undertow) actually
  collapse in the CSV. Nightwell's ticket says nothing in 10 days; the CSV shows
  her getting 16–21 offers/week throughout. Worth resolving before trusting
  either source alone.
- **Acceptance didn't drift seasonally — it stepped down exactly at the 12 Aug
  release**, while total offers sent stayed flat. That cuts against "it's just August."
- **In the routing code** (`00-rook/code/dispatch-routing/`), a decline and a
  timeout are scored identically, and the penalty (0.12, vs. a 0.08 credit for
  accepting) never decays — a 2019 TODO from Wen flags this exact question and
  was never resolved. Combined with the shorter timeout, this is the likely
  mechanism for why a few responders get frozen out, but that's a read of the
  code, not a tested conclusion.
- **Possible doc conflict to verify:** `about-rook.pdf` says cover identities are
  never stored; the glossary says Rook *does* hold them, just with no mapping to
  a legal identity. Don't rely on either until checked against Security Policy
  4.1 (not in this repo).

### Roadmap loose ends (Q3 roadmap, rev. 30 Jun)
- **Availability Confidence**, a confidence score shown next to stated availability
  and driven by support escalations, was *committed* for 4.2 but **does not appear in
  the 4.2 release notes.** It appears to have been quietly dropped. Committed items
  are "locked" and changes must go through Product, so raise it with Helen.
- Priya asked for a conversation with Helen about which squeezed-out items are still
  commitments. It has not happened.
- Other items: Supply requisition approval chains (4.3, committed); handler phone
  app and shared cover between responders (Q4, exploring).

### Asks inherited from Priya
- Write the first description of how routing decides who gets pinged (with Wen).
- Use the newcomer advantage in month one to examine calls nobody has looked at closely.

### How to help me
- Keep facts (from the docs), hypotheses (someone's read) and my own conclusions
  clearly separated. Name the source when it matters.
- For any metric claim, ask which data it comes from (Ravi's weekly, Marcus's
  rough pull, or ticket counts), and keep coverage gaps out of acceptance.
- With 4.2, several changes plus seasonality are moving at once. Push to separate
  them before attributing cause.
