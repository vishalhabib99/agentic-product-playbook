# Agent PRD template

**Phase: Define.** A PRD for an AI agent needs everything a normal PRD has, plus answers to three questions a normal PRD never asks: **what may it do on its own, what's the worst thing it could do, and when does it hand off to a human.**

Copy this file and fill in every section. If a section doesn't apply, write down why.

---

## 1. Problem and user

- **User:** who, in what situation
- **Job to be done:** what they're trying to get done, in their words
- **Today:** how it gets done now, and what it costs (time, money, contacts)
- **Why an agent:** why not a form, a rule, a search box, or a better FAQ? *(If you can't answer this, stop here.)*

## 2. Scope

| The agent WILL | The agent WILL NOT (even if asked) |
|---|---|
| | |

## 3. Autonomy levels

For each action, pick one rung of the [autonomy ladder](north-star-metric-tree.md#2-autonomy-ladder):

| Action | Level | Why this level |
|---|---|---|
| e.g. Look up order status | **Agent:** acts alone | Read-only, low harm |
| e.g. Issue a refund under $50 | **Agent:** acts, then tells the user | Reversible, capped |
| e.g. Change a payment method | **Copilot:** proposes, user confirms | Hard to reverse |
| e.g. Reset a router | **Assist:** gives the steps, user acts | User's own device |
| e.g. Close an account | **Manual:** hands off to a human | Irreversible |

## 4. Top-harm error

- **The single most harmful thing this agent could do:**
- **Who gets hurt, and how badly:**
- **Maximum acceptable rate:** (e.g. 0 failures in 1,000 eval cases, and under 1 in 10,000 in production)
- **What the eval can prove:** 0 failures in n cases bounds the true rate below about 3 ÷ n, at 95% confidence (the rule of three). 0 in 1,000 proves under 0.3%, not under 1 in 10,000; that would take 30,000 cases. Say which part of the target the eval proves and which part production monitoring must prove.
- **How it's prevented** (not just detected):

## 5. Handoff to a human

- **Triggers:** (e.g. user asks for a person, 2 failed attempts, low confidence, out-of-scope topic, anything touching the top-harm error)
- **What the human receives:** (full transcript? a summary? what the agent already tried?)
- **The user never has to repeat themselves:** yes / no, and how

## 6. Success metrics

| Metric | Definition | Target | Guardrail it must not break |
|---|---|---|---|
| Resolution rate | Solved with no repeat contact in 7 days | | |
| False containment | "Contained" but the user came back or gave up | | |
| Top-harm error rate | See section 4 | | |
| CSAT / effort | | | |

See the [metrics glossary](metrics-glossary.md) for definitions.

## 7. Evals and launch gates

Link to the [eval plan](eval-plan.md). Thresholds must be signed off **before** the first eval run.

## 8. Rollout

- **Stages:** internal → x% of traffic → 100%
- **Kill switch:** who can turn it off, and how fast
- **Rollback trigger:** the specific numbers that mean "stop"

## 9. Open risks

| Risk | Likelihood | Impact | Owner | Mitigation |
|---|---|---|---|---|

## Exit gate

**To leave Define:** every action has a rung on the autonomy ladder, the top-harm error has a maximum rate and a prevention (not just detection), handoff triggers are written, and the [eval plan](eval-plan.md) thresholds are signed off.

---

Previous: [Roadmap prioritization](roadmap-prioritization.md) · Next: [Eval plan](eval-plan.md) · Related: [Responsible AI review](responsible-ai-review.md) · See the [worked example](../examples/support-agent-worked-example.md#5-define)
