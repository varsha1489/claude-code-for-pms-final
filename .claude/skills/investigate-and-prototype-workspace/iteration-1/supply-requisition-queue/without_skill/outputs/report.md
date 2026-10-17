# Supply requisition approval queue — what's going on, and a recommendation

Scope note: this is Supply's product, not Dispatch (mine). I'm writing this up because
Halloran raised it directly and asked for it to be passed along — not because I'm proposing
to own the fix. Treat the recommendation below as "what I'd tell Helen / the Supply PM,"
not a Dispatch workstream.

## What I did
- Searched the whole repo (00-rook/) for anything Supply-side touching requisitions,
  approvals, or quartermasters: tickets, interviews, the Supply one-pager, the Q3 roadmap,
  release history, the glossary, and Priya's handover.
- Confirmed there is no requisition/approval dataset in this repo (only
  00-rook/data/callout-history.csv, which is Dispatch callout data) and no ticket
  among the 25 in 00-rook/feedback/tickets/ mentions requisitions, approvals, or
  quartermasters — all 25 are Dispatch callout complaints.
- Read the one source that does speak to this directly:
  00-rook/feedback/interviews/halloran.txt (5 Sept 2026, Sofia Marino interviewing
  Halloran — nominally a console-redesign interview, but Halloran used it to raise Supply
  complaints unprompted).
- Cross-checked against 00-rook/company/supply-one-pager.pdf (the core requisition flow)
  and 00-rook/company/roadmap-q3.pdf (committed roadmap items), plus the glossary and
  Priya's handover for any other mentions.

## Key findings

1. The only evidence in this repo is one interview, not a measured trend.
Halloran (handler for Sgt. Bulwark, also runs the gear cage) is the single source. There's
no Supply equivalent of Ravi's weekly acceptance export — no time series showing requisition
volume or approval latency over time. So "way slower than usual" is Halloran's lived
experience, not yet a confirmed data finding. That's the first gap to close, the same way
the Dispatch acceptance-rate drop needed Ravi's real numbers before trusting it.

2. What Halloran actually described (00-rook/feedback/interviews/halloran.txt, lines 24-40):
- A requisition for a cracked vest plate (flagged by Halloran as urgent, not cosmetic)
  sat 11 days waiting on a quartermaster signature.
- There is a priority field on a requisition, and Halloran used it — but it "doesn't
  seem to change anything about how fast it actually moves." His words: "One queue,
  everything in it, doesn't matter if it's laces or plate." So requests aren't actually
  triaged despite a field that implies they should be.
- Separately (not the queue-speed complaint, but related Supply debt he raised unprompted):
  field failure reports "go into a void" with no feedback on whether anything happened, and
  catalog search is weak (text match only, no category filters).
- He did confirm one thing is working: maintenance scheduling has gotten better about not
  booking a service into a week the responder is likely to be out — consistent with the
  Supply one-pager's description of maintenance reading the Responder Availability Record to
  schedule into low-callout windows.

3. This is not a new problem — it's already a named roadmap commitment.
00-rook/company/roadmap-q3.pdf (revised 30 Jun 2026) lists "Requisition approval
chains" — Supply — target 4.3 — status Committed — driver: Internal. That means Product/Eng
flagged this needed work before Halloran's complaint, independent of it. Per the roadmap's
own footer, committed items are "locked" and changes go through Product — so this isn't
something that quietly fell off; it's already on the books for 4.3.

4. I can't confirm whether 4.3 has shipped yet. The only release-history document in
this repo, 00-rook/company/release-history.pdf, covers Dispatch only (4.0 Apr, 4.1 Jun,
4.2 Aug 12) and explicitly says it's compiled by Dispatch Engineering — there's no Supply
release archive here. CLAUDE.md notes Dispatch and Supply ship on the same 4.x train
numbers, and today is 17 Oct, so 4.3 is plausibly due or already out — but this repo has no
way to confirm that one way or the other. That's a fact gap, not a hypothesis.

5. No link to the Dispatch 4.2 routing/acceptance investigation. The only documented
connection between the two products is the Responder Availability Record (Dispatch writes
it, Supply reads it, for maintenance scheduling only — per the Supply one-pager). That record
has nothing to do with requisition approvals. So this is a genuinely separate problem, not a
side effect of anything in the Dispatch 4.2 release.

## What's a fact vs. a hypothesis here
- Fact: an 11-day approval wait happened on one specific requisition (Halloran's account).
- Fact: "Requisition approval chains" is a committed Supply 4.3 roadmap item, decided
  before this complaint surfaced.
- Hypothesis (Halloran's, unverified against real data): the queue is undifferentiated —
  priority field set but not acted on — and that's the mechanism making the wait long.
- Open/unknown: whether this is a widespread pattern or one bad case; whether 4.3 has
  shipped; what the committed "requisition approval chains" scope actually covers (the
  roadmap has no written spec beyond the one line).

## Recommendation
1. Don't let this land on Dispatch's plate. It's Supply's product and already has an
   owner-level commitment (4.3). My job is to relay it clearly, not adopt it.
2. Get real data before calling it "way slower than usual." Ask whoever owns Supply
   metrics (no analog to Ravi is named anywhere in this repo for Supply — worth asking Helen
   who that is) for actual requisition approval turnaround times over the last few months,
   not just this one case. One interview anecdote is the same evidentiary weight as "Marcus's
   rough numbers" on the Dispatch side — useful as a lead, not as proof.
3. Flag to Helen now, specifically because it's time-sensitive and safety-adjacent. A
   cracked vest plate sitting 11 days because the priority field doesn't do anything is worth
   surfacing before the next roadmap review, so "Requisition approval chains" (4.3) gets
   checked against this specific failure mode (priority not enforced) rather than shipping
   something that doesn't fix what Halloran hit.
4. Suggest a stopgap in the same spirit as Dispatch's "handler override" recommendation:
   until 4.3's approval-chain rework lands (or if it's already shipped and didn't fix this),
   ask the Supply PM whether quartermasters can get a quick manual fast-lane for
   safety-flagged items in the meantime — cheap, reversible, doesn't wait on the bigger fix.
5. Pass along, don't fold in, the two adjacent Supply complaints Halloran raised (field
   failure reports going unanswered, weak catalog search) — real UX debt, but distinct from
   the queue-speed problem and not urgent in the same way.
6. Close the open fact gap: confirm with Helen or Supply eng whether 4.3 has already
   shipped, since the committed item may already be live and simply not resolved what
   Halloran is describing — in which case the ask isn't "ship the committed fix," it's
   "the committed fix as built doesn't cover this case."

## Sources used
- 00-rook/feedback/interviews/halloran.txt
- 00-rook/company/supply-one-pager.pdf
- 00-rook/company/roadmap-q3.pdf
- 00-rook/company/release-history.pdf
- 00-rook/company/glossary.docx
- 00-rook/company/notes/handoff-from-priya.docx
- 00-rook/feedback/tickets/*.txt (checked for Supply mentions — found none)
