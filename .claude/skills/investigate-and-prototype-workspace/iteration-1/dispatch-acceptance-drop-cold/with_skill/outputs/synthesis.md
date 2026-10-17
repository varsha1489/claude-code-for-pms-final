# Synthesis — Dispatch acceptance drop after 4.2 (12 Aug 2026)

One running file of record. Claims are tagged **Fact** (directly observed),
**Hypothesis** (someone's read), or **Conclusion** (hypothesis checked
against evidence, cited).

## Plain-language summary

Acceptance didn't fall evenly across all responders. Four responders
(Farlight, Meteor Mite, The Undertow, Vesper) went from a normal ~10-14
offers a week to almost nothing within a month of the 12 Aug release, and
that's most of the drop in the headline number. The likely mechanism: a
routing change made proximity count for more and recent-acceptance count
for less, and a scoring bug that's been in the code since 2019 — a decline
or timeout both cost a responder the same penalty, and that penalty never
fades on its own — turned a few early misses into a spiral these four
responders can't climb back out of on their own. A separate, smaller group
of responders lost real callouts to the shorter 60-second timeout, which is
a different problem with a different fix. Support tickets caught some of
this but missed two of the four worst-hit responders entirely.

## Facts

- **F1.** 4.2 shipped 12 Aug 2026 with three changes: proximity weight
  0.45→0.60, recent-acceptance weight 0.40→0.25, offer timeout 90s→60s.
  (`00-rook/code/dispatch-routing/config.py`, `CHANGELOG.md`)
- **F2.** `history.py`: accepting a callout adds +0.08; a decline *or* a
  timeout both subtract -0.12 — identical penalty, no distinction recorded
  downstream. A 2019 TODO from Wen explicitly asks whether the score should
  decay back toward neutral over time and says "leaving it as-is for now."
  Nothing in `routing.py`, `offer.py`, or `history.py` changed in 4.2 — only
  the three numbers in `config.py` moved. (`00-rook/code/dispatch-routing/*.py`)
- **F3.** Recomputed directly from `00-rook/data/callout-history.csv` (16
  responders, weeks 29 Jun–31 Aug 2026, 160 rows): pooled weekly acceptance
  (sum taken ÷ sum sent) is flat at 0.75-0.78 for six weeks, then steps down
  to **0.542** in the week of 10 Aug (the release lands mid-week), partially
  recovers to 0.658, 0.667, then 0.727 by the week of 31 Aug. This is a step
  at the release date, not a gradual seasonal drift — six weeks of pre-
  release stability make a slow seasonal slide implausible.
- **F4.** The 31-Aug "recovery" to 0.727 pooled is misleading: the
  **unweighted per-responder average** that same week is only **0.593**, not
  0.727. The gap exists because four responders have nearly dropped out of
  the denominator.
- **F5.** Four of 16 responders collapsed in volume, not gradually but
  progressively week over week, exactly after the release:
  - Farlight: 12→12→11→12→13→12 pre-release, then 10→3→1→0 post-release.
  - Meteor Mite: 11→12→10→11→12→11, then 10→4→2→1.
  - The Undertow: 12→11→12→13→12→12, then 11→4→1→1.
  - Vesper: 14→13→15→14→13→14, then 12→5→2→1.
  All four were offered a near-normal volume the very first post-release
  week (10 Aug), then the offer count itself fell off a cliff over the
  following three weeks — consistent with a self-reinforcing score spiral,
  not a one-time shock.
- **F6.** The other 12 responders' *offer volume* didn't fall — pooled
  offers to them actually rose slightly (+1%, pre vs. post average) — but
  their pooled *acceptance rate* also dropped and, as of the last week in
  the data, had not fully recovered (0.787 pre-release high → 0.727 by 31
  Aug, still below the ~0.75-0.78 pre-release band).
- **F7.** Total offers sent fell ~4% pre vs. post overall, but almost all
  of that is attributable to the 4 collapsed responders; excluding them,
  total offers to the other 12 were flat to slightly up. This is a
  redistribution signature (offers moved toward higher-proximity responders)
  rather than fewer incidents happening.
- **F8.** Ticket-vs-data mismatch, confirmed directly: tickets report 11
  distinct responders as "gone quiet" (`00-rook/feedback/tickets/*.txt`),
  but only 2 of those 11 (Farlight, The Undertow) actually collapse in the
  CSV. The other 9 named as quiet in tickets (Captain Vantage, Cindermark,
  Corporal Ashgrove, Halfmoon, Ironvale, Nightwell, The Longcast,
  Stormwrack, The Drift) show post-release offer ratios between 0.80 and
  1.31× their pre-release average — i.e., not actually quiet by volume.
  Nightwell's own ticket (T-009, "nothing in like 10 days") is directly
  contradicted: her weekly offer count through that period was 16, 18, 20,
  21 — rising, not falling. Two of the four responders who *did* collapse
  (Meteor Mite, Vesper) filed **zero** tickets and are known only from the
  Sofia Marino console-redesign interviews (Kip's and Aunt Dot's accounts).
- **F9.** Of 25 tickets, 16 describe "gone quiet," the rest describe an
  offer vanishing before the responder could answer (consistent with the
  60s timeout) — a separate complaint pattern from the quiet-responder one.
- **F10.** Interview evidence (Ambrose, Aunt Dot, Halloran) each
  independently and unprompted raised a callout moving on before the
  responder could answer. Ambrose additionally, unprompted, suspects
  routing now favors faster-arriving responders over slower ones — he
  frames this as a felt pattern over "years," not a measured one.
- **F11.** Console filter persistence shipped in 4.2 without ever
  appearing as a committed item in the Q3 roadmap (`00-rook/company/
  roadmap-q3.pdf`); conversely, "Availability Confidence" was committed for
  4.2 in that roadmap but does not appear in the 4.2 release notes.

## Hypotheses

- **H1 (Marcus, 14 Aug, now resolved — see Conclusions).** Was the routing
  rebalance meant to target responders who'd already been declining, or did
  that effect just fall out of the config change?
- **H2 (Priya).** The drop is seasonal ("August is always soft") and will
  recover in September on its own.
- **H3 (Ambrose, interview).** Routing now structurally favors
  faster/closer responders over ones who used to win on reliability.
- **H4 (mine, from the code).** The real driver isn't the weight rebalance
  alone — it's that rebalance *combined with* the pre-existing, never-decaying
  decline/timeout penalty (F2), which turns any early run of bad luck for a
  responder into a trap with no built-in way out.
- **H5 (mine).** The shorter 60s timeout and the acceptance-collapse trap
  are two different mechanisms producing two different ticket patterns (the
  ⅓ "vanished before I could answer" vs. the ⅔ "nothing at all"), and
  conflating them in one "acceptance is down" number hides that they need
  different fixes.

## Conclusions

- **C1 (resolves H1).** The ranking formula (`routing.py: score()`) recomputes
  fresh for every callout from three live inputs; there is no special-casing
  by decline history anywhere in the code. It hits anyone with a lower
  current acceptance score the same way, whether that score is low because
  of a long decline history or because they're simply new or had one bad
  stretch. Vesper — previously one of the *best* acceptance records in the
  dataset (0.82 pre-release, the highest of all 16) — collapsing just as hard
  as the others is direct evidence against "this only hits chronic
  decliners." **Confirmed, not a targeting bug — a uniform mechanism that
  happens to concentrate on whoever's had a few recent misses.**
- **C2 (resolves H2, rules it out).** F3 shows the drop is a step at the
  exact release date after six flat pre-release weeks, not a drift. F7
  shows total incident volume (proxied by total offers, excluding the 4
  collapsed responders) didn't fall — it's a redistribution, not fewer
  incidents happening in August. **Seasonality is not supported by this
  data; it was a reasonable hypothesis to hold before looking, but the one
  data file in the repo contradicts it.**
- **C3 (partially supports H3).** Proximity is now weighted more than
  recent-acceptance (0.60 vs. 0.25, F1), so Ambrose's felt pattern is
  directionally real — but "favors faster responders" isn't quite the
  mechanism. It's "favors whoever's currently ranked higher, and ranking is
  now dominated by travel time," which happens to look identical to
  "favors fast responders" from a handler's seat. **Partial — real
  phenomenon, imprecise framing.**
- **C4 (supports H4, the central finding).** F2 + F5 together are
  consistent with: a responder's score dips slightly (from one timeout or
  decline, no fault of their own, especially plausible after the timeout
  shortened from 90s→60s increasing timeout frequency) → the 4.2 rebalance
  makes that score matter less for ranking than before is wrong — actually
  it makes proximity matter *more*, so a responder whose current dip
  coincides with being slightly farther from incidents now ranks low on
  *two* dimensions instead of being rescued by a strong acceptance history
  → they get offered less → because `record_declined` fires on timeouts
  too and never decays, every missed offer (including ones they never even
  saw because they weren't offered) that *does* reach them and times out
  compounds the penalty → fewer offers → lower score → fewer offers. This
  is a mechanism read from the code (F2) matched against an observed
  pattern (F5's progressive, not sudden, collapse over 3-4 weeks) — strong
  circumstantial fit, but **not independently proven**: the data has no
  per-offer win/loss log, only weekly aggregates, so the week-by-week
  spiral can't be confirmed offer-by-offer, only inferred from the shape of
  the volume collapse.
- **C5 (supports H5).** The two ticket patterns (F9) map cleanly onto two
  separate code paths: "vanished before I could answer" → shorter
  `OFFER_TIMEOUT_SECONDS` (F1), a straightforward, already-understood
  mechanism; "gone quiet for weeks" → the scoring trap (C4). Treating "
  acceptance is down" as one problem with one fix (e.g., just reverting the
  timeout) would fix the ⅓ and leave the ⅔ — the larger piece — untouched.

## Open question — not settled by this data

Two of the four collapsed responders entered the trap differently: Vesper
had the *best* pre-release acceptance score and still collapsed just as
hard as the others, while the ticket record for Ashgrove/Halfmoon-style
"quiet but not collapsed" responders shows no scoring explanation at all
in this data. **The single highest-leverage missing input is per-responder
location / travel-time data** — nobody can confirm *why* these four
specific responders, and not others with similar pre-release acceptance
scores, were the ones who tipped into the trap, without seeing whether they
also happen to sit farther from where incidents cluster. That's a request
for Ravi (data) since this file's source CSV isn't yet confirmed as his
official export either.

## Confidence: 72/100

- High confidence (90+) that: the drop is not seasonal (C2); it is
  concentrated in a small subset of responders rather than spread evenly
  (F5); the shorter timeout and the scoring trap are two distinct
  mechanisms, not one (C5); the no-decay penalty bug is old and 4.2 exposed
  rather than created it (F2).
- Moderate confidence (~60-70) that the specific causal chain in C4 (dip →
  rebalance amplifies it → penalty locks it in) is the actual mechanism
  for these four, rather than some other unmeasured factor (e.g.,
  something about their actual location). This is a read of the code
  matched to an aggregate pattern, not a traced individual offer log.
- The prototype built in this investigation (see `prototype.html`) is
  **illustrative only** — it demonstrates that the described mechanism is
  *capable* of producing a collapse shaped like the real one, using a
  simplified simulation with assumed inputs (synthetic incident arrivals,
  assumed starting proximity). It does not prove this is what happened to
  Farlight, Meteor Mite, The Undertow, and Vesper specifically, and it does
  not move the confidence number on its own.
