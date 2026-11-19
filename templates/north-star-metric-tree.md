# Vision, North Star and metric tree template

**Phase: Vision.** Use it to agree on where the product is going, how far its autonomy should go, and the one number that tells you it's working.

The rule behind this template: **pick metrics that measure value delivered to the user, then add guardrails that catch you winning the wrong way.** An AI product can grow usage while making users worse off. The tree has to catch that.

---

## 1. Vision statement

One paragraph, written from the user's side, 3 years out. No model names, no features.

> In three years, **[user]** can **[outcome]** without **[today's pain]**, because the product **[what it does on their behalf]**.

## 2. Autonomy ladder

Where each workflow sits today, and where it should end up. Moving up a rung needs evidence, not ambition.

| Workflow | Today | Next rung | Target | Evidence needed to move up |
|---|---|---|---|---|
| | Manual / Assist / Copilot / Agent | | | e.g. acceptance rate ≥ 90% for 8 weeks, top-harm error 0 |

- **Manual:** no AI.
- **Assist:** AI suggests; the user does the work.
- **Copilot:** AI drafts or prepares; the user reviews and approves.
- **Agent:** AI acts on its own within set limits and reports back; a human handles exceptions.

## 3. North Star Metric

**Our North Star:**

Check it against all five tests. If it fails one, pick another.

- [ ] **Measures value the user got,** not activity. ("Tasks completed" beats "messages sent".)
- [ ] **Leads revenue.** When it goes up, retention or revenue follows within a quarter or two.
- [ ] **The team can move it** with what they ship.
- [ ] **Hard to game.** Inflating it without helping users would be obvious.
- [ ] **Understandable** by anyone in the company in one sentence.

**Bad North Stars for AI products:** tokens generated, prompts sent, chat sessions, "AI interactions", time spent in chat. Each one goes up when the product is confusing.

## 4. The metric tree

```
                      North Star Metric
                             │
     ┌───────────────┬───────┴───────┬────────────────┐
  Input metric    Input metric    Input metric    Input metric
  (breadth)       (depth)         (quality)       (efficiency)
     │               │               │                │
  Team KPIs       Team KPIs       AI quality      Cost / latency
                                   metrics

  Guardrails across all of it: harm, trust, cost, fairness
```

Fill it in:

| Layer | Metric | Definition | Current | Target | Owner |
|---|---|---|---|---|---|
| North Star | | | | | |
| Input: breadth | e.g. % of eligible users who use the AI feature weekly | | | | |
| Input: depth | e.g. AI-completed tasks per active user | | | | |
| Input: quality | e.g. % of AI outputs accepted without major edits | | | | |
| Input: efficiency | e.g. median time from request to finished task | | | | |
| Business KPI | e.g. retention, conversion, NRR, cost to serve | | | | |
| AI quality | e.g. eval pass rate, groundedness, task success | | | | |
| Guardrail | e.g. top-harm error rate, complaint rate, cost per task | | | | |

## 5. Guardrail metrics

Every input metric gets a counter-metric that would expose gaming or harm.

| If we push… | It could hurt… | Guardrail and limit |
|---|---|---|
| Automation rate | Users who needed a human | Correct-handoff rate ≥ 95% |
| Acceptance rate | Accuracy (users accept without checking) | Spot-check accuracy on accepted outputs |
| Usage | Cost | Cost per task ≤ $__ |
| Speed | Quality | Eval pass rate doesn't drop |
| Engagement | Trust | Complaint and opt-out rates |

## 6. Examples by product type

| | Consumer app | B2B SaaS | Marketplace | Developer platform |
|---|---|---|---|---|
| **North Star** | Weekly tasks completed with AI help | Weekly accounts where AI completes a workflow | Successful matches per week | Weekly active builders shipping with the API |
| **Breadth** | % of WAU using AI | % of seats active on AI | % of searches using AI | New keys making a first call |
| **Depth** | AI tasks per user | Workflows automated per account | Matches per buyer | Calls per active key |
| **Quality** | Acceptance rate, edit distance | Task success, override rate | Relevance (NDCG), match rating | Eval pass rate, tool-call success |
| **Guardrail** | Unsafe output rate, opt-outs | Escalations, data incidents, cost per task | Fraud, bias across segments | Error rate, abuse, cost per 1K calls |

## 7. Review cadence

- **Weekly:** input metrics and guardrails, with a named owner for each red number.
- **Monthly:** North Star trend and the AI quality metrics.
- **Quarterly:** is the North Star still the right one? Did input metrics actually move it?

## Exit gate

**To leave Vision:** one North Star that passes all five tests, three to five input metrics with a guardrail each, and a target rung on the autonomy ladder for each workflow with the evidence needed to move up.

---

Previous: [AI strategy memo](ai-strategy-memo.md) · Next: [Discovery kit](discovery-kit.md) · Definitions: [Metrics glossary](metrics-glossary.md) · Full catalog: [GenAI and agentic metrics](genai-agentic-metrics.md) · See the [worked example](../examples/support-agent-worked-example.md#2-vision)
