# Agent metrics glossary

**Phases: Vision → Operate.** The same word means different things on different teams. Agree on these definitions before anyone presents a number.

| Metric | Definition | Watch out for |
|---|---|---|
| **Containment rate** | % of conversations that ended without a human handoff | Counts giving up as a win. Never report it alone. |
| **Resolution rate** | % of conversations where the problem was solved: no repeat contact on the same issue within 7 days | Needs a way to match the repeat contact to the same issue |
| **False containment** | Contained conversations followed by a repeat contact, a call, or a cancellation within 7 days | The number that tells you whether containment is real |
| **Automation rate** | % of eligible tasks completed end-to-end with no human step | Define "eligible" up front, or the denominator shrinks until the number looks good |
| **Adoption** | % of eligible users who used the agent at least once in the period | Separate first use from repeat use. Repeat use is the real signal. |
| **Correct handoff rate** | % of conversations that *should* have gone to a human that actually did | Low = the agent is overconfident. Pair it with unnecessary-handoff rate. |
| **Unnecessary handoff rate** | % of handoffs a human closed with no action the agent couldn't have taken | High = the agent is too timid, and you're paying for humans |
| **Top-harm error rate** | Rate of the single most harmful error (from the PRD), measured on its own | Never folded into overall accuracy |
| **Silent failure rate** | % of tasks the agent reported as done where the tool actually returned empty, refusal, or wrong content | Invisible unless you look for it on purpose |
| **Wrong-tool rate** | % of tool calls to the wrong tool or with invalid arguments (track retries separately) | Usually a tool-description problem, not a model problem |
| **CSAT** | Mean post-conversation rating on a 1–5 scale | Response bias: happy and furious users answer, everyone else doesn't |
| **Cost per resolution** | Model + tool + human cost ÷ resolved conversations | Per *resolution*, not per conversation |

## One rule

Report containment, resolution, and false containment **together**, every time. Any one of them alone can be made to look good.

## Exit gate

**Before any metric is presented:** it uses the definition here, or the deck says how its definition differs.

---

Full catalog with formulas: [GenAI and agentic AI product metrics](genai-agentic-metrics.md)

Related: [Vision, North Star and metric tree](north-star-metric-tree.md) · [AI unit economics](ai-unit-economics.md) · [Agent PRD](agent-prd.md)
