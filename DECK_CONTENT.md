# Apex Loop — Demo Day Pitch Deck (content)

**Team 175 · Apex Loop · Innovation Week 2026**
Track: Agentic Product Development · Theme: Scale the business

> Self-contained slide content for any slide tool (Google Slides, NotebookLM, Gamma, Beamer) or for a teammate to build from. 6 slides, mapped to the 2-3 min Innovation → Execution → Impact arc. All AI-Assistant figures are from the real run in the report; the two other use cases are placeholders marked for the owners to confirm.

**Brand direction:** Fetch primary = orange (dominant), secondary = velvety purple (accent, sparingly — "avoid Halloween-y"). Use orange for emphasis/headers, purple for one accent per slide, dark text on white. (Exact hex/fonts: pull from Fetch Brand Guidelines deck, or the team snaps to brand in Slides.)

---

## Slide 1 — Title

**Headline:** Apex Loop
**Subhead:** A self-improvement loop that keeps AI agents good — automatically, with a human at the one decision that matters.

- Team 175 · Prakash Chaudhary, Sangam Man Buddhacharya, Akash Bhapkar
- Track: Agentic Product Development · Theme: Scale the business

**Speaker notes (~15s):** We built a loop that watches an agent in production, finds where it falls short, rewrites its own instructions, proves the change is real, and ships only what a human approves.

**Visual:** Fetch logo, orange title, one-line subhead. Clean.

---

## Slide 2 — The Problem *(Innovation, ~30s)*

**Headline:** Agents decay, and today one human babysits each one — forever.

- Agents drift: the world shifts, prompts age, new edge cases appear, quality quietly slips.
- Keeping one healthy today is manual: a person reads traces, guesses at a fix, hopes it didn't break something else.
- That cost grows linearly — one skilled human per agent, indefinitely.

**Metric we target:** time from a detected failure to a proven, promotable improvement (with no safety regression).
**Hypothesis:** a standardized, human-governed self-improvement loop turns agent failures into regression-tested candidate fixes — without a human hand-tuning each agent.

**Speaker notes (~30s):** State the problem + the metric + the bet. This is the Innovation "why."

**Visual:** simple before diagram — human ↔ one agent, repeated ×N. Orange highlight on "×N forever."

---

## Slide 3 — The Loop *(Innovation, ~30s)*

**Headline:** Six blocks around a dataset. Machines do the work; a human makes the call.

- **Capture** — pull real production traces + human labels
- **Signal** — score with LLM judges, then validate the judges themselves
- **Diagnose** — turn the signal into a bounded fix (which failure, which lever)
- **Improve** — rewrite the prompt (sandboxed, safety sections frozen)
- **Prove** — real A/B on a fixed benchmark + significance test + safety floor
- **Promote** — typed decision, halts for a human; eval-baseline only

**The one idea:** a self-improvement loop is only as good as its verifier. The trust machinery — validate-the-judge, significance, a hard safety floor — is the moat.

**Speaker notes (~30s):** Walk the ring once. Emphasize the verifier is the hard part, not the rewriting.

**Visual:** the six-block ring (reuse the report's ring diagram / demo-panel snapshot).

---

## Slide 4 — The Proof *(Execution, ~30-40s)*

**Headline:** On our hardest agent, it worked — and refused to ship an unsafe win.

Real run on the AI-Assistant conversational agent (already heavily hand-tuned; fuzzy LLM-judge signal):

- Captured **497** real production traces, all human-labeled
- **13/13** judges cleared validation before acting; top failure bucket = UX Quality (0.785)
- Proved a real gain: quality **+0.0404**, 95% CI [+0.009, +0.062], significant (P(Δ>0)=0.996) — on 152 matched pairs
- **Refused to ship it:** safety composite 0.8431 sat just under the 0.85 floor → gate held → nothing promoted

**The differentiator:** a quality gain never buys back a safety drop. That refusal is what makes the loop safe to run near production.

**Speaker notes (~35s):** This is the demo beat. The refusal is the "wow" — most loops would ship "quality went up"; ours gated on the whole system and held.

**Visual:** the decision-panel screenshot (all six blocks + HELD AT GATE verdict).

---

## Slide 5 — Three Use Cases *(Execution → Impact, ~30s)*

**Headline:** Same loop, three different agents — the signal changes, the loop doesn't.

| Use case | Signal type | What it shows | Owner |
|---|---|---|---|
| **AI Assistant** (shopping agent) | Fuzzy — LLM judges + human labels | Full trust stack; proved a gain, gated on safety | Prakash *(verified)* |
| **Async Matching** | Crisp — accept/reject | Loop with trust machinery relaxed (ground truth is cheap) | ← Sangam confirm + detail |
| **Buck Bronson** (coding/dev agent) | Objective — tests / CI / review | Loop where an objective signal permits near-automatic promotion | ← Akash confirm + detail |

**The point:** three substantially different systems, one loop shape. That's the evidence it generalizes.

**Speaker notes (~30s):** Breadth = the Impact criterion's "multiple teams." Each ~20s clip in the video slots here.

**Visual:** three columns / three small demo stills side by side.

> **NOTE for team:** the Async Matching + Buck Bronson rows are placeholders from the general framing — Sangam/Akash, confirm the one-liner + drop your real metric/clip.

---

## Slide 6 — Impact & Next Steps *(Impact, ~30s)*

**Headline:** Maintain many agents with a loop, not a person each.

- **Value:** agent quality scales without scaling headcount — humans stay only at the ship decision.
- **Reusable trust layer:** scorer-validation, human-label ground truth, significance testing, a hard safety floor — infrastructure any agent team at Fetch can build on.
- **Proven, not theoretical:** abstracted from a working eval system, run end-to-end on a real agent.

**Next:** the detection + proof machinery is solid; the frontier is the *generator* (authoring a rewrite that clears the bar unaided) and extending beyond prompts to tools/code.

**Speaker notes (~30s):** Close on scale (the Impact criterion) + one honest next step. Don't oversell — the refusal + the honest frontier are the credibility.

**Visual:** after diagram — one loop ↔ many agents. Orange highlight on "without scaling headcount."

---

## Appendix / links (for the info-slide + brief)

- Full report: https://ubiquitous-adventure-pz9vyqq.pages.github.io/apex-loop/apex_loop_report.html (org SSO)
- Framing brief: Akash's Google Doc
- All figures verified against the run-0028 compare report in the branch `feat/apex-loop-ai-assistant`.
