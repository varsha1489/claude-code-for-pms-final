# Report: console UX complaints brief + prototype

Task: "Our director wants to know what's going on with the handler console
complaints (readability, dark mode, alerts) — put together a brief and
something I can show her." Executed using
`.claude/skills/investigate-and-prototype/SKILL.md`.

## Phases followed, and how

- **Context file:** `CLAUDE.md`'s "Working context" section already exists
  and already flags this exact backlog ("The console UX backlog (dark mode,
  readability, alert routing, filter persistence) appears in zero of the 25
  tickets — it's real, but don't weigh it against the 4.2 investigation").
  Built on that rather than re-deriving it.
- **Phase 0 (confirm the ask):** Who — Helen, Director of Product. What — a
  brief plus something clickable, specifically about console complaints, kept
  separate from the already-running 4.2 routing investigation (CLAUDE.md is
  explicit that this backlog shouldn't crowd out that work, and the
  director-request precedent in `05-super-speed/` shows Helen wants
  something real to click, not a tidy slide). Confident enough on all four
  points (ask, audience, goal, my role) to proceed without stopping to ask.
- **Phase 1 (listen):** Read all 4 console-redesign interview transcripts in
  `00-rook/feedback/interviews/` in full (Ambrose, Aunt Dot, Halloran, Kip).
  Grouped by theme rather than by person; counted how many people raised each
  theme independently.
- **Phase 2 (count):** Looked for quantitative data to check the interview
  themes against. None exists for console usage — no session logs, no
  feature-request tallies. The only quantitative source in the repo
  (`00-rook/data/callout-history.csv`) is about routing/acceptance, not the
  console. Read all 25 support tickets in full as the closest available
  check and confirmed directly (not from the CLAUDE.md note, from the actual
  text) that none of the 25 mention dark mode, readability, or alert
  routing.
- **Phase 3 (reconcile):** The real finding here isn't confirm/contradict on
  a metric — it's that two completely different sourcing channels (tickets
  vs. structured interviews) produced two non-overlapping piles, and the
  reason is structural: handlers file tickets over missed callouts, not over
  squinting at a screen. Also reconciled an entanglement risk: 3 of 4
  interviews unprompted raised a callout vanishing before the responder could
  respond — that's evidence for the separate 4.2 investigation, not a console
  defect, and the brief explicitly pulls those mentions out so they aren't
  miscounted as UX bugs.
- **Phase 4 (mechanism):** Not applicable in the usual sense — there's no
  console/UI code in this repo to read (the only code present,
  `00-rook/code/dispatch-routing/`, governs scoring and offers, not
  rendering). Stated this directly in the brief rather than fabricating a
  mechanism-level finding.
- **Phase 5-7 (hypothesize/synthesize/recommend):** This task is a backlog
  triage, not a root-cause mystery, so these phases took the form of ranking
  themes by corroboration (how many independent people, unprompted) and
  resolvability (cheap/self-contained vs. needs scoping) rather than
  falsifiable causal hypotheses. Assigned confidence 55/100 on the
  resulting priority order — lower than a routing-style investigation would
  earn, because the sample is 4 self-selected interviewees and there's no
  usage data to check the ranking against. Named the highest-leverage missing
  evidence: a count of multi-responder handlers (sizes the alert-color item)
  and whether dark mode/readability has surfaced anywhere else before.
- **Phase 8 (prototype):** Built `console-settings-prototype.html` covering
  the top three ship-first items (status-text size, dark mode, per-responder
  alert color) as a working, clickable mock, with the deferred items
  (secondary handler notification channel, Supply complaints) shown but
  explicitly marked "proposed, not built" rather than left out silently. Ran
  one round of simulated feedback in character as Kip, Ambrose, and Aunt Dot,
  grounded strictly in what each says in their own transcript (stated plainly
  in the brief that this is simulated, not real user testing), and applied
  it: alert colors auto-assign instead of needing manual setup (Kip), a
  filter-reset now shows a visible warning instead of resetting silently
  (Ambrose), and Aunt Dot's gap (nothing in v1 for the handler who isn't
  holding the responder's phone) is named in the "Coming next" panel instead
  of quietly absent.
- **Phase 9 (package):** Two things kept separate: this report (what was
  done) and `console-ux-brief.md` (the actual answer, which itself opens with
  what's being asked and what it deliberately is not — a UX-fix brief, not a
  verdict on 4.2).

## Key findings

1. **Two themes converge independently and are the highest-confidence items:**
   status-badge readability, raised unprompted by both Ambrose and Aunt Dot in
   almost identical terms despite very different setups (a study with a second
   monitor vs. a kitchen counter next to a toaster).
2. **Dark mode is one voice, but an unusually loud and specific one** — Kip
   raised it in every interview he's given and already tried and rejected a
   workaround. Worth shipping even at n=1 because it's well-scoped and
   self-contained.
3. **"Alerts" is actually two unrelated asks** that would be a mistake to
   merge into one line item: distinct tones per responder (Kip — a
   differentiation problem, affects multi-responder handlers only) versus a
   second notification recipient for the handler (Aunt Dot — a who-gets-
   notified problem, bigger scope).
4. **A new, previously unlogged bug:** filter persistence (shipped in 4.2,
   never a roadmap commitment) silently reverts for Ambrose. Not in any
   ticket. Worth a bug report regardless of redesign timing.
5. **The ticket system is structurally blind to this entire backlog** — zero
   of 25 tickets mention any of these themes, confirmed by reading all 25
   directly. That's a process finding about which channel to trust for
   prioritization, not evidence the backlog doesn't matter.
6. **Entanglement risk flagged and separated out:** 3 of 4 console interviews
   spontaneously described a callout vanishing before the responder could
   respond (Vantage, Vesper, Bulwark) — this is the 4.2 routing/timeout
   investigation surfacing inside console research, not a UX bug, and the
   brief says so explicitly so the two decisions don't get conflated.
7. **Halloran's complaints are almost entirely Supply, not console** — "the
   console's fine... I don't live in it" — and his real list (requisition
   queue, field-failure void, catalog search) is flagged as out of scope and
   worth passing along, not solved here.

## Deliverables in this folder

- `console-ux-brief.md` — the brief for Helen: sourcing, themes, priority
  ranking, confidence (55/100), and what's explicitly out of scope.
- `console-settings-prototype.html` — self-contained, clickable mock of the
  top three items, built from the interview transcripts, with deferred items
  visibly marked rather than silently missing. Open directly in a browser.
- `report.md` — this file.

## What this is not

Not a verdict on the 4.2 routing drop, not a quantitative study (no console
usage data exists in this repo to run one), and not user-tested — the
feedback round in the brief is explicitly simulated, grounded only in the
interview transcripts, and stated as such.
