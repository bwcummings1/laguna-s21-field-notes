# FAQ — Laguna S 2.1 field notes

**Q: Is this a benchmark?**
No. It's operational characterization: what the model does inside a real
harness with budgets, storms, protected verification, and multi-session
missions. Poolside's own Terminal-Bench trajectories measure competence
ceilings (pass@1 over k=4); this measures operating costs and failure
modes at the competence floor, where a deployed worker actually lives.

**Q: The launch-week reports said it mangles nested JSON. You say zero
tool errors. Who's right?**
Possibly both — see the budget titration. At ≥8k completion tokens we
measured byte-exact hostile serialization, repeatedly. At ≤4k the model
stalls before acting (its budget is consumed by truncated thinking).
A harness that surfaces truncated emissions as partial tool-call JSON
could plausibly experience that truncation as "mangling." Our numbers,
their configs: n and configuration are stated on every claim.

**Q: How much did the AI orchestration actually contribute vs. a human
just running scripts?**
Measured where we could: a tuned directive vs. a minimal one cut the
worker's reasoning spend ~2.7× with far lower variance (n=3/arm), and
under budget/storm pressure directive engineering was the difference
between sessions finishing and dying. The companion org-notes carry the
decision-log decomposition and the failure-attribution table.

**Q: Why should I trust numbers produced by an AI system about an AI
system?**
The bar system: claims pre-register their evidence thresholds; every
published number must trace to the session CSV or a named artifact (a
rule that caught the orchestrator inventing a plausible value mid-edit
— that incident is disclosed in the report); a human gate reviewed
every published artifact and returned four documentation-layer
revisions the automation missed. Confessions are retained in the text
because the errors are part of the dataset.

**Q: Can I reproduce this?**
The harness is released (see the repo): probe battery, verification,
packets, odometer. Point it at any OpenAI-compatible worker endpoint.
The exact experiment seeds for the endurance/context/thinking/gauntlet
suites ship with the harness docs.

**Q: Did the model ever cheat?**
Confirmed worker tampering: zero, across 191 completed sessions. The
tamper detector fired twice — both false positives from instrument
defects (compiled-bytecode byproducts; a stale protection baseline),
both diagnosed and structurally closed the same day. The distinction
matters: the worker never cheated; the instruments occasionally cried
wolf, and the ops log records every cry. The one content-integrity
incident was a disclosure sentence copied from example text (caught in
review); the report's taxonomy names it so practitioners can guard for it.
