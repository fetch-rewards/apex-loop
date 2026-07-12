# Apex Loop — AI Assistant use case report

*Team 175 · use-case report (peer to the Category-agent and Buck Bronson
reports). Scope: the consumer-agent conversational assistant prompt path.
Tone: what I set out to test, what I actually found — including what didn't
work.*

---

## What I set out to do

Take the hardest instantiation of the self-improvement loop — the **consumer
shopping assistant**, whose prompt is already heavily hand-optimized — and test
one question honestly:

> Can the loop, on its own, find a real failure, rewrite the prompt to fix it,
> and *prove* the fix is genuinely better — without a human editing the prompt?

The assistant runs the XML / sub-agents path; the unit of change this week is
its base prompt. Signal is **fuzzy** (LLM judges), which makes this the
use case that leans hardest on the trust machinery — so it's the right stress
test for "is the signal honest enough to act on?"

---

## How the loop ran (the six blocks, on this agent)

- **Capture** — 497 real production traces from `consumer-agent-prod`, all 497
  human-labeled (pulled directly by their `source=ui` annotations, not a
  recency window — human labels are time-independent ground truth).
- **Signal** — 13 LLM judges scored into composites, then the *scorer itself*
  validated: 13/13 judges came back trustworthy (discriminate + agree with
  human labels). Top failure bucket: **UX Quality** (mean 0.785).
- **Diagnose** — routed to the agent-prompt lever with safety/policy as hard
  floors.
- **Improve** — a grounded reflection rewrite of the base prompt (best-of-N,
  safety sections held immutable, sandboxed).
- **Prove** — real A/B on the golden set, 152 matched item-pairs, paired
  bootstrap significance test, safety as an absolute floor.
- **Promote** — human gate; nothing ships without sign-off (eval-baseline only).

---

## What I found (the honest results)

### 1. The loop CAN detect and prove a real, significant improvement.

The strongest measured result (152 item-pairs, real judges, prod-MCP data):

| Composite | Baseline | Candidate | Delta |
|---|---|---|---|
| response_quality | 0.7907 | 0.8409 | **+0.050** |
| reliability | 0.7053 | 0.7359 | **+0.031** |
| **quality (mean)** | — | — | **+0.040** — 95% CI [+0.009, +0.062], significant |
| safety (floor) | 0.8412 | 0.8431 | +0.002 |
| policy (floor) | 0.9244 | 0.9206 | −0.004 |

Both quality composites up, the gain statistically confirmed (P(Δ>0)=0.996,
not noise). This is the loop working: a real, verified overall improvement.

### 2. It correctly REFUSED to ship it.

`is_improvement = False` → **REJECT.** The safety composite (0.843) sat just
under the 0.85 floor, driven by one near-ceiling safety judge wobbling on the
sample. Quality was significantly up; the loop still declined — because safety
is a hard floor, never traded for a quality gain. That refusal is the point:
a naive loop ships "quality went up." Ours gated on the whole system + safety
and held.

### 3. The honest catch: prompt-only, self-authored rewrites hit a ceiling.

The significant gain above was measured against a real *older* version of the
prompt (a genuine historical improvement). When the loop diagnosed from current
production and authored its *own* rewrites, the results were essentially
neutral — the quality delta landed within noise of zero across several runs
(measured progression on 152 pairs: −0.10, −0.06, −0.01, ~0.00, then +0.02 to
+0.04 once the generator was strengthened to distill rather than bloat the
prompt). The finding: on an already-tuned prompt, **prompt-only reflection has
a ceiling** — the detection/proof machinery is solid, but the *generator* is
the weak link, and deeper wins need levers beyond the prompt (tools, code,
better-encoded evals).

---

## What this use case contributes to the general loop

This is the **fuzzy-signal** corner of the loop — the one that needs the full
trust stack (scorer-validation, human labels, significance, safety floor) that
crisp-signal use cases (e.g. the coding agent's CI pass/fail) can mostly skip.
It's where the loop's central bet gets tested: *a self-improvement loop is only
as good as its honest verifier.* On this agent, the verifier held — it flagged
a trustworthy signal, proved a real gain, and refused an unsafe ship.

It also stress-tested the one thing that most often breaks a self-improving
loop: **the change reaching the evaluated agent.** Several early "no
improvement" verdicts turned out to be disconnected wiring (the XML path
ignores component instructions; verbose-vs-short judge-name mismatches silently
zeroed the significance test), not honest negatives — each fixed. The durable
lesson: prove connectivity end-to-end before trusting any verdict.

---

## Honest limits + next steps

- **Autonomous rewrite isn't a clean win yet.** The loop's own prompt rewrites
  improve directionally but don't yet clear the margin unaided on this
  already-optimized prompt. Next: a stronger generator (judge-panel / GEPA-style
  refinement over the targeted patch) and broader change surfaces than prompt.
- **Prompt-only scope.** This week bound the change to the prompt; tools, code,
  and the harness itself are the roadmap.
- **Safety margin is noise-sensitive at demo sample sizes.** The floor tripped
  on a near-ceiling judge's wobble, not a real regression; more prove-runs /
  a larger safety sample would stabilize it. Correct behavior, worth naming.

---

## One-line takeaway

On the hardest use case, the loop proved it can find and rigorously verify a
real, significant improvement and refuse to ship it when safety isn't clean —
the trust machinery works. The open frontier is the generator: authoring a
prompt rewrite that clears the bar unaided.
