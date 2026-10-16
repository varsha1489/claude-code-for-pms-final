# What Helen is actually asking for

One-pager breaking down `director-request.txt` into a clear ask, before
building anything in response to it. See `brief.md` for the proposal that
answers this.

## Context she gave
- Marcus told her the team traced the quiet-responder problem down into the
  routing code.
- Engineering says the fix is small enough to ship same-day, if asked.
- Wen left a comment next to that code years ago asking whether it should
  work this way, and nobody ever came back to it.

## What she's explicitly asking for
1. **Not a silent config change.** Her words: "I don't want to just quietly
   change a number and call it handled."
2. **Address Wen's old question properly**, not patch around it again.
3. **Before any code is touched:** see what we'd build *instead* — a real
   proposal, not permission to go fix the number.
4. **Told from the point of view of the person it happens to** — not a
   system description, a human one.
5. **Explicitly not a setting.** Her words: "Not a setting." A toggle or a
   config flag does not satisfy this ask.
6. **Something a handler would notice** — she names Kip specifically as the
   bar: would he see a difference?
7. **Something a responder who's gone quiet would feel differently about**
   — the test is on the responder's side, not just the handler's.

## What she asked for as the deliverable
- **Format:** a one-pager.
- **Polish:** explicitly not required — "doesn't need to be polished, I'd
  rather see something real than a tidy slide."
- **Bonus, not required:** something she can click through. Her phrasing —
  "even better" — makes this a nice-to-have, not a condition of approval.

## What she did not ask for
- She did not ask for the code fix itself yet — that's being held until she
  sees the alternative.
- She did not ask for a decision on reverting or reweighting 4.2 — this
  request is scoped to the quiet-responder experience specifically.
- She did not ask for a finished, production-ready design.

## Owner and next step
- Owner: me (Dispatch PM), responding directly to Helen.
- Needs: Marcus, for a gut-check on feasibility before this goes back to
  her; Sofia, if the eventual design touches the console or the app.
- Next step: `brief.md` is the one-pager that answers this ask. A clickable
  version is the stretch goal Helen mentioned, not yet started.
