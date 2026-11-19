# AI experimentation guide

**Phases: Build and Launch.** Use it to decide which test answers which question, and to run tests that hold up even though the model gives different answers to the same input.

The rule behind this guide: **offline evals tell you whether it's safe to ship. Online experiments tell you whether it helped.** You need both, and neither replaces the other.

---

## 1. Which test for which question

| Question | Test | When |
|---|---|---|
| Is quality above the launch bar? | Offline eval on the golden set ([eval plan](eval-plan.md)) | Every change, before any user sees it |
| Is the new version better than the old one? | Side-by-side offline comparison (pairwise judge + human sample) | Model, prompt, or retrieval changes |
| Does it work on live traffic without users seeing it? | Shadow mode: run on real inputs, don't show outputs | Before the first ramp |
| Does it move the metrics that matter? | A/B test against control | Launch and major changes |
| Does the effect last? | Long-term holdout (e.g. 5% never get the feature) | 1–3 months after launch |
| Ranking or recommendations? | Interleaving | When A/B needs too much traffic |
| Two-sided marketplace? | Switchback or geo split | When treatment leaks into control |

## 2. Handling non-determinism

The same input can produce different outputs. Plan for it.

- **Run each eval case several times** (3–5) and report both:
  - **pass@k:** passed at least once in k runs. Measures capability.
  - **pass^k:** passed every time in k runs. Measures reliability. **Use this one for launch gates.** Users only see one run.
- **Fix what you can:** pin the model version, set temperature for the use case, and log the full prompt and settings with every result.
- **Report variance,** not just the average. A version that scores 85% ± 10 is worse to ship than one at 82% ± 2.
- **Compare versions on the same cases** in the same run. A different test set is a different exam.

## 3. LLM-as-judge, done safely

- Write the rubric before the first run, with examples of pass and fail.
- Check the judge against humans on 100+ cases before trusting it. Track agreement.
- Have humans re-grade a sample (e.g. 10%) of every run.
- Swap the order in pairwise comparisons. Judges often favor the first answer, or the longer one.
- Never let the judge grade the top-harm slice alone. A human signs off on those cases.

## 4. A/B test plan

| Field | Fill in |
|---|---|
| Hypothesis | "Showing [change] to [users] will move [metric] by [amount] because [reason]." |
| Primary metric | One, from the [metric tree](north-star-metric-tree.md) |
| Guardrail metrics | Must not get worse: harm rate, complaints, cost per task, latency |
| Unit of randomization | User, account, or session. Use account for B2B so teammates see the same thing. |
| Minimum detectable effect | |
| Sample size and duration | From a power calculation. Run at least 2 full weeks. |
| Decision rule | Written before launch: ship / iterate / stop |

**AI-specific traps**
- **Novelty effect:** usage spikes in week one because it's new. Exclude at least week 1, and confirm the lift still holds in weeks 4+ (or in the long-term holdout) before calling it.
- **Acceptance is not accuracy:** users may accept wrong answers. Spot-check accepted outputs.
- **The model changed mid-test:** pin the version, or restart the test.
- **Cost is a metric:** a win that doubles cost per task may not be a win. Report it.

## 5. Ramp plan

| Stage | Traffic | Duration | Move on when | Roll back when |
|---|---|---|---|---|
| Internal dogfood | Employees | 1–2 weeks | No blocking bugs; feedback triaged | Any top-harm error |
| Shadow | Real traffic, hidden | 1 week | Shadow eval ≥ launch bar | Below bar |
| Canary | 1% | 1 week | Guardrails hold | Any guardrail breached |
| Ramp | 10% → 50% | 1–2 weeks each | Primary metric flat or up; guardrails hold | Any guardrail breached |
| GA | 100% (keep holdout) | | | Kill switch stays live |

## Exit gate

**To leave each ramp step:** the primary metric, guardrails, any non-inferiority margin, and the sample size were written before the test, and the decision follows the rule written in advance.

---

Related: [Eval plan](eval-plan.md) · [Launch readiness checklist](launch-readiness-checklist.md) · [Post-launch runbook](post-launch-runbook.md)
