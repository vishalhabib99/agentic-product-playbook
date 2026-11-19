# AI unit economics and pricing template

**Phases: Roadmap and Define,** revisited every quarter in Operate. Use it to answer one question before you build: **does each task make or lose money at the volume we expect?**

The rule behind this template: **measure cost per successful outcome, not cost per call.** A cheap call that fails and sends the user to a human is the most expensive path you have.

---

## 1. Cost per task

Fill in one row per step of the task. Use your provider's current prices. The numbers below are illustrative only.

| Cost line | Formula | Example |
|---|---|---|
| Model input | calls × input tokens × price per input token | 4 × 6,000 × $3 / 1M = $0.072 |
| Model output | calls × output tokens × price per output token | 4 × 500 × $15 / 1M = $0.030 |
| Retrieval, tools, APIs | per-task infra and third-party calls | $0.020 |
| Evals and monitoring | judge calls and logging, spread per task | $0.010 |
| **AI cost per task** | | **$0.132** |
| Human fallback | handoff rate × human cost per handled task | 45% × $5.00 = $2.250 |
| Failure cost | false-success rate × cost to fix a repeat contact | see §2 |

**Cost per successful outcome** = total cost ÷ tasks actually completed correctly.

## 2. Compare against today

| | Today (no AI) | With AI |
|---|---|---|
| Volume per month | | |
| Cost per task | | |
| Tasks completed correctly | | |
| Repeat contacts / rework caused | | |
| **Total monthly cost** | | |
| **Cost per successful outcome** | | |

The [worked example](../examples/support-agent-worked-example.md#unit-economics-the-cost-check-for-the-roadmap-gate) walks through this table with real arithmetic.

## 3. Levers, in the order to try them

1. **Route by difficulty.** Send easy tasks to a smaller, cheaper model and hard ones to the large model.
2. **Cut input tokens.** Retrieve fewer, better chunks. Trim system prompts and history.
3. **Cache.** Use prompt caching for repeated context; cache answers to repeated questions.
4. **Fewer calls per task.** Merge steps; stop agent loops early when the task is done.
5. **Batch** work that doesn't need an instant answer.
6. **Fine-tune a smaller model** for a narrow, high-volume task, once you have labeled data.

Re-run the eval after every lever. A cheaper model that drops quality moves cost to the human fallback line.

## 4. Sensitivity check

What happens to margin if…

| Scenario | Cost per task | Margin | Still viable? |
|---|---|---|---|
| Base case | | | |
| Usage per user doubles | | | |
| Handoff rate is 15 points worse than planned | | | |
| Model price drops 50% | | | |
| We must switch to a pricier model for quality | | | |

## 5. Pricing

How you charge decides who carries the cost risk.

| Model | How it works | Fits when | Risk |
|---|---|---|---|
| **Included in the seat** | AI is part of the existing price | AI is table stakes, or drives retention | Heavy users erode margin; set fair-use limits |
| **Paid add-on per seat** | Extra monthly fee per user | Clear value to a subset of users | Low attach rate if value is unproven |
| **Usage-based** | Per task, per credit, or per 1K calls | Usage varies widely; developer products | Bill shock; users ration use |
| **Outcome-based** | Per resolved ticket, per qualified lead, per completed task | The outcome is measurable and both sides agree on it | You carry quality risk; define "outcome" in the contract |
| **Hybrid** | Platform fee plus included credits plus overage | Most B2B AI products | More complex to explain |

**Pricing checklist**
- [ ] Gross margin stays above our floor (__%) for the 90th-percentile user, not just the average.
- [ ] We know what one heavy user costs us per month.
- [ ] The price metric matches the value metric in the [metric tree](north-star-metric-tree.md).
- [ ] Rate limits or fair-use terms exist for abuse and runaway agents.
- [ ] We re-check margin on every model change.

## Exit gate

**To leave Roadmap:** cost per successful task at target volume is written down, the sensitivity table covers the two biggest cost drivers, and the bet still makes sense in its worst row.

---

Related: [Roadmap prioritization](roadmap-prioritization.md) · [Metrics glossary](metrics-glossary.md) (cost per resolution) · [Post-launch runbook](post-launch-runbook.md)
