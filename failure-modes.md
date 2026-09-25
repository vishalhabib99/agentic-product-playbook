# 7 ways AI agents fail in production (and how a PM catches each one before launch)

None of these show up in a demo. All of them show up in week two.

Each one comes with **the question to ask in review**, **the signal to watch**, and **the template in this repo that covers it**.

---

## 1. The agent says "done" and nothing happened

The tool call "succeeded" and the agent told the user the task was done. But the response was really a refusal ("I can't access that account"), an empty payload, or a result that ignores what was asked. The agent reads success off the status code, not the content.

- **Ask in review:** "Show me what the agent sees when a tool returns success but the content is empty or a refusal."
- **Watch:** Tasks the agent marked complete that a user later re-opens or repeats within 24 hours.
- **Covered by:** [Eval plan](templates/eval-plan.md), row "Silent failure". For MCP tools, [`mcp-reality-check`](https://github.com/vishalhabib99/mcp-reality-check) checks this automatically.

## 2. Tools lie about what they touch

A tool labeled "read-only" writes to a database. A tool labeled "safe" deletes files. The agent (and your permission layer) trusts the label. This happens in real, popular projects: in one audit, [13 of 15 tools in a public MCP server](https://github.com/DeusData/codebase-memory-mcp/issues/2118) had wrong read-only or destructive labels.

- **Ask in review:** "Who verified that every tool's side effects match its label? Show me the list."
- **Watch:** Any write or delete that came from a tool marked read-only. That count should be zero.
- **Covered by:** [Launch readiness checklist](templates/launch-readiness-checklist.md), section "Tools & actions".

## 3. Containment that is really abandonment

"60% of conversations never reached a human" looks great until you find out a third of those users gave up. Containment counts conversations that didn't escalate. It doesn't count problems that got solved.

- **Ask in review:** "Of the contained conversations, how many were actually resolved? How do we know?"
- **Watch:** Contained conversations followed by a repeat contact, a call, or a cancel within 7 days.
- **Covered by:** [Metrics glossary](templates/metrics-glossary.md): containment vs. resolution vs. false containment.

## 4. Grading the exam after seeing the answers

The team runs the eval, sees 81%, then decides 80% was the bar. Launch gates set after the results exist aren't gates. They're just descriptions of the results.

- **Ask in review:** "When were these pass/fail thresholds written down, and by whom? Before or after the first run?"
- **Watch:** Threshold changes in the eval doc's history after results were posted.
- **Covered by:** [Eval plan](templates/eval-plan.md): thresholds get signed off before the first run.

## 5. The average hides the one error that hurts

94% accuracy overall. But the 6% includes moving money to the wrong account, and that error happens 1 time in 500. Averages across all tasks bury the rare, expensive mistake.

- **Ask in review:** "What is the single most harmful thing this agent could do? What's its error rate on that alone?"
- **Watch:** A separate metric, with its own threshold, for the top-harm error.
- **Covered by:** [Agent PRD](templates/agent-prd.md), "Top-harm error" section, and the [eval plan](templates/eval-plan.md).

## 6. Tool descriptions the model can't use

The model picks tools based on their names, descriptions, and parameter docs. When those are missing or vague, it guesses. Across 40+ open-source MCP servers audited with [`mcp-doctor`](https://github.com/vishalhabib99/mcp-doctor), undocumented parameters and missing descriptions were the most common issue, including in projects with thousands of stars.

- **Ask in review:** "Could a new engineer pick the right tool from the descriptions alone? The model has less context than they do."
- **Watch:** Wrong-tool calls and retries per task.
- **Covered by:** [Launch readiness checklist](templates/launch-readiness-checklist.md), section "Tools & actions".

## 7. Nobody owns it after launch

The model gets upgraded, a prompt gets tweaked, a downstream API changes a field. Quality drifts and nobody notices until customer complaints spike, because the eval suite only ran once, before launch.

- **Ask in review:** "Who gets paged when quality drops? Which eval re-runs on every model, prompt, or tool change?"
- **Watch:** The launch eval re-run weekly and on every change, trended over time.
- **Covered by:** [Launch readiness checklist](templates/launch-readiness-checklist.md), section "Launch & operations".

---

**How ready is your agent?** Take the [3-minute Agent Readiness Scorecard](https://vishalhabib99.github.io/agentic-product-playbook/). It maps your weakest area back to the failure modes above.
