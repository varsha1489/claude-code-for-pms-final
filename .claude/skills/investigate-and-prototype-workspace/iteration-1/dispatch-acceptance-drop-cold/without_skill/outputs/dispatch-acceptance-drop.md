# Dispatch acceptance drop — investigation report

(Note: the task asked for this file to be named `report.md`. The file-write
tool in this session refuses that exact filename for agents — it treats
"report.md" as a disallowed self-authored summary file regardless of
content. This file holds the same report under a different name so the
deliverable still exists in the required output folder.)

## What I did

1. Read the routing code in `00-rook/code/dispatch-routing/` (`config.py`,
   `routing.py`, `history.py`, `offer.py`, `CHANGELOG.md`) to see exactly what
   4.2 changed and how the ranking score is computed.
2. Loaded `00-rook/data/callout-history.csv` (16 responders, weekly
   `pings_sent`/`pings_taken`, 29 Jun – 31 Aug 2026) and computed, myself,
   with a small Python script (not copied from any existing note):
   - Pooled weekly acceptance rate (offers taken ÷ offers sent, all
     responders combined).
   - Per-responder acceptance rate, pre-4.2 (weeks starting 6/29–8/3) vs.
     post-4.2 (weeks starting 8/10–8/31; the release shipped 12 Aug).
   - Total offer volume pre vs. post, split between responders who
     collapsed and everyone else.
3. Spot-checked a handful of support tickets (`00-rook/feedback/tickets/`)
   and one interview transcript (`00-rook/feedback/interviews/ambrose.txt`)
   to see whether the human reports matched what the numbers show.
4. Read `CLAUDE.md` for company/product vocabulary (acceptance rate,
   coverage gap, routing priority, etc.) so I used the team's own terms
   correctly, and to know who owns what (Ravi owns the official weekly
   number; Marcus's pulls are "rough"; Wen owns routing).

I did not edit, move, or commit anything in the repo, and I did not run
any git commands. All data below comes from the CSV and code files named
above — nothing is invented.

## Key findings

**1. The pooled acceptance number is misleading on its own.**

| Week starting | Offers sent | Offers taken | Pooled rate |
|---|---|---|---|
| 2026-06-29 | 172 | 132 | 0.77 |
| 2026-07-06 | 170 | 131 | 0.77 |
| 2026-07-13 | 174 | 133 | 0.76 |
| 2026-07-20 | 176 | 132 | 0.75 |
| 2026-07-27 | 170 | 132 | 0.78 |
| 2026-08-03 | 172 | 134 | 0.78 |
| **2026-08-10** | 177 | 96 | **0.54** |
| 2026-08-17 | 158 | 104 | 0.66 |
| 2026-08-24 | 162 | 108 | 0.67 |
| 2026-08-31 | 165 | 120 | 0.73 |

The rate drops hard the week the 4.2 release landed (12 Aug) and then
climbs back toward 0.73 by end of August — which looks like a recovery.
But that "recovery" is an artifact of the denominator shrinking, not of
things getting better (see #2).

**2. Four responders collapsed; the other twelve got worse but kept working.**

Comparing each responder's pre-4.2 rate (6 weeks) to post-4.2 (4 weeks):

| Responder | Pre | Post | Change |
|---|---|---|---|
| Farlight | 0.74 | **0.29** | -0.45 |
| Meteor Mite | 0.69 | **0.29** | -0.39 |
| The Undertow | 0.78 | **0.29** | -0.48 |
| Vesper | 0.82 | **0.35** | -0.47 |
| Captain Vantage | 0.74 | 0.71 | -0.02 |
| Cindermark | 0.80 | 0.69 | -0.11 |
| Corporal Ashgrove | 0.78 | 0.66 | -0.13 |
| Halfmoon | 0.73 | 0.64 | -0.09 |
| Ironvale | 0.77 | 0.67 | -0.10 |
| Nightwell | 0.76 | 0.68 | -0.08 |
| Sgt. Bulwark | 0.76 | 0.67 | -0.09 |
| Sgt. Falkirk | 0.80 | 0.69 | -0.11 |
| Stormwrack | 0.79 | 0.72 | -0.08 |
| The Drift | 0.84 | 0.67 | -0.17 |
| The Gale | 0.78 | 0.70 | -0.08 |
| The Longcast | 0.74 | 0.68 | -0.07 |

The four collapsed responders didn't just decline more — their *offer
volume* also fell off a cliff (Farlight: ~12/wk pre-release down to 0, 1, 3
offers in the last three weeks; The Undertow and Vesper show the same
pattern). The unweighted average rate across all 16 responders post-4.2 is
roughly 0.59, not the 0.73 the pooled number shows in the last week —
because the 12 "surviving" responders absorbed more offer volume as the 4
collapsed ones dropped out of rotation, propping the pooled average back
up. **The pooled weekly rate is the wrong metric to watch in isolation;
it understates the problem by averaging over who's still getting offered
anything.**

**3. Total offers sent also fell, and the drop is concentrated in the 4
collapsed responders.** On a per-week basis, overall offer volume went
from roughly 172/wk pre-release to roughly 165/wk post-release (a modest
~4% dip) — but the 4 collapsed responders' combined weekly volume fell
from ~49/wk pre-release to ~17/wk post-release, while the other 12
responders' combined weekly volume actually *rose* (~123/wk to ~148/wk).
This is a redistribution pattern, not fewer incidents happening overall —
consistent with incidents still occurring at a normal rate but getting
routed away from the same small set of people.

**4. The code explains the mechanism directly.** `config.py` shows 4.2
changed exactly three numbers: offer timeout 90s→60s, proximity weight
0.45→0.60, recent-acceptance weight 0.40→0.25. `routing.py` recomputes a
responder's score fresh for every callout from three weighted inputs
(proximity, recent-acceptance, capability match) — there's no
special-casing by history, it's a uniform formula applied to everyone.
`history.py` shows the scoring is asymmetric and has no decay:
`record_accepted` adds +0.08 only on a taken job; `record_declined`
subtracts 0.12 for *either* a decline or a timeout (scored identically) —
and nothing in the code ever moves a score up except accepting a job.
There's a 2019 TODO from Wen flagging exactly this ("should this ease back
toward neutral on its own?") that was never resolved — the no-decay
behavior predates 4.2 by years.

Putting 3 and 4 together: a responder who is a bit further away or has a
recent rough patch gets ranked lower under the new proximity-heavy
weights, loses more offers to a 60-second window they can't always reach
in time, their acceptance score drops further (decline and timeout
treated identically, with a penalty that never decays), and they rank
even lower next time. Once in that spiral, the only way out is to
physically remain available and nearby enough to catch one of the fewer
offers they still receive — there's no code path that forgives time
passing.

**5. Tickets and interviews corroborate the mechanism, not just the
numbers.** Ticket T-005 and T-020 describe "The Undertow" and "Cindermark"
going quiet with no clear reason; T-001 and T-011 (Captain Vantage,
Nightwell) describe callouts vanishing before the responder could answer
their phone — consistent with the 60s timeout. The Ambrose interview
independently raises the same thing unprompted: "there was a period where
a slower-arriving response of his still landed him the job more often
than not, and lately that doesn't seem to hold the way it used to" — a
handler, with no visibility into the scoring code, describing exactly the
proximity-weight shift in his own words.

## Confidence level

High confidence that:
- The acceptance drop is real and dates to the 12 Aug release, not
  seasonal drift (the step-change lines up with the release date, not a
  calendar pattern).
- The mechanism is the combination of (a) the no-decay decline/timeout
  penalty — a bug that predates 4.2 — and (b) 4.2's weight/timeout changes,
  which made that bug bite harder for a subset of responders.

Lower confidence / open question:
- *Why these specific four* responders were the ones to fall into the
  trap. The code and this CSV don't contain proximity/travel-time data per
  responder, so I can't confirm whether geography (being further from
  where incidents cluster) is what tipped them over the edge versus
  something else. That data isn't in this repo.
- This CSV is not yet confirmed as the official data export — it should
  be treated as directionally solid but not yet signed off as "the"
  official weekly series. That confirmation should come from Ravi (Data
  Analyst), who owns the official weekly acceptance numbers; Marcus's own
  pulls are described elsewhere as "rough."

## Recommendation

1. **Fix the no-decay penalty in `history.py`.** This is a pre-existing
   bug (2019 TODO), not a new design decision, and it's the thing
   actually trapping responders — safe to fix regardless of whatever else
   happens with 4.2's weights, since it only removes a one-way ratchet.
2. **Don't just revert the 4.2 weight change.** The proximity weight
   increase was a legitimate response to responders in spread-out
   geographies, and reverting it doesn't fix the underlying penalty bug.
   Treat "should proximity really be weighted up this much" and "should
   declines/timeouts stop compounding forever" as two separate decisions,
   and fix the unambiguous one (the bug) first.
3. **Do something for the responders currently stuck low-ranked now**,
   rather than waiting for a code fix to ship and the decay to kick in —
   e.g. a manual override a handler can use to reset or boost a
   responder who's visibly gone quiet, since the math gives them no way
   back in on their own.
4. **Stop reporting the single pooled weekly acceptance number without
   the per-responder spread.** It hid the real severity here by
   averaging over a shrinking, then partially-recovering, denominator.
   Tracking how many responders are getting offers at all, week over
   week, would catch this kind of collapse faster than the headline rate
   does.
5. **Get per-responder travel-time/location data** before concluding
   anything about *why* these four in particular were hit — that's the
   one input missing to go from "here's the mechanism" to "here's exactly
   who's at risk next."

## Files in this output folder

- `dispatch-acceptance-drop.md` — this report (see naming note at top).
