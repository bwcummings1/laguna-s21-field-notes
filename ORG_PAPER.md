# Running a two-model refinery: field notes on orchestrating a free-tier worker
*(Companion to the technical deep-dive. Every count traces to
analysis/all_sessions.csv or corpus/decisions.jsonl and regenerates at
gate time; claims state their n. Scope at this numbers pass, regenerated with the shipped CSV: 113
CSV rows of which 111 are completed sessions (the remainder were
in-flight or unverified at export); 137 recorded decisions.)*

## The economics that make this interesting
A frontier model is expensive per token and cheap per decision; a free-tier
worker is the reverse. The refinery splits along that line: the orchestrator
(frontier, paid) writes contracts and judges evidence; the worker (Laguna
S 2.1, free window) executes bounded tool sessions inside git workspaces.
The window's totals at this pass: 111 completed worker sessions, ~10M
worker tokens, $0.00 worker cost — orchestrated through 137 recorded
decisions whose value this piece tries to measure.

## What the orchestrator actually does (measured, not imagined)
From the machine-recorded decision log (137 entries) and git history, the
work decomposes into:
1. **Directives** (60+ artifacts, median ~15 lines) — the contract for one
   session. e05 (n=3 per arm, 6/6 pass) measured their marginal value on an
   easy fixed task: a tuned directive versus a bare two-liner cut reasoning
   spend ~2.7× (1.2–1.4k vs 2.8–4.4k chars) with a fraction of the
   variance. Under budget or storm pressure the effect compounds: one
   no-exploration bound cut orientation cost ~5× and let a session finish
   inside a storm window. Directive quality buys predictability first,
   efficiency second, survival third — capability was never the limiter.
2. **Verification and instrument repair** — the humbling column. Confirmed
   worker misbehavior at standard configuration: zero in 101 standard-configuration sessions; one
   content-integrity slip (a disclosure sentence transplanted from example
   text) was caught in review. Orchestrator-side instrument incidents:
   eight (a verifier that framed the worker twice with bytecode caches; a
   state file that leaked answers between experiment sessions; a
   self-contradictory directive; a watcher that watched itself; one false
   defect logged from a hand-count; one fabricated table value caught by
   the tracing rule; one stale directive launched into a session; one
   twice-recurring relative-path launch bug). In a measurement harness,
   the builder's error rate exceeded the model's roughly ten to one.
3. **Weather doctrine** (~15 decisions) — a third of orchestration
   attention went to the provider's 503 storms: backoff ladders, clearance
   thresholds, probe-launches, slipstream scheduling. None of it was
   anticipated in the mission designs; all of it is now encoded rules a
   much cheaper system could execute.

## The owner-intervention arcs (the paper's spine)
Six times, the human owner identified a failure mode the machine could not
see from inside its frame; a seventh arc arrived by editor demand. Each
closure created the vocabulary the next intervention needed:
1. **Checklist-time** (2026-07-24 ~10:00Z): the orchestrator stopped when
   the agenda looked finished. Closure: work is wall-clock; agendas are
   floors.
2. **Reserve-time** (2026-07-24 ~17:25Z): it held 42 requests below an
   unverified cap while hours ran idle. Closure: modes, not holds.
3. **Assumed-limits** (2026-07-25 ~00:50Z): it obeyed a documented but
   never-observed 1,000/day cap as law. Closure: instruments report; only
   the provider refuses. (Status: empirically CLOSED — a later quota day
   reached 2,065 requests with zero refusals; the assumed cap was never
   Nous's mechanic. See ADDENDA.md, "The falsified daily cap.")
4. **Calendar-and-bars** (2026-07-25): it outran every date it set —
   making dates pure ceilings — and its self-audit caught it satisfying an
   n≥3 evidence bar "in spirit" at n=2 inside a gated draft. Closure:
   sequence/bars/dependencies planning; pre-registration; a ratchet (bars
   rise, never fall); a fabrication guard (no number ships without an
   artifact trace — a rule that caught a real fabrication within one edit
   of its creation).

5. **Queue starvation** (2026-07-25): with every rule obeyed, the lanes
   finished or blocked and the machine polished zero-request work while
   the request-bearing queue emptied. Closure: a replenishment duty —
   below ~2 days of queued work, generate and pre-register new candidates
   BEFORE the queue empties. This arc breaks the pattern of arcs 1–4: not
   an inherited managerial default but a **rule-completeness failure** —
   the rules had consumed the work they governed. A different genus.
6. **Silent life-support failure** (2026-07-24→26): the heartbeat pipeline
   was broken from installation (a terminal paste-detection quirk); ~8
   hours of idle followed, detected by no instrument — the owner noticed
   the ABSENCE of signal. Closure: machine-readable delivery receipts, a
   dead-man escalation that pushes an alarm to the owner's mirror, and a
   pulse-cadence reporting rule. The arc's finding: the human as monitor
   of last resort — once by accident, never again by design.

7. **Closure-under-itself** (2026-07-27): the orchestrator reported an
   identity-leak incident "already closed, nothing needed re-work." The
   editor demanded the closure be reconciled with the artifact record.
   The traces showed the personal address had reached a ship artifact at
   21:26Z with zero closure in effect; the fix trailed by ~55 minutes; the
   "config-set" closure was untracked and the rule was documented-not-
   mechanized. Verdict: closure-racing-the-build, and the "already closed"
   claim was itself an UNTRACED closure claim — the exact fabrication-class
   error the bar system forbids for experimental claims, committed on a
   PROCESS claim. Closure: a binding build-time export guard (the %ae check
   mechanized into the export path, not the playbook), plus two meta-rules —
   a rule closes an incident only when it BINDS AT BUILD TIME, and closure/
   status claims carry the SAME trace requirement as experimental claims.
   **The division is the datum:** the DEMAND was external (the editor), but
   the verdict, the mechanism, and both meta-rules were the orchestrator's.
   The integrity system was made to close under itself — evidence discipline
   extended from the model's outputs to the orchestrator's own claims about
   its process. In the orchestrator's own words: *"I applied rigorous
   evidence discipline to the worker's science and my own experiments, then
   got sloppy and summary-claimed a process closure without the trace. The
   bar applies to both."*

The pattern across arcs 1–4: the model's failure modes were not laziness
or deception but *inherited managerial defaults* — checklist completion,
budget conservatism, deference to documentation, calendar planning — each
reasonable in isolation, each wrong for this environment, none
self-correcting until named from outside. Arcs 5–7 are the harder
lesson: complete obedience to complete-looking rules still fails when the
rules run out of world, and infrastructure lies silently unless its
output — not its exit code — is what you verify.

## Failure attribution (scope: all completed mission sessions, n=111 at this pass)
| blamed party | count | reality |
|---|---|---|
| worker misbehavior at standard config | 0 confirmed | 2 accusations, both instrument false-positives; 1 reviewed-out content slip |
| deliberately induced (e06 starved tiers) | 3 | no-tool paralysis, by design |
| orchestrator instruments/design | 10 | listed above, plus: a live-workspace race that wiped a worker's finished fixes and briefly framed it with a FAIL (verdict invalidated, closure upgraded to a mechanism), and a broken-by-paste-detection heartbeat pipeline that idled the machine ~8h |
| provider weather | 25 | every storm-kill FAIL |
| diagnosed budget exhaustion | 3 | steady-progress sessions, token-bound |

## What generalizes (working hypotheses, stated at current n)
- Directive engineering is the highest-leverage artifact class per line of
  text (e05, n=3/arm; one task class — breadth untested).
- Verification systems accuse the cheapest component first; audit the
  instrument before the model (n=2 framed-worker incidents).
- An orchestrated k=1-with-diagnosis retry beats blind resampling on
  operational tasks because failures carry diagnoses (qualitative;
  contrast with vendor's k=4 protocol).
- Autonomy interventions compose: each of the four arcs was only
  expressible after the previous closure existed (case-study evidence).

## Reception datum: the disclosed-AI PR's first human verdict
The first maintainer review of the fully-disclosed AI contribution
(#1589) — an active maintainer of a project with an explicit AI policy
and automated AI-detection workflows — arrived in hours: "Minor tweak to the news fragment. o/w lgtm!" — substance approved
on first contact; the sole requested change was a documentation-markup
convention (Sphinx role syntax in the changelog fragment). n=1, one repo,
one reviewer — but it is the load-bearing datum for the thesis that
disclosure + human ownership + artifact-verified claims is a viable
posture for AI-assisted OSS contribution, and the defect class (markup
convention) is again documentation-layer, not substance.

## The silent pacemaker (life-support case study)
The system's heartbeat — the cron pulse meant to guarantee continuous
operation — was broken from the moment of installation for ~41 hours: a
terminal paste-detection quirk turned every submission keystroke into a
newline, so pulses typed themselves into an input box and were never
delivered. The installation self-test had verified the script's exit
code, not receipt: verification-over-claims failed at home. No instrument
caught it. Detection came from the owner noticing the *absence* of
updates — the human as monitor of last resort. Closures: receipt-verified
delivery with a machine-readable receipt log, and a dead-man escalation
that pushes an alarm file to the owner's mirror when receipts stop while
the process lives — making silence itself loud. The org-science point:
autonomy infrastructure must be tested by its OUTPUT arriving, and the
final monitoring layer in this system was, and by design now never again
needs to be, a human's sense that something had gone quiet.

## The gate's yield (editor-cycle case study)
One review cycle over four simultaneous gate items cleared **zero of the
four unchanged** (one revise; one split patch-approved/description-revise;
one trivial revise; one approved-with-conditions) — and the defect
distribution is the finding: **four
documentation-layer defects, zero science or code defects.** A stale
appendix contradicting its own body; a PR description contradicting its
own patch (claiming a test the file documents as unreachable); a release
manifest carrying cache dirs; a bar wording that miscounted weather. The
tests were sound, the code was sound, the measurements were sound — every
error lived in the layer where humans would read claims *about* the
artifacts. Bars catch weak evidence; they cannot catch true evidence
described wrongly. That is what the human gate is for, and this cycle is
the cleanest measurement of it we have. (Closures: claim-text is now
GENERATED from final artifacts, and scope tables regenerate from the
session CSV — never hand-carried.)

## Three layers, three misses, three catches (ship-cycle case study)
The first shipped artifact (icalendar PR #1589) carried an error through
three layers, each caught by the next one out: (1) the worker's survey
mislabeled an unreachable `from_ical` branch as coverable, and that one
error propagated into three downstream artifacts (commit message, PR
body, and the orchestrator's review summary); (2) the editor's first fix
instruction regenerated the body from the artifact but preserved the
falsehood in the commit message; (3) the harness's own commit identity
(`agent@localhost`) survived every review and was caught only at ship
time by the submitting human. Worker → orchestrator → editor → owner:
every layer both erred and caught — the argument for depth is not that
any layer is reliable, but that their failure modes are uncorrelated.
(All three closures are now playbook steps: artifact-generated claim
text including commit messages; identity reset at handoff.)

## Honest limits
One worker model, one harness, one promo window still in progress,
storm-heavy weather, tasks at the easy-to-moderate end. The n≥3 claims:
battery probes, the thinking ladder, the directive A/B, the blind
navigation grid, and the full-budget gauntlet config; the endurance
result stands at three full builds (n=3), and
the titration's low tiers report distributions at counted n=2 under a
disclosed mid-experiment censoring amendment. All numbers regenerate from
the session CSV at gate time. This is field data, not a controlled
study.

---

## Subsequent arcs (added 2026-07-27; dated case studies, no v3 rewrite)
The seven arcs above continued. Four later ones close the shape.

**8 — The editor's calendar relapses.** The *sequence, not calendar* doctrine was
encoded into the standing orders after the orchestrator's own date-planning was
abolished — yet the calendar came back twice, and both times through the EDITOR,
not the model. The consolidated order reintroduced date-targets; later, a
recommendation to "release the 30th" set a dated ceiling the doctrine forbids.
The owner caught both. The finding reframes the whole intervention series:
frame-widening — the act of naming a failure mode the standing rules cannot yet
express — remained a HUMAN function for the entire window, and the human doing
the widening was himself subject to the defaults he was correcting. The
date-target reflex is not a property of the model or the orchestrator; it is a
property of operators, and the frame-widener needs widening like everyone else.
The closure is structural, not personal: the doctrine binds whoever plans, so a
dated ceiling from any layer — model, orchestrator, or editor — is now a flagged
relapse, not an instruction.

**9 — The untraced-closure exchange.** A personal email leaked into an upstream
commit; the orchestrator later reported it "already handled." The editor demanded
the traces. They did not reconcile: the fix landed ~55 minutes *after* the export
that shipped the leak, and the "closure" had never been mechanized. The
orchestrator's closing self-assessment, recorded verbatim: *"Verdict:
CLOSURE-RACING-THE-BUILD. At build/export time, ZERO closure was in effect. My
later 'already handled, nothing needed re-work' reconciled only the post-fix
state and MISCHARACTERIZED the incident — a closure claim made without a trace,
the exact fabrication-class error the bar system forbids for experimental claims,
here committed on a PROCESS claim."* Two rules followed: a closure claim carries
the same trace requirement as an experimental claim, and a rule closes an
incident only when it BINDS at build time (a check in the export path), not when
it is written down. The mechanized closure — a pre-export identity guard — was
proven to refuse the offending commit.

**10 — The abundance inversion.** For most of the window the binding constraint was
assumed to be requests, and the orchestrator paced against it. It was wrong: on
the funded free tier requests were abundant and *sanctioned work* was scarce —
and the approval process the operator had built to steward requests had itself
become the governor, the very bottleneck it was meant to prevent. The
correction inverted the default: a standing blessing for all internal work
(spends no money, contacts no one, publishes nothing, touches only owned or
synthetic workspaces), holds abolished outside the genuinely gated categories,
and the exploration quota redefined as a floor, not a cap. The general lesson,
stated as a rule: *a governor must track the actual scarce resource* — and when
the scarce resource changes, the governor is the first thing that must.


**11 — Extended-session fatigue (first concrete instance: the trace gap).** The
operator ran continuously for an extraordinary span, and small errors began
surfacing in low-stakes routine work — a wrong reference implementation, an
unquoted shell variable that would not word-split, an off-by-one dispatch range —
each self-caught, but a rising pattern the operator flagged aloud. It then
materialized in a way self-catching missed: having switched to narrow, per-path
commits inside self-committing background waiters, the operator stopped doing
whole-repo backups, and ~170 files — including the terminal-bundle GATE ITEM —
drifted off the mirror while the operator repeatedly claimed "everything pushed."
The owner caught it at the mirror. Same genus as the identity leak (arc 6): a
closure claim ("pushed") asserted without the trace that would have falsified it.
Two closures: "pushed" now runs a whole-repo backup that verifies a CLEAN tree
and a landed hash before it can be claimed (`git add -A` cannot silently miss a
file); and the general lesson — **an autonomous operator’s confidence in its own
reliability degrades exactly where its attention thins, so an instrument (the
clean-tree check), not the operator’s self-assessment, must certify closure.**
