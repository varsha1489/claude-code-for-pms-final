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
- **Marcus's 14 Aug question — now answered (16 Oct 2026):** he asked whether
  the routing change was *meant* to apply to responders who'd already been
  declining, or whether that "just fell out" of the config. Wen never
  replied, but the code settles it: the ranking formula recalculates fresh
  for every callout with no special-casing by history — it hits anyone with
  a lower current acceptance score uniformly, whether that's from a long
  decline history or just being new. Vesper (one of the 4 collapsed
  responders, previously one of the *best* acceptance records) is direct
  evidence it isn't decliner-specific. Reply sent to Marcus; full reasoning
  in `notes/responder-priority-and-pov.md`.
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
  release.** Correction: total offers sent actually fell too (~6-7%, not flat
  as earlier noted here), but nearly all of that drop traces to the 4
  collapsed responders — a redistribution signature, not fewer incidents
  happening. Still cuts against "it's just August."
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

### Interview signal check (14 Oct 2026)
Four console-redesign interviews (`00-rook/feedback/interviews/`, Ambrose, Aunt
Dot, Halloran, Kip) weren't about 4.2 directly, but three of four brought up
callouts moving on too fast anyway — unprompted, which is itself worth noting.
Full triage of all eight themes, sorted by priority with quotes, is in a
published artifact called **Console Feedback Triage** (ask me to reopen or
rebuild it; it isn't a file in this repo).
- Matches the data: 3/4 describe a callout vanishing before the responder
  could answer. One handler (Ambrose) independently suspects routing favors
  faster responders over slower ones now — unprompted, and consistent with
  the known 4.2 weight change.
- Two handlers (Kip, Aunt Dot) independently noticed the same feast-or-famine
  pattern — dead quiet some weeks, gone instantly in others — that matches
  the 4-responders-collapsed finding above.
- The rest (readability, alert routing, filter persistence, dark mode) is
  real but unrelated UX debt — don't let it crowd out the 4.2 investigation.
- Halloran (also runs the gear cage) raised three Supply complaints unprompted
  — slow/undifferentiated approval queue, field failure reports with no
  feedback loop, bad catalog search. Not mine to fix; worth passing along.

### Responder priority (15 Oct 2026)
Full ranked table, the pre/post methodology, and a regroup-ready point of
view are in `notes/responder-priority-and-pov.md`. The one thing to not
forget: of the 4 collapsed responders, **Meteor Mite and Vesper filed zero
tickets** — they're only known from interviews. Ticket volume alone would
have missed Dispatch's two worst cases.

### Pile comparison (15 Oct 2026)
- Interviews and tickets draw from almost entirely different responders —
  only Farlight and The Undertow appear in both piles. Neither pile alone
  would have found the worst cases: tickets-only misses Meteor Mite and
  Vesper entirely; interviews-only misses that "gone quiet" is the dominant
  complaint (16 of 25 tickets) and has no dates precise enough to tie the
  drop to the release rather than to seasonality.
- Tickets never theorize a cause. The routing-fairness hypothesis exists
  only because Ambrose said it, unprompted, in an interview.
- The console UX backlog (dark mode, readability, alert routing, filter
  persistence) appears in **zero** of the 25 tickets — it's real, but don't
  weigh it against the 4.2 investigation using ticket volume as the yardstick.
- `notes/responder-priority-and-pov.md` was revised: raw frequency counts
  replaced with a corroboration-vs-contradiction framework (sources
  *agreeing* vs. sources *contradicting* aren't the same kind of evidence),
  a sourcing error fixed (Farlight has no interview mention — that citation
  was actually Aunt Dot's account of Vesper), and Captain Vantage
  reclassified as the key informant behind the routing-fairness theory
  rather than a casework priority, since his own data shows no issue.

### Root-cause investigation: five lenses converged (16 Oct 2026)
Five independent analyses (code mechanism, tickets vs. data, interviews,
redistribution patterns, timing) each formed their own hypothesis from this
repo alone, then debated each other. All five converged: the 4.2 routing
rebalance plus a decline/timeout penalty that never decays created a
self-reinforcing trap — a responder ranked low gets offered less, which
lowers their score further, which gets them offered even less, with nothing
in the code to reverse it on its own. Seasonality was ruled out by every
lens, including the one looking for evidence of it. Full debate and the
open "two entry paths" wrinkle (Vesper vs. Meteor Mite) are in
`notes/responder-priority-and-pov.md`.
- **No built-in recovery exists.** Checked directly against the code: the
  only way a frozen-out responder's score improves is accepting an offer —
  but they can't get offered one while ranked low, and nothing decays over
  time. Only a lucky nearby incident, a code fix, or possibly a handler
  override (unconfirmed whether that even feeds the scoring) could break
  the loop.
- **Top open ask, named independently by every agent:** per-responder
  location/travel-time data — nobody can confirm *why* these specific four
  collapsed without it.

### 4.2 is config-only — and the bug predates it (16 Oct 2026)
Confirmed from the code's own comments and the changelog (not an independent
git diff — this repo has no version history to verify against, so treat as
documented, not audited): 4.2 changed exactly three numbers in
`config.py` — the timeout (90→60s) and two weights (proximity 0.45→0.60,
acceptance 0.40→0.25). No logic changed in `routing.py`, `offer.py`, or
`history.py`. **The no-decay penalty bug itself is not new** — Wen's
comment flagging it is dated 2019, years before 4.2. 4.2 didn't create the
trap; it changed the dial settings that exposed it. Scoring is simple and
asymmetric: exactly one function anywhere in the code adds points
(`record_accepted`, +0.08, only on an accepted job); `record_declined`
(a decline *or* a timeout, treated identically) subtracts 0.12, and nothing
else ever moves a score up.
- **Spec vs. shipped, checked against the Q3 roadmap:** 2 of 3 committed
  4.2 items matched what shipped (routing rebalance, timeout tuning).
  Console filter persistence shipped **without ever being a roadmap
  commitment** — scope that appeared without being planned, the mirror
  image of Availability Confidence being planned and dropped (below).
  No numeric targets exist in any doc, so even the two "matching" items
  can only be checked against the code, not against a written spec.

### Recommended next steps (16 Oct 2026)
Full reasoning in `notes/responder-priority-and-pov.md`. Short version:
send the Ravi location/travel-time request now (don't wait for the
regroup); push the no-decay fix regardless of what that data shows, since
it's safe either way; do something this week for the 4 trapped responders
(handler override as a stopgap) rather than waiting on a code fix; at the
regroup, isolate the bug from the "should we revert 4.2" question — don't
relitigate the weight change itself; hold off on recommending the weight
numbers change back until the proximity data confirms or kills that theory.

### Director brief and prototypes (17 Oct 2026)
Answered Helen's note in `05-super-speed/director-request.txt` with
`05-super-speed/brief.md` — the **Second Chance** proposal (a guaranteed
offer after 7 days of silence, travel-range safeguard, early-warning tier
for decliners not yet at zero) — and `director-ask-one-pager.md` (what she
actually asked for, kept separate from the answer). Built 3 clickable
prototypes, each saved under `05-super-speed/` and refined through two
rounds of simulated feedback in character as the most-affected responders
(Vesper, Meteor Mite, Farlight, The Undertow):
- **Guaranteed Offer Override** — the manual stopgap; now has a 3-minute
  timeout window, capped retries with handler escalation instead of an
  implied infinite wait, and a "tell the responder" visibility default.
- **Ticket Data Reconciliation** — for Nadia's team; now has a "Next step"
  on every verdict and a severity-tiered "Needs attention" panel that finds
  declines automatically, ticket or no ticket.
- **Why Wasn't I Offered** — the routing explainer; now visually
  distinguishes real vs. assumed inputs, links low scores to the actual
  fixes, and states who sees it and that it's a snapshot, not a verdict.
- A recurring theme across all three feedback rounds: "flagged" or
  "confirmed" isn't the same as "acted on" — worth remembering as a
  standing requirement for anything built next, not just these three.
- **Confidence in the overall direction: 72/100** (see
  `notes/responder-priority-and-pov.md`) — the engineering fix
  recommendation is now sharp and data-backed, but Ashgrove/Halfmoon's case
  still isn't explained by scoring at all, so the Ravi proximity-data ask
  remains the open item.

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
- **Explain things in plain, simple language by default** — short sentences,
  everyday words, no jargon. Give me the detailed/technical version only
  when I ask for it, or when I'm clearly in document/spreadsheet mode myself.
- Keep facts (from the docs), hypotheses (someone's read) and my own conclusions
  clearly separated. Name the source when it matters.
- For any metric claim, ask which data it comes from (Ravi's weekly, Marcus's
  rough pull, or ticket counts), and keep coverage gaps out of acceptance.
- With 4.2, several changes plus seasonality are moving at once. Push to separate
  them before attributing cause.
