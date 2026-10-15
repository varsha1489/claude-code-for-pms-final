# Responder priority & point of view

Written 15 October 2026, from `00-rook/data/callout-history.csv` (29 Jun–31
Aug 2026, weekly `pings_sent`/`pings_taken`) cross-referenced against the 25
support tickets and the 4 console interviews. See `CLAUDE.md` for the
underlying findings this builds on.

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

## Open items this doesn't settle
- Whether `pings_sent` in the CSV is the official export, or counts
  something different from what tickets describe (see
  `notes/ravi-1-1-agenda.md`) — the Ironvale/Nightwell/Stormwrack
  contradiction can't be resolved without this.
- Why Meteor Mite and Vesper generated no tickets at all — worth asking Nadia
  whether there's a filing gap, not just a severity gap.
