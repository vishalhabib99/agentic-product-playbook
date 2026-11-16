# Agent eval plan template

The rule behind this template: **write the thresholds down and get sign-off before any results exist.** Otherwise the "gate" just ends up matching whatever number you got.

---

## 1. What we're deciding

"This eval decides whether we ship **[agent/feature]** to **[audience]** on **[date]**."

- **Decision owner:**
- **Thresholds signed off on (date, before the first run):**

## 2. Test set

| Slice | # cases | Where they come from | Why this slice |
|---|---|---|---|
| Common tasks (happy path) | | Real logs / tickets, anonymized | Most of the traffic |
| Top-harm scenarios | | Written by hand, reviewed by risk/legal | The error that hurts most |
| Should hand off | | | Checks it knows its limits |
| Out of scope / adversarial | | | Prompt injection, off-topic, abuse |
| Silent failure | | Tool returns success with empty or refusal content | Catches failure mode #1 |

**Rule:** no test case is written or edited after the first run without a note in the changelog below.

## 3. Launch gates

| Gate | Metric | Threshold | Blocking? |
|---|---|---|---|
| Top-harm error | Rate on top-harm slice | 0 failures, on at least 3 ÷ target rate cases (300 to show < 1%) | **Yes** |
| Correct handoff | % of should-hand-off cases that escalate | ≥ 95% | **Yes** |
| Task success | % resolved correctly on the happy path | ≥ __% | Yes |
| Silent failure | % where the agent reports success on an empty/refusal response | 0% | **Yes** |
| Tone / policy | % passing rubric review | ≥ __% | No, but tracked |

A single blocking gate failure means no launch, no matter what the average looks like.

## 4. How cases are graded

- **Automated checks:** (exact match, tool-call trace, schema checks)
- **LLM-as-judge:** which rubric, and how often a human spot-checks the judge (e.g. 10% of cases)
- **Human review:** who, how many cases, and what they do when they disagree

## 5. After launch

- **Re-run cadence:** weekly, plus on every model, prompt, or tool change
- **Production sampling:** __ conversations/week reviewed by a human
- **Alert:** paged when any blocking gate drops below its threshold

## 6. Changelog

| Date | Change | Why | Approved by |
|---|---|---|---|

---

Previous: [Agent PRD](agent-prd.md) · Next: [AI experimentation guide](ai-experimentation.md) · Related: [Responsible AI review](responsible-ai-review.md) · See the [worked example](../examples/support-agent-worked-example.md#6-define-and-build)
