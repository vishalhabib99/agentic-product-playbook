# AI strategy memo template

**Phase: Strategy.** Use it to decide where AI earns a place in your product before anyone writes a PRD. One memo, 2–4 pages, read by leadership in a meeting and decided in the same meeting.

The rule behind this template: **start from a customer or business problem, never from "we need an AI strategy."** If a section can't be filled in without hand-waving, that is the finding.

---

## 1. The decision we need

"We are asking for **[people / budget / time]** to pursue **[N] AI bets** in **[product area]** over **[horizon]**."

- **Decision owner:**
- **Date of decision meeting:**

## 2. Context

- **Company goal this serves:** (link the OKR)
- **What changed:** why now? (a model capability, a cost drop, a competitor move, a customer ask, a regulation)
- **What happens if we do nothing for 12 months:**

## 3. Where to play

List the candidate problems. Score each one honestly.

| Candidate problem | Who has it | How often | Cost to them today | Is AI the right tool? (see §4) | Keep? |
|---|---|---|---|---|---|
| | | | | | |

## 4. "Is AI the right tool?" test

For each problem you keep, answer all five. Two or more "no" answers means use rules, search, better UX, or a form instead.

1. **The input is messy.** Natural language, documents, images, or open-ended requests that rules can't cover.
2. **A good-enough answer is valuable.** Users gain from an answer that is right most of the time and checkable, or the cost of an error is low or catchable.
3. **We can measure "right."** We can build a test set and a rubric before launch.
4. **The volume justifies it.** Enough tasks that the time or cost saved beats the build and run cost (see [AI unit economics](ai-unit-economics.md)).
5. **We can contain the worst case.** The top-harm error can be prevented or caught (see [Responsible AI review](responsible-ai-review.md)).

## 5. Right to win

Models are available to everyone. Advantage comes from what surrounds the model.

| Source of advantage | Do we have it? | Evidence |
|---|---|---|
| **Proprietary data** that improves quality and grows with use | | |
| **Distribution**: users already in our product daily | | |
| **Workflow position**: we own the system of record or the moment of decision | | |
| **Feedback loop**: user actions tell us what was right, at scale | | |
| **Trust**: brand, compliance posture, or domain credibility competitors lack | | |

If every row is "no", the AI feature is table stakes, not a moat. That can still be worth building, but say so and fund it as parity.

## 6. Build, buy, or fine-tune

Default to the cheapest option that clears the quality bar. Move down the table only when the row above fails your eval.

| Option | Use when | Typical time to first eval | Watch out for |
|---|---|---|---|
| **Buy a finished product** (vendor tool) | The problem isn't core to your differentiation | Days | Data sharing terms, lock-in, no control over quality |
| **Hosted model + prompting** | Most new use cases. Start here. | Days | Prompt fragility; re-test on every model version |
| **Hosted model + retrieval (RAG) + tools** | Answers depend on your private or changing data | Weeks | Retrieval quality is usually the bottleneck, not the model |
| **Fine-tune a model** | Narrow, high-volume task; consistent format or tone; need a smaller, cheaper model | Weeks to months | Needs labeled data and a retraining plan; freezes knowledge |
| **Train your own model** | Rare. Unique data at large scale, and the model *is* the product | Months to years | Cost, talent, and the risk that hosted models catch up |

**Our choice and why:**

## 7. The bets

Two or three, not ten.

| Bet | Customer outcome | North Star input it moves | Autonomy target (Assist / Copilot / Agent) | First proof point and date | Kill criteria |
|---|---|---|---|---|---|
| | | | | | |

Autonomy levels, from the [autonomy ladder](north-star-metric-tree.md#2-autonomy-ladder): **Manual** (no AI), **Assist** (suggests, user does the work), **Copilot** (drafts, user approves), **Agent** (acts on its own within limits). Most bets should start one level lower than the team wants.

## 8. What we will not do

Name the tempting AI ideas you are saying no to, and why. This section prevents roadmap creep later.

## 9. Risks

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Quality never reaches the bar | | | Feasibility spike in Discovery before committing headcount | |
| Inference cost breaks margins | | | Unit economics model before Roadmap | |
| Regulatory or reputational harm | | | Risk tier assigned in Discovery | |
| A platform vendor ships it for free | | | Lean on the right-to-win rows above | |

## 10. The ask

People, budget, timeline, and the date we come back with evidence.

## Exit gate

**To leave Strategy:** leadership has picked the bet (or said no), named an owner and a team, and agreed the kill criteria and the date the team comes back with evidence.

---

Next: [North Star and metric tree](north-star-metric-tree.md) · [Discovery kit](discovery-kit.md) · See the [worked example](../examples/support-agent-worked-example.md#1-strategy)
