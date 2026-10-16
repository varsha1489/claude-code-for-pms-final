# Second Chance — a guaranteed offer for a responder who's gone quiet

One-pager for Helen, in answer to her note in `director-request.txt`.

## The problem, from the person it happens to

Vesper used to get 12–14 callouts a week. By the end of August she was
getting one — some weeks, none. Her handler, Aunt Dot, described it as a
race that stopped feeling fair: "he loses it more than he wins it." Meteor
Mite's handler, Kip, watched two responders in the same city go in opposite
directions the same week — one buried in work, one silent, texting to ask
if something was broken. Neither of them ever filed a ticket. We only know
their stories because they happened to be in an unrelated interview.

That's the real shape of the problem: **once someone goes quiet, nothing in
Dispatch ever asks them again in a way that could change that.** Not
because the system decided they're unreliable — because the only way back
in is getting offered a job, and the only reason you stop getting offered
jobs is the same thing that's keeping you out. It's a locked door that only
opens from the inside.

**Why now, specifically:** Nadia flagged on 26 Aug that a handler had
emailed her directly about this — her words, "which never happens." This
isn't cooling off; it's still finding new ways to surface. Marcus already
has a 4.2 regroup planned for the team now that the new PM is up to speed —
this brief is meant to walk into that meeting, not just answer this email.

## Why a config change isn't the fix

Engineering's right that the immediate bug — a bad mark that never fades —
is small to patch. But patching it only prevents new people from falling
in. It does nothing for Vesper and Meteor Mite today, and it's invisible to
everyone else: a number moving in a config file doesn't change what Kip
sees on his screen, or what it feels like for a responder whose phone has
been quiet for two weeks. That's the gap this brief is for.

## What we'd build: Second Chance

**The rule:** if a responder gets zero callout offers for a set stretch —
proposing 7 days, tunable — the very next callout that matches their
capability tags **and falls within a realistic travel range** goes to them
first, regardless of where they currently rank. One offer. No ranking math.
A real chance, on a real job, that the current scoring would otherwise
never give them.

The travel-range check matters: if someone's been shut out because they're
genuinely far from most incidents, guaranteeing them *any* matching job
could mean assigning them to one they can't reach in time. So the guarantee
only fires on the next matching callout that's also within a widened
distance window — not the full 45-minute cutoff the normal scoring uses,
something looser, so the guarantee can't quietly become an operational risk
on a real incident. (If Ravi's travel-time data shows distance isn't
actually the driver here, this check costs us nothing — it just rarely
triggers.)

**What the responder sees:** after two silent weeks, their phone buzzes
again. Not a notification about a fix — a real job, offered the normal way,
like it used to work.

**What the handler sees (this is the Kip part):** a status line on the
responder's profile — *"No offers in 9 days — next matching callout is
guaranteed."* Right now a handler just watches the silence and guesses.
This turns an unexplained gap into something visible, with an end in sight.
It directly answers what Aunt Dot and Kip both asked for in their
interviews, without us having to build a whole console redesign to do it.

**What it isn't:** a permanent override, a quota, or a way to game the
system. One guaranteed shot, same as anyone else gets when a job comes in —
just guaranteed to come, instead of maybe never arriving. It also isn't
mutual aid (cross-region coverage, still unsupported, still a Q4 question)
and it isn't a reversal of 4.2 — the routing decision itself is untouched;
this only guarantees a floor under it.

## Before it gets to zero: an early-warning tier

Second Chance only fires once someone hits zero offers for a week — which
means Vesper, Meteor Mite, Farlight, and The Undertow qualify, but
Ashgrove and Halfmoon, who are declining on the same trajectory and just
haven't hit bottom yet, get nothing from this. I'd pair Second Chance with
a lighter, earlier signal: once someone's weekly offers drop by roughly a
third from their own baseline for two weeks running, flag it on the
handler's view the same way — visible, not acted on automatically. It
doesn't guarantee anything yet; it just means a handler sees the slide
starting instead of only hearing about it once their responder has gone
fully silent.

## What we already know for certain

- The scoring code has exactly one way up (accepting a job, +0.08) and one
  way down (a decline or timeout, treated identically, −0.12), and nothing
  anywhere lets a score recover on its own. Confirmed directly in the code.
- Four responders collapsed to near-zero offers after 4.2 and never
  recovered across three full weeks of data — not a dip, a flat line with
  no bounce.
- Two of the four — Meteor Mite and Vesper — never filed a single ticket.
  Ticket volume would have missed them completely; the real number of
  people affected is likely larger than Support's view of it.

## What we're still confirming

We believe the four who collapsed are disadvantaged by distance, not by a
worse track record — Vesper had one of the *best* acceptance rates of
anyone before 4.2, which argues against "they were already struggling."
We've asked Ravi for travel-time data to confirm this before we recommend
touching the weights themselves. Second Chance doesn't need that answer —
it works regardless of *why* someone went quiet.

## Build order — we don't have to wait on engineering to start helping people

Second Chance and the timeout-penalty fix are the core ask below, and they
take real engineering time. In parallel, three things can start immediately,
each one not dependent on the others or on Ravi's data landing first:

**#1 — starting now.** A guaranteed-offer override a handler can trigger by
hand today, for Farlight, Meteor Mite, The Undertow, and Vesper specifically
— the same idea as Second Chance, done manually, with the same travel-range
safety check, live before any code ships. Clickable prototype:
**Guaranteed Offer Override**. This is the one that actually reaches the
four people who need it, this week, not after a build cycle.

**#2 — fast follow.** A reconciliation check between tickets and the real
weekly data, so Nadia's team stops chasing false "gone quiet" tickets
(Ironvale, Nightwell, Stormwrack all showed *rising* offers while filing
them) and stops missing real ones the way it missed Meteor Mite and Vesper.

**#3 — fast follow.** An explainer on the handler's console showing why a
responder ranked where they did for a given callout. This is a durable,
every-time answer to the question Marcus asked on 14 Aug, not a one-off
reply in Slack.

## The ask

Approve Second Chance (plus the early-warning tier) as the thing to
actually build — on its own, not bundled with a decision about reverting or
reweighting 4.2. The no-decay patch should still ship, separately, since
it's safe either way. We're starting on #1 above immediately and will fast
follow with #2 and #3 while the main build is underway. Wen's back from PTO
and is the natural owner on the engineering side, since her own 2019
comment on the routing code is where this whole question started. Full
findings and the evidence behind all of this are in
`notes/responder-priority-and-pov.md`.
