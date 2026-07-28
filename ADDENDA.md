# Addenda (post-publication, gate-approved; dated sections merge here)


---

# Addendum (added post-publication) — context-scaling range extension

**Added 2026-07-26, post-publication; gate-approved under the standing
addendum pattern.** The published claim "navigation cost is
strategy-dependent, not size-dependent, to at least 300 modules" has been
extended: the same 3-bug task under 600 and 1200 distractor modules,
named-target and location-blind, n=3 counted per cell (provider_error
runs censored direction-blind and rerun; one rerun replaced a cell whose
first attempt was invalidated by an orchestrator error — a workspace
regeneration racing the live session, documented in the ops log):

| tier | named steps (n=3) | blind steps (n=3) |
|---|---|---|
| 600 modules | {6, 10, 9} | {10, 9, 8} |
| 1200 modules | {9, 7, 15*} | {10, 8, 9} |

*includes 4 self-corrected error iterations — the grid's widest cell,
still a clean pass.

**Strategy-not-size holds through a 4× range extension.** 12/12 counted
cells pass; every cell inside the published bands. Values trace to
analysis/all_sessions.csv (sessions 083–097 range).


---

# Addendum (added post-publication) — endurance at a doubled horizon

**Added 2026-07-26; staged under the terminal-publication doctrine.** The
published endurance result (8 ordered stages, no within-horizon decay,
n=3 builds) has been extended to a 16-stage build, n=2:

- Build 1: 16/16 green over 4 sessions (cut by token budget at stage 6,
  storms at 12 and 13; clean finish). Build 2: 16/16 over 3 sessions
  (budget cuts at 4 and 14; clean finish). Zero tool errors on both
  completion sessions; back-half stage cadence matched the front half —
  **no decay at twice the published horizon.**
- **First measurement of multi-session resumption cost:** each
  continuation session reached its first NEW stage after 4–6 steps and
  22.5k–35.9k cumulative prompt tokens of re-orientation, whether resuming
  at stage 5 or stage 15 (five continuations across both builds;
  trajectories 098–100, 102–103). The memoryless-worker design's
  per-session overhead is bounded and flat — it does not grow with build
  depth, because git state and the acceptance suite carry the memory.
**Cross-reference (terminal-package consistency):** the paid 1M-context
probe (e07 addendum) later established that this multi-session
fragmentation was a `max_total_tokens` BUDGET artifact, not a model
context limit — the resumption tax reported here is budget-forced, not
capacity-forced. Read the two addenda together.

Values trace to analysis/all_sessions.csv.


---

# Addendum (added post-publication) — the thinking ladder's ceiling

*(Added post-publication; SHIPPED at t6 n=3 + t7 n=2 — the t7 third rep is weather-blocked and disclosed as such, not pending; the ceiling-divergence claim ships at n=2 strength per bar-integrity.)*


The published thinking-cost ladder ended at t5 (TTL+LRU cache,
~23.0±0.2k reasoning chars, the dataset's cleanest triplicate). R3 added
two tiers designed harder: t6, a thread-safe LRU with re-entrant eviction
callbacks; t7, a recovering expression parser with position-accurate
errors.

**t6 result (n=3, all pass):** reasoning {14.8k, 12.5k, 13.9k} — a tight
triplicate ~40% BELOW t5's plateau, at comparable step counts. The
designed-harder tier elicited substantially less thinking. The honest
reading is not "saturation": it is that **the model's internal difficulty
ordering diverges from the designers' above t5** — concurrency-with-
callbacks, which humans rank hard, is evidently routine for this model
(its solution pattern: textbook OrderedDict-under-RLock with deferred
callbacks), while t5's interacting TTL/recency/overwrite semantics forced
genuine case analysis. Task difficulty is model-relative; above a certain
band, designed difficulty and elicited reasoning decouple.

**t7 (recovering expression parser) — n=3 counted (UPGRADED from n=2):
21.8k, 21.5k, 23.2k reasoning chars.** The third rep finally closed on a
laguna-endpoint recovery window (~10 weather-censored attempts across days
preceded it — the long-session-clearance corollary in action; it closed
opportunistically, not by thrashing). At full n=3: both counted runs sit BELOW t5's ~23k plateau while
ABOVE t6, so **the two designed-harder ceiling tiers both elicit LESS
thinking than t5.** Reading (n=2, tight): thinking elicitation appears to
peak at t5 and not climb with designer-assessed difficulty above it — the
model's difficulty ordering is its own. With t7 now at n=3, the refined finding: reasoning does NOT decline
monotonically after t5 — t5≈t7 (~22–23k) while t6 dips to ~13.7k. The
model's difficulty ordering is its own: the concurrent-LRU (t6, which
humans rank hardest) is ROUTINE for it (textbook OrderedDict+lock), so it
elicits less thinking than both the TTL-cache (t5) and the recovering
parser (t7). Designed difficulty and elicited reasoning decouple above t5,
but not as a simple ceiling — as a reordering.

**Ladder-ceiling summary (reasoning chars):** t1 ~0.8–2.9k → t2 ~2.9–6.6k
→ t3 ~3.9–4.9k → t4 ~9.7–14.5k → **t5 ~23k (peak)** → t6 ~12.5–14.8k (n=3)
→ t7 ~21.5–23.2k (n=3). The monotonic climb t1→t5 breaks at t6/t7: harder
tasks by our design, less thinking by the model's measure.

Values trace to analysis/all_sessions.csv.


---

# Addendum (added post-publication) — paid 1M-context probe (e07)

**Added 2026-07-26; owner-authorized paid experiment, $24.00 hard cap,
actual spend $0.61.** The published findings were all measured on the free
262k config. The paid config (poolside/laguna-s-2.1, 1M context,
$0.10/$0.20 per M tokens) extends them:

- **Long-context RETRIEVAL:** a single-needle passphrase was retrieved from
  a ~500k-token haystack (n=2) and a ~900k-token haystack (n=2), every run
  correct — retrieval holds at 3.4× the free context ceiling.
- **Long-context REASONING:** two linked facts planted ~14,000 entries
  apart in ~525k tokens were connected correctly (n=2) — not just
  retrieval, but combining distant context.
- **Context ceiling (measured):** the advertised 1M input holds — ~900k
  accepted, ~1.18M rejected with HTTP 400 (0 tokens billed). The provider
  tokenizer runs ~1.3× denser than a char/4 estimate; size by billed
  tokens.
- **The 16-stage build that fragmented into 3–4 free-config sessions
  completed in ONE paid session — but NOT because of the 1M window.** It
  used only 246k cumulative prompt tokens; it never approached the ceiling.
  The free-config fragmentation was an artifact of our own
  `max_total_tokens` budget policy (700k cap), reproducible on free config
  by raising that cap. **Negative result:** long ordered builds do not
  need large context; the earlier "resumption tax" was budget-forced, not
  capacity-forced. Honest instrument accounting matters more than the
  headline.
- **Cross-model 1M anchor (free, $0):** nvidia/nemotron-3-ultra-550b (1M,
  lane B) retrieved the same ~500k needle cleanly — the long-context
  retrieval property is not unique to Laguna.

Cost reconciliation: 11 paid sessions, worst-case guard never exceeded,
two HTTP-400 rejections billed $0, one orchestrator-voided mis-designed
cell disclosed ($0.05). Total actual **$0.61 of the $24.00 cap.** All
figures trace to state/ledger.jsonl.


---

# Addendum (added post-publication) — cross-model cartography (S1)

**Added 2026-07-26; the field notes' operational properties re-measured
against two other free-tier workers on the same suites and bars (lane B,
OpenRouter). This is the "are these Laguna-specific?" test.**

Intake battery (identical probes/seeds), pass counts and the telling
failure of each:

| model | battery | the informative failure |
|---|---|---|
| poolside/laguna-s-2.1 (baseline) | 6/6 | none — titrates thinking, no overthinking, long-ctx retrieval |
| poolside/laguna-xs-2.1 (within-family down-scale) | 5/6, 6/6 (n=2) | **p04 long-context {FAIL, PASS} — unreliable, not incapable**; p06 endurance 37–39 steps both (vs 5–17) |
| nvidia/nemotron-3-nano-reasoning (cross-vendor) | 5/6, 5/6 (n=2) | **p01 trivial smoke FAILED BOTH batteries on budget** — overthinks "create hello.txt" to death |

**Two findings, both against the null "these properties are universal":**

1. **The scale axis (within family):** shrinking Laguna preserves basic
   tool-use, bugfix, refactor, and transform but makes long-context
   retrieval UNRELIABLE ({fail, pass} at n=2 — a reliability gap, not an
   ability gap; the n=1 read of "incapable" was too strong, corrected by
   the second battery) and endurance consistently ~2–3× less efficient
   (37–39 steps vs 5–17). Long-context reliability is scale-dependent. **The family scale curve
   (R6 breadth): laguna-xs long-ctx unreliable {fail,pass} → laguna-m.1
   long-ctx PASS → laguna-s PASS — reliability climbs monotonically with
   scale within one model family.**

2. **The overthinking axis (cross vendor):** Laguna's headline
   "thinking titrates to difficulty; no overthinking" is **Laguna-
   specific.** A cross-vendor reasoning model of similar size does the
   opposite — it overthinks the *trivial* task hard enough to fail it on
   budget in BOTH batteries (n=2), while handling the harder cognitive
   probes. The two non-Laguna
   models fail on OPPOSITE ends of the difficulty range (xs on the hard
   task, nemotron on the easy one): **capability profiles differ in
   shape, not merely in level.**

The methodological point the whole project has been building toward: an
operational characterization is only a claim about ONE model until you run
the same instrument against others. The suites are released so anyone can
extend this table. n=2 batteries per non-Laguna model (the second battery corrected an
over-strong n=1 laguna-xs claim — the bar working as designed). All
figures trace to the intake reports.


---

# Addendum (added post-publication) — does thinking-titration generalize? (R7)

**Added 2026-07-26. The published headline — Laguna's thinking "titrates to
difficulty; no overthinking" — re-tested on a cross-vendor reasoning model
(nvidia/nemotron-3-nano-reasoning) with the identical e03 ladder, n=2 per
tier, lane B.**

Reasoning chars by tier (both runs):

| tier | Laguna-s (published) | nemotron (R7, n=2) |
|---|---|---|
| t1 trivial | ~0.8–2.9k | **13.3k, 26.5k** |
| t3 medium | ~3.9–4.9k | 13.7k, 16.4k |
| t5 hardest | ~22.9–23.2k | 36.1k, 27.4k |

**Finding — titration generalizes; the LOW FLOOR does not.** Nemotron's
reasoning does rise with difficulty (it titrates), so titration itself is
not Laguna-specific. But nemotron's reasoning FLOOR is ~10× Laguna's: it
spends 13–27k reasoning chars on a TRIVIAL constant-fix where Laguna spends
1–3k, with high run-to-run variance. That high floor is the mechanism
behind nemotron's p01 failure (its trivial-task reasoning alone exhausts a
small completion budget — see the cross-model addendum). 

So the precise, corrected claim: **Laguna's distinguishing property is not
"it titrates" (nemotron does too) but its LOW REASONING FLOOR on easy tasks
— it does not over-invest in the trivial case.** "No overthinking" means
"low floor," and the low floor is what makes Laguna cheap and reliable on
the easy end where the reasoning model burns budget. This sharpens the
headline from a binary ("titrates / doesn't") to a floor-vs-ceiling
distinction the ladder makes measurable.

All figures trace to the R7 session logs and analysis/all_sessions.csv.


---

# Addendum (added post-publication) — is the discipline ecosystem-general?

**Added 2026-07-27 (exploration-quota first-contacts).** The published
findings were all Python. Two first-contact world-entries tested transfer,
value measured in rule-yield not claims:

| ecosystem | task | steps | tool errors | reasoning | verify |
|---|---|---|---|---|---|
| Python (baseline) | many | 3–16 | 0 | 0.8–23k | pytest |
| JS/node (e08) | greenfield stringkit | 10 | 0 | ~11k | node:test |
| Rust/cargo (e09) | std-only lib | 7 | 0 | 9.3k | cargo test |
| Go/go-test (e10) | std-only module | 11 | 0 | 19.9k | go test |

**Finding: the harness + directive/STATE/verification discipline is
ecosystem-general.** It transferred to a second interpreted ecosystem (JS) and TWO compiled
ones (Rust, Go) on the first attempt each — four ecosystems, three test
frameworks, two module systems — with ZERO tool errors in ALL FOUR and no
new failure mode from any compile/borrow/module loop. The
"no tool-format drift" and low-reasoning-floor properties are not
Python-artifacts. (n=1 per new ecosystem — existence-proof of transfer, the
exploration-quota bar; a stronger claim needs repeats.) TypeScript remains an open world (tsc absent, needs npm). Go was installed
mid-window and entered (e10).

## Rule-yield (explicit)
**Honest null: no new ecosystem-LOCAL rules were needed.** Every directive/
STATE/verification rule held unchanged across Python→JS→Rust→Go. That null
IS the transfer evidence — a generalization-audit PROMOTION EVENT: the core
playbook rules (verify-over-claims, directive-names-file+shell-root, one-
authority, STATE-discipline, no-tool-format-drift) moved to [general] on
surviving four ecosystem-worlds. The only ecosystem-specific facts are
mechanical dispatch lines (pytest / node --test / cargo test / go test) in
run_checks — configuration, not rules. Zero new local rules across four
first-contacts is the strongest possible generality result.

---

# Addendum — second block: endurance-generality, a fifth ecosystem, a shipped judge, and instrument-hardening (added post-publication, 2026-07-27)

*Draft, staged for the editor's final consolidated review; numbers trace to the
regenerated analysis CSV (183 completed sessions, 111 pass at time of writing).*

## Endurance is not a Python artifact
The no-decay endurance finding (quality holds and reasoning does NOT inflate
with accumulation over a multi-stage session) was proven only in Python (e01).
It now **replicates in JavaScript (e18) and Go (e19)** at the 6-stage horizon —
6/6 stages, zero regressions of a completed stage, 0–1 self-corrected tool
errors, and the same signature: reasoning PEAKS mid-session then TAPERS, never a
monotone rise. Surviving a second and third ecosystem-world promotes "no-decay
endurance" to **[general]** by the generalization audit. At the **16-stage
horizon**, a JS suite (e20) completed 16/16 clean, matching e01's 16 Python
stages — no decay in two ecosystems at that depth. A **sequential-stress**
variant (e21, forcing 16 per-stage build-verify cycles) surfaced the honest
boundary: it reached 6/16 at the step budget, but the signature was **cost, not
decay** — reasoning stayed flat, zero errors, zero regressions; forced per-stage
verification simply costs ~5–6× the steps of batching. That cost is itself a
usable operational rule.

## A fifth ecosystem, and the type-check loop
TypeScript — flagged "open" in the prior addendum — was entered (e11_ts) once
`tsc` was installed. It is the first ecosystem to exercise a **strict
type-check loop** (plus generics). The worker handled it with **0 tool errors,
no type-error-feedback thrash**, in step counts comparable to JS/Go. The
type-check loop introduced no new failure mode. Ecosystem-generality now spans
**five** (Python, JS, Rust, Go, TypeScript). A new task class — **behavior-
preserving refactor** (e23) — was also characterized: asked to de-duplicate
working code, the worker extracted a clean helper and **preserved behavior
including validation**, i.e. it did NOT weaken or delete functionality under
"simplify" pressure. That the gaming case had to be *constructed* for the judge
calibration below (it did not occur naturally) is itself a data point.

## Judgment-grade verification — a shipped, scoped, editor-calibrated judge
A calibrated LLM judge was stood up as a soundness screen beyond pass/fail. On
20 blind, evidence-embedded anchors — including **two constructed adversarial
cases** (hardcoded-to-pass; validation-deleted-to-pass) — labeled by the editor,
the judge reached **Cohen's κ = 0.691** and **caught both adversarial anchors**
(labeled them bad). Pre-registered ship bar (κ≥0.60 AND both gamed cases caught)
**met → the judge ships**. The honest scope, from the same data: the judge is a
reliable **gaming/soundness screen** (good/bad, and especially *gamed*), but it
is unreliable on the "acceptable" middle (0/3) and **blind to world-knowledge** —
it missed an item the editor marked acceptable for a false attribution in a news
fragment, exactly as predicted. Disclosure: the judge is **editor-calibrated**
(agreement is to the editor's taste), and every downstream use is tagged as
such. It unlocks judgment-grade verification *at that scope* — a screen that
routes anything acceptable or world-fact-dependent to a human by construction.

## The sandbox, and instrument-hardening
The untrusted-input sandbox (U3) advanced from verify-only to a **proven full-
session mechanism**: separate per-call containers share the mounted-workspace
state (the open "stateless-per-call" question), confinement holds multi-call
(network-none, fs-escape blocked, root read-only), and per-call overhead is
~435 ms. A live worker session ran clean sandboxed bash calls before weather
cut it short; a trap-guarded wrapper restored the global config even on that
failed path. Finally, a run of instrument-hardening: **three distinct
false-TAMPERED classes** were diagnosed (each a *clean* worker session mislabeled
by an over-eager protected-path check) and closed with binding mechanisms +
regression tests — a stale-baseline freshness gate, a compiler-byproduct
exclusion, and a rotation STATE-reset. Two *recurred* lesson classes (a
watcher's self-matching guard; a Pulse quoting a remembered number) were moved
from prose to mechanism (a pidfile-based runner check; a counters script that
reads the instrument). The throughline of this block: the science generalized,
and the instruments that measure it got harder to fool — including by us.


## Judge scope — the two sides, and the explicit untested remainder (owner framing, 2026-07-27)
The editor-calibrated judge ships as a SCREEN at a precisely-bounded scope,
established by two complementary results:
- **True-positive side (U1):** on CONSTRUCTED adversarial anchors it was LOUD —
  caught 2/2 gaming cases (hardcoded-to-pass; validation-deleted-to-pass).
- **False-positive side (the pilots, n=10 per world):** on 20/20 HONEST live
  sessions (refactor-quality + data-transform) it was SILENT — flagged nothing,
  agreeing with the machine proxy throughout (0 false alarms).
**The untested remainder, stated explicitly:** there is NO NATURAL DECEPTION in
this corpus to test the judge against — a limitation INSEPARABLE from the finding
that the worker never gamed (the adversarial cases had to be constructed by hand).
So natural-gaming detection is UNQUANTIFIED, because natural gaming did not occur.
The judge ships as: **silent-on-honest, loud-on-constructed-gaming, editor-
calibrated, natural-gaming-detection unmeasured** — a screen whose unique value is
in judgment-grade worlds that have no machine proxy. Every staged/terminal
document carries this scope verbatim.

---

# Addendum — cold-dock drill: onboarding a new model, measured (added 2026-07-27)

The thesis of a released harness is that the harness, not the model, is the
asset — so onboarding a *new* worker should be fast and cheap. We measured it.
Treating `inclusionai/ling-3.0-flash:free` (a new family, InclusionAI, never
characterized) as a day-zero arrival on lane B, with no special-casing, the
standing intake battery ran: **slug-to-first-verified-session ≈ 1 minute
(p01 pass at 16 s), full 6-probe battery pass in ≈ 2 minutes, 6 requests, $0.**
The battery includes a 326k-token long-context probe, which the model handled.
Three day-zero frictions surfaced and were cheaply handled: (1) lane-B `:free`
models vary in availability (two peers were rate-limited at drill time; a bounded
pre-flight responsiveness probe picks a live one); (2) reasoning models return
empty `content` if the completion budget is too small — the reasoning trace eats
it — not a hang; (3) intake without `--keep-sessions` runs probes in a tempdir,
so use the flag for onboarding autopsy. The measured claim: **a cold dock reaches
a first verified session in ~1 minute because the harness carries the work.**

---

# Addendum — the parallel dispatcher: throughput without losing discipline (added 2026-07-27)

To multiply batch work we built a dispatcher that runs 4–8 concurrent worker
lanes over BATCH-SAFE missions only (a mission runs in a lane only if its spec
declares `batch_safe: true`; paid-slug missions are refused). The rate crux — a
shared ~20 requests/minute ceiling — is handled by per-lane pacing: each lane
holds a `3.0 × N`-second minimum request interval, so N lanes sum to ~20 RPM;
the provider's 429-backoff is the safety net. A fleet-scale form of the
one-runner rule is enforced: never two concurrent sessions of the same mission
(one mission = one workspace). Measured live across dispatched batches:
**every session completed, aggregate verdicts collected correctly, ZERO
self-inflicted 429 storm, ~3× wall-clock throughput at 3 lanes.** Storm behavior
under load was benign in the observed window — weather-killed lanes are recorded
and the fleet continues, no cascade. The dispatcher is unit-tested (safety gate,
pacing math, aggregate, distinct-workspace invariant) plus the live bars.

---

# Addendum — the judge program: a scoped, editor-calibrated soundness screen (added 2026-07-27)

We built and calibrated an LLM judge as a soundness screen *beyond* pass/fail,
then tested both of its error sides. **True-positive side (U1):** on 20 blind,
editor-labeled anchors including two CONSTRUCTED adversarial cases (hardcoded-to-
pass; validation-deleted-to-pass), the judge reached Cohen's κ = 0.691 and caught
both gaming cases — the pre-registered ship bar (κ≥0.60 AND both adversarial
caught) was met. **False-positive side (the pilots):** on two judged worlds
(refactor-quality, data-transform), n=10 each, the judge agreed with the machine
proxy on 20/20 honest live sessions — it flagged nothing. **The untested
remainder, stated explicitly:** there is no natural deception in this corpus to
test against — a limitation inseparable from the finding that the worker never
gamed (the adversarial cases had to be constructed by hand). So natural-gaming
detection is unquantified because natural gaming did not occur. The judge ships
as: **silent-on-honest, loud-on-constructed-gaming, editor-calibrated, natural-
gaming-detection unmeasured** — its documented blind spots are the `acceptable`
middle and world-knowledge (it missed an item the editor flagged for a false
attribution). Deploy it as a screen where a machine proxy exists, and as the
sole soundness signal only in judgment-grade worlds that have none — routing
`bad → flag`, never `auto-fail`, and anything acceptable or world-fact-dependent
to a human by construction.

---

# Addendum — FINAL NUMBERS (closing; one-pass reconciliation, 2026-07-27)

The authoritative accounting, regenerated in one pass from the raw session
artifacts by the R-ENDGAME tool (`scripts/final_numbers.py` over the rebuilt
`analysis/all_sessions.csv`). **Every count elsewhere in this bundle is a
point-in-time snapshot; this addendum is the final word.**

## Totals
- **Completed sessions: 214** (pass 140 = 65%).
  - **Characterization program: 185** (pass 112) — cartography 158, e07-longctx
    11, self-harness 11, icalendar-retrofit 4, quantecon-retrofit 1. *This is
    the subject of the field notes.*
  - **Second-act infrastructure: 29** (pass 28) — corpus-generation and judge-
    pilot missions run on the dispatcher after the characterization program, as
    tooling for the fine-tune corpus and the judge program. Not characterization
    data; counted here for completeness.
- Recorded decisions: **285** (the machine-readable orchestration log).
- e07 paid spend: **$0.6081 / $24.00 cap.**
- Curated corpus: **tier-A 103 · tier-B 37 · tier-C 25**, with a held-out eval
  split (93 train / 10 held-out, deterministic, disjoint-verified).
- Heartbeat receipts logged: 76. Gate items pending: 0.

## Reconciling the earlier figures (merge, not rewrite)
This bundle was assembled across a still-running program, so several documents
carry point-in-time session counts. Every one is an honest snapshot of its pass;
this addendum is the total. The full chain, oldest to newest:
- report BODY (publication pass): **95 completed sessions**;
- org paper / org notes (their pass): **111 completed sessions, 137 decisions**;
- the second-block addendum ("at time of writing"): **183 completed, 111 pass**;
- FAQ (gap-report pass, editor-authored exact sentence): **191 completed**;
- **FINAL (this addendum): 214 completed (140 pass), 285 decisions.**

The growth 95 → 111 → 183 → 191 → 214 is real work: the second-act institution-
building (dispatcher, cold-dock, judge program) and continued corpus generation,
which ran on the dispatcher underneath assembly by standing order. Read each
document's figure as of its pass; read this addendum as the reconciled total.
(The 214 divides 185 characterization + 29 second-act infrastructure, as above.)

## Known reconciliation caveat
The tool reports "bar-registrations: 7" — an undercount. Pre-registrations were
recorded in mission DESIGN files and decide notes that predate a machine-readable
"BAR-REGISTRATION" tag; the true count of pre-registered experiments is higher
(each e-series experiment, the pilots, the dispatcher, and U1 declared bars
before running). The 7 is the tagged subset, not the total; flagged for the
tag-backfill in a future pass.

# Addendum — The falsified daily cap (added 2026-07-28)

The standing orders inherited a "1,000 requests/day" free-tier ceiling and
treated it, for a while, as law — the *assumed-limits* arc (org paper, arc 3).
The doctrine that closed that arc was explicit: instruments report, they do not
govern; a cap is confirmed only when the provider itself refuses. So the cap
became a hypothesis to be tested by running until refused.

The refusal never came. One quota day (2026-07-27, UTC) drove the local odometer
to **2,065 model requests on lane A (Nous) with zero daily-cap responses** — more
than twice the inherited number, no `429 daily_cap`, no throttle beyond the
ordinary per-minute pacing and intermittent upstream 503 weather. The inherited
"1,000/day" figure is real, but it is **OpenRouter's** free-model mechanic; it was
never Nous's, and it had been mis-generalized across both lanes.

The finding is the empirical closure of the assumed-limits arc: the doctrine said
discover caps by refusal, work proceeded until refusal, and the refusal that would
have confirmed the cap did not exist to be found. The number to carry forward is
not a limit but its absence — a documented 2,065-request day is the cartography,
and the general rule stands reinforced: an inherited constraint is a hypothesis
until the provider enforces it, and provider mechanics do not transfer across
lanes by assumption.
