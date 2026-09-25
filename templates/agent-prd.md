# Agent PRD template

A PRD for an AI agent needs everything a normal PRD has, plus answers to three questions a normal PRD never asks: **what may it do on its own, what's the worst thing it could do, and when does it hand off to a human.**

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

For each action, pick one level:

| Action | Level | Why this level |
|---|---|---|
| e.g. Look up order status | **Acts alone** | Read-only, low harm |
| e.g. Issue a refund under $50 | **Acts, then tells the user** | Reversible, capped |
| e.g. Change a payment method | **Proposes, user confirms** | Hard to reverse |
| e.g. Close an account | **Hands off to a human** | Irreversible |

## 4. Top-harm error

- **The single most harmful thing this agent could do:**
- **Who gets hurt, and how badly:**
- **Maximum acceptable rate:** (e.g. 0 in 1,000 eval cases, and under 1 in 10,000 in production)
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
