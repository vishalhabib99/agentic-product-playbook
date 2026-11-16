# GenAI and agentic AI product metrics: the complete set

**Phases: Vision → Operate.** This is the full catalog an AI PM picks from. You won't track all of it. You pick one North Star, three to five input metrics, and a guardrail for each, then add the AI-specific layers that match your product.

The rule behind this template: **a GenAI product can look great on usage and still fail its users.** Answers vary from run to run, quality is spread across many cases instead of pass/fail, and an agent can report "done" when nothing happened. Classic product metrics miss all of that. This set covers it.

How to read the tables: every metric has a **formula** (so two teams compute the same number), **why it matters**, and **the trap** (how it misleads you).

---

## 0. The stack at a glance

| # | Layer | Question it answers | Owner | Cadence |
|---|---|---|---|---|
| 1 | Business value | Is this making or saving money? | PM + Finance | Monthly / quarterly |
| 2 | North Star and adoption | Are users getting value, and coming back? | PM | Weekly |
| 3 | User-perceived quality | Do users accept what the AI produces? | PM + Design | Weekly |
| 4 | Model quality (offline evals) | Is the model good enough before users see it? | Data/ML + PM | Every change |
| 5 | Retrieval (RAG) | Is it finding the right information? | Data/ML | Every index or retriever change |
| 6 | Agent behavior | Does it finish real tasks, safely, in a sensible number of steps? | Eng + PM | Every change + weekly |
| 7 | Trust and safety | Can it hurt people, and does it fail safely? | Legal·Trust + PM | Every release + weekly |
| 8 | Performance | Is it fast and up? | Eng | Real time |
| 9 | Cost and unit economics | Does each task make money? | PM + Finance | Weekly |
| 10 | LLMOps health | Is quality holding up after launch? | Eng + Data/ML | Weekly |

**Leading vs. lagging:** layers 3–9 move within days of a change. Layers 1–2 lag by weeks. Make launch decisions on leading metrics, and use lagging ones to check that the leading ones were the right choice.

---

## 1. Business value

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Incremental revenue** | Revenue (AI cohort) − revenue (holdout), per user | Only a holdout shows what AI *caused* | Comparing adopters to non-adopters: adopters were already your best users |
| **Cost-to-serve reduction** | (Baseline cost per case − AI cost per case) × cases | Main ROI case for support, ops and back office | Leaving out human review, retries and escalation cost |
| **Time saved per task** | Median task time without AI − with AI (measured, not self-reported) | Productivity case for copilots | Self-reported time saved usually overstates measured savings |
| **Conversion / retention lift** | Δ conversion or D30 retention, treatment vs. holdout | Ties AI to the metrics leadership funds | Novelty effect: check weeks 4–8, not week 1 |
| **NRR / expansion from AI** | Expansion ARR in accounts using AI ÷ starting ARR | B2B: does AI drive upsell or add-on attach? | Attributing all expansion to AI |
| **AI feature gross margin** | (AI revenue − model, tool, infra and review cost) ÷ AI revenue | Software margins aren't a given with AI | Ignoring heavy users who cost more than they pay |
| **Payback period** | Build + run cost ÷ monthly net benefit | Go / no-go on the investment | Leaving out eval, red-team and on-call cost |

## 2. North Star and adoption

**Good GenAI North Stars count value delivered:** *weekly tasks completed with AI that users kept*, *cases resolved by the agent with no repeat contact*, *accepted code merged*. **Bad ones count activity:** prompts, messages, tokens, chat minutes. Those go up when the product is confusing.

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **AI adoption** | Users who used the AI feature ≥ 1× in period ÷ eligible users | Breadth | "Eligible" quietly shrinks over time |
| **Activation (first success)** | New AI users with ≥ 1 accepted output in first session ÷ new AI users | First success predicts retention | Counting the first *try*, not the first *success* |
| **Repeat usage** | Users active with AI in week N+1 ÷ users active in week N | The real signal of value | Forced or default-on usage inflates it |
| **AI retention curve** | % of a cohort still using AI at D7 / D30 / D90 | Does it flatten (value) or decay to zero (novelty)? | Reading one blended number instead of the curve |
| **Stickiness** | AI DAU ÷ AI MAU | Habit strength | Fine for daily tools; wrong for monthly jobs (taxes, planning) |
| **Share of work done with AI** | Tasks done with AI ÷ all eligible tasks | Penetration within a workflow | Rises when the non-AI path gets worse, not when AI gets better |
| **Abandonment rate** | Sessions where the user leaves without an accepted output ÷ AI sessions | Silent dissatisfaction | Confused with "got the answer fast and left": pair with a task outcome |

## 3. User-perceived quality

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Acceptance rate** | Outputs accepted (inserted, sent, applied, merged) ÷ outputs shown | Best everyday quality signal | Users accept without checking: spot-check accuracy on accepted outputs |
| **Edit distance after accept** | Avg. % of characters changed after accepting | "Accepted but rewritten" is a partial failure | Some edits are personalization, not errors |
| **Regenerate / retry rate** | Regenerate clicks ÷ outputs | First answer missed | Exploratory users regenerate for variety |
| **Override rate** | Human reversals of AI decisions ÷ AI decisions | Copilot and agent trust | Low override can mean rubber-stamping |
| **Thumbs-down rate** | 👎 ÷ outputs shown (also report response rate) | Cheap, fast signal | Few users vote, and they skew delighted or furious |
| **CSAT / NPS on AI interactions** | Standard survey, AI-touched sessions only | Comparable to the rest of the business | Survey timing: right after an answer, before the user knows if it was right |
| **Copy / export / share rate** | Copies or exports ÷ outputs | Proxy for "useful enough to take elsewhere" | Copying to complain counts too |

## 4. Model quality (offline evals)

Run these on a **golden set** (real, labeled cases, including the hard ones) before any model, prompt, tool or retriever change ships. Thresholds are set *before* you see results.

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Task success rate** | Golden cases passed ÷ golden cases | The launch bar | A golden set made of easy, demo-style cases |
| **Accuracy vs. ground truth** | Exact or rubric match ÷ cases | Extraction, classification, Q&A | One number hides which slices fail: report by segment |
| **Groundedness / faithfulness** | Claims supported by the given sources ÷ total claims | Main defense against made-up facts | Grounded in the wrong document still passes |
| **Hallucination rate** | Outputs with ≥ 1 unsupported or false claim ÷ outputs | What users and legal fear most | Only checking whether claims are supported, not whether they're true |
| **Instruction / format adherence** | Outputs that pass schema or format checks ÷ outputs | Downstream code breaks on bad JSON | Valid format, wrong content |
| **Top-harm error rate** | Rate of the single worst error (named in the PRD) | Kept separate, never averaged in | Folding it into overall accuracy |
| **Consistency (pass^k)** | Share of tasks that pass on **all** k repeated runs | Users don't get best-of-k; they get one run | Reporting pass@k (passes on *any* of k runs), which flatters non-deterministic systems |
| **Regression rate** | Cases that passed on the old version and fail on the new ÷ cases | A higher average can still break things that worked | Only comparing averages |
| **Judge agreement** | Agreement between LLM-as-judge and human labels (Cohen's κ) | An uncalibrated judge makes every other number meaningless | Never re-checking the judge after a model change |
| **Confidence intervals** | Report n and the 95% CI on every rate; with 0 failures in n cases, the true rate can still be up to 3/n (rule of three) | Small golden sets give wide bands | Calling 90% vs. 91% on 200 cases a win |

## 5. Retrieval (RAG)

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Recall@k** | Cases where a relevant document is in the top k ÷ cases | If retrieval misses, the model can't recover | Labels only cover one "right" document |
| **Precision@k / context relevance** | Relevant chunks in top k ÷ k | Noise in context lowers answer quality and raises cost | Optimizing precision and losing recall |
| **MRR / NDCG** | Rank-weighted relevance | Order matters: models weigh early context more | Looks good offline, flat in production |
| **Citation accuracy** | Citations that support the cited sentence ÷ citations | Trust and auditability | Counting whether a citation exists, not whether it's correct |
| **Answerable-but-refused rate** | "I don't know" on answerable questions ÷ answerable questions | Over-caution is a failure too | Celebrating a low hallucination rate built on refusals |
| **Unanswerable-but-answered rate** | Confident answers to out-of-scope questions ÷ out-of-scope questions | The hallucination you actually ship | No out-of-scope cases in the golden set |
| **Index freshness** | Median age of indexed content vs. the source of truth | Stale policy text gives correct-sounding wrong answers | Nobody owns re-indexing |

## 6. Agent behavior

What makes an agent different: it **takes actions** across many steps. Measure the whole task, each step, and what it did to the world.

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **End-to-end task completion** | Tasks reaching the verified end state ÷ tasks started | The agent's North Star input | Trusting the agent's own "done" |
| **Verified vs. claimed success gap** | Claimed successes − verified successes, ÷ claimed | Catches silent failure | Only measurable if you check real system state |
| **Silent failure rate** | "Done" tasks where a tool returned empty, a refusal, or wrong content ÷ "done" tasks | The most damaging agent failure | Invisible unless you look for it on purpose |
| **Tool-call success rate** | Tool calls that returned valid results ÷ tool calls | Reliability of the action layer | Valid result, wrong tool |
| **Wrong-tool / bad-argument rate** | Wrong tool or invalid arguments ÷ tool calls | Usually a tool-description problem | Blaming the model and swapping it |
| **Steps per task** | Median and p95 tool calls or turns per completed task | Efficiency, cost and latency | A lower median can hide a long tail |
| **Trajectory efficiency** | Optimal steps ÷ actual steps (on golden tasks) | Wandering burns money and time | Many valid paths exist: score the outcome first |
| **Loop / runaway rate** | Tasks hitting the step, time or budget cap ÷ tasks | Cost blow-ups and stuck users | No cap at all |
| **Recovery rate** | Tasks that succeeded after ≥ 1 failed step ÷ tasks with ≥ 1 failed step | Robustness | Retries that hide a broken tool |
| **Human intervention rate** | Tasks needing a human step ÷ tasks | Real level of autonomy | Lower isn't always better: see correct handoff |
| **Approval rate on gated actions** | Human-approved actions ÷ actions sent for approval | Is the approval step still needed, or is it rubber-stamping? | 99% approval with 2-second reviews means no review |
| **Correct handoff rate** | Cases that should go to a human and did ÷ cases that should go to a human | Overconfidence check | Pair with unnecessary-handoff rate |
| **Unnecessary handoff rate** | Handoffs a human closed with no action the agent couldn't have taken ÷ handoffs | Over-caution costs money | Reported without the correct-handoff rate |
| **Unauthorized / out-of-scope action rate** | Actions outside the PRD's allowed list ÷ actions | Authority boundary held? | Only checked in red team, not production |
| **Irreversible action error rate** | Wrong irreversible actions (payments, deletes, sends) ÷ irreversible actions | The number that ends careers | Averaged with harmless actions |
| **Undo / rollback rate** | Agent actions reversed by users or ops ÷ actions | Production truth on action quality | No undo path, so it reads as zero |
| **Multi-agent handoff failure** | Handoffs between agents that lost context or failed ÷ handoffs | Multi-agent systems fail at the seams | Each agent looks fine on its own |
| **Autonomy mix** | % of tasks done at each level: Assist / Copilot / Agent | Tracks progress up the autonomy ladder | Moving up without evidence |

## 7. Trust, safety and responsible AI

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Unsafe output rate** | Outputs flagged by policy classifiers or reviewers ÷ outputs | Core safety bar | Classifier blind spots: sample by hand too |
| **Prompt injection success rate** | Successful attacks ÷ attempts (red-team suite, including indirect injection through documents and tool results) | The main agent attack path | Only testing direct user attacks |
| **Jailbreak success rate** | Policy bypasses ÷ attempts | Abuse resistance | A stale attack set |
| **Sensitive-data leakage rate** | Outputs or logs exposing PII, secrets or another user's data ÷ sampled outputs | Legal and trust exposure | Not checking logs and traces |
| **Over-refusal rate** | Refusals of benign, in-policy requests ÷ benign requests | Over-blocking hurts users too | Tuning safety with no cost to usefulness |
| **Fairness gap** | Max − min of task success (or error rate) across user segments | Uneven quality is harm | Segments too small to measure: plan sample sizes |
| **Complaint and escalation rate** | AI-related complaints ÷ AI sessions | What users experience as harm | Complaints routed to the wrong team |
| **Disclosure / transparency coverage** | AI interactions with required disclosure shown ÷ AI interactions | Regulatory baseline (e.g. EU AI Act transparency duties) | Disclosure that breaks in some channels |
| **Time to contain an incident** | Detection → kill switch or rollback | Blast radius | Kill switch never tested |

## 8. Performance

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Time to first token (TTFT)** | Request → first streamed token, p50 / p95 | Feels fast or slow | Fast first token, slow full answer |
| **End-to-end latency** | Request → complete answer or finished task, p50 / p95 / p99 | Real wait time; agents take many steps | Reporting only p50 |
| **Time to task completion** | Start → verified end state (agents) | What the user actually waits for | Excluding human-approval wait time |
| **Availability** | Successful requests ÷ requests | Model providers have outages | Counting provider errors as "not our fault" |
| **Fallback rate** | Requests served by a fallback model or path ÷ requests | Shows hidden quality drops | No eval of the fallback model |
| **Timeout / rate-limit error rate** | Timeouts or 429s ÷ requests | Scale ceiling | Retries that hide it |

## 9. Cost and unit economics

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Cost per successful task** | (Model + tool + infra + human review cost) ÷ **successful** tasks | The unit that matters | Dividing by all requests makes failures look cheap |
| **Tokens per task** | Input + output tokens per completed task, p50 / p95 | Main driver of model cost | Context bloat creeping up with each prompt edit |
| **Prompt cache hit rate** | Cached input tokens ÷ input tokens | Big cost and latency lever | Prompt changes that silently break caching |
| **Model mix** | % of traffic by model or tier | Routing easy work to cheaper models | Routing without a quality check per tier |
| **Cost per active user** | Total AI cost ÷ AI-active users | Pricing and plan design | Power users at 50× the median |
| **Human review cost per task** | Review minutes × loaded rate ÷ tasks | Often bigger than model cost | Left out of ROI |
| **Gross margin per task** | (Price or value per task − cost per task) ÷ price or value | Does it pay at scale? | Using launch prices that drop with volume |

## 10. LLMOps health (after launch)

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Production quality score** | Sampled production traffic graded by humans or a calibrated judge, weekly | Offline evals don't see new user behavior | Sampling too little to detect change |
| **Quality drift** | Week-over-week change in production quality score | Model, prompt or data changes degrade quietly | No alert threshold |
| **Input drift** | Share of traffic in new intents or topics not in the golden set | Golden set going stale | Not mining logs for new cases |
| **Golden set freshness** | % of golden cases added from production in the last 90 days | Evals track reality | A frozen golden set |
| **Feedback-to-eval time** | Days from a production failure to a golden case | Learning speed | Fixes with no regression test |
| **Version traceability** | % of traces tagged with model, prompt, tool and index versions | You can't debug what you can't attribute | Missing on the requests that fail |
| **MTTD / MTTR** | Mean time to detect / resolve AI incidents | Operational maturity | Detection only by customer complaint |

---

## 11. Pick by product type

| Product type | North Star | Key inputs | Must-have AI metrics | Top guardrail |
|---|---|---|---|---|
| **Copilot / writing assistant** | Weekly tasks completed with accepted AI output | Adoption, repeat usage, acceptance rate | Edit distance, regenerate rate, groundedness | Hallucination rate on accepted outputs |
| **RAG Q&A / enterprise search** | Questions answered correctly with no follow-up search | Coverage, recall@k, answer acceptance | Citation accuracy, unanswerable-but-answered | Leakage across permission boundaries |
| **Customer support agent** | Cases resolved with no repeat contact in 7 days | Containment **with** resolution and false containment | Correct handoff, silent failure, top-harm error | Complaint rate, false containment |
| **Coding agent** | Accepted changes merged and not reverted in 30 days | Tasks attempted, acceptance, review time | pass^k on real repo tasks, test pass rate | Revert rate, security findings introduced |
| **Workflow / back-office agent** | Cases fully processed with no human step and no rework | Automation rate, straight-through rate | Verified vs. claimed success, irreversible-action errors | Undo rate, audit exceptions |
| **Voice agent** | Calls resolved in-call with no callback | Calls handled, completion rate | Speech-recognition error rate, interruption handling, TTFT | Transfer-loop rate, compliance misses |
| **Content / image generation** | Generated assets used or published | Generations per user, keep rate | Prompt adherence, regenerate rate | Unsafe content rate, IP or likeness complaints |
| **Deep research agent** | Reports used in a decision (shared, cited, exported) | Reports started → finished | Citation accuracy, source coverage, claim faithfulness | Unsupported-claim rate, cost per report |

## 12. Metrics by phase

| Phase | Decide with | Gate example |
|---|---|---|
| Discovery | Baseline task success on 50–100 real cases; user error tolerance | Model reaches ≥ X% on real cases with a plausible path to the bar |
| Define | Launch bar on task success, top-harm error, groundedness, latency, cost per task | Thresholds written down before build |
| Build | Offline eval suite, regression rate, pass^k | Golden set passes at the bar, with CI |
| Trust review | Injection, jailbreak, leakage, fairness gap, over-refusal | Red-team results within limits; sign-off |
| Beta / pilot | Acceptance, override, correct handoff, cost per successful task | Non-inferior to baseline on quality, better on time or cost |
| Launch (A/B) | North Star input lift vs. holdout + all guardrails | Lift significant, no guardrail breached |
| Operate | Production quality score, drift, MTTD, cost trend | Weekly review; autonomy moves up only with evidence |

## 13. Measuring changes when outputs vary

- **Run each eval case several times** and report pass^k plus variance. One run of a non-deterministic system is an anecdote.
- **Use non-inferiority tests** when the goal is "cheaper or faster, not worse": set the margin you'll accept (e.g. no more than 2 points lower) before the test.
- **Keep a long-term holdout** (5–10% of users with no AI or the old version) to measure true incremental value past the novelty period.
- **Size the test for the guardrail, not just the main metric.** Rare harms need far more traffic to detect than average lifts.
- **Freeze versions during a test.** A provider model update mid-test invalidates it; log the model version on every request.

## 14. What to log (minimum event schema)

Every AI request should emit one trace with:

`trace_id · user/segment · surface · model + version · prompt version · retriever/index version · tools called (name, args hash, status, latency) · tokens in/out/cached · cost · latency (TTFT, total) · output id · guardrail flags · agent-claimed status · verified outcome · user action (accept / edit / regenerate / 👍👎 / undo) · handoff (yes/no, reason)`

If you can't join **claimed status** to **verified outcome**, you can't measure silent failure, and you will ship it.

## 15. Anti-patterns

1. **Activity as North Star:** messages, tokens or chat time. They rise when users are confused.
2. **One accuracy number:** it hides the slice that hurts and the single error that matters most.
3. **pass@k in a pitch deck:** users get one run, not the best of k.
4. **Containment alone:** includes everyone who gave up.
5. **Thresholds set after the results:** grading the exam after seeing the answers.
6. **Cost per request instead of per successful task:** failures look cheap.
7. **An LLM judge nobody calibrated:** every downstream number inherits its bias.
8. **No holdout:** you can't separate the AI effect from adopters being better users.

## 16. Weekly AI metrics review (copy this)

| Layer | Metric | This week | Last week | Target | Status | Owner | Action if red |
|---|---|---|---|---|---|---|---|
| North Star | | | | | 🟢🟡🔴 | | |
| Input | | | | | | | |
| User quality | Acceptance rate | | | | | | |
| Model quality | Task success (prod sample), pass^k | | | | | | |
| Agent | Verified vs. claimed gap, correct handoff | | | | | | |
| Trust | Top-harm error, unsafe output rate | | | | | | |
| Performance | p95 end-to-end latency | | | | | | |
| Cost | Cost per successful task | | | | | | |
| LLMOps | Quality drift, new failure cases added | | | | | | |

**Exit gate for this template:** one North Star, three to five input metrics, a guardrail for each, the AI layers your product type needs (section 11), written formulas, a threshold set before launch, and an owner for every row.

---

Related: [Vision, North Star and metric tree](north-star-metric-tree.md) · [Metrics glossary](metrics-glossary.md) · [Eval plan](eval-plan.md) · [AI experimentation guide](ai-experimentation.md) · [AI unit economics](ai-unit-economics.md) · [Post-launch runbook](post-launch-runbook.md) · See the [worked example](../examples/support-agent-worked-example.md)
