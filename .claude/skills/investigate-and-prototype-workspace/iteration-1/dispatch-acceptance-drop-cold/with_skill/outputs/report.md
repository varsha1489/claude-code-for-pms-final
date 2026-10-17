# Dispatch 4.2 acceptance drop — investigation report

Task: "Acceptance rate for Dispatch dropped after the August release and
nobody can agree on why — dig in and tell me what's actually going on, and
what we should do about it." Investigated using the
`investigate-and-prototype` skill.

## Phases followed

- **Context file.** `CLAUDE.md`'s "Working context" section already existed
  for this product. Used it for vocabulary, people, and product mechanics,
  but re-derived every quantitative and code claim myself from the primary
  sources rather than taking its narrative on trust, per this run's
  instruction to do real computation rather than guess at numbers.
- **Phase 0 (confirm the ask).** High confidence already: the requester is
  the Dispatch PM, the ask is "find the real cause of the acceptance drop
  and recommend what to do," and the deliverable is an internal
  investigation plus something concrete to show. Didn't stop to ask —
  context made this clear.
- **Phase 1 (Listen).** Read all 25 tickets (`00-rook/feedback/tickets/`)
  and all 4 console-redesign interview transcripts
  (`00-rook/feedback/interviews/`). Grouped by theme: "gone quiet for
  weeks" (16/25 tickets) vs. "an offer vanished before I could answer"
  (rest). Interviews (Ambrose, Aunt Dot, Halloran) each independently and
  unprompted raised the "vanished before I could answer" pattern; Ambrose
  additionally floated a routing-fairness theory unprompted.
- **Phase 2 (Count).** Loaded `00-rook/data/callout-history.csv` (160 rows,
  16 responders, 10 weeks) into Python and recomputed every number myself —
  pooled weekly acceptance, unweighted per-responder average, per-responder
  pre/post offer volume, and offer-volume trajectories week by week. Did
  not accept any reported headline number as-is.
- **Phase 3 (Reconcile).** Checked every ticket claim against the recomputed
  data. Found a sharp mismatch: 11 responders reported as "quiet" in
  tickets, but only 2 actually collapse in the data; 2 of the 4 responders
  who really did collapse (Meteor Mite, Vesper) filed zero tickets and are
  visible only in the interviews.
- **Phase 4 (Mechanism).** Read all five files in
  `00-rook/code/dispatch-routing/` end to end (`config.py`, `history.py`,
  `offer.py`, `routing.py`, `availability.py`) plus the `CHANGELOG.md`.
  Confirmed exactly which numbers changed in 4.2 versus what was already
  there (the no-decay penalty bug predates 4.2 by seven years).
- **Phase 5 (Hypothesize).** Tested four hypotheses (Marcus's targeting
  question, Priya's seasonality claim, Ambrose's fairness theory, my own
  compounding-penalty theory) each against evidence actually in the repo,
  rather than ranking them by how plausible they sounded.
- **Phase 6 (Synthesize).** Wrote `synthesis.md` — fact/hypothesis/
  conclusion tagged, with a plain-language summary up top and a stated,
  moving confidence number.
- **Phase 7 (Recommend).** Confidence: **72/100** — see synthesis for what
  would move it up or down. Named the single highest-leverage missing
  input: per-responder location/travel-time data.
- **Phase 8 (Prototype).** Built `prototype.html`, a standalone page with
  four real-data panels (pooled vs. unweighted rate, the four-responder
  collapse trajectories, the tickets-vs-data mismatch table, and the code
  mechanism) and one clearly-marked illustrative simulation showing the
  compounding-penalty mechanism is *capable* of producing a collapse shaped
  like the real one — explicitly labeled as not proof, and not counted
  toward the confidence score. Did not run simulated stakeholder feedback
  rounds on it in this pass, given the scope of this single-session task;
  flagging that as a gap rather than skipping it silently.
- **Phase 9 (Package).** This file is the "what they asked for /
  the answer" packaging: the ask above, the answer below.

## Key findings

**Fact.** Pooled weekly acceptance (recomputed from the CSV) sits at
0.75-0.78 for six weeks, steps down to 0.542 the week of the 12 Aug
release, and only partially recovers to 0.727 by 31 Aug — a step at the
release date, not a seasonal drift.

**Fact.** That 0.727 "recovery" is an artifact of a shrinking denominator.
The unweighted per-responder average that same week is 0.593. Four of 16
responders — Farlight, Meteor Mite, The Undertow, Vesper — collapsed from
~10-14 offers/week to 0-1, progressively over three to four weeks, and
nearly dropped out of the pool.

**Fact.** Tickets and data disagree sharply on who's affected: 11
responders reported quiet in tickets, only 2 actually collapsed; Nightwell's
ticket says "nothing in 10 days" while her real weekly offer count rose
(16-18-20-21). Meteor Mite and Vesper — two of the four worst-hit — filed
zero tickets and are known only from interviews.

**Fact (from the code).** `history.py` gives +0.08 for an accepted callout
and -0.12 for a decline *or* a timeout, treated identically, with no decay
back toward neutral — a gap Wen flagged in a 2019 TODO and never resolved.
4.2 changed exactly three numbers in `config.py` (timeout 90->60s,
proximity weight 0.45->0.60, acceptance weight 0.40->0.25); it touched no
logic. The bug is old; 4.2 is what exposed it.

**Conclusion.** Marcus's 14 Aug question — was the rebalance meant to hit
chronic decliners, or did that just fall out of the config — is resolved:
the ranking formula recomputes fresh for every callout with no
special-casing by history. Vesper, previously the *best* acceptance score
in the dataset, collapsing just as hard as the others is direct evidence
this isn't decliner-specific.

**Conclusion.** Priya's seasonality hypothesis is not supported: the step
happens exactly at the release, six pre-release weeks are flat, and total
offer volume excluding the four collapsed responders didn't fall — it's a
redistribution of who gets offered what, not fewer incidents in August.

**Hypothesis -> best-supported mechanism (moderate confidence, not proven).**
The 4.2 weight rebalance (proximity up, acceptance down) plus the
pre-existing no-decay penalty bug combine into a trap: a responder dips
slightly (more likely now, given the shorter 60s timeout), ranks lower
under the new weights, gets offered less, can't recover because the only
way the score goes up is accepting an offer they're not being given, and
nothing decays the penalty on its own. This fits the shape of the real
four-responder collapse (gradual over 3-4 weeks, not a single-week
cliff) but isn't confirmed offer-by-offer — the CSV only has weekly
aggregates.

**Separate finding.** The "vanished before I could answer" complaint (1/3 of
tickets, and every interview) maps cleanly to the shortened 60s timeout —
a distinct, already-understood mechanism from the "gone quiet" collapse
(2/3 of tickets, the bigger problem). Treating "acceptance is down" as one
problem risks fixing the timeout and leaving the larger trap untouched.

## Confidence: 72/100

High confidence (90+) that the drop is not seasonal, is concentrated in a
small subset of responders rather than spread evenly, and that the shorter
timeout and the scoring trap are two distinct mechanisms. Moderate
confidence (60-70) that the specific causal chain (dip -> rebalance
amplifies it -> no-decay penalty locks it in) is the actual mechanism for
these four responders specifically, versus some other unmeasured factor.
The prototype's simulation is illustrative only and does not move this
number — it shows the mechanism is capable of the pattern, not that it
caused it.

**Highest-leverage missing evidence:** per-responder location/travel-time
data. Without it, nobody can confirm why these four specific responders —
and not others with similar pre-release acceptance scores — are the ones
who tipped into the trap.

## Recommendation

1. Don't treat this as one bug. Fix the timeout-driven "vanished before I
   could answer" complaints and the no-decay scoring trap as two separate
   problems with two separate owners.
2. Push the no-decay fix (let `recent_acceptance` ease back toward 0.5
   over time, per Wen's unresolved 2019 TODO) regardless of further data —
   it's safe either way and directly addresses the mechanism most
   consistent with the four-responder collapse.
3. Do something this week for the four already-trapped responders
   (Farlight, Meteor Mite, The Undertow, Vesper) rather than waiting on a
   code fix — a handler override / guaranteed offer is a reasonable
   stopgap, since they structurally cannot climb back out under current
   scoring.
4. Get the per-responder location/travel-time data before deciding whether
   to walk back the 4.2 proximity/acceptance weight rebalance itself — that
   question is separate from the scoring-trap fix and shouldn't be bundled
   with it in the same decision.
5. Reconcile reporting channels: tickets alone would have missed Meteor
   Mite and Vesper entirely. Any ongoing monitoring of this issue should
   pull from the weekly data directly, not rely on tickets as the signal
   that something's wrong.

## Files in this folder

- `synthesis.md` — the running file of record (Phase 6), fact/hypothesis/
  conclusion tagged, with the plain-language summary and open question.
- `prototype.html` — standalone, open in any browser. Real-data panels
  (pooled vs. unweighted rate, the four collapsed responders' weekly
  trajectories, the tickets-vs-data mismatch table, the code mechanism)
  plus one clearly-labeled illustrative simulation.
- `report.md` — this file.

## Notes on scope / what wasn't done

- Did not run the Phase 8 simulated-stakeholder-feedback rounds on the
  prototype in this pass (no multi-round critique-and-revise cycle as the
  skill recommends) — flagging this rather than letting the deliverable
  read as more validated than it is.
- Did not verify whether `00-rook/data/callout-history.csv` is Ravi's
  confirmed official export; CLAUDE.md notes this hasn't been confirmed
  either. Treated it as the best available real data, not as settled
  ground truth.
- Did not attempt to resolve the Security Policy 4.1 / cover-identity doc
  conflict or any Supply-side complaints (Halloran's requisition-queue and
  catalog-search issues) — out of scope for this specific ask, noted only
  as a pass-along.
