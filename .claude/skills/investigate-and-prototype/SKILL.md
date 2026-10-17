---
name: investigate-and-prototype
description: Use this skill whenever something changed and nobody's sure why — a metric dropped, a release landed badly, complaints are rising, usage collapsed for some but not all users, or a stakeholder wants a defensible answer plus something to show for it, not just a write-up. Make sure to trigger on phrases like "why did X drop", "something broke after the release", "help me figure out what happened with Y", "I need a brief/case/one-pager/PRD/deck for [person]", "build a case and a prototype for Z", "what's going on with [metric]", or any open-ended request to investigate a product problem from scratch — even if the user never says "investigate" or "root cause" outright. Also trigger proactively the moment the user is clearly doing this kind of work without naming it — reading tickets, pulling real data, reading code to explain a regression, comparing what people are saying against what the numbers show, or asking for a stakeholder-facing deliverable (brief, one-pager, PRD, or slide deck) about a product problem rather than a routing/technical doc. Covers the full arc: confirm the ask, read the qualitative and quantitative evidence, reconcile them, read the mechanism, form testable hypotheses, keep one running synthesis, give a confidence-scored recommendation, build and validate something to show (a prototype, and — only if the stakeholder needs it — a one-pager, PRD, or slide deck with speaker notes), and package it for a stakeholder.
---

# Investigate and prototype

A method for going from "something's wrong and nobody's sure why" to a
defensible answer and something real to show for it. It generalizes a
pattern that keeps working: read everything, don't trust a single number,
check the qualitative story against the real data, read the actual
mechanism, state causes as things that can be proven wrong, keep one file
that's always current, say how sure you are out loud, then build and test
something before it goes to a stakeholder.

The value of this skill isn't any one step — it's resisting the shortcuts
that make investigations wrong in ways that feel confident. The most common
failure mode isn't missing data, it's trusting the first plausible number,
the loudest complaint, or a prototype nobody who's actually affected has
seen. Each phase below exists to catch one of those specific failure modes.

## Before you start: find or build the context file

Don't re-derive what's already known. Look for a working-context file for
this product or team — often `CLAUDE.md`'s own "Working context" section,
or a similarly-named notes file. If one exists, read it first and build on
it rather than restating things it already covers. If none exists, this is
itself the first deliverable: read whatever onboarding material exists
(product docs, org charts, glossaries, prior handover notes) and write a
compact working-context file before going further. An investigation that
starts from zero every time is slower and more error-prone than one that
accumulates.

## Phase 0 — Confirm the ask before investigating

This phase exists because the most expensive mistake in this whole method
isn't missing a data point — it's spending Phases 1–9 doing careful, honest
work on the wrong question. A clarifying question up front is cheap. A full
investigation built on a misread ask is not.

Before starting Phase 1, hold yourself to something close to a 95%
confidence bar on four things:

1. **What they're actually asking for** — the specific question or
   deliverable, not your first guess at it.
2. **Who they are** — their role, what they can actually decide or act on,
   who they answer to.
3. **What they're trying to achieve** — the underlying goal behind the
   request, which is sometimes broader or narrower than the literal words.
4. **Your own role in this** — are you producing an internal working
   analysis, a stakeholder-ready brief, a prototype, a recommendation meant
   to be acted on directly, something else? These call for different
   depth and different framing.

If you're genuinely below that bar on any of the four, ask before you dig
in — a short, specific question beats guessing and finding out nine phases
later. If you're close but not certain, it's often enough to state your
read out loud and give them a chance to correct it ("I'm taking this as
[X], for [audience], aimed at [goal] — tell me if that's off") rather than
stopping to ask outright; that keeps momentum for the cases where your read
was right, while still catching the cases where it wasn't.

Don't turn this into a ritual, though. If there's already rich context
available — an existing working-context file, a conversation history that
makes the ask obvious — use it instead of asking about things it already
answers. The bar is confidence, not caution for its own sake; asking a
question you could have answered yourself from context already in front of
you wastes the same trust a wrong guess would.

## Phase 1 — Listen (the qualitative pile)

Read every qualitative source connected to the problem: support tickets,
interview transcripts, Slack threads, emails, anything where people
describe what they're experiencing in their own words.

- Group what you find by theme, not by source. The same complaint said five
  ways is one finding, not five.
- Count how many distinct people/sources raised each theme. Frequency is a
  real signal, but don't confuse "loudest" with "most important" yet — that
  gets checked in Phase 3.
- If there's more than one qualitative source (e.g. tickets *and*
  interviews), explicitly compare them: what shows up in one but not the
  other? Two piles of feedback that don't fully overlap is itself a
  finding, not noise to average away.
- Quote people directly where it matters. A paraphrase loses the thing that
  made someone bring it up unprompted.

## Phase 2 — Count (the real data)

Find the actual quantitative data behind the problem — a database, an
export, a log, anything with real numbers — and read it yourself rather
than accepting a reported headline figure secondhand.

This is where investigations most often go wrong silently. A single
aggregate number can look fine while hiding a severe problem for a subset
of the population — a dropped average can mean "everyone got a little
worse" or "a few people got much worse and everyone else is fine," and
those call for completely different responses. Before trusting any
headline number:

- Recompute it yourself from the raw rows if you can. Check what it's
  averaged over.
- Split it by whatever cohorts make sense (time period, user segment,
  region, anything plausible) and see if the aggregate is hiding a
  collapsed subgroup.
- If there's a specific change or event in question (a release, a
  migration, a policy change), check whether the data moves exactly at that
  point or drifts gradually — that's often the cleanest signal for whether
  the suspected cause is the real one.

## Phase 3 — Reconcile (qualitative vs. quantitative)

This is usually where the real findings live, and it's the step most
investigations skip. For every notable claim from Phase 1, check it
directly against the data from Phase 2 and label it:

- **Confirmed** — the claim matches what the data shows.
- **Contradicted** — the data says something different, sometimes the
  opposite.
- **Partial** — something real is being described, but the specific framing
  overstates or understates it, or conflates two different problems.
- **Silent but real** — the data shows a real problem nobody reported at
  all. These are often the most important findings in the whole
  investigation, because whatever reporting channel exists failed to catch
  them — which usually means there are more cases like it that nobody's
  found yet either.

Don't treat volume of complaints as a proxy for severity until you've done
this check. It's common for the people reporting the loudest to turn out
fine in the data, while someone with a worse real problem never says
anything.

## Phase 4 — Read the mechanism

If there's code, a configuration, or a defined system behind the behavior
in question, go read it directly rather than reasoning about it from the
outside.

- Map specific patterns found in Phase 2/3 to specific lines or logic. "The
  code does X, which would produce exactly the pattern we're seeing" is a
  much stronger claim than "the code probably does something like this."
- Separate what's new in the suspected change from what was already there.
  A regression is often not a new bug — it's an old, dormant issue that a
  recent change finally exposed. That distinction changes both the fix and
  who needs to be involved.
- If there's any kind of spec, ticket, or stated requirement for the
  change, check what was actually built against what was asked for. Gaps in
  either direction (something promised that didn't ship, something that
  shipped without being asked for) are worth surfacing even if they're not
  the root cause.

## Phase 5 — Hypothesize

State candidate causes as genuinely falsifiable claims, not impressions.
For each one:

- Write a one-sentence hypothesis and the specific prediction that would
  follow if it's true.
- Say what evidence would confirm it and what would rule it out.
- Rank hypotheses by how resolvable they are with evidence you have or can
  get — not just by how plausible they feel. A hypothesis you can close out
  today with existing data is worth more right now than a more exciting one
  that needs data nobody has yet.
- If the stakes are high enough to justify it, split the investigation
  across a few independent angles — run them in parallel (e.g. as separate
  subagents, each handed only their own lens) so they aren't anchored on
  each other's conclusions, then have them compare notes and see where they
  agree or disagree. Genuine independent convergence is much stronger
  evidence than one line of reasoning extended further.

## Phase 6 — Synthesize

Keep exactly one running file of record for the investigation — not notes
scattered across a conversation or several partial documents. Append to it
as you learn things, and *correct it in place* when something turns out to
be wrong rather than leaving the old claim standing next to the new one.

Structure every claim in the file by what kind of claim it is:

- **Fact** — directly observed in a document, the data, or the code. Cite
  where it came from.
- **Hypothesis** — someone's read on what's happening, including your own
  early ones. Name whose read it is.
- **Conclusion** — what you've concluded after checking a hypothesis
  against evidence, with the evidence cited.

Keeping these three visibly distinct is what lets someone else — or you,
later — tell the difference between "we know this" and "we think this."
Collapsing them is the single most common way investigations quietly
become less reliable than they look.

Also keep a short plain-language version of the current state alongside the
detailed one, a few sentences any stakeholder could read without the
background. Write the technical version for precision; write the plain one
so nothing gets lost in translation when someone skims instead of reading
closely.

See `references/synthesis-template.md` for a starting structure.

## Phase 7 — Recommend, with a number attached

Turn the synthesis into an actual point of view: what you think is going
on, what you'd do about it, and what you'd explicitly hold off on until
more evidence arrives.

State your confidence as a number (0–100 is a reasonable scale), and treat
it as something that moves, not a one-time score:

- It should go up when new *real* evidence lands that either confirms a
  hypothesis or rules out an alternative.
- It should **not** move when you build something illustrative with
  made-up inputs, even a well-reasoned one — a demonstration that the
  mechanism is *capable* of producing the pattern you saw is not the same
  as proof that it did. Say this explicitly whenever you build one; it's an
  easy thing to accidentally let inflate confidence if you're not careful.
- When it goes down, say why — don't quietly let a declining number happen
  without explaining the thing that moved it. A number that only ever goes
  up is a sign something's being overlooked, not a sign of a well-run
  investigation.

Name the single highest-leverage piece of missing evidence — the one thing
that, if you got it, would most change what you're confident about. That's
usually worth more than ten lower-value asks.

## Phase 8 — Prototype

A finding without something built from it is harder to act on than it
needs to be, and a recommendation that stays abstract is easy for a
stakeholder to nod at and then not act on.

- Before building, ask which deliverable(s) actually fit the moment — don't
  default to a prototype alone just because that's the usual output. A
  clickable prototype is the right default for anything the stakeholder
  will click through or hand to design/eng, but check whether they also
  want a one-pager (a tight, skimmable case for someone short on time), a
  PRD (when the next step is actually building it), or a slide deck with
  speaker notes (when this needs to be presented live, e.g. at a regroup
  or to a director). More than one of these can be true at once — a
  prototype to show *and* a one-pager to leave behind are not mutually
  exclusive. Asking costs one question; guessing wrong costs a rebuild.
- Build something real and clickable, grounded in actual data wherever
  possible rather than placeholder content.
- Where an input genuinely isn't available yet (e.g. a measurement nobody
  has made), it's fine to use a reasonable placeholder — but mark it
  visibly as assumed, not just in a footnote. The rest of the interface
  should make clear, at a glance, which parts are real and which are
  guesses standing in for something not yet measured.
- Before it goes anywhere near a real stakeholder, pressure-test it with
  feedback from whoever would actually be affected by it. If you can't get
  real people, simulate them *in character*, grounded strictly in what's
  already known or documented about them — never invent opinions or facts
  about a real person wholesale. State plainly that the feedback is
  simulated when you report it back, so nobody mistakes it for research.
- Apply what the feedback surfaces, then simulate again. Two or three
  rounds, each closing out the previous round's findings, produces a much
  sturdier result than one round taken as final. Watch for the same kind of
  gap showing up more than once across different rounds or different
  prototypes — that's usually a signal of a structural issue worth fixing
  everywhere, not just patching once.

## Phase 9 — Package for the stakeholder

Produce two things, kept distinctly separate even though they're related:

1. **What they actually asked for** — a plain breakdown of the original
   request: the context they gave, what they explicitly asked for, the
   format they wanted, and anything they explicitly said they *didn't*
   want. This is worth writing down on its own, not folded into the answer,
   so the ask and the answer can be checked against each other
   independently later.
2. **The answer** — the brief or proposal itself, grounded in the synthesis
   file, with the prototype(s) linked and the confidence level stated
   plainly rather than implied.

Keep the brief scoped to what's actually being decided. If the
investigation surfaced a bigger, separate decision along the way (e.g.
whether to reverse an earlier change entirely), don't fold that into a
brief about a narrower fix — name it as its own open question instead. A
brief that tries to settle two decisions at once usually settles neither
convincingly.

## A note on pace

This looks like a lot of phases, but they're not always sequential in
practice — Phase 3's reconciliation often sends you back to re-read
something from Phase 1 or 2 with a sharper question, and that's normal, not
a sign of doing it wrong. What matters is that none of the phases get
skipped silently: if you're moving fast and skipping one, say so, rather
than letting a brief read as more thorough than the work actually was.
