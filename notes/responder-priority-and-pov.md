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

## Priority: frequency × importance
**Importance** = how severe the data shows it is. **Frequency** = how many
independent signals corroborate it (tickets + interview mentions). These two
axes disagree for several responders, which is the whole point of splitting
them out.

| | High frequency (2+ signals) | Low frequency (0–1 signals) |
|---|---|---|
| **High importance** (collapsed/declining) | **Confirmed & urgent** — The Undertow, Farlight, The Drift, Sgt. Falkirk, Corporal Ashgrove, Cindermark | **Silent but severe — biggest blind spot:** Meteor Mite, Vesper |
| **Low importance** (mild/stable in the data) | **Loud but unclear** — tickets contradict the data: Ironvale, Nightwell, Stormwrack, Halfmoon, The Longcast, Captain Vantage | **Noise** — Sgt. Bulwark, The Gale |

### Ranked order
1. **The Undertow** — collapsed (0.78→0.17), 3 tickets incl. one High severity. Highest severity, highest confidence.
2. **Vesper** — collapsed (0.82→0.12), **zero tickets**. Only surfaced via an unrelated design interview.
3. **Meteor Mite** — collapsed (0.69→0.14), **zero tickets**. Same blind spot as Vesper.
4. **Farlight** — collapsed (0.74→0.00), 1 ticket despite the worst accept rate in the set.
5. **The Drift** — accept rate fell 14 pts even with volume holding; same-day paired tickets from handler and responder.
6. **Sgt. Falkirk** — real decline, but oscillating ("on and off") rather than a clean drop.
7. **Corporal Ashgrove** — steady, unresolved decline; no loss event yet.
8. **Cindermark** — shows the quiet-then-lose-it trap directly in his own ticket history.
9–14. Ironvale, Nightwell, Stormwrack, Halfmoon, The Longcast, Captain Vantage — **don't act on these yet.** Three (Ironvale, Nightwell, Stormwrack) have tickets claiming silence the CSV contradicts — a data-trust problem, not necessarily a routing one. Vantage's own data is fine, but his is the quote behind the routing-fairness hypothesis — keep that separate from his personal case.
15–16. Sgt. Bulwark, The Gale — no real signal either way.

**Why this matters:** ranking by ticket volume alone would start with Ironvale or Nightwell (3 tickets each) and never surface Meteor Mite or Vesper, who are in worse shape than anyone filing tickets. Rows 2 and 3 are the ones worth raising specifically because no one else will.

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

## Open items this doesn't settle
- Whether `pings_sent` in the CSV is the official export, or counts
  something different from what tickets describe (see
  `notes/ravi-1-1-agenda.md`) — the Ironvale/Nightwell/Stormwrack
  contradiction can't be resolved without this.
- Why Meteor Mite and Vesper generated no tickets at all — worth asking Nadia
  whether there's a filing gap, not just a severity gap.
