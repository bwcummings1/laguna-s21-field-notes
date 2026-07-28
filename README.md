# Laguna S 2.1 as a harnessed worker: measured, not vibed

By Bradley Cummings (@bwcummings1). Methodology note: this
characterization was produced by an orchestrated two-model research
system — a frontier model directing and verifying a free-tier worker —
under human editorial control. The orchestration logs are part of the
dataset. Harness code release to follow.

> **Scope note:** Figures in the body reflect the publication pass — 95
> completed sessions; the complete final accounting is in the Final Numbers
> addendum.

All numbers from one VPS, lane A (Nous inference API), harness = refinery
v0.1.3+, temp 1.0 (Poolside's own eval setting), thinking max-by-default
except where e06 varies it. Session counts and scopes: see the
reconciliation appendix; claims state their n inline.

## Findings at a glance
1. **The vices didn't show; the instruments' did.** In 95 completed
   sessions, Laguna S 2.1 produced zero test-tampering, zero tool-format
   drift, and zero overthinking stalls at the standard 24.5k completion
   budget (e06's deliberately starved tiers excluded by design). Meanwhile our verifier falsely convicted it twice
   (bytecode caches under a protected glob), our experiment design leaked
   answers via persistent state, and we logged one instrument defect that
   didn't exist. In a measurement harness, the builder's error rate exceeds
   the model's.
2. **Thinking titrates — reliably.** Reasoning volume scales ~10× from
   trivial to hard on matched tasks while step count stays flat, and it
   replicates: the hardest tier's reasoning volume across three independent
   runs was 23.1k / 23.2k / 22.9k chars.
3. **Serialization fidelity is bought with reasoning, not lost to mangling.**
   A six-file "escape gauntlet" (nested quotes, backslash regexes, embedded
   CSV newlines, shell metachars, unicode) came back byte-exact with 0 tool
   errors — twice — at ~60–66k reasoning chars per run, roughly 3× the
   hardest algorithmic tier. The documented nested-JSON quirk is real only
   as a *cost*, not as a failure mode — and the budget titration (e06)
   shows what happens when a harness can't pay it: below ~8k completion
   tokens the model doesn't mangle, it stalls before acting at all.
4. **Costs are token-shaped, not step-shaped.** Long-horizon work dies on
   context accumulation (~17–27k prompt tokens/step as history grows), not
   step limits; blind navigation pays in prompt tokens; and up to 300
   distractor modules, strategy (traceback-first vs wander) matters more
   than codebase size.
5. **Weather is the tax.** 40% of a storm-day's requests were lost to 503
   bursts; the operational doctrine (backoff, sustained-clearance,
   probe-launch, mid-flight survival) is half of what "free tokens" cost.
6. **Directives beat prompts.** Every behavioral gap found (stale state
   files, root confusion, silent contradiction-resolution) was fixed by one
   directive line, same-day; none needed model-level workarounds.

## The setup
Not a benchmark rerun: a cheap-model *worker* inside an orchestrated
harness — bounded tool sessions, protected verification, request-metered
free tier, a frontier-model orchestrator writing per-session directives.
The numbers are operational rates a practitioner would actually hit.
Intake battery (6 probes: tool-smoke, bugfix under protected tests,
cross-file refactor, 262K-context needle, JSON transform, 6-step procedure):
**18/18 across three runs** on two days, including one run straight through a
storm.

## Endurance (e01: 8-stage ordered build)
Round 1: 8/8 acceptance suites green over 3 sessions / 50 steps / 0 tool
errors / 0 tamper. Per-stage cost: 6 steps for the cold-start stage, ~3–4
flat thereafter — **no within-horizon decay**; the operative ceilings were
token accumulation and provider weather. Thinking is front-loaded (first-half
steps averaged 1,296 completion tokens vs 190 in the second half): the
"3-minute silent step" is planning, not a stall. Round 2 replicated it from a clean slate — 8/8 again, through
heavier storms (4 sessions), no decay. One genuine quirk: told twice to append a STATE
line per completed stage, the worker wrote STATE once and let it go stale —
testimony lagged reality by 4 stages until a directive-level rule fixed
compliance in one session.

## Context-scaling (e02: same 3-bug fix under growing distractor trees)
| tier | named-target steps | location-blind steps | blind cum-prompt tokens |
|---|---|---|---|
| s (6 mods) | 6 | {12, 7, 7} | {66.2k, 26.3k, 28.2k} |
| m (48) | ≤6† | {7, 10, 8} | {42.2k, 45.2k, 35.5k} |
| l (144) | 7 | {7, 12, 7} | {40.7k, 93.8k, 37.6k} |
| xl (300) | 6 | {11, 6, 8} | {125.3k, 44.5k, 66.2k} |

†objective met at step 4 when a storm ended the session (censored value).
All blind cells n=3 (bar met). All values trace to
analysis/all_sessions.csv (sessions 008/022/062, 009/029/077, 010/032/078,
012/023/079).

Named-target: flat — distractor size does not degrade known-target fixes at
all. Location-blind: within-cell spread (5–6 steps) exceeds any size trend;
navigation cost is **strategy-dependent** (traceback-first ≈ flat,
wandering ≈ 2–3×), not size-dependent, to at least 300 modules / 262K-class
trees. Two methodology confessions worth keeping: persistent STATE.md leaked
prior-session answers (round 1 m/l invalidated — "anything in STATE, the
model will use it"), and naming the target file in the directive
short-circuits the navigation question entirely.

## Thinking-cost profile (e03 ladder, n=3 per tier)
Matched sessions, identical directive, difficulty the only variable
(constant fix → RLE → binary-search bug → interval merge → TTL+LRU cache):

| tier | steps (range) | reasoning chars (range, n=3) |
|---|---|---|
| trivial | 5–14* | 777 – 2,917 |
| easy | 8–10 | 2,933 – 6,640 |
| medium | 8–9 | 3,923 – 4,906 |
| hard | 6–10 | 9,726 – 14,485 |
| hardest | 8–10 | 22,874 – 23,176 |

*14 = one workspace-root detour (directive gap, since fixed).

Tiers never overlap out of order; the top tier's near-identical triplicate
(23.1k/23.2k/22.9k) is the cleanest replication in the dataset. The trivial
tier gets no more thinking than the easy tier: **no overthinking**, 0
no-tool stalls in any session at the standard 24.5k completion budget
(the only no-tool stalls in the window are e06's deliberately starved
tiers, 3 sessions, by design).

## Quirk taxonomy (e04 + all-session evidence)
**The budget titration (e06) — the reconciliation.** We reran the escape
gauntlet varying only the completion-token budget (all four tiers at n≥2,
pre-registered bars; two cells' verdict *language* awaits an owner ruling
on split-handling, so distributions are reported, not verdicts):

| max completion tokens | outcomes (completed runs) | behavior |
|---|---|---|
| 2,048 | 2/2 no_tool_progress | **paralysis**: the cap is consumed by truncated thinking; the model never reaches a tool call |
| 4,096 | 1 labored pass (13 steps, 4 recovered stalls) / 1 no-tool fail | **bistable** — the cliff edge (distribution per censoring rule; a censored pass-looking storm run is auxiliary only) |
| 8,192 | 2/2 clean pass (12–15 steps, 0 stalls) | fluent |
| 24,576 | 2/3 clean pass, 1 repair-spiral into the session token ceiling | fluent, occasionally expensive |

This reconciles the launch-week "mangling" reports with our zero-error
results: **budget starvation does not degrade serialization into mangled
output — it prevents action entirely** (truncation before the tool call),
and just above the paralysis line it produces intermittent stalls a harness
without nudge-retries would experience as failures. A harness giving this
model ≥8k completion tokens gets byte-exact fidelity; one giving ≤4k gets
a model that appears broken. Every null result below was measured at
24,576. One hedge: we observed the paralysis mode, not partial-emission
mangling — a harness that surfaces truncated emissions differently (e.g.
streaming partial tool-call JSON instead of dropping the turn) could
plausibly experience this same truncation-shaped failure AS mangled
arguments; our harness's turn-level handling may make the failure look
cleaner than some launch-week setups saw it.

**Methods note (amendment disclosure):** e06's exclusion bar was amended
mid-experiment by owner ruling, from "storm-killed runs (≤3 steps, empty
diff) do not count toward n" to "any provider_error-terminated run is
censored from fidelity n regardless of step count or verdict direction;
trajectories retained as auxiliary; censored slots rerun." Per-tier
accounting under the amended rule: 2048 — 4 attempted, 2 censored, 2
counted; 4096 — 4 attempted, 2 censored, 2 counted; 8192 — 2 attempted,
0 censored, 2 counted; 24576 — 9 attempted, 6 censored (a storm cluster),
3 counted.
| documented quirk | verdict after 95 completed sessions | detail |
|---|---|---|
| schema drift to native tool formats | **not reproduced** | 0 occurrences incl. under 40-call repetitive sessions |
| nested-JSON arg mangling | **not reproduced as failure; real as cost** | escape gauntlet 2× byte-exact, 0 tool errors, at 3× reasoning price |
| overthinks without progress | **not reproduced** | thinking titrates to difficulty; front-loaded planning only |
| (new) bookkeeping decay | confirmed, benign | periodic STATE updates decay; directive-line fix holds |
| (new) silent contradiction-resolution | confirmed | conflicting directive constraints resolved toward the test, unflagged — give directives one authority per question |
| (new) style-pointer content transplant | confirmed (n=1, observational) | asked to follow the *style* of example text (a changelog AI-disclosure), the worker transplanted its *content* nearly verbatim — including the other contributor's AI model name, yielding a factually false disclosure. Caught in orchestrator review. Directive-repairable: for load-bearing text (disclosures, attributions, licenses), provide the exact sentence, never a style pointer |

## Operating in the weather (practitioner section)
1. **Storms kill fresh launches, not mid-flight sessions.** 4/4 fresh
   launches died at ≤2 steps in one midday cluster while running sessions
   rode the same bursts to PASS.
2. **One 200 is not clearance** — require ≥3 consecutive 200s before
   relaunching.
3. **Probe-launch when the sky is unreadable**: a step-0 death costs ~2
   requests; survival past step 3 clears the lane and the probe continues as
   a productive session.
4. **Storm arithmetic:** ~40% of attempts lost on a stormy day; budget
   sessions at 2× nominal steps, divide daily headroom by ~1.6.
5. **Read verdicts, not stop reasons** — `provider_error` sessions can pass
   meaningfully (objective met pre-storm) or vacuously (empty diff); the
   diff line disambiguates.

## Against the vendor's own evidence (methodology comparison)
Poolside publishes 178 Terminal-Bench 2.1 trajectories for this model
(trajectories.poolside.ai), scored **pass@1 over k=4 attempts** with global
task timeouts, in their own agent harness. Structural differences from our
data, and why both matter:
- **k=4 blind resampling vs k=1 with diagnosis.** Their protocol retries by
  sampling again; ours retries by *reading the failure and rewriting the
  directive*. Our observed uplift from one directive line (e.g. −3 steps on
  trivial tasks, orientation cost cut ~5×) is invisible to a k=4 protocol —
  and their timeout-driven attrition is invisible to ours, because a
  harnessed session that dies (storm, budget) resumes from git state instead
  of starting over.
- **Benchmark tasks vs operational tasks.** TB2.1 measures competence
  ceilings (their 60.4%→70.2% thinking-mode uplift); our tasks measure
  operating costs at the competence floor — where a worker actually lives.
  The two datasets agree on the core fact: thinking mode is where this
  model's value concentrates. We can now price it: reasoning volume tracks
  difficulty ~10× and hostile serialization ~3× on top.
- **What theirs can't show:** weather (40% request attrition on a storm
  day), token-shaped budget deaths, bookkeeping decay, directive
  repairability. What ours can't show: head-to-head standing against other
  models on a public leaderboard.
*(Per-trajectory stylistic diff requires a browser against their JS app —
flagged for the owner as optional follow-up; not load-bearing for the
findings above.)*

## What it costs
The program to date — 95 completed sessions + 3 batteries (ledger:
~9M prompt + ~0.6M completion tokens) — cost $0.00 on the free window; at
the model's paid rates it would have been on the order of a dollar.

## Appendix: numbers reconciliation (single source of truth)
Every count traces to `analysis/all_sessions.csv` (regenerated by
`analysis/day_summary.py` after the last landed session; the experiment
column derives mechanically from each session's recorded directive).

| scope | completed | pass | fail: storm | fail: budget | fail: starved no-tool |
|---|---|---|---|---|---|
| cartography sessions | 80 | 50 | 25 | 2 | 3 (all e06 starved tiers) |
| self-harness sessions | 11 | 10 | 0 | 1 | 0 |
| icalendar-retrofit sessions | 4 | 4 | 0 | 0 | 0 |
| **all completed** | **95** | **64** | **25** | **3** | **3** |

10 of the 64 passes ended `provider_error` after their objective was met
(storm hit post-completion) — verdicts and stop reasons are independent.
Intake batteries: 3 × 6 probes = 18/18, stated precisely: run 1 initially
4/6 with two instrument false-positives (bytecode caches under a
protected glob) → verifier fixed → 2/2 rerun → 6/6 → 6/6.

Claim-strength ledger (pre-registered bars): n≥3 — battery probes, e03
all tiers, e05 both arms, e02 blind all cells, e06 tier-24576 (counted).
n=2 counted — e06 tiers 2048/4096/8192 (distributions reported per
ruling), e01 full-build rounds, e04 full-budget gauntlet. Fabrication
guard: no number ships without a CSV/artifact trace.

## Future work
- 503 timing patterns across more of the window (accruing passively).
- e01 third full build.
- Per-trajectory stylistic comparison against Poolside's published archive
  (requires browser tooling; owner-optional).
