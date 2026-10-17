# Supply requisition approval queue — synthesis

Started 17 Oct 2026. One running file — corrected in place, not left as
stacked snapshots.

## In plain words
Halloran (handler, also runs the gear cage) told Sofia in a console
interview that Supply's requisition approval queue is slow — he waited 11
days for a cracked vest plate, and setting the "priority" flag didn't speed
anything up because everything sits in one undifferentiated queue. That's
the *only* evidence of this problem anywhere in this repo. There are no
Supply tickets, no Supply usage data, and no Supply code here to check it
against. Product already has a fix for exactly this ("Requisition approval
chains") committed for Supply's next release, 4.3. This also isn't my
product — I'm the Dispatch PM — so my job here is to size up what's known,
flag the gap, and hand it to whoever owns Supply, not to build a fix myself.

## What we know for certain (facts)
- Halloran filed three Supply complaints, unprompted, during a console
  interview on 5 Sep 2026: (1) requisition approval queue is slow and
  undifferentiated by priority, (2) field failure reports get no feedback
  after filing, (3) catalog search is weak. Source:
  `00-rook/feedback/interviews/halloran.txt`.
- His specific example: a requisition for a cracked vest plate sat 11 days
  waiting on a quartermaster signature. He set the "priority" field; it had
  no apparent effect on queue position — "it just sits in the same queue
  as somebody ordering a spare set of boot laces." Source: same interview.
- Supply's documented core flow is exactly one queue: requisition →
  quartermaster approval → fulfillment, with a single quartermaster-signoff
  step and a priority field that exists in the UI. Source:
  `00-rook/company/supply-one-pager.pdf`.
- "Requisition approval chains" is a **committed** Q3 roadmap item for
  **Supply 4.3** (driver: Internal). Per the roadmap's own rule, committed
  items are locked and changes go through Product. Source:
  `00-rook/company/roadmap-q3.pdf`.
- No Supply release notes exist yet past 4.2 in this repo, so I can't tell
  whether 4.3 has shipped, slipped, or what "requisition approval chains"
  actually does. Source: `00-rook/company/release-history.pdf` (stops at
  4.2, dated 12 Aug 2026).
- Zero of the 25 support tickets in this repo mention requisitions,
  quartermasters, or Supply at all (checked by text search). Source:
  `00-rook/feedback/tickets/t-001.txt` through `t-025.txt`.
- No Supply queue data, timestamps, or volume figures exist anywhere in
  this repo. `00-rook/data/callout-history.csv` is Dispatch-only (pings
  sent/taken per responder).
- There is no Supply-specific code in `00-rook/code/` — only
  `dispatch-routing/`.
- The team directory lists no one as owning Supply product day-to-day;
  Helen Achebe owns "Roadmap and commitments" across both Dispatch and
  Supply, Nadia (tickets) and Ravi (numbers) are shared across both
  surfaces. Source: `00-rook/company/who-does-what.xlsx`.
- CLAUDE.md's own working notes already flagged this: "Halloran ... raised
  three Supply complaints unprompted ... Not mine to fix; worth passing
  along."

## What we think, and why (hypotheses)
- **H1 — chronic design gap, not a recent regression.** Halloran's own
  language ("I've been sitting on it a while," no date tied to a change)
  describes a standing complaint about how the queue is built — one FIFO-ish
  queue with a priority field that doesn't change ordering — not something
  that got worse recently. Confidence: medium. What would confirm: Supply's
  own queue-time data over time, showing a flat (not spiking) approval-time
  trend. What would rule it out: a visible step-change in approval time
  around a specific date, which would point to a real regression instead
  (e.g., a quartermaster staffing change, a recent Supply release, seasonal
  load).
- **H2 — "way slower than usual" (the framing in this request) may not be
  an established fact.** Nobody in this repo's sources — including Halloran
  — describes the queue getting *slower*, only that it's *slow*, with no
  before/after comparison. Confidence: medium-high that the "slower than
  usual" framing is unverified rather than wrong — I simply have no data
  either way. What would confirm or rule out: Supply's own approval-time
  trend from Ravi or Supply's analyst equivalent.
- **H3 — the single-queue, non-decaying-priority design is sufficient on
  its own to explain long waits for urgent items**, independent of any
  volume change. Confidence: medium — it's a plausible mechanism consistent
  with the one example given (priority flag set, no effect), but it's one
  person's account of one requisition, not verified against the actual
  approval logic (no Supply code in this repo to check).

## What we've concluded (after checking hypotheses against evidence)
- **This is not resolvable from this repo as a standalone Dispatch
  investigation.** Every quantitative and multi-source qualitative check
  this method normally runs (Phase 2 data, Phase 3 reconciliation across
  tickets vs. interviews, Phase 4 mechanism-reading) has no Supply-side
  material to work with here. The honest conclusion is "evidence is thin
  and one-sided," not a root cause.
- **The fix Halloran is asking for is already a locked roadmap commitment**
  ("Requisition approval chains," Supply 4.3). Based on:
  `00-rook/company/roadmap-q3.pdf`. This changes the right next step from
  "diagnose and propose a fix" to "confirm the in-flight fix actually
  addresses what Halloran described, and get him an answer."

## Reconciliation: qualitative vs. quantitative
| Claim | Source | What the data shows | Verdict |
|---|---|---|---|
| Requisition queue is "way slower than usual" | Task framing / implicitly Halloran | No Supply queue-time data exists in this repo at any point in time | **Silent but real, unconfirmed** — can't be checked either way; treat as unverified, not as established fact |
| Priority flag has no effect on queue order | Halloran interview | Supply one-pager confirms a single queue + a priority field exists, but doesn't say how (or whether) priority affects ordering | **Partial** — consistent with the one-pager's design description, not independently confirmed from code or data |
| Supply queue problems are widespread | Task implies "Halloran's complaining," singular | Zero Supply mentions across 25 tickets; this repo has exactly one source (Halloran) | **Contradicted as "widespread," confirmed as "at least one real case"** — n=1 in this repo, severity of that one case (11 days on a cracked vest plate) is real regardless |

## Confidence (track this as a number over time, with dates)
- 17 Oct 2026: **25/100** — one credible but single-source anecdote,
  no quantitative corroboration available in this repo, no mechanism
  (code) to check, and the one thing that would most resolve it (a real
  queue-time trend) doesn't exist here. This number is about *causal
  certainty*, not about whether Halloran's 11-day case was real — I take
  that as a real, reported fact; the "way slower than usual" framing and
  any root-cause claim beyond "single queue, no priority differentiation"
  are what's unconfirmed.

## Open items this doesn't settle
- **Highest-leverage missing evidence: Supply's own requisition queue-time
  data** (per-requisition timestamps: submitted → quartermaster action),
  ideally split by priority flag and by whether a quartermaster was newly
  understaffed or request volume changed recently. This would resolve H1
  vs. a real regression, and confirm or kill the "way slower than usual"
  framing outright.
- Whether "Requisition approval chains" (committed, Supply 4.3) actually
  addresses priority-based triage, or something else entirely (e.g., just
  multi-step sign-off) — unknown from this repo; the roadmap line gives a
  title, not a spec.
- Whether 4.3 has already shipped, is in progress, or has slipped — no
  Supply release notes past 4.2 exist here.
- Who, specifically, owns Supply product day-to-day (no Supply PM appears
  in the team directory) — needed to know who to actually hand this to
  besides Helen.
- Halloran's other two unprompted Supply complaints (field failure reports
  going into a void with no feedback; weak catalog search) are out of
  scope for "the queue is slow" but came from the same source at the same
  time and are worth bundling into the same hand-off rather than losing.

## Recommended next steps
- Safe to do now: pass Halloran's full account (quote-level) to whoever
  owns Supply product (via Helen, since no Supply PM is listed) along with
  this synthesis, so it reaches the people already building the 4.3 fix
  while it's still in flight and can be shaped by a real case. Safe because
  it adds a concrete example to an already-committed initiative; it doesn't
  ask anyone to change direction.
- Safe to do now: ask whoever owns Supply data for the queue-time
  equivalent of what Ravi provides on the Dispatch side, so "way slower
  than usual" can actually be checked instead of assumed.
- Hold off on: proposing a specific fix design beyond illustrating the
  problem (e.g. priority tiers, SLA timers) as more than a conversation
  starter — Supply already has a committed, presumably-scoped item in
  flight, and designing competing specifics without knowing what 4.3
  already covers risks duplicating or contradicting work already planned.
- Hold off on: treating this as a Dispatch-side investigation with the
  same rigor as the 4.2 routing work — this repo simply doesn't contain the
  Supply-side material (data, tickets, code) that method depends on.
