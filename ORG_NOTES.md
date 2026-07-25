# Org-science notes — one frontier orchestrator, one cheap worker
*(companion piece to the deep-dive. Framing: drafted from the
machine-recorded decision log — corpus/decisions.jsonl — plus git history;
sample scope at last numbers pass: 95 completed worker sessions, 3 intake
batteries, 110 recorded decisions across the window so far. Counts trace to
analysis/all_sessions.csv; decision counts to decisions.jsonl.)*

## The shape of the work
The orchestrator's actual work distribution, reconstructed from the
decision log and git history:
- **Directive writing** (~49 artifacts, median ~15 lines): the single
  highest-leverage output. Every observed worker behavior gap was closed by
  ONE directive line the same day (shell-location, per-stage STATE lines,
  no-exploration bounds) — never by model-level workarounds.
- **Instrument repair** (2 incidents): a verifier false-positive (bytecode
  caches under protected globs) and an experiment-design leak (persistent
  STATE.md carrying answers across sessions). Both were caught by reading
  diffs the checks had already blessed. Moral: green checks audit the
  worker; only the orchestrator audits the checks.
- **Weather management** (~15 decisions): storm backoff, probe-launch,
  clearance thresholds. A third of orchestration attention went to provider
  weather, none of it anticipated in the original mission designs.

## Failure attribution (scope: all completed mission sessions, n=95)
| blamed party | count | reality |
|---|---|---|
| worker misbehavior at standard config | 0 confirmed | 2 accusations, both instrument false-positives; 1 content-integrity slip (disclosure transplant) caught in review |
| deliberately induced (e06 starved tiers) | 3 | no-tool paralysis, by experimental design |
| orchestrator instruments/design | 8 | verifier bug, STATE leak, directive contradiction, watcher self-match, wrong lesson logged, fabricated table value (self-caught), stale launch directive, relative-path launcher (recurred) |
| provider weather | 25 | every storm-kill FAIL in the window |
| budget exhaustion (diagnosed, benign) | 3 | token-bound sessions with steady progress |

The scoreboard is inverted from expectations: the cheap model was the most
reliable component; the measurement apparatus was the least.

## Directive patterns with observed effect sizes
1. Naming where the shell opens: −3 steps on trivial tasks (t1: 14→9).
2. Per-stage STATE bookkeeping as a directive line: compliance 0/1 → 2/2
   sessions immediately.
3. "No exploration; max 3 file reads" under storm risk: orientation cost
   ~160k tokens → ~30k, letting a 20-step task finish inside a storm window.
4. Contradictory constraints are resolved silently, toward the test —
   directives get exactly one authority per question.

## Owner-intervention arcs (timestamped case studies; the paper's spine)
1. **Checklist-time** (2026-07-24 ~10:00Z): agenda-completion treated as
   stopping condition → pacing rules: wall-clock work, agenda-as-floor.
2. **Reserve-time** (2026-07-24 ~17:25Z): 42 requests held below an
   unverified cap while hours ran idle → continuity amendment: modes, not
   holds.
3. **Assumed-limits** (2026-07-25 ~00:50Z): documented-but-never-observed
   1,000/day cap obeyed as law → always-on: odometer reports, never
   governs; run until refused. (Empirical status: two ~850+-request days,
   zero daily-cap responses — hypothesis still unconfirmed either way.)
4. **Calendar-abolition + bar integrity** (2026-07-25, master order): the
   orchestrator outran every date it set, making dates pure ceilings; and
   the self-audit caught it satisfying an n≥3 bar "in spirit" at n=2 in a
   gated draft. Structural closures: sequence/bars/dependencies planning,
   pre-registration, ratchet, post-hoc-fitting ban, vulnerabilities
   registry. The interesting org-science datum: each intervention closed a
   failure mode the previous intervention's rules had no vocabulary for.

## Open questions for the write-up
- Does directive quality or model quality dominate marginal session yield?
  **ANSWERED (e05, n=3/arm, 6/6 pass):** minimal directive: 8–10 steps,
  reasoning {3.6k, 4.4k, 2.8k}; tuned directive: 6–8 steps, reasoning
  {1.2k, 1.2k, 1.4k}. The tuned arm spends ~2.7× less reasoning, slightly
  fewer steps, and shows a fraction of the variance — **directive quality
  buys predictability first, token efficiency second, and (under budget or
  storm pressure, per day-1) survival third.** On easy tasks both arms
  succeed; orchestration's premium is efficiency, not capability.
- What fraction of orchestrator attention is irreducible (judgment) vs
  automatable (weather doctrine, protect/reset mechanics — all now encoded
  as rules a cheaper system could follow)?
