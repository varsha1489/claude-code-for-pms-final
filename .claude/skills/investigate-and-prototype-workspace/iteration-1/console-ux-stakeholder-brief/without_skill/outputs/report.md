# Report: console UX complaints brief

## Task
Director (Helen) asked what's going on with handler console complaints —
readability, dark mode, alerts. Deliverable: a brief plus something
showable.

## What I did
1. Read all 25 support tickets in `00-rook/feedback/tickets/` to check
   whether readability/dark mode/alert complaints show up there. They do
   not — every ticket is about callouts being missed, delayed, or going
   quiet (the 4.2 acceptance story); none mention UX, readability, theme,
   or alert routing.
2. Read all 4 design-research interview transcripts in
   `00-rook/feedback/interviews/` (Ambrose, Aunt Dot, Halloran, Kip) —
   these are the actual source of the UX complaints, run by Sofia Marino
   in early September for console-redesign research.
3. Checked `00-rook/company/roadmap-q3.pdf` to confirm dark mode,
   readability, and alert routing are not on the Q3 roadmap at any status
   (not even "Exploring") — this is unscoped feedback, not a slipped
   commitment.
4. Wrote `brief.md`: a stakeholder-ready summary organized by theme (dark
   mode, readability, alert routing, smaller asks), with direct quotes,
   who said what, and an honest read on signal strength (small, non-random
   sample; real but not ticket-validated).
5. Built `console-ux-concept.html`, a local standalone clickable mockup
   with three interactive toggles — dark mode, larger status-badge text,
   per-responder alert coloring — mapped directly to the interview quotes.
   Verified it in the browser: all three toggles work (screenshot-checked
   light and dark states).

## Key findings
- **Real complaints, invisible in ticket data.** Dark mode (Kip),
  readability of status/text at a glance (Ambrose, Aunt Dot
  independently), and alert routing/differentiation (Kip, Aunt Dot) are
  consistent, specific, and unprompted across 4 interviews — but appear in
  zero of the 25 support tickets. Ticket volume alone would have
  completely missed this category.
- **Not on the roadmap at all.** Unlike Availability Confidence (committed
  then quietly dropped — a separate, already-flagged issue), this UX
  backlog was never a commitment. It's net-new scope to weigh, not a
  broken promise.
- **Two "alert" asks are actually different features:** per-responder
  sound differentiation (Kip, managing 2 responders on one screen) vs. a
  handler-side notification so the handler isn't relying on overhearing
  the responder's phone buzz (Aunt Dot). Worth not conflating into one
  ticket.
- **Readability complaint has two independent sources** with different
  work setups (desk-bound older handler vs. standing-at-the-counter
  handler) converging on the same ask — larger status-badge text. That
  convergence is a stronger signal than either comment alone.
- **One shipped feature has a reliability bug, not new scope:** filter
  persistence (shipped in 4.2) has silently reverted to default twice for
  Ambrose after backend updates — worth a quick engineering look.
- **Halloran had no real console complaints** ("Console's fine... I don't
  live in it") — his substantive list is all Supply-side (requisition
  queue, no feedback loop on failure reports, catalog search), out of
  scope for Dispatch and noted for hand-off only.
- Kept this deliberately separate from the 4.2 routing/acceptance
  investigation — different cause, different owner, already a live
  workstream — consistent with the repo's own guidance not to let UX debt
  crowd out that investigation, and not to blend the two either direction.

## Brief summary
`brief.md` leads with the bottom line (real complaints, invisible to
tickets, not on roadmap), lays out the three themes with quotes and named
sources, flags what the feedback is *not* (not a size estimate, not the
4.2 problem), and recommends folding this into the console redesign
backlog Sofia is already running rather than treating it as urgent —
except the filter-persistence revert, which is a bug worth a quick look.

## Files produced (all in this output folder)
- `brief.md` — the stakeholder brief for Helen
- `console-ux-concept.html` — clickable local prototype (dark mode toggle,
  larger status-text toggle, per-responder alert-color toggle), standalone
  HTML, verified working in-browser
- `report.md` — this file

No existing files in the repository were edited, moved, or deleted, and no
git commands were run.
