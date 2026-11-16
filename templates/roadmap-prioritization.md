# AI roadmap prioritization template

**Phase: Roadmap.** Use it to rank AI bets against each other and against non-AI work, then lay them out as Now / Next / Later.

The rule behind this template: **AI bets carry two costs normal features don't: the cost to run them and the risk that quality never gets there.** Score both, or the roadmap overpromises.

---

## 1. Scoring

Standard RICE (Reach × Impact × Confidence ÷ Effort), plus two AI columns.

| Bet | Reach (users / quarter) | Impact (0.25–3) | Confidence (%) | Effort (person-weeks) | RICE | Run cost per task | Risk tier | Rank |
|---|---|---|---|---|---|---|---|---|
| | | | | | | | | |

**How to score the AI parts**

- **Confidence** comes from evidence, not enthusiasm:
  - 80–100%: feasibility spike passed on real inputs, and users asked for it
  - 50%: one of the two
  - 20%: neither, it's a hunch
- **Run cost per task** comes from the [unit economics model](ai-unit-economics.md). A bet with a negative margin at target volume drops in rank, however high its RICE.
- **Risk tier** comes from the [Responsible AI review](responsible-ai-review.md#1-risk-tier). Higher tiers add review time to Effort.

## 2. Now / Next / Later

Outcomes, not features. Dates only in Now.

| | Now (this quarter) | Next (1–2 quarters) | Later (exploring) |
|---|---|---|---|
| **Outcome** | | | |
| **Bet** | | | |
| **Autonomy level** | | | |
| **Metric it moves** | | | |
| **Proof needed to move forward** | | | |

## 3. Kill criteria

Write these down when the bet starts, not when it's failing.

| Bet | We stop or rethink if… | Checked on |
|---|---|---|
| | e.g. offline eval below 80% after 6 weeks of iteration | |
| | e.g. cost per task above $0.50 at 10% traffic | |
| | e.g. weekly repeat use below 20% after 8 weeks | |

## 4. Dependencies

| Bet | Depends on | Owner | Status |
|---|---|---|---|
| | Data access, platform features, legal review, vendor contract | | |

## 5. Questions for the roadmap review

1. Which bet would we cut first if headcount dropped by a third?
2. Which bets depend on a model capability we haven't tested?
3. What's the total inference spend at target volume across all Now bets?
4. Which non-AI work did we drop to fund these, and is that the right trade?

---

Previous: [Discovery kit](discovery-kit.md) · Next: [Agent PRD](agent-prd.md) · See the [worked example](../examples/support-agent-worked-example.md#4-roadmap)
