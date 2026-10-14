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

| Responder | Data | Tickets | Interview | Sources | Tier |
|---|---|---|---|---|---|
| The Undertow | Collapsed | 3 (incl. High) | — | Agree | Confirmed & urgent |
| Vesper | Collapsed | 0 | Yes | Agree (data + interview only) | Confirmed & urgent — invisible to tickets |
| Meteor Mite | Collapsed | 0 | Yes | Agree (data + interview only) | Confirmed & urgent — invisible to tickets |
| Farlight | Collapsed | 1 | No | Agree | Confirmed & urgent |
| The Drift | Declining | 2 | — | Agree | Confirmed & urgent |
| Sgt. Falkirk | Declining | 2 | — | Agree | Confirmed & urgent |
| Corporal Ashgrove | Declining | 2 | — | Agree | Confirmed & urgent |
| Cindermark | Declining | 2 | — | Agree | Confirmed & urgent |
| Ironvale | Stable/rising | 3 | — | **Contradict** | Unresolved — don't act yet |
| Nightwell | Stable/rising | 3 | — | **Contradict** | Unresolved — don't act yet |
| Stormwrack | Stable/rising | 2 | — | **Contradict** | Unresolved — don't act yet |
| Halfmoon | Mild | 2 | — | Roughly consistent | Monitor |
| The Longcast | Mild | 2 | — | Roughly consistent | Monitor |
| Captain Vantage | Stable | 1 | Yes | Not about him | Key informant, not a casework priority |
| Sgt. Bulwark | Stable | 0 | Yes (passing) | No real signal | Noise |
| The Gale | Stable, positive | 0 | Yes (contrast) | No real signal | Noise |

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

## Open items this doesn't settle
- Whether `pings_sent` in the CSV is the official export, or counts
  something different from what tickets describe (see
  `notes/ravi-1-1-agenda.md`) — the Ironvale/Nightwell/Stormwrack
  contradiction can't be resolved without this.
- Why Meteor Mite and Vesper generated no tickets at all — worth asking Nadia
  whether there's a filing gap, not just a severity gap.
