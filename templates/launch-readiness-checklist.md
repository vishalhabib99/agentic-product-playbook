# Agent launch readiness checklist

**Phase: Launch.** Go through it in the launch review. Every **"no"** needs an owner and a date, or a written reason why it's acceptable.

The same questions make up the interactive [Agent Readiness Scorecard](https://vishalhabib99.github.io/agentic-product-playbook/).

---

## Problem & scope
- [ ] We can say in one sentence why this is an agent and not a form, rule, or search box.
- [ ] Written list of what the agent will **not** do, even if asked.
- [ ] Every action has a rung on the [autonomy ladder](north-star-metric-tree.md#2-autonomy-ladder): Agent (acts alone, or acts then tells), Copilot (proposes, user confirms), Assist (gives steps, user acts), or Manual (hands off to a human).

## Evals
- [ ] Launch thresholds were signed off **before** the first eval run.
- [ ] The top-harm error has its own test slice and its own threshold, separate from the average.
- [ ] The test set includes cases where a tool returns success with empty or refusal content.

## Tools & actions
- [ ] Every tool's real side effects were verified against its read-only/destructive label.
- [ ] Every tool and parameter has a description a new engineer could act on.
- [ ] Irreversible actions require user confirmation or a human.

## Human handoff
- [ ] Handoff triggers are written down and tested (including "user asks for a person").
- [ ] The human gets the full context, so the user never repeats themselves.
- [ ] We measure resolution and false containment, not just containment.

## Launch & operations
- [ ] Staged rollout with a named kill-switch owner who can turn it off in minutes.
- [ ] The launch eval re-runs automatically on every model, prompt, or tool change.
- [ ] A named owner reviews sampled production conversations every week.

## Exit gate

**Go / no-go:** every box is checked, or a named owner has accepted the gap in writing with a date. The decision, who made it, and the ramp plan are recorded.

---

Related: [7 failure modes](../failure-modes.md) · [Agent PRD](agent-prd.md) · [Eval plan](eval-plan.md) · [Metrics glossary](metrics-glossary.md) · [AI experimentation guide](ai-experimentation.md) · Next: [Post-launch runbook](post-launch-runbook.md) · See the [worked example](../examples/support-agent-worked-example.md#8-launch)
