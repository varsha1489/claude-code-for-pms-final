# Handler console complaints — brief for Helen

*Dispatch PM, 17 Oct 2026. Sources: `00-rook/feedback/interviews/*.txt` (4 transcripts,
Sofia Marino's console-redesign research, 2–5 Sep 2026) and `00-rook/feedback/tickets/*.txt`
(all 25 tickets, 13 Aug–5 Sep 2026).*

## What's actually being asked, and what this is not

This is about the **console UX backlog** — readability, dark mode, alerts, filter
behavior. It is a separate question from the 4.2 routing/acceptance drop that's
already being tracked (handover notes, `notes/responder-priority-and-pov.md`).
Keeping them apart matters because **three of the four console interviews
spontaneously mention a callout vanishing before the responder could respond** —
that is the *other* investigation surfacing inside this one, not a console bug.
A UI change to the console wouldn't touch the scoring/timeout mechanism at all.
I've pulled those mentions out below so they don't get mistaken for UX asks.

## Where this comes from, and a limitation to say out loud

- **The qualitative pile:** 4 structured interviews Sofia ran specifically about
  the console redesign (Ambrose, Aunt Dot, Halloran, Kip).
- **The quantitative check:** none exists. There's no console telemetry file in
  this repo (no session logs, no feature-request counts) — only the 25 support
  tickets, which I read in full. **Zero of 25 tickets mention dark mode,
  readability, or alert routing.** That's not because the backlog is fake — it's
  because handlers don't file a ticket over squinting at a screen, only over a
  missed callout. The ticket channel is structurally blind to this category of
  complaint, so its silence shouldn't be read as "nobody cares."
- **Net effect:** this brief rests on 4 interviews, not a usage study. Sofia
  selected people with something to say, which is a real selection bias. Treat
  the ranking below as a reasonable first triage, not a validated priority order.

## Findings, by theme

| Theme | Who raised it | How many independently | Notes |
|---|---|---|---|
| Status-badge readability (text size/contrast) | Ambrose, Aunt Dot | **2 of 4**, unprompted, same UI element | Ambrose: can't tell engaged vs. available without leaning in. Aunt Dot: same complaint, different words, standing at a kitchen counter. Independent convergence on one specific element is the strongest signal in this pile. |
| Dark mode | Kip | 1 of 4, but insistent and repeated across multiple conversations ("I will say it as many times as you let me") | Self-contained, no dependency on other teams. Tried the OS color-filter workaround; called it unusable ("everything's got a fever"). |
| Distinct alert tone per responder | Kip | 1 of 4 | Specific to handlers managing 2+ responders — Kip can't tell Meteor Mite's alert from The Gale's without looking. Affects a subset, not every handler; worth sizing before committing effort. |
| Secondary notification channel to the handler, not just the responder's phone | Aunt Dot | 1 of 4 | Different problem from the one above — this is about *who* gets notified, not how it sounds. She's the one who hears it buzz through the ceiling and wants her own screen to show it too. Bigger scope than a settings toggle. |
| Filter persistence reverts silently | Ambrose | 1 of 4 — new finding, not in any ticket | Filter persistence shipped in 4.2 (as a cosmetic fix, per the roadmap it was never a committed item). Ambrose says it has silently reset to default twice post-release and he didn't notice for 15 minutes. This undermines a feature that already shipped — worth a bug ticket regardless of the redesign timeline. |
| Capability tag legend hard to find | Ambrose | 1 of 4, low frequency ("once a fortnight") | Lowest priority of the set. |
| Supply complaints (approval queue, field-failure void, catalog search) | Halloran | 1 of 4 | Not console, not Dispatch. Halloran volunteered these when asked about the console and had almost nothing to say about it ("Console's fine... I don't live in it"). Passing these along to whoever owns Supply rather than addressing here. |

### Pulled out, not a console finding
Ambrose, Aunt Dot, and (secondhand, from Halloran about Bulwark) all describe a
callout moving to someone else before the responder could respond — Captain
Vantage mid-suit-up, Vesper's handler watching the "fairness of the race, phone
to stairs" get worse, Bulwark's boots. These match the 4.2 timeout/scoring
pattern already under investigation elsewhere and are evidence for that
workstream, not a console UI defect. Flagging here only so nobody reads this
brief and concludes "the console needs a bigger accept button."

## Priority ranking

1. **Status-badge readability** — ship first. Two independent, unprompted
   reports on the same element, cheap fix, no backend dependency.
2. **Dark mode** — ship alongside #1. Only one voice, but it's a self-contained
   theme change and the ask is specific and well understood.
3. **Distinct per-responder alert tones** — worth doing, but check first how
   many handlers actually run multi-responder consoles; this only matters to
   that subset.
4. **Secondary handler-side notification channel** — real need (Aunt Dot), but
   it's a new feature (a second notification recipient), not a quick toggle.
   Needs scoping before it goes on a release.
5. **Filter-persistence reliability bug** — route to engineering now as a bug,
   independent of any redesign timeline, since it quietly breaks something
   that already shipped.
6. **Capability tag legend placement** — low frequency, nice-to-have.
7. **Supply complaints (Halloran)** — out of scope for Dispatch; hand off.

## Confidence: 55/100

This is a clean triage of what exists, not a validated priority order. It drops
from where a routing-style investigation would sit because the sample is small
(n=4) and self-selected, and there's no usage data to check it against — unlike
the 4.2 work, there's no equivalent of `callout-history.csv` for the console.
**Highest-leverage missing piece:** a rough count of (a) how many handlers run
multi-responder consoles, to size the alert-differentiation item, and (b)
whether dark mode/readability has come up anywhere else (past surveys, Slack,
support chat) to confirm these aren't just the two loudest voices in a small
sample.

## What I'd show Helen

A clickable mock of the top three items (`console-settings-prototype.html` in
this folder): larger/higher-contrast status text, a working dark-mode toggle,
and distinct per-responder alert colors — with the deferred items (secondary
notification channel, tag legend) shown but visibly marked "proposed, not
built," so nothing looks finished that isn't. That distinction — flagged versus
actually built — came up as a recurring theme in the Second Chance prototype
work too (`05-super-speed/brief.md`) and applies here just as much.

## One round of simulated feedback (simulated, not real users — stated plainly)

Grounded only in what each person actually says in their transcript:

- **Kip:** would want the alert-color assignment to happen automatically when a
  responder is added to his roster, not as a manual per-responder setup step —
  he's managing this at 2am and won't go find a settings screen.
- **Ambrose:** satisfied by the larger status text, and specifically wants the
  filter-reset issue to *notify* him rather than silently reset — matches his
  stated ask ("I'd rather it warned me").
- **Aunt Dot:** likes the larger text, but points out nothing here is for her —
  she doesn't hold the responder's phone, she hears it through a ceiling. That's
  the secondary-notification-channel gap (item 4), correctly scoped out of this
  first pass rather than quietly dropped.

Applied: the prototype auto-assigns alert colors (no manual setup), shows a
visible "filter reset" notice instead of a silent revert, and has an explicit
"Coming next" panel naming the deferred items so Aunt Dot's gap is visible
rather than invisible.
