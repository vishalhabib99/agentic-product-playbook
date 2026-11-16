# Worked example: a support agent, strategy to sunset

One product taken through every template in this repo, filled in. Blank templates show the format; this shows the judgment calls.

**Ledgerly is a made-up company, and every number here is illustrative.** The arithmetic is real, so you can check it and swap in your own figures.

> **Ledgerly:** accounting and payroll software for small businesses. 9,000 business customers. 40,000 support tickets a month, handled by people at about $6.00 a ticket.

| Phase | Template used |
|---|---|
| [1. Strategy](#1-strategy) | [AI strategy memo](../templates/ai-strategy-memo.md) |
| [2. Vision and North Star](#2-vision-and-north-star) | [Vision, North Star and metric tree](../templates/north-star-metric-tree.md) |
| [3. Discovery](#3-discovery) | [Discovery kit](../templates/discovery-kit.md) |
| [4. Roadmap](#4-roadmap) | [Roadmap prioritization](../templates/roadmap-prioritization.md) |
| [5. Unit economics](#5-unit-economics) | [AI unit economics and pricing](../templates/ai-unit-economics.md) |
| [6. Define and build](#6-define-and-build) | [Agent PRD](../templates/agent-prd.md) · [Eval plan](../templates/eval-plan.md) · [AI experimentation](../templates/ai-experimentation.md) |
| [7. Trust review](#7-trust-review) | [Responsible AI review](../templates/responsible-ai-review.md) |
| [8. Launch](#8-launch) | [Launch readiness checklist](../templates/launch-readiness-checklist.md) · [AI experimentation](../templates/ai-experimentation.md) |
| [9. Operate](#9-operate) | [Post-launch runbook](../templates/post-launch-runbook.md) |

---

## 1. Strategy

**Decision asked for:** one squad (PM, designer, 4 engineers, 1 ML engineer) for two quarters to pursue one AI bet in customer support, with a second bet staged behind it.

**What changed:** support volume grew 30% year over year while headcount stayed flat. Response times went from 4 hours to 9 hours, and "slow support" became the top reason given in churn surveys.

**Is AI the right tool?** For the support problem, yes on all five tests:

1. **Messy input:** customers write in their own words, often with screenshots.
2. **Good-enough is valuable:** most questions have a checkable answer in Ledgerly's own data.
3. **Measurable:** two years of resolved tickets give us real cases and known-good answers.
4. **Volume:** 40,000 tickets a month.
5. **Worst case containable:** the agent never moves money or changes bank details.

**Right to win:** Ledgerly owns the system of record (the customer's actual payroll runs, invoices and bank feeds), so the agent can check facts instead of guessing. A general chatbot vendor can't. That gives Ledgerly a workflow-position and data advantage.

**Build, buy, or fine-tune:** a hosted model with retrieval over help articles and read-only tools into account data. Fine-tuning was rejected for now: help content changes weekly, and there's no labeled data for the format yet.

**Will not do:** AI tax advice, AI-written financial reports, or any agent action that moves money.

## 2. Vision and North Star

**Vision:** In three years, a small-business owner gets any account question answered correctly in under a minute, at any hour, without waiting for a person, and reaches an expert right away when it matters.

**North Star Metric:** *support issues resolved per week with no repeat contact within 7 days* (across people and the agent). It counts value delivered, not conversations handled, so the agent can't win by deflecting customers.

| Layer | Metric | Today | Target (2 quarters) |
|---|---|---|---|
| North Star | Issues resolved, no repeat contact in 7 days, per week | 8,600 | 9,400 |
| Input: breadth | % of tickets the agent handles first | 0% | 60% of eligible intents |
| Input: quality | Agent resolution rate | n/a | ≥ 55% |
| Input: efficiency | Median time to resolution | 9 hours | under 10 minutes for agent-resolved |
| Guardrail | False containment | n/a | ≤ 10% |
| Guardrail | Top-harm error rate | n/a | 0 |
| Guardrail | CSAT (1–5) | 4.4 | no more than 0.2 below human-handled |
| Guardrail | Cost per resolution | $6.00 | ≤ $3.00 blended |

**Autonomy ladder:** look-ups start at **agent** (read-only). Resending an invoice starts at **copilot** (customer confirms). Anything touching bank details or payroll changes stays **manual** (handed to a person).

## 3. Discovery

**Ticket mining (2,000 tickets, last 90 days):** 62% fell into 8 intents. The largest were bank-feed sync errors (14%), "did my payroll run?" (11%), invoice resends (9%), and login and 2FA (8%).

**Interviews (12 owners, 2 bookkeepers):** the key finding for the PRD was error tolerance. A wrong help article was "annoying." Being told payroll ran when it hadn't was "the worst thing that could happen. My people don't get paid."

**Feasibility spike (100 real tickets, graded before the run):**

| Result | Count |
|---|---|
| Correct | 71 |
| Partly correct | 18 |
| Wrong | 9 |
| Harmful | 2 |

Both harmful cases said payroll had run, based on a help article, without checking the account. **Finding:** status questions must be answered from the system of record through a tool, never from help content. The agent must also check the tool's returned content, not just its success code. This is [failure mode #1](../failure-modes.md#1-the-agent-says-done-and-nothing-happened).

**Risk tier: Medium.** The agent handles sensitive financial data, but it can't take irreversible actions.

## 4. Roadmap

| Bet | Reach (customers / qtr) | Impact | Confidence | Effort (person-wks) | RICE | Run cost | Risk tier |
|---|---|---|---|---|---|---|---|
| A. Support agent, top 8 intents | 6,000 | 2 | 80% | 30 | **320** | $0.13 / ticket | Medium |
| B. AI expense categorization | 4,000 | 1 | 50% | 20 | 100 | $0.01 / item | Low |
| C. Cash-flow Q&A chat | 3,000 | 1 | 20% | 40 | 15 | $0.40 / query | High |

**Now:** A. **Next:** B. **Later:** C, after a feasibility spike and a legal read on financial-advice risk.

**Kill criteria for A:** offline eval below the launch bar after 6 weeks of iteration, or blended cost per resolution above $4.00 at 10% traffic.

## 5. Unit economics

**AI cost per ticket:** 4 model calls × (6,000 input tokens at $3 per million + 500 output tokens at $15 per million) = $0.102, plus $0.02 retrieval and tools, plus $0.01 evals and monitoring = **$0.132.**

Plan: the agent resolves 55%, hands off 45% (a person handles a handoff for $5.00, cheaper than $6.00 because the context comes with it), and 8% of agent-resolved tickets come back as repeat contacts at $6.00 each.

| Monthly | Today | With the agent |
|---|---|---|
| AI cost (40,000 × $0.132) | $0 | $5,280 |
| People handling tickets | 40,000 × $6.00 = $240,000 | 18,000 handoffs × $5.00 = $90,000 |
| Repeat contacts (22,000 × 8% × $6.00) | n/a | $10,560 |
| **Total** | **$240,000** | **$105,840** |
| **Saving** | | **$134,160 a month (56%)** |

The agent-only cost per real resolution is $5,280 ÷ 20,240 = **$0.26.**

**Sensitivity:**

| Scenario | Monthly total | Saving |
|---|---|---|
| Base case | $105,840 | $134,160 |
| Resolution 15 points worse (40%) | $132,960 | $107,040 |
| False containment 15% instead of 8% | $115,080 | $124,920 |
| Model prices double | $109,920 | $130,080 |

The model's price barely matters. The resolution rate and the human fallback cost dominate. So the team spends its effort on resolution quality, not on a cheaper model.

**Pricing:** included in every plan. The goal is retention and cost to serve, and the cost per customer is small enough not to need an add-on.

## 6. Define and build

**From the Agent PRD**

| Action | Autonomy level |
|---|---|
| Look up payroll run status, bank-feed status, invoice status | Acts alone (read-only tools) |
| Resend an invoice | Proposes, customer confirms |
| Reconnect a bank feed | Gives guided steps; customer acts |
| Change bank details, cancel payroll, refunds | Hands off to a person |

- **Top-harm error:** telling a customer payroll ran, or a payment went out, when it didn't. Maximum rate: 0 in the eval; 0 tolerated in production (any case is a SEV1).
- **Handoff triggers:** asks for a person; 2 failed attempts; any bank-detail or payroll-change intent; tool returns empty or an error; the customer sounds upset.

**From the eval plan** (thresholds signed off before the first run)

| Slice | Cases | Gate | Blocking |
|---|---|---|---|
| Common intents | 400 | Task success ≥ 85% | Yes |
| Top-harm (payroll and payment status) | 60 | 0 failures, pass^3 | **Yes** |
| Should hand off | 80 | ≥ 95% escalate | **Yes** |
| Adversarial and injection | 40 | 0 successful | **Yes** |
| Silent failure (tool returns success, empty content) | 20 | 0 false "done" | **Yes** |
| **Total** | **600** | | |

**Build loop (illustrative):** the first full run passed every gate except silent failure, where 3 of 20 cases failed: the agent read "200 OK" as done. The fix was a content check on every tool result. After 5 weeks, the run passed all gates twice in a row, which was the exit criterion.

## 7. Trust review

- **Red team:** 9 people, including 3 from outside the squad. 212 attack attempts. 4 worked: 3 indirect prompt injections through text in customer-uploaded PDFs, and 1 system-prompt leak. All fixed, and all 4 added to the eval set permanently.
- **Bias check:** the same 50 requests sent under varied names and regions. Resolution rates were within 2 points of each other.
- **Oversight:** the agent introduces itself as an AI assistant, offers "talk to a person" in every reply, and the support lead can turn it off in under 5 minutes.
- **Sign-off:** product, engineering, security and privacy approved. Residual risk accepted: new injection techniques. Mitigation: monthly red-team refresh.

## 8. Launch

**Ramp:** dogfood with the support team (2 weeks) → shadow on live tickets (1 week) → 1% → 10% → 50% (A/B test) → GA with a 5% holdout.

**A/B test at 50%, weeks 2–4 (illustrative):**

| Metric | Control (people first) | Agent first | Guardrail held? |
|---|---|---|---|
| Resolution, no repeat contact in 7 days | 91% | 90% | Yes |
| Agent resolution rate | n/a | 57% | |
| False containment | n/a | 7% | Yes (≤ 10%) |
| Median time to resolution | 9 hours | 6 minutes | |
| CSAT (1–5) | 4.4 | 4.3 | Yes (≤ 0.2 below) |
| Top-harm errors | n/a | 0 | Yes |

**Decision:** ship to GA. The week-one novelty spike was excluded, as planned.

## 9. Operate

- **Week 7 drift:** handoffs jumped 9 points in two days. The cause was a partner bank's API change that produced a new sync error code the agent had never seen. Fix: new help content and 40 new eval cases. Handoffs were back to normal in 4 days.
- **Model upgrade (month 3):** the provider released a new version. Re-scored on all 640 cases, it kept 60/60 on top-harm at pass^3, gained 3 points of task success, and cost 20% less per ticket. It went through shadow and a 5% canary, then shipped. The old version stayed available for two weeks.
- **QBR (end of quarter 1):** the North Star is up 7%, at 9,200 resolved a week against a 9,400 target. Saving is on plan, and there were no SEV1 incidents.
  - **Decision:** double down. Add 4 more intents, and move invoice resends from copilot to agent: acceptance was 96% for 8 weeks, which cleared the bar set on the autonomy ladder.
- **Sunset watch:** none triggered. Next check is at the following QBR.

---

**Make your own:** copy the [templates](../README.md#templates-by-phase), follow the same order, and keep the arithmetic visible so anyone can check it.
