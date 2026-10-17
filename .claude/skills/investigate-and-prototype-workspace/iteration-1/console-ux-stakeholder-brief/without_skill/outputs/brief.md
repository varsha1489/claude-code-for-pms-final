# Console UX complaints: brief for Helen

**From:** [New PM, Rook Dispatch]
**Date:** 17 Oct 2026
**Subject:** What's going on with the handler console complaints (readability, dark mode, alerts)

## Bottom line

Four design-research interviews run by Sofia in early September surfaced a
real, consistent set of console complaints — dark mode, text/status
readability, and alert routing. They're genuine and worth fixing. But they
show up in **zero of the 25 support tickets**, so they're easy to miss if
you're only scanning ticket volume. None of them are on the Q3 roadmap —
not even in "Exploring." This brief lays out what was said, by whom, and a
rough size/effort read so you can decide where it sits against everything
else in flight (notably the 4.2 acceptance-drop work, which is a separate,
higher-stakes track covered elsewhere).

## Where this came from

- **Source:** 4 recorded interviews Sofia Marino (Product Design) ran for
  console-redesign research, 2–5 Sep 2026 — Ambrose, Aunt Dot (Dorothy
  Pell), Halloran, Kip. All four are handlers.
- **Not from tickets.** I checked: none of the 25 support tickets filed
  Aug–Sep mention readability, dark mode, or alert routing. The ticket
  stream is entirely about callouts being missed or going quiet (that's
  the 4.2 story, tracked separately). So this feedback would not have
  surfaced from support data alone — it only exists because Sofia asked.
- **Not on the roadmap.** The Q3 roadmap (rev. 30 Jun) lists exactly six
  items; dark mode, readability, and alerts aren't among them, at any
  status. This is unscoped work, not a slipped commitment.

## What was actually said

### Dark mode — one request, strongly felt
Kip (handler for 2 responders, works nights across 3 monitors) brought it
up unprompted and repeated it: *"dark mode. I am begging... the console is
just this wall of white light."* Says he's tried an OS-level color filter
as a workaround and it "makes the whole thing look sick." When asked what
one thing he'd want from the redesign: *"Dark mode. I will say it as many
times as you let me."*
- Signal: 1 of 4 interviewees, but unprompted and the single most
  emphatic ask in the whole set. Worth weighing more than "1 vote" — this
  reads like a workaround already in use, which usually means more people
  have the same problem and didn't say so.

### Readability — text and status at a glance
Two separate handlers raised this independently, for different reasons:
- **Ambrose** (desk setup, older eyes): wants **larger text on the status
  badge** — *"at a glance I sometimes cannot tell engaged from available
  without leaning in."*
- **Aunt Dot** (works standing at a kitchen counter, glancing between
  tasks): *"the text is small enough that from a normal standing distance
  I sometimes can't tell if something's changed unless I lean right in"* —
  and notes leaning in with wet hands over a laptop isn't practical.

Same root complaint (status/text too small for a glance-based workflow),
surfaced by two people with very different setups. That's a stronger
signal than either comment alone.

### Alert routing — who gets told, and how
Two distinct asks, both about notifications:
- **Kip** manages two responders on one screen and wants a **different
  alert sound per responder** — right now both just "bing" and he has to
  look to tell which one: *"That trips me up more than I'd like to admit
  at 2am."*
- **Aunt Dot** wants the **console itself to notify the handler**, not
  just the responder's phone — she currently finds out a callout came in
  by overhearing her nephew's phone buzz through the ceiling: *"maybe
  something that tells me too, not just him... I'm not trying to tell you
  your business."*

These are two different features (sound differentiation vs. a handler-side
notification channel), both filed under "alerts" but not the same fix.

### Smaller, related asks
- **Ambrose:** capability-tag legend is buried in a drawer off the
  responder detail panel; wants it closer to hand.
- **Ambrose:** filter persistence (shipped in 4.2) has reverted to default
  twice without warning after what he assumes were backend updates — he'd
  rather be warned than have it silently reset. Worth a bug look since
  it's a feature that already shipped, not new scope.
- **Halloran:** no console complaints of substance (*"Console's fine... I
  don't live in it"*) — his actual list is all Supply-side (requisition
  approval queue, no feedback loop on field failure reports, bad catalog
  search). Flagging because he raised it unprompted, but it's not console
  UX and not mine to action — passing to whoever owns Supply.

## What this is not

- Not a routing or acceptance-rate problem. That's the 4.2 investigation
  (separate, already in progress, higher stakes — declining acceptance,
  trapped responders). Don't let this list get folded into that one; they
  have different causes and different owners.
- Not evidence of how many handlers are affected. Four interviews is a
  small, non-random sample (console-redesign research subjects, not a
  survey). Treat this as "these problems are real and worth knowing
  about," not "this is how big the problem is."

## Suggested next step

Nothing here is urgent enough to interrupt the 4.2 work. Suggest folding
dark mode, readability (status badge text size), and alert differentiation
into the console redesign backlog Sofia is already running research for —
it's literally what she was interviewing for. The filter-persistence
revert bug is worth a quick engineering look since it's a regression on
something already shipped, not new scope.

A clickable mock of what "fixed" could look like (dark mode, larger status
text, per-responder alert styling) is attached separately —
`console-ux-concept.html` — for discussion, not a committed design.
