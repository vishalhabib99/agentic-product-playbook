# Agentic Product Playbook

**The AI product manager's playbook, from strategy to sunset, with extra depth on shipping AI agents that hold up in production.** Templates, a fully worked example, and interactive diagrams. Written for product managers, product leaders, and the engineers who build with them. Works for any AI product at any company: consumer, B2B SaaS, marketplace, or developer platform.

### [→ Take the 3-minute Agent Readiness Scorecard](https://vishalhabib99.github.io/agentic-product-playbook/)

15 questions. You get a readiness score, your weakest area, and the specific failure mode it leaves you open to. Nothing you enter leaves your browser.

---

## Templates by phase

The full lifecycle, in order. Each phase ends with a gate; each template tells you what that gate is.

| # | Phase | Template | The question it answers |
|---|---|---|---|
| 1 | Strategy | [AI strategy memo](templates/ai-strategy-memo.md) | Where does AI earn a place, and build, buy, or fine-tune? |
| 2 | Vision | [Vision, North Star and metric tree](templates/north-star-metric-tree.md) | What does winning look like, and how much autonomy should it reach? |
| 3 | Discovery | [Discovery kit](templates/discovery-kit.md) | Is the problem real, and can a model actually solve it? |
| 4 | Roadmap | [Roadmap prioritization](templates/roadmap-prioritization.md) · [AI unit economics and pricing](templates/ai-unit-economics.md) | Which bets first, and does each task make money? |
| 5 | Define | [Agent PRD](templates/agent-prd.md) · [Eval plan](templates/eval-plan.md) · [Metrics glossary](templates/metrics-glossary.md) | What may it do alone, and what's the launch bar? |
| 6 | Build | [AI experimentation guide](templates/ai-experimentation.md) | How do we test something that answers differently each time? |
| 7 | Trust review | [Responsible AI review](templates/responsible-ai-review.md) | How can it hurt people, and does it fail safely? |
| 8 | Launch | [Launch readiness checklist](templates/launch-readiness-checklist.md) | Are we ready for go / no-go? |
| 9 | Operate | [Post-launch runbook](templates/post-launch-runbook.md) | Who owns it after launch, and when do we retire it? |

**See every template filled in:** [Worked example: a support agent, strategy to sunset](examples/support-agent-worked-example.md). It's a made-up company with illustrative numbers and real arithmetic you can check.

## Agents in production: 7 ways they fail

None of these show up in a demo. All of them show up in week two. **[Read all seven, with the review question that catches each one →](failure-modes.md)**

1. **The agent says "done" and nothing happened.** The tool returned success, but the content was empty or a refusal.
2. **Tools lie about what they touch.** A "read-only" tool writes. In one public audit, [13 of 15 tools had wrong safety labels](https://github.com/DeusData/codebase-memory-mcp/issues/2118).
3. **Containment that is really abandonment.** "Never reached a human" includes everyone who gave up.
4. **Grading the exam after seeing the answers.** Thresholds chosen after the results exist.
5. **The average hides the one error that hurts.** 94% accuracy, and the 6% includes moving money to the wrong account.
6. **Tool descriptions the model can't use.** It picks tools by their docs. Missing docs mean it guesses.
7. **Nobody owns it after launch.** The model or a prompt changes, quality drifts, and nobody notices.

## See it: the AI PM playbook, strategy to sunset

### [→ Open the AI PM playbook](https://vishalhabib99.github.io/agentic-product-playbook/ai-pm-playbook/)

The full product development lifecycle for any AI product, as one sequence diagram across eight roles: strategy, vision, customer research, roadmap, define, build, trust review, launch, and operate. Each phase ends with a gate. Includes the metrics stack (North Star Metric, input metrics, KPIs, AI quality, guardrails) with examples for consumer, B2B SaaS, marketplace and developer products, customer research methods by phase, and the pitfall to avoid at each step.

## See it: how GenAI and agentic requests flow

### [→ Open the interactive request-flow diagrams](https://vishalhabib99.github.io/agentic-product-playbook/request-flow/)

Nine production patterns as clickable sequence diagrams, from user input to output: assistant → RAG → tool agent, multi-agent team, coding agent, computer-use agent, deep research, real-time voice, image generation, background agent, and document AI. Every step shows what it does, what it protects against, and what happens when it fails. Includes a RAG explorer (vector, hybrid + rerank, GraphRAG, agentic, SQL) and a pattern map. Reviewed regularly as the field moves; see "What changed" at the bottom of the page.

## Go further

- **Run these as Claude Code skills:** [`ai-pm-skills`](https://github.com/vishalhabib99/ai-pm-skills) turns the thinking behind `/build-or-not`, `/eval-plan`, and `/agent-trust-review` into commands you can run on a real PRD or repo.
- **Check your agent's tools automatically:** if your agent uses MCP servers, [`mcp-doctor`](https://github.com/vishalhabib99/mcp-doctor) (docs and labels), [`mcp-fuzz`](https://github.com/vishalhabib99/mcp-fuzz) (does it fail safely), and [`mcp-reality-check`](https://github.com/vishalhabib99/mcp-reality-check) (is "success" real) test for failure modes #1, #2, and #6. [`mcp-trust-check`](https://github.com/vishalhabib99/mcp-trust-check) runs all three in CI.

## Who wrote this

[Vishal Habib](https://www.linkedin.com/in/vishal-habib/), Lead Product Manager building an agentic AI digital advisor at Vanguard. Previously launched one of the first autonomous enterprise agentic AI platforms in US telecom at T-Mobile (75% adoption, 80% CSAT). Stanford GSB.

Everything here is general practice. Nothing is internal to any employer.

## Contributing

Seen a failure mode that isn't on the list, or a template that's missing a question? [Open an issue](https://github.com/vishalhabib99/agentic-product-playbook/issues/new/choose) with what happened (anonymized) and how you caught it, or should have. Good additions get credited. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT. Use it at work, copy it into your wiki, adapt it freely.
