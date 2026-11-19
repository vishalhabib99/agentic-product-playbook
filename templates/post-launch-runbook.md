# Post-launch runbook

**Phase: Operate.** Use it to keep an AI product healthy after launch: weekly reviews, model upgrades, incidents, and the decision to grow, fix, or retire it.

The rule behind this runbook: **an AI product changes even when your code doesn't.** Models get updated, users bring new requests, and data drifts. Someone has to own it every week.

---

## 1. Owners

| Role | Name | Responsible for |
|---|---|---|
| Product owner | | Weekly review, roadmap changes, sunset decision |
| On-call engineer | | Alerts, incidents, kill switch |
| Quality owner | | Eval set, production sampling, judge calibration |
| Risk contact | | Harm incidents, regulator or legal questions |

## 2. Weekly review (30 minutes)

| Item | Source | Red if… |
|---|---|---|
| North Star and input metrics | Dashboard | Down 2 weeks in a row |
| Guardrails: top-harm rate, complaints, opt-outs | Dashboard | Any breach |
| AI quality: eval re-run, sampled production grades | Eval pipeline, human review | Below launch bar |
| Cost per successful outcome | Billing + metrics | Above plan by 20% |
| Latency p95 | Monitoring | Above target |
| Top 5 failure patterns this week | Sampled reviews, feedback tags | New pattern appears |
| Feedback → eval cases added | Eval changelog | Zero added |

**Output:** a short list of actions, each with an owner and a date.

## 3. Drift signals

| Signal | What it suggests | First move |
|---|---|---|
| Inputs look different (new topics, languages, lengths) | User or data drift | Sample and label; add to the eval set |
| Acceptance or resolution dropping, eval steady | The eval set no longer matches reality | Refresh the eval set from recent traffic |
| Eval dropping after no change on your side | Provider changed the model or a dependency changed | Check the model version; pin it; open a vendor ticket |
| Retrieval returning stale content | Index out of date | Check ingestion jobs |
| Cost rising, volume flat | Longer prompts, loops, or more retries | Check calls and tokens per task |

## 4. Model upgrade protocol

New model versions arrive often. Treat each one as a release.

1. **Re-score** the new model on the full golden set, including top-harm and red-team cases. Use pass^k (every case must pass on all k runs), not a single run.
2. **Compare** side by side with the current model: quality, cost per task, latency.
3. **Shadow** on live traffic for a few days.
4. **Canary** at 1–5%, then ramp using the [experimentation guide](ai-experimentation.md#5-ramp-plan).
5. **Keep the old version available** for fast rollback until the new one has held for two weeks.
6. **Log it** in the eval changelog.

## 5. AI incident response

| Severity | Example | Response time | Who's paged |
|---|---|---|---|
| **SEV1** | Top-harm error reached a user; data exposed; harmful content spreading | Minutes | On-call, product owner, risk contact |
| **SEV2** | Guardrail breached; quality well below bar; major cost spike | Within hours | On-call, product owner |
| **SEV3** | Single bad output reported; minor regression | Next business day | Quality owner |

**Steps**
1. **Contain:** use the kill switch, roll back the model or prompt, or narrow the scope. Containing comes before diagnosing.
2. **Assess:** how many users, what harm, since when. Pull the traces.
3. **Communicate:** internal channel first; affected users and regulators as your policy requires.
4. **Fix:** root cause in data, prompt, retrieval, tool, or model.
5. **Prevent:** add the failing case to the eval set so it can't come back unnoticed.
6. **Review:** a blameless write-up within a week.

## 6. Feedback loop

```
User feedback and corrections ─┐
Sampled production reviews ────┼──▶ Label ──▶ Golden set ──▶ Every eval run
Incidents and red-team finds ──┘                 │
                                                 └──▶ Roadmap: top failure patterns
```

## 7. Quarterly business review

- North Star trend vs. target
- ROI: value delivered vs. total cost (model, infra, people, human fallback)
- Cost per successful outcome trend
- Risk log: incidents, open risks, regulatory changes
- Autonomy: ready to move any workflow up a rung? (see the [autonomy ladder](north-star-metric-tree.md#2-autonomy-ladder))
- Decision: **double down, fix, pivot, or sunset**

## 8. Sunset criteria

Retire or rethink the feature when any of these hold for two quarters:

- Weekly repeat use stays below __%.
- Cost per successful outcome stays above the value it creates.
- Quality can't reach the bar and no planned fix would change that.
- A platform or vendor now does it better, and users have moved on.

**Retiring it well:** tell users in advance, offer the old workflow or an export, keep the kill switch until the last user is moved, and archive the eval set and lessons learned.

## Exit gate

**At every QBR:** a grow, fix, or retire decision is recorded with the evidence behind it, any autonomy change cites the evidence the ladder asked for, and an owner is named for the next quarter.

---

Related: [Launch readiness checklist](launch-readiness-checklist.md) · [Eval plan](eval-plan.md) · [7 failure modes](../failure-modes.md) (#7: nobody owns it after launch) · See the [worked example](../examples/support-agent-worked-example.md#9-operate)
