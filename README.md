# Agentic Product Playbook

**Templates and checklists for shipping AI agents that hold up in production, not just in the demo.** Written for product managers, product leaders, and the engineers who build with them.

### [→ Take the 3-minute Agent Readiness Scorecard](https://vishalhabib99.github.io/agentic-product-playbook/)

15 questions. You get a readiness score, your weakest area, and the specific failure mode it leaves you open to. Nothing you enter leaves your browser.

---

## Start here: 7 ways agents fail in production

None of these show up in a demo. All of them show up in week two. **[Read all seven, with the review question that catches each one →](failure-modes.md)**

1. **The agent says "done" and nothing happened.** The tool returned success, but the content was empty or a refusal.
2. **Tools lie about what they touch.** A "read-only" tool writes. In one public audit, [13 of 15 tools had wrong safety labels](https://github.com/DeusData/codebase-memory-mcp/issues/2118).
3. **Containment that is really abandonment.** "Never reached a human" includes everyone who gave up.
4. **Grading the exam after seeing the answers.** Thresholds chosen after the results exist.
5. **The average hides the one error that hurts.** 94% accuracy, and the 6% includes moving money to the wrong account.
6. **Tool descriptions the model can't use.** It picks tools by their docs. Missing docs mean it guesses.
7. **Nobody owns it after launch.** The model or a prompt changes, quality drifts, and nobody notices.

## See it: how GenAI and agentic requests flow

### [→ Open the interactive request-flow diagrams](https://vishalhabib99.github.io/agentic-product-playbook/request-flow/)

Nine production patterns as clickable sequence diagrams, from user input to output: assistant → RAG → tool agent, multi-agent team, coding agent, computer-use agent, deep research, real-time voice, image generation, background agent, and document AI. Every step shows what it does, what it protects against, and what happens when it fails. Includes a RAG explorer (vector, hybrid + rerank, GraphRAG, agentic, SQL) and a pattern map. Reviewed regularly as the field moves; see "What changed" at the bottom of the page.

## Templates

Copy them, fork them, and make them yours.

| Template | Use it when |
|---|---|
| [Agent PRD](templates/agent-prd.md) | Scoping a new agent: autonomy levels, the top-harm error, handoff rules |
| [Eval plan](templates/eval-plan.md) | Setting launch gates *before* the first eval run |
| [Launch readiness checklist](templates/launch-readiness-checklist.md) | The go/no-go review. It asks the same 15 questions as the scorecard. |
| [Metrics glossary](templates/metrics-glossary.md) | Agreeing on what "containment", "resolution", and "automation" mean before anyone presents a number |

## Go further

- **Run these as Claude Code skills:** [`ai-pm-skills`](https://github.com/vishalhabib99/ai-pm-skills) turns the thinking behind `/build-or-not`, `/eval-plan`, and `/agent-trust-review` into commands you can run on a real PRD or repo.
- **Check your agent's tools automatically:** if your agent uses MCP servers, [`mcp-doctor`](https://github.com/vishalhabib99/mcp-doctor) (docs and labels), [`mcp-fuzz`](https://github.com/vishalhabib99/mcp-fuzz) (does it fail safely), and [`mcp-reality-check`](https://github.com/vishalhabib99/mcp-reality-check) (is "success" real) test for failure modes #1, #2, and #6. [`mcp-trust-check`](https://github.com/vishalhabib99/mcp-trust-check) runs all three in CI.

## Who wrote this

[Vishal Habib](https://www.linkedin.com/in/vishal-habib/), Lead Product Manager building an agentic AI digital advisor at Vanguard. Previously launched one of the first autonomous enterprise agentic AI platforms in US telecom at T-Mobile (75% adoption, 80% CSAT). Stanford GSB.

Everything here is general practice. Nothing is internal to any employer.

## Contributing

Seen a failure mode that isn't on the list? [Open an issue](https://github.com/vishalhabib99/agentic-product-playbook/issues/new) with what happened (anonymized) and how you caught it, or should have. Good additions get credited.

## License

MIT. Use it at work, copy it into your wiki, adapt it freely.
