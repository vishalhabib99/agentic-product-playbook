# AI discovery kit

**Phase: Discovery.** Use it to prove two things before you commit a team: **the problem is real and frequent, and AI solves it better than a simpler fix.**

The rule behind this kit: **test with real inputs from real users, not a demo you wrote yourself.** Five cherry-picked examples tell you nothing about the other 95%.

---

## 1. Research plan

| Question we need answered | Method | Who / how many | Done by |
|---|---|---|---|
| How often does the problem happen, and what does it cost? | Analytics, funnel and cohort data, support ticket mining | All users / last 90 days | |
| What is the job, in the user's words? | Interviews (JTBD) | 8–12 users per segment | |
| How do people do it today? | Contextual inquiry / shadowing | 4–6 sessions | |
| How much AI error will they tolerate, and how do they check? | Interviews + Wizard-of-Oz | Same users | |
| Can a model do it well enough? | Feasibility spike on 50–100 real cases | Data / ML | |

## 2. Interview guide

Keep it to 45 minutes. Ask about the past, not the future. "Tell me about the last time…" beats "Would you use…".

**The job (15 min)**
1. Tell me about the last time you had to [task]. Walk me through it.
2. What triggered it? What did you do first?
3. What was the hardest or slowest part?
4. What did you do when you got stuck?
5. How did you know you were done, and that it was right?

**Today's workarounds (10 min)**
6. What tools, people, or tricks do you use for this now?
7. What have you tried that didn't work? Why did you stop?
8. If this took zero time tomorrow, what would you do with that time?

**Trust and error tolerance (15 min).** This section is what makes it an AI interview.
9. Imagine someone else did this for you. What would you need to see to trust the result without redoing it?
10. Which mistakes would be annoying, and which would be unacceptable? Give me an example of each.
11. How would you check the work? How long would checking take?
12. When would you want to do it yourself, even if a tool could do it?
13. Have you used AI tools for anything like this? What made you trust them or stop trusting them?

**Wrap-up (5 min)**
14. Is there anything about this I should have asked?
15. Can I follow up and show you something rough in a couple of weeks?

## 3. Wizard-of-Oz test plan

A human (or a model a human checks) secretly produces the output, so you learn whether users want the result before you build anything.

- **What the user sees:** (the UI or message they get)
- **Who produces the output behind the scenes:**
- **Inputs:** real requests from __ users over __ days
- **What we measure:**
  - Do they use the output as-is, edit it, or ignore it?
  - Time saved vs. their usual way
  - Which outputs they rejected, and why
  - Would they be upset if it went away? (Ask at the end.)
- **Disclosure:** tell participants a person may be involved, before or after, as your research ethics policy requires.

## 4. Feasibility spike

Run before the roadmap commits a team. Takes days, not weeks.

1. Pull **50–100 real, anonymized inputs**, spread across easy, typical, and hard cases.
2. Write down what "correct" looks like for each one, before running anything.
3. Run the simplest approach (a hosted model with a prompt, maybe retrieval).
4. Grade every case: correct / partly correct / wrong / harmful.

| Result | What it means |
|---|---|
| Most cases correct, no harmful ones | Feasible. Move to Roadmap. |
| Many partly correct | Likely feasible with retrieval, tools, or better prompts. Budget for iteration. |
| Harmful outputs on realistic inputs | Stop and assign a risk tier before going further. |
| Mostly wrong | Not feasible today. Note what would change that, and park it. |

## 5. Opportunity solution tree

Keep the outcome at the top so solutions stay tied to it.

```
Outcome (from the metric tree)
 ├── Opportunity: a user need or pain from research
 │    ├── Solution A ── Experiment
 │    └── Solution B ── Experiment
 └── Opportunity
      └── Solution C ── Experiment
```

AI solutions sit next to non-AI ones on the same tree. If a better form or a rule solves the opportunity, choose it.

## 6. Synthesis

| Insight | Evidence (how many users, which data) | Opportunity | Confidence (high / med / low) |
|---|---|---|---|
| | | | |

## 7. Exit gate

- [ ] The problem is frequent and costly enough, with numbers.
- [ ] We know which errors users tolerate and which they don't.
- [ ] The feasibility spike shows a model can clear the bar on real inputs.
- [ ] The [AI is the right tool test](ai-strategy-memo.md#4-is-ai-the-right-tool-test) still passes.
- [ ] A risk tier is assigned (see [Responsible AI review](responsible-ai-review.md#1-risk-tier)).

---

Previous: [North Star and metric tree](north-star-metric-tree.md) · Next: [Roadmap prioritization](roadmap-prioritization.md) · See the [worked example](../examples/support-agent-worked-example.md#3-discovery)
