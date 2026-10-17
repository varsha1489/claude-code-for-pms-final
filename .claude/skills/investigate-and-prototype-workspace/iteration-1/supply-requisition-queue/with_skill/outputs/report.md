# Investigation record: Supply requisition queue

Task: "Supply's requisition approval queue is way slower than usual and Halloran's complaining — can you figure out what's going on and put together a recommendation?" Produced using the `investigate-and-prototype` skill (`.claude/skills/investigate-and-prototype/SKILL.md`).

## Phases followed, and how
- **Context file check:** CLAUDE.md's "Working context" section already covers this and even flags this exact complaint: "Halloran ... raised three Supply complaints unprompted ... Not mine to fix; worth passing along." Built on that instead of re-deriving it.
- **Phase 0 (confirm the ask):** Confirmed via existing context rather than asking — requester is the Dispatch PM, not Supply's owner; underlying goal is an informed hand-off, not a Dispatch-side fix. Flagged back that "way slower than *usual*" is itself an unverified premise (see Phase 3).
- **Phase 1 (listen):** Only one qualitative source exists in this repo on this topic — Halloran's 5 Sep console-redesign interview (`00-rook/feedback/interviews/halloran.txt`), where he raised Supply complaints unprompted, twice. Checked all 25 support tickets by text search for requisition/quartermaster/supply — zero hits. No second pile to compare against, which is itself a finding.
- **Phase 2 (count):** No Supply quantitative data exists anywhere in this repo. `00-rook/data/callout-history.csv` is Dispatch-only (responder ping/acceptance data).
- **Phase 3 (reconcile):** Checked Halloran's claims against the Supply one-pager (confirmed: single queue, a priority field exists) and the roadmap (confirmed: a fix is already committed). Central finding: the task's "way slower than usual" framing is **unconfirmed** — even Halloran describes a chronic problem ("I've been sitting on it a while"), not a recent worsening, and nothing in this repo shows a before/after.
- **Phase 4 (mechanism):** No Supply code exists in this repo (only `00-rook/code/dispatch-routing/`), so the actual queue/approval logic can't be read. Roadmap shows "Requisition approval chains" committed for **Supply 4.3**; no Supply 4.3 release notes exist yet to check spec-vs-shipped.
- **Phase 5 (hypothesize):** Three falsifiable hypotheses (below). No parallel subagent split — evidence is effectively n=1, too thin to justify independent lenses.
- **Phase 6 (synthesize):** One running file, `synthesis.md`, with facts/hypotheses/conclusions kept visibly separate, a reconciliation table, and a dated confidence line.
- **Phase 7 (recommend):** Confidence given as a number (25/100), with the single highest-leverage missing piece of evidence named.
- **Phase 8 (prototype):** Built one illustrative, clickable mockup (`prototype-requisition-triage.html`) — today's single queue vs. a tiered-by-urgency concept — every item/number marked assumed except Halloran's real vest-plate example. Pressure-tested with one round of simulated in-character feedback from Halloran (grounded in his transcript, clearly labeled simulated, recorded verbatim in `simulated-feedback-round-1.md`). Only one round run, since the evidence base is a single documented voice; that round already surfaced a clear, actionable gap. Per the skill's own rule, this does not move the confidence number.
- **Phase 9 (package):** Ask-vs-answer breakdown and the answer produced as separate sections in `brief.md`, linking rather than restating the synthesis and prototype.

## Key findings (tagged)
- **Fact:** Halloran's 5 Sep interview is the only evidence for this entire problem in this repo — an 11-day wait for a cracked vest plate, the priority field set with no apparent effect, and a description of one undifferentiated queue ("doesn't matter if it's laces or plate").
- **Fact:** "Requisition approval chains" is already a **committed** Supply 4.3 roadmap item (`00-rook/company/roadmap-q3.pdf`) — a fix in this space is already locked in, independent of this ask.
- **Fact:** No Supply data, Supply tickets, Supply code, or named Supply PM exist anywhere in this repo (team directory lists Helen as owning roadmap/commitments across both surfaces; Nadia and Ravi are shared across both but nothing Supply-specific appears in their outputs here).
- **Hypothesis (mine):** this is a chronic design gap (single queue, non-decaying/non-differentiating priority field), not a recent regression — "way slower than usual" isn't established by anything in this repo.
- **Hypothesis (mine):** the single-queue design is sufficient on its own to explain the one known case, independent of any volume change.
- **Conclusion:** the right next step is not a Dispatch-side root-cause fix — it's an informed hand-off to whoever scopes Supply 4.3, plus a request for real Supply queue-time data to actually check "way slower than usual" instead of assuming it.

## Confidence
**25/100** that "single queue, non-differentiating priority field" is the actual mechanism behind Halloran's case. His account is taken as true; what's uncertain is everything beyond it — representativeness, trend direction, and whether 4.3 already fixes it. Highest-leverage missing evidence: Supply's own per-requisition queue-time data (submitted → quartermaster decision), which this repo doesn't contain.

## Final recommendation
1. This is Supply's problem, not Dispatch's — route a full, quoted hand-off of Halloran's account (all three unprompted complaints: queue speed, silent field-failure reports, weak catalog search) to whoever scopes Supply 4.3, via Helen Achebe (no dedicated Supply PM is listed in the team directory).
2. Request Supply's equivalent of Ravi's weekly export — real per-requisition queue-time data — before treating "way slower than usual" as a confirmed fact rather than a plausible, unverified complaint.
3. Use the attached prototype only as a conversation aid to make the problem vivid, not as a design spec. Attach the two sharp questions the simulated feedback raised: who/what assigns the urgency tier (and can it be gamed if self-reported), and does tiering actually change real approval *time* or only display order — his words: "flagged isn't fixed," which echoes a recurring theme already seen in this repo's separate Dispatch prototype work.
4. Don't redesign or relitigate the already-committed 4.3 item from here; confirm it addresses triage-by-urgency and close the loop with Halloran once it does.

All source citations above point to real files already in the repo: `00-rook/feedback/interviews/halloran.txt`, `00-rook/company/supply-one-pager.pdf`, `00-rook/company/roadmap-q3.pdf`, `00-rook/company/release-history.pdf`, `00-rook/company/who-does-what.xlsx`, `00-rook/feedback/tickets/t-001.txt` through `t-025.txt` (checked, no hits), `00-rook/data/callout-history.csv` (checked, Dispatch-only), `00-rook/code/dispatch-routing/` (checked, no Supply code present). No files in the repo were edited, deleted, moved, or committed; no git commands were run.
