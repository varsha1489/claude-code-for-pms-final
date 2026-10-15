# Responder priority & point of view

Written 15 October 2026, from `00-rook/data/callout-history.csv` (29 Jun–31
Aug 2026, weekly `pings_sent`/`pings_taken`) cross-referenced against the 25
support tickets and the 4 console interviews. See `CLAUDE.md` for the
underlying findings this builds on.

## In plain words
On August 12, Rook changed how it picks which responder gets asked to help
first. Right after that, something broke.

**The big number:** before the change, about 132 jobs got accepted every
week. After, it's about 111 a week. That's real help not going out, every
week, for three weeks straight.

**The four people hit hardest:** Farlight, Meteor Mite, The Undertow, and
Vesper basically stopped being asked to help at all. Each used to get 10–14
jobs a week; by the end of August most were down to 0 or 1 — and it never
got better. Every week it got a little worse, not better.

**Why:** it looks like once someone gets skipped a few times, the system
starts skipping them even more — like a bad grade that never gets erased, so
it keeps dragging the average down. The code backs this up: the part meant
to let a bad mark fade over time was never built.

**The surprising part:** the people complaining the loudest in support
tickets aren't the ones in the most trouble. Three people (Ironvale,
Nightwell, Stormwrack) wrote in a lot saying "I'm not getting work" — but the
numbers show they were actually getting *more* work than before. Meanwhile
the two worst-off people, Meteor Mite and Vesper, never filed a single
complaint. We only found out about them because their handlers mentioned it
in an unrelated conversation.

**So what:** if you only listen to who complains the loudest, you'd help the
wrong people and miss the two who needed it most.

**One line for the boss:** *"Since the August change, some of our most
reliable responders got stuck in a trap where the system stopped asking them
for help — and the people hurt the worst aren't even the ones telling us
about it."*

## Method note: the pre/post split
4.2 shipped mid-week (Tue 12 Aug), inside the CSV's `2026-08-10` week. To
avoid blending a few days of old behavior with a few days of new behavior
into one data point, that week is **excluded** from both sides:
- **Pre:** 6 full weeks, 29 Jun–3 Aug
- **Excluded:** week of 10 Aug (the release itself — also the single sharpest
  drop in the dataset, pooled rate 0.54)
- **Post:** 3 full weeks, 17–31 Aug

**Same before/after:** total offers sent per week stayed flat (~170–177 pre,
~158–165 post) — incident demand didn't change, which rules out "quiet
month" as the explanation.

**Different before/after:** pooled acceptance rate fell from ~0.77 to
0.66–0.73 and hasn't fully recovered; even the 12 responders who didn't
collapse still sit below their pre-4.2 rate (~0.77 → ~0.69–0.74); and the
flat total volume became concentrated on fewer responders, because 4 of the
16 collapsed toward zero.

## Priority: corroboration, not raw frequency
*(Revised 15 Oct 2026.)* The first pass scored "frequency" as a flat count of
tickets + interview mentions. That conflated two different things: **sources
agreeing** (strong signal) vs. **sources contradicting** (an open question,
not evidence). Ironvale, Nightwell and Stormwrack all had "high frequency"
under the old count, but their tickets and the CSV actively disagree — that's
not corroboration. Captain Vantage also doesn't belong with them: his own
data shows no issue, but he's the one source behind the routing-fairness
theory — a key informant, not a casework priority.

**Correction:** Farlight was previously listed with an interview mention
("handler-phone-app brief"). That brief actually cites Aunt Dot's account of
**Vesper**, not Farlight — Linda Pruitt (Farlight's handler) never appears in
any interview or brief. Farlight is data + tickets only.

| Responder | Data | Offer collapse (`pings_sent` trend, pre-rate) | Tickets | Interview | Sources | Tier |
|---|---|---|---|---|---|---|
| The Undertow | Collapsed | 11→1/wk, monotonic, no bounce; pre-rate 0.78 (avg) | 3 (incl. High) | — | Agree | Confirmed & urgent |
| Vesper | Collapsed | 12→1/wk, monotonic, no bounce; pre-rate 0.82 — **highest of the 4**, argues against "already weak" | 0 | Yes | Agree (data + interview only) | Confirmed & urgent — invisible to tickets |
| Meteor Mite | Collapsed | 10→1/wk, monotonic, no bounce; pre-rate 0.69 — lowest of the 4 | 0 | Yes | Agree (data + interview only) | Confirmed & urgent — invisible to tickets |
| Farlight | Collapsed | 10→0/wk, monotonic, no bounce; pre-rate 0.74 | 1 | No | Agree | Confirmed & urgent |
| The Drift | Declining | — (sent held, rate dipped 14 pts instead) | 2 | — | Agree | Confirmed & urgent |
| Sgt. Falkirk | Declining | — | 2 | — | Agree | Confirmed & urgent |
| Corporal Ashgrove | Declining | — | 2 | — | Agree | Confirmed & urgent |
| Cindermark | Declining | — | 2 | — | Agree | Confirmed & urgent |
| Ironvale | Stable/rising | — | 3 | — | **Contradict** | Unresolved — don't act yet |
| Nightwell | Stable/rising | — | 3 | — | **Contradict** | Unresolved — don't act yet |
| Stormwrack | Stable/rising | — | 2 | — | **Contradict** | Unresolved — don't act yet |
| Halfmoon | Mild | — | 2 | — | Roughly consistent | Monitor |
| The Longcast | Mild | — | 2 | — | Roughly consistent | Monitor |
| Captain Vantage | Stable | — | 1 | Yes | Not about him | Key informant, not a casework priority |
| Sgt. Bulwark | Stable | — | 0 | Yes (passing) | No real signal | Noise |
| The Gale | Stable, positive | — | 0 | Yes (contrast) | No real signal | Noise |

*"Offer collapse" is pulled from `pings_sent`, not `pings_taken` — it shows
these four stopped being offered callouts, not that they started declining
more of what they got. Only the 4 collapsed rows have this pattern; for
everyone else, sent volume held or rose (see "The Drift" row — its problem
is in the rate, not the offer count). Full breakdown and the monotonic-decline
chart are in the section below.*

### Ranked order
1. **The Undertow** — collapsed (0.78→0.17), 3 tickets incl. one High severity. Highest severity, highest confidence.
2. **Vesper** — collapsed (0.82→0.12), **zero tickets**. Only surfaced via an unrelated design interview.
3. **Meteor Mite** — collapsed (0.69→0.14), **zero tickets**. Same blind spot as Vesper.
4. **Farlight** — collapsed (0.74→0.00), 1 ticket despite the worst accept rate in the set.
5. **The Drift** — accept rate fell 14 pts even with volume holding; same-day paired tickets from handler and responder.
6. **Sgt. Falkirk** — real decline, but oscillating ("on and off") rather than a clean drop.
7. **Corporal Ashgrove** — steady, unresolved decline; no loss event yet.
8. **Cindermark** — shows the quiet-then-lose-it trap directly in his own ticket history.
9–11. **Ironvale, Nightwell, Stormwrack** — don't act on these yet. Their tickets claim silence the CSV contradicts; resolve what `pings_sent` actually counts before trusting either source.
12–13. **Halfmoon, The Longcast** — mild, low stakes, tickets and data roughly agree.
14. **Captain Vantage** — not a casework priority; his data is fine. He's the one source behind the routing-fairness theory and should be tracked as that, separately from his personal case.
15–16. **Sgt. Bulwark, The Gale** — no real signal either way.

**Why this matters:** ranking by ticket volume alone would start with Ironvale or Nightwell (3 tickets each) and never surface Meteor Mite or Vesper, who are in worse shape than anyone filing tickets. Rows 2 and 3 are worth raising specifically because no one else will.

## Point of view (for the regroup)
> Since 4.2, we're not seeing a slow dip in engagement — we're watching
> individual responders get trapped. The Undertow's handler reports he "lost
> it before he could answer... gone by the time he'd even finished reading
> who it was for," after two silent weeks, and Aunt Dot describes the same
> dynamic as a race her responder "loses... more than he wins it." What makes
> this urgent rather than something to study next quarter is that our two
> worst-hit cases, Meteor Mite and Vesper, never generated a single support
> ticket — the system is actively hiding its most severe failures from the
> channel we rely on to catch them. Every week we wait, more responders cycle
> through this same quiet-then-lose-it loop, which means the cost of delay
> isn't flat — it compounds, and it's compounding on the people who trust us
> most.

## Why the 4 collapsed responders never recovered (16 Oct 2026)
Looking at `pings_sent` alone, not just the rate, for Farlight, Meteor Mite,
The Undertow and Vesper:

| Week | Farlight | Meteor Mite | The Undertow | Vesper |
|---|---|---|---|---|
| 10 Aug | 10 | 10 | 11 | 12 |
| 17 Aug | 3 | 4 | 4 | 5 |
| 24 Aug | 1 | 2 | 1 | 2 |
| 31 Aug | 0 | 1 | 1 | 1 |

- **They aren't declining more — they've simply stopped being offered
  callouts.** `pings_sent` collapsed along with `pings_taken`, which places
  this in routing/ranking, not responder behavior. If they were still being
  asked and just saying no more, sent would've stayed flat while taken fell;
  it didn't.
- **All four decline monotonically, every week, with zero bounce**, across
  the full 3 weeks of post-release data available — unlike the other 12,
  whose rate visibly recovers week over week. That shape (still falling, not
  plateauing) is what a self-reinforcing loop looks like: offered less → no
  chance to accept → ranked lower next week → offered even less. It's
  consistent with the no-decay decline/timeout penalty already flagged in
  `history.py`, though the CSV alone can't prove the mechanism (no
  decline/timeout split, no score field).
- **Pre-release acceptance rate doesn't predict who collapsed.** Vesper's
  pre-release rate (0.82) was the *highest* of the four — above the group
  average — which argues against "these responders were already less
  reliable." It points instead toward something structural they share, most
  plausibly proximity/travel-time, since 4.2 explicitly shifted weight from
  acceptance history (0.40→0.25) toward proximity (0.45→0.60). The CSV has
  no location field, so this stays an inference from the weight change, not
  something the data proves directly — region or travel-time data from Ravi
  would settle it.

## Tickets vs. the data: agree on *when*, not on *who* (16 Oct 2026)
Comparing all 25 tickets (`00-rook/feedback/tickets/`) against
`callout-history.csv` directly.

**Timing agrees.** First ticket (T-001) is dated 13 Aug — the day after the
release — landing in the exact week the CSV already shows its sharpest drop.
No lag in aggregate.

| Week | Tickets filed | Pooled accept rate | Offers accepted |
|---|---|---|---|
| 10 Aug (release) | 2 | 0.542 | 96 |
| 17 Aug | 8 | 0.658 | 104 |
| 24 Aug | 8 | 0.667 | 108 |
| 31 Aug | 7 | 0.727 | 120 |

**But the two series point opposite directions after that.** The pooled rate
climbs every week — looks like recovery. Ticket volume jumps immediately and
then **stays flat at 7–8/week** for the rest of the file. Whatever's
improving in the aggregate rate isn't making complaints go down — consistent
with the rate recovering partly because the 4 collapsed responders drop out
of the denominator, not because the problem resolved.

**Who is affected disagrees badly, in both directions.**

| Responder | Ticket claim | What the data shows | Verdict |
|---|---|---|---|
| The Undertow | Quiet, then lost one (3 tickets) | Collapsed, 12→1/wk | Agree |
| Farlight | Quiet ~2 wks (T-018, filed 30 Aug) | Already near-zero by **17 Aug** — 13-day lag before anyone filed | Agree on substance, large lag |
| Meteor Mite | — (0 tickets) | Collapsed, 10→1/wk | **No ticket ever filed** |
| Vesper | — (0 tickets) | Collapsed, 12→1/wk | **No ticket ever filed** |
| Ironvale | "No callouts since the 12th," "still nothing," finally offered after a month and lost it | Sent **rose** every week: 8→9→10→11→12 | Flat contradiction |
| Nightwell | "Nothing in 10 days" (3 tickets) | Sent **rose**: 15→16→18→20→21 | Flat contradiction |
| Stormwrack | "Quietest in 2 years" (2 tickets) | Sent **rose**: 13→14→16→18→19 | Flat contradiction |
| Cindermark | "Quiet for 10 days" | Sent held steady; it's the *rate* that fell (0.80→0.71) | Right problem, wrong description |
| The Drift | "Barely anything, 2 callouts total" | Sent held steady; rate fell 14 pts instead | Right problem, wrong description |

Two distinct failure modes: Ironvale/Nightwell/Stormwrack are flatly wrong
about volume (their offers never dropped). Cindermark/The Drift aren't wrong,
exactly — they're describing a *rate* problem (losing more of what they get)
in the language of a *volume* problem ("quiet"). The CSV distinguishes those;
tickets alone can't.

**Bottom line:** if ticket volume were the only guide to who's hurting and
whether it's improving, both conclusions would be wrong — wrong about who
(chasing Ironvale/Nightwell/Stormwrack over Meteor Mite/Vesper), and wrong
about trajectory (reading flat ticket volume as "still bad" while the pooled
rate says "recovering," when the raw accepted-callout count is still down
16% three weeks out — see the POV above).

## Two responders, side by side (16 Oct 2026)
One confirmed collapse, one of the nine "unconfirmed" responders (reported
quiet in tickets, but not one of the 4 who actually collapse in the CSV) —
same week-by-week format, to show what each pattern actually looks like in
the raw rows.

### Vesper (handler: Aunt Dot) — confirmed, and invisible to tickets
| Week | Pinged | Took |
|---|---|---|
| 29 Jun | 14 | 11 |
| 06 Jul | 13 | 10 |
| 13 Jul | 15 | 13 |
| 20 Jul | 14 | 11 |
| 27 Jul | 13 | 11 |
| 03 Aug | 14 | 12 |
| 10 Aug (release) | 12 | 6 |
| 17 Aug | 5 | 1 |
| 24 Aug | 2 | 0 |
| 31 Aug | 1 | 0 |

Six steady weeks, then a dip the release week (12 pinged, only 6 taken), then
a cliff: 5 → 2 → 1 pinged over the next three weeks, never recovering. She
never filed a ticket — the only reason we know her story is Aunt Dot's
interview.

### Nightwell (handler: Marjorie Sung) — unconfirmed, filed 3 tickets
| Week | Pinged | Took |
|---|---|---|
| 29 Jun | 15 | 11 |
| 06 Jul | 14 | 10 |
| 13 Jul | 15 | 12 |
| 20 Jul | 16 | 12 |
| 27 Jul | 15 | 11 |
| 03 Aug | 15 | 12 |
| 10 Aug (release) | 16 | 9 |
| 17 Aug | 18 | 14 |
| 24 Aug | 20 | 13 |
| 31 Aug | 21 | 15 |

Six steady weeks, a real dip the release week (16 pinged, only 9 taken) —
then pinged *more* than ever: 18 → 20 → 21, still taking most of them. She
filed three tickets in that same window saying she'd gone quiet ("nothing in
10 days," filed 22 Aug) while the data shows the opposite. Either she
genuinely wasn't noticing the pings — pointing at an alerts problem, not a
routing one — or `pings_sent` isn't counting what her ticket describes.
Either way, trusting her ticket alone would send the investigation in the
wrong direction.

**The contrast is the point:** both responders show the same real dip in the
release week. One keeps falling and is silent in support. The other recovers
in the data and is loud in support. Frequency of complaint and severity of
the actual problem are not the same thing.

## Five-agent root-cause investigation (16 Oct 2026)
Five independent investigations, one per lens, each working only from this
repo's files, then shown each other's findings and asked to debate and try
to converge.

**In plain words:** the August 12 change made the app care more about *how
close* a responder is and less about *how often they say yes*. At the same
time, there's a bug: once someone misses or turns down a job, that mark
against them never goes away on its own. Put those together, and a responder
who starts out slightly disadvantaged — most likely because they're farther
away — gets offered less, which drags their score down further, which gets
them offered even less. A trap with no way out built in. The shorter
60-second window is a second, separate problem: it makes *everyone*
occasionally lose a job they'd have caught before, but it isn't what's
trapping the four hardest-hit responders.

### Round 1 — five lenses, five independent hypotheses
| Lens | One-line hypothesis | Confidence |
|---|---|---|
| Code & mechanism | Proximity weight up (0.45→0.60) + acceptance weight down (0.40→0.25) + a hard 45-min proximity floor (score = 0.0 past it) + a non-decaying decline/timeout penalty (−0.12 vs. +0.08 for accepting) creates a self-reinforcing trap; `offer.dispatch()` stops at the first acceptance, so ranked-low means rarely-reached. | Medium |
| Tickets vs. data | Two separate problems are tangled in the tickets — a broad timeout effect, and a narrow routing-driven collapse — but tickets are a poor proxy for *who*: most "gone quiet" tickets name responders whose volume was flat or rising; the two worst actual cases (Meteor Mite, Vesper) filed none. | Medium-high on unreliability, medium on why |
| Interviews / qualitative | Three of four handlers raised a version of "offers vanish/feast-or-famine" unprompted, in interviews that weren't about 4.2; Marcus independently flagged the same undifferentiated decline/timeout penalty in Slack, 3 days after launch. | Medium |
| Redistribution & volume patterns | 4.2 redistributed offers along an axis not in the CSV (most likely geography) — losses (40.67/wk) and gains (35.33/wk) are close in size but spread across many responders, not 1-2 pairs. One handler (Kip) has both a collapsed responder (Meteor Mite) and the single biggest gainer (The Gale) — rules out handler-level cause for that pair. | Medium |
| Timing & seasonality | Onset lands exactly in the release week; the first full post-release week shows the cliff; it keeps worsening for 3 straight weeks with zero reversion — the signature of a step-function change, not a seasonal dip. Flagged a correction: total weekly offers actually fell ~6-7% post-release, not flat as previously written here. | Medium-high that it isn't seasonal |

### Debate round — where they converged
All five, independently, arrived at close variants of the same statement
after reading each other's work — including the timing specialist, whose
whole job was to find evidence *for* seasonality:

> 4.2's routing rebalance (proximity weighted up, recent-acceptance history
> weighted down) combined with a decline/timeout penalty that never decays
> created a self-reinforcing trap: a responder who starts slightly
> disadvantaged gets offered less, which lowers their score further, which
> gets them offered even less — with no built-in way to recover. The shorter
> 60-second timeout is a separate, broader problem that makes everyone
> occasionally lose a job they'd have caught before, but it isn't what
> trapped these four specifically. The seasonal explanation doesn't hold up
> against any of the five lenses.

**What moved people:** Agent 4's finding that pre-release acceptance rate
does *not* predict who collapsed (Meteor Mite was the lowest of all 16 at
0.687; Vesper was 3rd-highest at 0.819) pushed Agents 1 and 3 to rank
proximity/geography as the dominant lever rather than the acceptance-penalty
spiral alone. That same finding made Agent 5 (timing) drop their own
seasonality hedge — if a real seasonal dip were driving this, the
highest-acceptance responders shouldn't be among the casualties.

### What's still open — the one thing every agent asked for
All five, independently, named the same gap: **no responder location or
travel-time data exists anywhere in this repo.** The mechanism is
well-supported; *who* falls into it (and why Vesper, with a strong
pre-release record, collapsed as hard as Meteor Mite, who didn't) can't be
confirmed without it. This is now the single highest-leverage ask for Ravi,
above even the pings_sent definitional question.

**A wrinkle, not yet resolved:** Meteor Mite's profile (already the weakest
acceptance rate of all 16) fits a straightforward "already a bit shaky, then
crushed" story. Vesper's (3rd-highest acceptance rate) doesn't. There may be
**two different entry paths into the same trap** — one via geography, one via
an unlucky run of declines/timeouts — rather than one uniform cause.

### Where the agents pushed back on each other
- The interviews agent treated three handlers independently describing
  "feast or famine" as strong corroboration. Two other agents pushed back:
  several tickets making the identical claim (Ironvale, Nightwell,
  Stormwrack — "nothing in weeks") are flatly false against the CSV, raising
  the question of whether people were repeating a story they'd heard rather
  than reporting what actually happened to them.
- The timing agent argued the *silent, no-offer* problem (⅔ of tickets) is
  the one best explained by the data, and cautioned against the more
  dramatic, easier-to-tell "vanished right in front of me" stories (⅓ of
  tickets) pulling attention toward the smaller problem.

## How the code explains every data finding, step by step (16 Oct 2026)
Walking `00-rook/code/dispatch-routing/` from "incident happens" to "phone
buzzes," and tying each step directly to a finding above.

1. **Ranking weighs proximity highest now (`routing.py`) → explains the
   offer collapse.** If the 4 collapsed responders are consistently farther
   from incidents, they now rank near the bottom almost every time — which
   is why their `pings_sent` fell, not just `pings_taken`.
2. **The decline/timeout penalty never decays (`history.py`) → explains why
   they never bounced back.** The only way to repair standing is to accept a
   job — but you can't accept a job you're never offered. Not "got worse and
   worse," but "nothing was ever going to let them climb back out."
3. **The 60-second clock (`config.py`, `offer.py`) → explains the "it
   vanished" complaints**, a separate, more widespread pattern that doesn't
   require being far away or having a bad record — just a short enough
   window that a normal response gets missed sometimes. Matches why Ambrose,
   Aunt Dot, Halloran, and several ticket-filers outside the collapsed group
   all describe near-misses.
4. **Nobody's removed from the list; the search stops at the first "yes"
   (`routing.py`, `offer.py`) → explains the redistribution.** Total work
   didn't disappear, it moved to whoever now ranks higher — spread across
   many responders, matching the loss/gain math above. Explains the Kip
   case directly: Meteor Mite and The Gale share a handler and a job pool,
   so Meteor Mite's lost jobs didn't vanish, they went to whoever ranked
   above him — The Gale among them.
5. **Rate vs. volume are tracked completely separately in the code →
   explains the contradictions.** Ironvale, Nightwell, and Stormwrack likely
   still rank well overall (good proximity keeps offer volume climbing), but
   still occasionally lose the 60-second race on a specific job — which
   feels like "nothing's coming" in the moment even while their weekly total
   keeps rising. Same mix-up as Cindermark/The Drift's "right problem, wrong
   description" tickets.
6. **Why Vesper and Meteor Mite collapsed the same way for different
   reasons.** Meteor Mite already had the weakest acceptance record of all
   16 — the no-decay penalty catching him fits cleanly. Vesper had one of
   the *best* records, but the code doesn't care how good your history is if
   you're ranked low on proximity for most jobs: one bad week (her taken
   count dropped from 12 to 6 right at the release) and she'd rarely be
   offered anything again, regardless of her track record. Same trap, two
   different ways in — consistent with the "two entry paths" wrinkle the
   five-agent debate left open.

## Open items this doesn't settle
- **Top priority:** per-responder location/travel-time data, for the 4
  collapsed responders plus Ashgrove and Halfmoon (flagged by multiple agents
  as possibly heading the same direction) — the one data point every
  investigation independently asked for.
- Whether `pings_sent` in the CSV is the official export, or counts
  something different from what tickets describe (see
  `notes/ravi-1-1-agenda.md`) — the Ironvale/Nightwell/Stormwrack
  contradiction can't be resolved without this.
- Why Meteor Mite and Vesper generated no tickets at all — worth asking Nadia
  whether there's a filing gap, not just a severity gap.
- Whether Meteor Mite and Vesper represent two different ways into the same
  trap (geography vs. penalty-spiral) rather than one shared cause.
