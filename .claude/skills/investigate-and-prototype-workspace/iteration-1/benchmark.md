# Benchmark: investigate-and-prototype — iteration 1

Evals run: supply-requisition-queue, console-ux-stakeholder-brief, dispatch-acceptance-drop-cold
Runs per configuration: 1

## Summary

| Metric | with_skill | without_skill | delta |
|---|---|---|---|
| Pass rate | 1.00 ± 0.00 | 0.80 ± 0.20 | +0.20 |
| Time (s) | 300.4 ± 33.2 | 152.9 ± 19.9 | +147.5 |
| Tokens | 108964 ± 7060 | 86914 ± 3945 | +22051 |

## Per-eval pass rate

| Eval | with_skill | without_skill |
|---|---|---|
| supply-requisition-queue | 5/5 | 5/5 |
| console-ux-stakeholder-brief | 5/5 | 4/5 |
| dispatch-acceptance-drop-cold | 5/5 | 3/5 |

## Notes

- supply-requisition-queue's assertions were too weak to differentiate the configs — both hit 5/5 despite without_skill producing no prototype and no synthesis file. Flagged for the next iteration's assertions.
- with_skill runs take ~2x the time and ~25% more tokens than baseline — the cost of the extra phases (confirm-the-ask, synthesis file, confidence scoring, simulated feedback, prototype build).
- Where assertions were well-specified (console-ux, dispatch-acceptance), without_skill consistently failed exactly the structural-discipline checks: ask-vs-answer separation, numeric confidence, named ticket-vs-data contradictions, guaranteed prototype production.
- Baselines likely outperform what a from-scratch agent would do with no project context, since this repo's own CLAUDE.md already asks for fact/hypothesis/conclusion separation and sourcing.
