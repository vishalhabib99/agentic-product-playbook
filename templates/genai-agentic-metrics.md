# GenAI and agentic AI product metrics: the reference set

**Phases: Vision → Operate.** This is the catalog an AI PM picks from. You won't track all of it.

The rule behind this template: **a GenAI product can look great on usage and still fail its users.** Answers vary from run to run. Quality is spread across many cases instead of pass/fail. An agent can report "done" when nothing happened. Classic product metrics miss all of that, and this set covers it.

**In five lines**
1. Pick one North Star that counts value delivered, not activity.
2. Pick three to five input metrics and a guardrail for each. Start from your product type (section 1).
3. Gate every model, prompt or tool change on offline evals, with thresholds and sample sizes set before you see results (section 6).
4. For agents, measure *verified* outcomes, not the agent's own "done" (sections 8–9).
5. Review weekly with an owner for every red number (section 21).

How to read the tables: every metric has a **formula** (so two teams compute the same number), **why it matters**, and **the trap** (how it misleads you).

---

## 0. The stack at a glance

| § | Layer | Question it answers | Owner | Cadence |
|---|---|---|---|---|
| 2 | Business value | Is this making or saving money? | PM + Finance | Monthly / quarterly |
| 3 | North Star and adoption | Are users getting value, and coming back? | PM | Weekly |
| 4 | User-perceived quality | Do users accept what the AI produces? | PM + Design | Weekly |
| 5 | Reliance and teaming | Do people trust it the right amount? | PM + Design + Research | Monthly |
| 6 | Model quality (offline evals) | Is the model good enough before users see it? | Data/ML + PM | Every change |
| 7 | Retrieval and context | Is it finding and using the right information? | Data/ML | Every index or retriever change |
| 8 | Task outcomes | Did the job actually get done? | PM | Weekly |
| 9 | Agent behavior | Does it finish tasks in a sensible number of steps? | Eng + PM | Every change + weekly |
| 10 | Agent permissions and security | Can it only do what it should? | Security + Eng | Every release + weekly |
| 11 | Multi-agent systems | Is the team of agents worth its overhead? | Eng | Every change |
| 12 | Memory and personalization | Does it remember the right things, for the right user? | Eng + Legal·Trust | Weekly |
| 13 | Trust and safety | Can it hurt people, and does it fail safely? | Legal·Trust + PM | Every release + weekly |
| 14 | Performance | Is it fast and up? | Eng | Real time |
| 15 | Voice and multimodal | Does it hear, speak and see well enough? | Eng + Data/ML | Every change |
| 16 | Cost and unit economics | Does each task make money? | PM + Finance | Weekly |
| 17 | LLMOps health | Is quality holding up after launch? | Eng + Data/ML | Weekly |

**Offline vs. online:** most layers have both. The offline part (golden set, red-team suite, labeled samples) gates a change *before* users see it. The online part (production traffic) shows up within days of a release. Business value and the North Star lag by weeks. Gate releases on offline results, make ramp decisions on the fast online numbers, and use the lagging ones to check that the fast ones were the right choice. Section 17 checks whether offline gains actually predict online ones.

---

## 1. Pick by product type

Every metric named here is defined in the sections that follow.

| Product type | North Star | Key inputs | Must-have AI metrics | Top guardrail |
|---|---|---|---|---|
| **Copilot / writing assistant** | Weekly tasks completed with accepted AI output | Adoption, repeat usage, acceptance rate | Edit distance, over-reliance, faithfulness | Hallucination rate on accepted outputs |
| **RAG Q&A / enterprise search** | Questions answered correctly with no follow-up search | Hit rate@k, acceptance rate, answerable-but-refused | Citation accuracy, unanswerable-but-answered | Permission-boundary leakage |
| **Customer support agent** | Cases resolved with no repeat contact in 7 days | Containment **with** resolution and false containment | Correct handoff, claimed vs. verified gap, top-harm error | Complaint rate, false containment |
| **Coding agent** | Accepted changes merged and not reverted in 30 days | Acceptance rate, review time per change | pass^k on real repo tasks, test pass rate | Revert rate, security findings introduced |
| **Workflow / back-office agent** | Cases fully processed with no human step and no rework | Automation rate, straight-through rate | Claimed vs. verified gap, irreversible-action errors | Undo rate, unauthorized actions |
| **Voice agent** | Calls resolved in-call with no callback | Containment, resolution | Word error rate, voice-to-voice latency, barge-in success | Transfer-loop rate, compliance misses |
| **Content / image generation** | Generated assets used or published | Keep rate, regenerate rate | Generation prompt adherence | Unsafe output rate, content marking, complaint rate |
| **Deep research agent** | Reports used in a decision (shared, cited, exported) | End-to-end task completion | Citation accuracy, factuality, source coverage | Unsupported-claim rate, cost per successful task |

---

## 2. Business value

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Incremental revenue** | Revenue per user (AI cohort) − revenue per user (holdout) | Only a holdout shows what AI *caused* | Comparing adopters to non-adopters: adopters were already your best users |
| **Cost-to-serve reduction** | (Baseline cost per case − AI cost per case) × cases | Main ROI case for support, ops and back office | Leaving out human review, retries and escalation cost |
| **Time saved per task** | Median task time without AI − with AI, **measured** (logs or a timed study), not self-reported | Productivity case for copilots | Self-reports can point the wrong way. In [METR's early-2025 study](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/), experienced developers believed AI made them 20% faster; measured, they took 19% longer |
| **Conversion / retention lift** | Δ conversion or D30 retention, treatment vs. holdout | Ties AI to the metrics leadership funds | Novelty effect: exclude at least week 1, and confirm the lift still holds in weeks 4+ (or in the long-term holdout) before calling it |
| **Expansion lift (B2B)** | Expansion ARR ÷ starting ARR, AI-using accounts minus matched non-AI accounts (matched on size, segment and prior growth) | Does AI drive upsell or add-on attach? | Unmatched comparisons credit AI for accounts that were growing anyway. For full NRR, also subtract churn and contraction |
| **Payback period** | One-time build cost ÷ (monthly benefit − monthly run cost) | Go / no-go on the investment | Leaving eval, red-team and on-call cost out of run cost |

AI feature gross margin is in section 16.

## 3. North Star and adoption

**Good GenAI North Stars count value delivered:** *weekly tasks completed with AI that users kept*, *cases resolved by the agent with no repeat contact*, *accepted code merged and not reverted*. **Bad ones count activity:** prompts, messages, tokens, chat minutes. Those go up when the product is confusing.

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **AI adoption** | Users who used the AI feature ≥ 1× in period ÷ eligible users | Breadth | "Eligible" quietly shrinks over time |
| **Activation (first success)** | New AI users with ≥ 1 accepted output in their first session ÷ new AI users | First success predicts retention | Counting the first *try*, not the first *success* |
| **Repeat usage** | Users active with AI in **both** week N and week N+1 ÷ users active in week N | Shows whether value brings people back | Forced or default-on usage inflates it |
| **AI retention curve** | % of a cohort still using AI at D7 / D30 / D90 | Does it flatten (value) or decay to zero (novelty)? | Reading one blended number instead of the curve |
| **Stickiness** | AI DAU ÷ AI MAU | Habit strength | Fine for daily tools; wrong for monthly jobs (taxes, planning) |
| **Share of work done with AI** | Tasks done with AI ÷ all eligible tasks | Penetration within a workflow | Rises when the non-AI path gets worse, not when AI gets better |
| **Abandonment rate** | Sessions where the user leaves without an accepted output ÷ AI sessions | Silent dissatisfaction | Confused with "got the answer fast and left": pair it with a task outcome |

## 4. User-perceived quality

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Acceptance rate** | Outputs accepted (inserted, sent, applied, merged) ÷ outputs shown | Best everyday quality signal | Users accept without checking. See over-reliance in section 5 |
| **Edit distance after accept** | Avg. % of characters changed after accepting | "Accepted but rewritten" is a partial failure | Some edits are personalization, not errors |
| **Regenerate / retry rate** | Regenerate clicks ÷ outputs | The first answer missed | Exploratory users regenerate for variety |
| **Override rate** | Human reversals of AI decisions ÷ AI decisions | Copilot and agent trust | Low override can mean rubber-stamping |
| **Thumbs-down rate** | Thumbs-down ÷ outputs shown (also report the vote rate) | Cheap, fast signal | Few users vote, and they skew delighted or furious |
| **CSAT on AI interactions** | Mean rating on a 1–5 scale, AI-touched sessions only (same scale and question as human-handled sessions) | Comparable to the rest of the business | Timing: asked right after an answer, before the user knows if it was right |

## 5. Reliance and teaming

The goal isn't maximum trust. It's *calibrated* trust: people accept the AI when it's right and catch it when it's wrong.

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Over-reliance rate** | Wrong outputs accepted ÷ wrong outputs shown (from a labeled sample) | Acceptance hides errors users didn't catch | Needs labeled samples; clicks alone can't show it |
| **Under-reliance rate** | Correct outputs rejected or redone by hand ÷ correct outputs shown | You pay for AI nobody trusts | Read as "the model is bad" when it's a UX or trust-cue problem |
| **Verification time per output** | Median time users spend checking an output before accepting | The hidden cost that eats time saved | Falling verification time with flat accuracy means rubber-stamping |
| **Human + AI vs. human alone** | Task accuracy (or speed) of a human with AI − without AI, on the same tasks | The real teaming question | Human + AI can be worse than either alone |
| **Sycophancy rate** | Cases where the model drops a correct answer, or agrees with a false premise, after user pushback ÷ pushback cases | Agreeable models tell users what they want to hear | Only tested with polite pushback |

## 6. Model quality (offline evals)

Run these on a **golden set** (real, labeled cases, including the hard ones) before any model, prompt, tool or retriever change ships. Thresholds are set *before* you see results. Statistics for reading them are in section 19.

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Task success rate** | Golden cases passed ÷ golden cases | The launch bar | A golden set of easy, demo-style cases. When the current model scores near 100%, the set is saturated: add harder cases |
| **Accuracy vs. ground truth** | Exact or rubric match ÷ cases | Extraction, classification, Q&A | One number hides which slices fail: report by segment |
| **Pairwise win rate** | (Wins + ½ ties) ÷ comparisons, new version vs. baseline, judged side by side with order swapped, plus a human-judged sample | The standard way to compare two versions when there's no single right answer | Judge position, length and self-preference bias ([AI experimentation guide](ai-experimentation.md#3-llm-as-judge-done-safely)) |
| **Faithfulness (to context)** | Claims supported by the retrieved or given sources ÷ total claims | Main defense against made-up content in RAG and agents | Faithful to the wrong or outdated document still passes |
| **Factuality (to the world)** | Claims that are true, checked against a trusted reference ÷ checkable claims | Catches errors faithfulness can't: wrong sources, closed-book answers | Expensive to label; sample it |
| **Hallucination rate** | Outputs with ≥ 1 claim that fails faithfulness *or* factuality ÷ outputs | The headline number users and legal care about | State which of the two checks it includes |
| **Instruction / format adherence** | Outputs that pass schema or format checks ÷ outputs | Downstream code breaks on bad JSON | Valid format, wrong content |
| **Top-harm error rate** | Rate of the single worst error (named in the PRD) | Kept separate, never averaged in | Folding it into overall accuracy; too few cases to bound it (section 19) |
| **Calibration (ECE)** | Σ over confidence bins of (cases in bin ÷ all cases) × \|accuracy in bin − mean confidence in bin\| | Confidence cues and abstention only work if confidence means something | A verbal "I'm sure" isn't a probability |
| **Selective accuracy (abstention curve)** | Accuracy on answered cases at each coverage level (% of cases answered) | Shows the trade between answering more and being right | Picking one point on the curve without saying what was given up |
| **Consistency (pass^k)** | Share of tasks that pass on **all** k runs. With n ≥ k runs per task and c passes, estimate it per task as C(c,k) ÷ C(n,k), then average | Users get one run, not the best of k | Report pass@k (passes on *any* of k runs) as capability, but **gate launches on pass^k** |
| **Regression rate** | Cases that passed on the old version and fail on the new ÷ cases | A higher average can still break things that worked | Only comparing averages |
| **Judge agreement** | LLM judge vs. human labels: Cohen's κ for categorical labels; weighted κ or Spearman for ordinal scores. Compare to human-vs-human agreement, which is the ceiling | An uncalibrated judge makes every other number meaningless | Never re-checking the judge after a model or rubric change |
| **Golden label quality** | Agreement between two human labelers on a sample of golden cases (κ) | If humans disagree on the answer, no model score above that agreement means anything | Single-labeler golden sets nobody audits |

## 7. Retrieval and context

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Hit rate@k** | Queries with ≥ 1 relevant document in the top k ÷ queries | If retrieval misses everything, the model can't recover | Labels only cover one "right" document |
| **Recall@k** | Relevant documents in the top k ÷ all relevant documents for the query, averaged over queries | Multi-part answers need *all* the pieces | Needs complete relevance labels; usually built from a sample |
| **Precision@k / context relevance** | Relevant chunks in top k ÷ k | Noise in context lowers answer quality and raises cost | Optimizing precision and losing recall |
| **MRR** | Mean of 1 ÷ (rank of the first relevant result) | Is the best result near the top? | Ignores everything after the first hit |
| **NDCG@k** | DCG@k ÷ ideal DCG@k, where DCG = Σ (graded relevance ÷ log₂(rank + 1)) | Graded ranking quality: what the reranker and the top-k cutoff keep | Graded labels are expensive, so teams fall back to binary and lose the point |
| **Context position sensitivity** | Answer accuracy with the key fact placed at the start vs. middle vs. end of a long context | Models often use the middle of long contexts less well, so good retrieval can still produce a wrong answer | Only testing short contexts |
| **Context overflow / truncation rate** | Requests where history, documents or tool results were cut to fit the context window ÷ requests | Truncation silently drops the instruction or fact that mattered | Not logged, so it never shows up |
| **Citation accuracy** | Citations that support the cited sentence ÷ citations | Trust and auditability | Counting whether a citation exists, not whether it's correct |
| **Answerable-but-refused rate** | "I don't know" on answerable questions ÷ answerable questions | Over-caution is a failure too | Celebrating a low hallucination rate built on refusals |
| **Unanswerable-but-answered rate** | Confident answers to out-of-scope questions ÷ out-of-scope questions | The hallucination you actually ship | No out-of-scope cases in the golden set |
| **Index lag** | Median time between a source document changing and the index reflecting it | Stale policy text gives correct-sounding wrong answers | Nobody owns re-indexing |

## 8. Task outcomes

What the user or business actually got. These mirror the [metrics glossary](metrics-glossary.md); report containment, resolution and false containment together.

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Containment rate** | Conversations that ended without a human handoff ÷ conversations | Cost lever for support and voice | Counts giving up as a win. Never report it alone |
| **Resolution rate** | Conversations with no repeat contact on the same issue within 7 days ÷ conversations | Did the problem actually get solved? | Needs a way to match the repeat contact to the same issue |
| **False containment** | Contained conversations followed by a repeat contact, call or cancellation within 7 days ÷ contained conversations | Tells you whether containment is real | Short follow-up windows hide it |
| **Automation rate** | Eligible tasks completed end to end with no human step ÷ eligible tasks | Workflow agent progress | Define "eligible" up front, or the denominator shrinks |
| **Straight-through rate** | Cases processed with no human step **and** no later rework or correction ÷ cases | Automation that sticks | Rework found weeks later, outside the window |
| **Transfer-loop rate** | Calls or chats transferred more than once, or back to the agent after a transfer ÷ transferred contacts | Users bounced between bot and people | Only counting the first transfer |
| **Compliance misses** | Contacts where a required statement or step (disclosure, verification, consent) was skipped ÷ contacts | Regulated workflows | Checked by keyword, not by meaning |
| **Test pass rate (coding)** | Generated changes whose existing test suite passes ÷ changes | Minimum bar for code | Agents that edit or delete the tests |
| **Review time per change (coding)** | Median human review minutes per AI-generated change | The cost the agent pushes onto reviewers | Falling review time with rising reverts means rubber-stamping |
| **Revert rate (coding)** | AI changes reverted or hot-fixed within 30 days ÷ AI changes merged | Production truth for code | Fixes made in a later change instead of a revert |
| **Security findings introduced** | New static-analysis or security-scan findings per merged AI change | Code that works but adds risk | Scanner not run on AI branches |
| **Keep rate (generation)** | Generated assets saved, used or published ÷ generated assets | Did the output earn a place? | Bulk-generation workflows keep everything |
| **Source coverage (research)** | Key sources found by the agent ÷ key sources in an expert reference list | Research that misses the main sources misleads | Reference list written after seeing the agent's sources |
| **Unsupported-claim rate (research)** | Claims not supported by their cited source ÷ claims (the complement of faithfulness, on cited claims) | The research-agent version of hallucination | Only checking that a citation exists |

## 9. Agent behavior

What makes an agent different: it **takes actions** across many steps. Measure the whole task, each step, and what it did to the world.

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **End-to-end task completion** | Tasks reaching the verified end state ÷ tasks started | The agent's North Star input | Trusting the agent's own "done" |
| **Claimed vs. verified gap** | Claimed successes that were **not** verified ÷ claimed successes | Catches agents that say "done" when nothing happened | Only measurable if you check real system state |
| ↳ **Silent failure rate** (subtype) | "Done" tasks where a tool returned empty, a refusal, or wrong content ÷ "done" tasks | The most common cause of the gap, and the most damaging | Invisible unless you look for it on purpose |
| **Tool-call success rate** | Tool calls that returned valid results ÷ tool calls | Reliability of the action layer | Valid result, wrong tool |
| **Wrong-tool / bad-argument rate** | Calls to the wrong tool or with invalid arguments ÷ tool calls (track retries separately) | Usually a tool-description problem | Blaming the model and swapping it |
| **Steps per task** | Median and p95 tool calls or turns per completed task | Efficiency, cost and latency | A lower median can hide a long tail |
| **Trajectory efficiency** | Optimal steps ÷ actual steps (on golden tasks) | Wandering burns money and time | Many valid paths exist: score the outcome first |
| **Loop / runaway rate** | Tasks hitting the step, time or budget cap ÷ tasks | Cost blow-ups and stuck users | No cap at all |
| **Recovery rate** | Tasks that succeeded after ≥ 1 failed step ÷ tasks with ≥ 1 failed step | Robustness | Retries that hide a broken tool |
| **Human intervention rate** | Tasks needing a human step ÷ tasks | Real level of autonomy | Lower isn't always better: see correct handoff |
| **Approval rate on gated actions** | Human-approved actions ÷ actions sent for approval (also log review time) | Is the approval step still needed, or is it rubber-stamping? | 99% approval with 2-second reviews means no review |
| **Correct handoff rate** | Cases that should go to a human and did ÷ cases that should go to a human | Overconfidence check | Pair it with unnecessary-handoff rate |
| **Unnecessary handoff rate** | Handoffs a human closed with no action the agent couldn't have taken ÷ handoffs | Over-caution costs money | Reported without the correct-handoff rate |
| **Irreversible action error rate** | Wrong irreversible actions (payments, deletes, sends) ÷ irreversible actions | The errors you can't take back | Averaged in with harmless actions |
| **Undo / rollback rate** | Agent actions reversed by users or ops ÷ actions | Production truth on action quality | No undo path, so it reads as zero |
| **Autonomy mix** | % of tasks at each level: Manual / Assist / Copilot / Agent | Tracks progress up the [autonomy ladder](north-star-metric-tree.md#2-autonomy-ladder) | Moving up a rung without evidence |

## 10. Agent permissions and security

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Unauthorized / out-of-scope action rate** | Actions outside the PRD's allowed list ÷ actions | Did the authority boundary hold? | Only checked in red team, not in production |
| **Least-privilege coverage** | Tools whose granted scopes match what the task needs (no broader) ÷ tools | Limits the damage any failure or attack can do | Broad tokens granted "for now" during the pilot |
| **Destructive actions blocked** | Destructive or high-risk calls stopped by a policy or approval gate ÷ destructive calls attempted | Shows the gate works in practice | Zero attempts in testing means the gate was never exercised |
| **Prompt injection success rate** | Successful attacks ÷ attempts, on a red-team suite that includes **indirect** injection through documents, web pages and tool results | The main agent attack path | Only testing direct user attacks |
| **Tool-description poisoning** | Third-party tools or MCP servers whose descriptions contain hidden instructions or mislabeled side effects ÷ tools reviewed | The agent treats tool docs as instructions | Reviewing a tool once at install, never on update |
| **Permission-boundary leakage** | Answers containing content from documents or records the user isn't allowed to see ÷ sampled answers (target: 0) | RAG and agents can bypass access controls | Indexing everything under one service account |
| **Credential exposure** | Traces, outputs or logs containing secrets or tokens ÷ sampled traces | One leaked key undoes every other control | Not scanning traces and tool arguments |

## 11. Multi-agent systems

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Token multiplier** | Tokens per task (multi-agent) ÷ tokens per task (best single agent) | Multi-agent setups can use several times the tokens; the quality gain has to pay for it | Comparing against a weak single-agent baseline |
| **Quality gain vs. single agent** | Task success (multi-agent) − task success (single agent), same golden set | The reason to pay the multiplier | No single-agent baseline at all |
| **Handoff failure rate** | Handoffs between agents that lost context or failed ÷ handoffs | Multi-agent systems fail at the seams | Each agent looks fine on its own |
| **Duplicated work rate** | Subtasks done by more than one agent ÷ subtasks | Wasted cost and conflicting answers | Hidden inside the token total |
| **Orchestrator overhead** | Orchestrator tokens and latency ÷ total tokens and latency | Planning cost that does no task work | Grows quietly as you add agents |

## 12. Memory and personalization

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Wrong or stale memory rate** | Responses that used an incorrect or outdated remembered fact ÷ responses that used memory (labeled sample) | Bad memory is worse than none | Only checking that memory was used, not that it was right |
| **Memory helpfulness** | Task success with memory − without, on the same users' tasks | Proves memory earns its risk | Measured only on users who chose to turn it on |
| **Cross-user memory leakage** | Responses containing another user's or tenant's remembered data ÷ sampled responses (target: 0) | A severe privacy incident | Testing only single-user sessions |
| **Deletion compliance** | Deletion requests fully applied (memory, indexes, caches, logs) within the promised time ÷ deletion requests | Users' right to be forgotten, and their trust | Deleting the memory store but not derived indexes or caches |

## 13. Trust, safety and responsible AI

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Unsafe output rate** | Outputs flagged by policy classifiers or reviewers ÷ outputs | Core safety bar | Classifier blind spots: sample by hand too |
| **Jailbreak success rate** | Policy bypasses ÷ attempts | Abuse resistance | A stale attack set |
| **Sensitive-data leakage rate** | Outputs or logs exposing PII ÷ sampled outputs | Legal and trust exposure | Not checking logs and traces |
| **Over-refusal rate** | Refusals of benign, in-policy requests ÷ benign requests | Over-blocking hurts users too | Tuning safety with no cost to usefulness |
| **Fairness gap** | Max − min of task success (or error rate) across user segments | Uneven quality is harm | Segments too small to measure: plan sample sizes |
| **Complaint rate** | AI-related complaints ÷ AI sessions | What users experience as harm | Complaints routed to the wrong team |
| **AI disclosure and content marking** | AI interactions that show a disclosure ÷ AI interactions; generated media carrying machine-readable marking (metadata or watermark) ÷ generated media | Baseline transparency duties, e.g. [EU AI Act Article 50](https://artificialintelligenceact.eu/article/50/), which applies from 2 August 2026 (check current guidance for transition periods) | A UI notice alone: Article 50 also covers machine-readable marking of synthetic content |
| **Time to contain an incident** | Median time from detection to kill switch or rollback taking effect | Blast radius | Kill switch never tested |

## 14. Performance

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Time to first token (TTFT)** | Request sent → first streamed token, p50 / p95 | Feels fast or slow | Fast first token, slow full answer |
| **Output speed** | Output tokens per second after the first token; its inverse is time per output token (TPOT) | Reading speed for long answers; generation time for agents | Averages hide slow periods under load |
| **End-to-end latency** | Request → complete answer, p50 / p95 / p99 | Real wait time | Reporting only p50 |
| **Time to task completion** | Start → verified end state (agents) | What the user actually waits for | Excluding human-approval wait time |
| **Availability** | Successful requests ÷ requests | Model providers have outages | Counting provider errors as "not our fault" |
| **Fallback rate** | Requests served by a fallback model or path ÷ requests | Shows hidden quality drops | No eval of the fallback model |
| **Timeout / rate-limit error rate** | Timeouts or 429 responses ÷ requests | Scale ceiling | Retries that hide it |

## 15. Voice and multimodal

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Word error rate (WER)** | (Substitutions + deletions + insertions) ÷ reference words | Speech recognition sets the ceiling for everything after it | Measured on clean audio; real calls have noise and accents. Report by segment |
| **Voice-to-voice latency** | End of user speech → start of agent audio, p50 / p95 | The pause users notice | Measuring model latency only, not the full audio path |
| **Barge-in success** | User interruptions the agent stopped for and handled correctly ÷ interruptions | Natural conversation | Agent talks over users, or stops at background noise |
| **Speech quality (MOS)** | Mean opinion score from rated samples of the agent's voice (1–5 scale) | Unnatural audio cuts trust and completion | Rated once at launch, never after a voice change |
| **Image / document understanding accuracy** | Correct extractions or answers on a labeled set of real images or scans ÷ cases | Multimodal inputs fail differently from text | Golden set of clean scans only |
| **Generation prompt adherence** | Generated outputs that meet every requirement in the prompt (rated) ÷ outputs | Core quality for image, video and audio generation | Rating looks, not requirements |

## 16. Cost and unit economics

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Cost per successful task (blended)** | (Model + tool + infra + human review and handoff cost) ÷ **successful** tasks | The unit that matters | Dividing by all requests makes failures look cheap. An agent-only figure is fine as a component, if labeled |
| **Quality per dollar** | Task success at each candidate model or configuration plotted against cost per task; keep the options on the frontier | Picks the cheapest option that clears the bar | Choosing on cost or quality alone |
| **Tokens per task** | Input + output tokens per completed task, p50 / p95 | Main driver of model cost | Context bloat creeping up with each prompt edit |
| **Prompt cache hit rate** | Input tokens read from the provider's prompt cache ÷ input tokens | Big cost and latency lever | Prompt changes that silently break caching |
| **Model mix** | % of traffic by model or tier | Routing easy work to cheaper models | Routing without a quality check per tier |
| **Cost per active user** | Total AI cost ÷ AI-active users | Pricing and plan design | Power users at many times the median |
| **Human review cost per task** | Review minutes × loaded hourly rate ÷ 60 ÷ tasks | Often bigger than model cost | Left out of ROI |
| **AI feature gross margin** | (AI revenue − model, tool, infra and review cost) ÷ AI revenue | Software margins aren't a given with AI | Ignoring heavy users who cost more than they pay. For internal tools with no price, use cost-to-serve reduction (section 2) |

## 17. LLMOps health (after launch)

| Metric | Formula | Why it matters | Trap |
|---|---|---|---|
| **Production quality score** | Weekly sample of production traffic graded by humans or a calibrated judge: passed ÷ sampled | Offline evals don't see new user behavior | Sampling too little to detect a change |
| **Offline–online agreement** | Across shipped changes, share where the offline eval change and the online metric change had the same direction (or their correlation, once you have enough launches) | Tells you whether your golden set predicts what users experience | Never checked, so a stale golden set keeps approving changes that don't help |
| **Quality drift** | Week-over-week change in production quality score, with an alert threshold | Model, prompt or data changes degrade quietly | No alert threshold |
| **Input drift** | Share of traffic in intents or topics not covered by the golden set | Golden set going stale | Not mining logs for new cases |
| **Golden set freshness** | % of golden cases added from production in the last 90 days | Evals track reality | A frozen golden set |
| **Feedback-to-eval time** | Median days from a production failure to a golden case covering it | Learning speed | Fixes with no regression test |
| **Version traceability** | Traces tagged with model, prompt, tool and index versions ÷ traces | You can't debug what you can't attribute | Missing on the requests that fail |
| **MTTD / MTTR** | Mean of (detected − started) and (resolved − detected) across AI incidents | Operational maturity | Detection only by customer complaint |

---

## 18. Metrics by phase

- **Before build (Discovery, Define):** baseline task success on 50–100 real cases, then a written launch bar on task success, top-harm error, faithfulness, latency and cost per task, with sample sizes.
- **Build and trust review:** offline evals, pairwise win rate, regression rate and pass^k against the bar; red-team results for injection, jailbreak, leakage and permissions.
- **Pilot and launch:** acceptance, over-reliance, correct handoff and cost per successful task in the pilot; then North Star input lift vs. holdout, with every guardrail holding.
- **Operate:** production quality score, offline–online agreement, drift and cost trend in the weekly review. Autonomy moves up only with evidence.

## 19. Measuring changes rigorously

The basics of non-determinism (run each case several times, pin versions, swap judge order) and A/B test design are in the [AI experimentation guide](ai-experimentation.md). These are the statistics on top.

- **Intervals on every rate.** Report n and a 95% interval. For small n or rates near 0% or 100%, use a Wilson interval or a bootstrap, not ± 1.96 × standard error. A 1-point difference on 200 cases is noise.
- **Zero-failure gates need enough cases.** With 0 failures in n cases, the approximate 95% upper bound on the true rate is 3 ÷ n (rule of three, valid for n ≳ 30). To claim a rate below p, you need at least 3 ÷ p cases: 300 to show under 1%, 3,000 to show under 0.1%.
- **Paired comparisons.** Old and new run on the same cases. With one run per case, use McNemar's test on pass/fail. With several runs per case, use a paired bootstrap that resamples whole cases, which also handles the fact that repeat runs of one case aren't independent.
- **Non-inferiority** when the goal is "cheaper or faster, not worse": set the margin you'll accept (e.g. no more than 2 points lower) before the test.
- **Multiple comparisons.** Testing many metrics or slices at once? Correct for it, or pre-register one primary metric.
- **No peeking.** Decide the duration up front, or use a sequential test built for early looks.
- **Variance reduction.** CUPED (adjusting for each user's pre-test behavior) detects smaller effects with the same traffic.
- **Eval contamination.** If golden cases leak into prompts, few-shot examples or fine-tuning data, scores go up and mean nothing. Keep a held-out slice nobody tunes on.

## 20. What to log (minimum event schema)

Every AI request should emit one trace with:

`trace_id · user/segment · surface · model + version · prompt version · retriever/index version · context tokens used vs. limit (truncated?) · tools called (name, args hash, scopes, status, latency) · tokens in/out/cached · cost · latency (TTFT, total) · output id · guardrail flags · agent-claimed status · verified outcome · user action (accept / edit / regenerate / thumbs / undo) · handoff (yes/no, reason) · memory read/write ids`

If you can't join **claimed status** to **verified outcome**, you can't measure silent failure.

## 21. Weekly AI metrics review (copy this)

| Layer | Metric | This week | Last week | Target | Status (G / Y / R) | Owner | Action if red |
|---|---|---|---|---|---|---|---|
| North Star | | | | | | | |
| Input | | | | | | | |
| User quality | Acceptance rate | | | | | | |
| Reliance | Over-reliance (sampled) | | | | | | |
| Model quality | Production quality score, pass^k | | | | | | |
| Task outcomes | Resolution, false containment (or your product's equivalent) | | | | | | |
| Agent | Claimed vs. verified gap, correct handoff | | | | | | |
| Security | Unauthorized actions, injection findings | | | | | | |
| Trust | Top-harm error, unsafe output rate | | | | | | |
| Performance | p95 end-to-end latency | | | | | | |
| Cost | Cost per successful task | | | | | | |
| LLMOps | Quality drift, offline–online agreement | | | | | | |

**Exit gate for this template:** one North Star, three to five input metrics, a guardrail for each, the layers your product type needs (section 1), a written formula for every metric, thresholds and sample sizes set before launch, and an owner for every row.

---

Related: [Vision, North Star and metric tree](north-star-metric-tree.md) · [Metrics glossary](metrics-glossary.md) · [Eval plan](eval-plan.md) · [AI experimentation guide](ai-experimentation.md) · [AI unit economics](ai-unit-economics.md) · [Responsible AI review](responsible-ai-review.md) · [Post-launch runbook](post-launch-runbook.md) · See the [worked example](../examples/support-agent-worked-example.md)
