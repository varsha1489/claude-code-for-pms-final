# Synthesis file — starting structure

This is a starting point, not a form to fill in mechanically. Adapt section
names to the actual investigation; keep the underlying discipline — fact vs.
hypothesis vs. conclusion, one file, corrected in place, dated sections.

```markdown
# [Investigation name] — synthesis

Started [date]. One running file — new findings get appended below with a
date, and anything that turns out wrong gets corrected in place (with a
note on what changed and why), not left standing next to the correction.

## In plain words
[A few sentences any stakeholder could read without background. Rewrite
this whenever the technical picture below changes meaningfully — it should
never fall behind the detail.]

## What we know for certain (facts)
- [Claim]. Source: [file/data/code reference].
- ...

## What we think, and why (hypotheses)
- [Hypothesis], per [who/what source]. Confidence: [low/medium/high] —
  [why]. What would confirm or rule this out: [specific evidence needed].
- ...

## What we've concluded (after checking hypotheses against evidence)
- [Conclusion]. Based on: [specific evidence that moved this from
  hypothesis to conclusion].
- ...

## Reconciliation: qualitative vs. quantitative
| Claim | Source | What the data shows | Verdict |
|---|---|---|---|
| ... | ... | ... | Confirmed / Contradicted / Partial / Silent but real |

## Confidence (track this as a number over time, with dates)
- [Date]: [score]/100 — [why: what moved it since last time, what's still
  missing]

## Open items this doesn't settle
- [The single highest-leverage missing piece of evidence, named first]
- [Other open questions]

## Recommended next steps
- [What's safe to act on now, and why it's safe even with open questions]
- [What to explicitly hold off on until more evidence arrives, and why]
```

## Why this shape

**Fact / hypothesis / conclusion, kept visibly separate.** The moment these
blur together, a reader (including a future version of you) can no longer
tell confident knowledge from an educated guess, and decisions start
getting made on guesses dressed up as facts.

**One file, corrected in place.** Scattered notes drift out of sync with
each other. A single file that gets edited as understanding improves stays
trustworthy; a pile of dated snapshots requires the reader to figure out
which one is current.

**A plain-language section that's kept in sync, not written once at the
end.** The detailed sections are for precision. The plain section is for
anyone who skims — and it has to be actively maintained, or it becomes the
most convenient and most wrong summary in the document.

**A confidence number with a history, not just a current value.** Seeing
that confidence moved from 60 to 75 after a specific piece of evidence
landed is far more informative than a single static "we're pretty sure."
It also makes it obvious when something *should* have moved confidence and
didn't — a useful check on your own reasoning.
