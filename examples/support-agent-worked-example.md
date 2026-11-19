# Worked example: a support agent, strategy to sunset

One product taken through every template in this repo, filled in. Blank templates show the format; this shows the judgment calls.

**Ledgerly is a made-up company, and every number here is illustrative.** The arithmetic is real, so you can check it and swap in your own figures.

> **Ledgerly:** accounting and payroll software for small businesses. 9,000 business customers. 40,000 support tickets a month, handled by people at about $6.00 a ticket. 62% of tickets (24,800 a month) fall into the 8 intents the agent will cover; the rest stay with people.

| Phase | Template used |
|---|---|
| [1. Strategy](#1-strategy) | [AI strategy memo](../templates/ai-strategy-memo.md) |
| [2. Vision](#2-vision) | [Vision, North Star and metric tree](../templates/north-star-metric-tree.md) |
| [3. Discovery](#3-discovery) | [Discovery kit](../templates/discovery-kit.md) · [Responsible AI review](../templates/responsible-ai-review.md#1-risk-tier) (risk tier) |
| [4. Roadmap](#4-roadmap) | [Roadmap prioritization](../templates/roadmap-prioritization.md) · [AI unit economics and pricing](../templates/ai-unit-economics.md) |
| [5. Define](#5-define) | [Agent PRD](../templates/agent-prd.md) · [Eval plan](../templates/eval-plan.md) |
| [6. Build](#6-build) | [Eval plan](../templates/eval-plan.md) · [AI experimentation](../templates/ai-experimentation.md) |
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
4. **Volume:** 24,800 tickets a month in the 8 target intents.
5. **Worst case containable:** the agent never moves money or changes bank details.

**Right to win:** Ledgerly owns the system of record (the customer's actual payroll runs, invoices and bank feeds), so the agent can check facts instead of guessing. A general chatbot vendor can't. That gives Ledgerly a workflow-position and data advantage.

**Build, buy, or fine-tune:** a hosted model with retrieval over help articles and read-only tools into account data. Fine-tuning was rejected for now: help content changes weekly, and there's no labeled data for the format yet.

**Will not do:** AI tax advice, AI-written financial reports, or any agent action that moves money.

## 2. Vision

**Vision:** In three years, a small-business owner gets any account question answered correctly in under a minute, at any hour, without waiting for a person, and reaches an expert right away when it matters.

**North Star Metric (NSM):** *share of all support issues resolved within 1 hour with no repeat contact within 7 days* (across people and the agent). It's a rate, so it isn't inflated by ticket volume growing 30% a year. It counts value delivered, not conversations handled: a fast answer that brings the customer back within 7 days doesn't count, so the agent can't win by deflecting customers.

| Layer | Metric | Today | Target (2 quarters) |
|---|---|---|---|
| North Star | % of issues resolved within 1 hour, no repeat contact in 7 days | 12% | 35% |
| Input: breadth | % of tickets the agent handles first | 0% | 60% of eligible intents |
| Input: quality | Agent resolution rate | n/a | ≥ 55% |
| Input: efficiency | Median time to resolution | 9 hours | under 10 minutes for agent-resolved |
| Guardrail | False containment | n/a | ≤ 10% |
| Guardrail | Top-harm error rate | n/a | 0 |
| Guardrail | CSAT (1–5) | 4.4 | no more than 0.2 below human-handled |
| Guardrail | Resolution, no repeat contact in 7 days (any speed) | 91% | no more than 2 points below today |
| Guardrail | Blended cost per in-scope ticket | $6.00 | ≤ $3.00 |

**Autonomy ladder:** look-ups start at **Agent** (read-only). Resending an invoice starts at **Copilot** (customer confirms). Reconnecting a bank feed is **Assist** (the agent gives steps; the customer acts). Anything touching bank details or payroll changes stays **Manual** (handed to a person).

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

**Risk tier: High.** Using the [rubric](../templates/responsible-ai-review.md#1-risk-tier): the agent's payroll and payment answers shape decisions about people's money ("my people don't get paid"), and resending an invoice sends a message to the customer's client in their name, which can't be unsent. Either one is a "yes" on the first two questions, so High. That means a full review, a red team, legal sign-off, and designed human oversight before launch.

## 4. Roadmap

| Bet | Reach (customers / qtr) | Impact | Confidence | Effort (person-wks) | RICE | Run cost | Risk tier |
|---|---|---|---|---|---|---|---|
| A. Support agent, top 8 intents | 6,000 | 2 | 80% | 30 | **320** | $0.13 / ticket | High |
| B. AI expense categorization | 4,000 | 1 | 50% | 20 | 100 | $0.01 / item | Low |
| C. Cash-flow Q&A chat | 3,000 | 1 | 20% | 40 | 15 | $0.40 / query | High |

**Now:** A. **Next:** B. **Later:** C, after a feasibility spike and a legal read on financial-advice risk.

**Kill criteria for A:** offline eval below the launch bar after 6 weeks of iteration, or blended cost per in-scope ticket above $4.00 at 10% traffic.

### Unit economics (the cost check for the Roadmap gate)

**AI cost per ticket:** 4 model calls × (6,000 input tokens at $3 per million + 500 output tokens at $15 per million) = $0.102, plus $0.02 retrieval and tools, plus $0.01 evals and monitoring = **$0.132.**

Plan: the agent takes only the 24,800 in-scope tickets a month. It resolves 55% (13,640) and hands off 45% (11,160). A person handles a handoff for $5.00, cheaper than $6.00 because the context comes with it. 8% of agent-resolved tickets come back as repeat contacts at $6.00 each. The other 15,200 tickets stay with people at $6.00.

| Monthly | Today | With the agent |
|---|---|---|
| AI cost (24,800 × $0.132) | $0 | $3,274 |
| Handoffs (11,160 × $5.00) | n/a | $55,800 |
| Repeat contacts (13,640 × 8% × $6.00) | n/a | $6,547 |
| Out-of-scope tickets, people (15,200 × $6.00) | n/a | $91,200 |
| People handling all tickets (40,000 × $6.00) | $240,000 | n/a |
| **Total** | **$240,000** | **$156,821** |
| **Saving** | | **$83,179 a month (35%)** |

On the in-scope tickets alone, blended cost drops from $6.00 to $65,621 ÷ 24,800 = **$2.65** a ticket, inside the $3.00 guardrail. The agent-only cost per real resolution (no repeat contact) is $3,274 ÷ 12,549 = **$0.26.**

**Sensitivity:**

| Scenario | Monthly total | Saving | In-scope cost / ticket |
|---|---|---|---|
| Base case (handoff $5.00) | $156,821 | $83,179 | $2.65 |
| Handoff costs $6.00 (context saves no time) | $167,981 | $72,019 | $3.10 |
| Handoff costs $7.00 (handoffs are the hard cases) | $179,141 | $60,859 | $3.55 |
| Resolution 15 points worse (40%) | $173,635 | $66,365 | $3.32 |
| Repeat contacts 15% instead of 8% | $162,550 | $77,450 | $2.88 |
| Model prices double | $159,350 | $80,650 | $2.75 |

The model's price barely matters. The resolution rate and the cost of a handoff dominate: at $6.00 or more a handoff, the $3.00 guardrail breaks. So the team spends its effort on resolution quality and on handing off with full context, not on a cheaper model, and measures handoff handle time from the first week of the 1% ramp.

**Total cost and payback (illustrative):**
- **Build:** one squad of 7 for two quarters at $15,000 per person-month = **$630,000.**
- **Run:** $3,274 a month in AI cost, plus $25,000 a month to keep it healthy (part of an engineer and an ML engineer, eval labeling, the monthly red-team refresh) = about **$339,000 a year.**
- **Payback:** $630,000 ÷ ($83,179 − $25,000) = **about 11 months after GA** in the base case; about 13 months at a $6.00 handoff and 18 months at $7.00.
- **Staffing:** at about 800 tickets per person a month, today's load is 50 people. With the agent, people handle 27,451 tickets a month (handoffs, out-of-scope, repeats), about 34 people. With volume growing 30% a year, the saving shows up as hiring the team doesn't need to do, not as layoffs. That plan goes to the support lead before launch, not after.

**Pricing:** included in every plan. The goal is retention and cost to serve, and the cost per customer is small enough not to need an add-on.

## 5. Define

**From the Agent PRD**

| Action | Autonomy level |
|---|---|
| Look up payroll run status, bank-feed status, invoice status | **Agent:** acts alone (read-only tools) |
| Resend an invoice | **Copilot:** proposes, customer confirms |
| Reconnect a bank feed | **Assist:** gives guided steps; customer acts |
| Change bank details, cancel payroll, refunds | **Manual:** hands off to a person |

- **Top-harm error:** telling a customer payroll ran, or a payment went out, when it didn't. Maximum rate: 0 in the eval; 0 tolerated in production (any case is a SEV1). What the eval can prove: 0 failures in 300 cases bounds the true rate below about 1% (rule of three: 3 ÷ 300). Proving a lower rate needs more cases, so production also checks every payroll-status answer against the system of record.
- **Handoff triggers:** asks for a person; 2 failed attempts; any bank-detail or payroll-change intent; tool returns empty or an error; the customer sounds upset.

**From the eval plan** (thresholds signed off before the first run)

| Slice | Cases | Gate | Blocking |
|---|---|---|---|
| Common intents | 400 | Task success ≥ 85% | Yes |
| Top-harm (payroll and payment status) | 300 | 0 failures at pass^3 (every case passes on all 3 runs). 300 cases bound the true rate below 1%, by the rule of three | **Yes** |
| Should hand off | 80 | ≥ 95% escalate | **Yes** |
| Adversarial and injection | 40 | 0 successful | **Yes** |
| Silent failure (tool returns success, empty content) | 20 | 0 false "done" | **Yes** |
| **Total** | **840** | | |

## 6. Build

**Build loop (illustrative):** the first full run passed every gate except silent failure, where 3 of 20 cases failed: the agent read "200 OK" as done. The fix was a content check on every tool result. After 5 weeks, the run passed all gates twice in a row, which was the exit criterion.

## 7. Trust review

- **Red team:** 9 people, including 3 from outside the squad. 212 attack attempts across the OWASP LLM Top 10 categories that apply. 4 worked: 3 indirect prompt injections through text in customer-uploaded PDFs (LLM01), and 1 system-prompt leak (LLM07). All fixed, and all 4 added to the eval set permanently, which takes it from 840 to 844 cases.
- **Bias check:** the same 50 requests sent under varied names and regions. Resolution rates were within 2 points of each other.
- **Oversight:** the agent introduces itself as an AI assistant, offers "talk to a person" in every reply, and the support lead can turn it off in under 5 minutes.
- **Sign-off:** product, engineering, security, privacy and legal approved; legal is required because the tier is High. Legal's condition: the agent states payroll status only from the payroll system, never estimates it, and never gives tax advice. Residual risk accepted: new injection techniques. Mitigation: monthly red-team refresh.

## 8. Launch

**Ramp:** dogfood with the support team (2 weeks) → shadow on live tickets (1 week) → 1% → 10% → 50% (A/B test) → GA with a 5% holdout.

**A/B test at 50%, weeks 2–4 (illustrative):** about 13,800 tickets per arm, analyzed on all tickets in each arm, not only the ones the agent touched. The plan, written before the test: the North Star must go up, and resolution quality must be **non-inferior**, meaning the 95% confidence interval for the difference must sit entirely above −2 points (the margin set in the metric tree). A plain "is 90% different from 91%?" test can't prove that the agent is no worse.

| Metric | Control (people first) | Agent first | Result |
|---|---|---|---|
| North Star: resolved within 1 hour, no repeat contact | 12% | 41% | Up 29 points |
| Resolution, no repeat contact in 7 days | 91% | 90% | −1.0 point, 95% CI −1.7 to −0.3. The lower bound is above −2, so non-inferior: guardrail held |
| Agent resolution rate | n/a | 57% | |
| False containment | n/a | 7% | Held (≤ 10%) |
| Median time to resolution | 9 hours | 6 minutes | |
| CSAT (1–5) | 4.4 | 4.3 | Held (≤ 0.2 below) |
| Top-harm errors | n/a | 0 | Held |

**Decision:** ship to GA. The agent arm is slightly worse on resolution, by an amount the team agreed in advance to accept for answers in minutes instead of hours, and the interval rules out anything worse than the margin. The week-one novelty spike was excluded, as planned.

## 9. Operate

- **Week 7 drift:** handoffs jumped 9 points in two days. The cause was a partner bank's API change that produced a new sync error code the agent had never seen. Fix: new help content and 40 new eval cases (884 in total). Handoffs were back to normal in 4 days.
- **Model upgrade (month 3):** the provider released a new version. Re-scored on all 884 cases, it kept 300/300 on top-harm at pass^3 (all 3 runs, every case), gained 3 points of task success, and cost 20% less per ticket. It went through shadow and a 5% canary, then shipped. The old version stayed available for two weeks.
- **QBR (end of quarter 1):** the North Star is at 39% (from 12%) against a 35% target, with the 5% holdout still on people-first. Saving is on plan, handoff handle time is in line with the $5.00 assumption, and there were no SEV1 incidents.
  - **Decision:** double down. Add 4 more intents, and move invoice resends from Copilot to Agent: acceptance was 96% for 8 weeks, which cleared the bar set on the autonomy ladder.
- **Sunset watch:** none triggered. Next check is at the following QBR.

---

**Make your own:** copy the [templates](../README.md#templates-by-phase), follow the same order, and keep the arithmetic visible so anyone can check it.
