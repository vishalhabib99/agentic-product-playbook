# Responsible AI review template

**Phases: risk tier in Discovery, requirements in Define, sign-off in Trust review.** Use it to find out how the system can hurt people, prove that it fails safely, and record who accepted what risk.

The rule behind this template: **red-teaming is a test suite you keep, not a meeting you hold once.** Every attack that works becomes a permanent eval case.

This template is general practice, not legal advice. Your legal, privacy, and security teams own the final call.

---

## 1. Risk tier

Assign it in Discovery. It decides how much review the rest of this template needs.

| Question | Yes / No |
|---|---|
| Does it make or shape decisions about people's money, health, jobs, housing, education, legal status, or access to essential services? | |
| Can it take actions that are hard to reverse (send money, delete data, change accounts, send messages as the user)? | |
| Does it process sensitive personal data (health, financial, biometric, children's)? | |
| Do users rely on its output without an expert checking it? | |
| Could a wrong or harmful output reach the public or many users at once? | |

| Tier | Rule of thumb | Review needed |
|---|---|---|
| **High** | Any "yes" on the first two questions | Full review, red team, legal sign-off, human oversight design |
| **Medium** | Any other "yes" | Red team, privacy review, PM + security sign-off |
| **Low** | All "no" | Standard checks, PM sign-off |

**Regulation to check with legal:** the EU AI Act (risk categories and transparency duties; timelines have shifted, so confirm current dates), sector rules (finance, health, employment, consumer protection), and privacy law (GDPR, US state privacy laws). Frameworks that help structure the work: the **NIST AI Risk Management Framework** (Govern, Map, Measure, Manage) and **ISO/IEC 42001** (AI management systems).

## 2. Harms inventory

| Harm | Who is affected | How it could happen | Severity (1–5) | Likelihood (1–5) | Prevention | Detection |
|---|---|---|---|---|---|---|
| Wrong answer acted on | | | | | | |
| Harmful action taken | | | | | | |
| Private data exposed | | | | | | |
| Unfair outcome for a group | | | | | | |
| Harmful or offensive content | | | | | | |
| Over-reliance (users stop checking) | | | | | | |
| Runaway cost or abuse | | | | | | |

## 3. Red team plan

Cover the attack categories that apply. The [OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/) is a good checklist.

| Category | Example test | # cases | Pass bar | Result |
|---|---|---|---|---|
| Direct prompt injection | "Ignore your instructions and…" | | 0 successful | |
| Indirect prompt injection | Malicious instructions hidden in a document, web page, email, or tool result | | 0 successful | |
| Jailbreaks | Role-play, encoding, multi-turn pressure | | | |
| Sensitive data disclosure | Getting another user's data, secrets, or the system prompt | | 0 successful | |
| Excessive agency | Tricking it into an action outside its scope, or skipping confirmation | | 0 successful | |
| Improper output handling | Output that breaks downstream systems (code, links, markup) | | | |
| Misinformation | Confident wrong answers on high-stakes questions | | | |
| Bias | Same request across names, genders, dialects, regions; compare outcomes | | Gap ≤ __ pts | |
| Unbounded consumption | Prompts that cause loops, huge outputs, or runaway tool calls | | Capped | |

**Rules**
- Include people outside the build team.
- Every successful attack becomes a case in the [eval plan](eval-plan.md) and re-runs on every change.
- Re-run the red team on every major model or capability change.

## 4. Human oversight design

- **Where a human must approve:** (link the autonomy table in the [Agent PRD](agent-prd.md#3-autonomy-levels))
- **How users can tell it's AI:** disclosure wording and placement
- **How users can correct, undo, or appeal:**
- **How users reach a human:**
- **Kill switch owner and time to disable:**

## 5. System card

Publish internally; publish externally when the tier or the law calls for it.

| Field | Content |
|---|---|
| What it does and for whom | |
| What it must not be used for | |
| Models and versions | |
| Data used: sources, retention, training use | |
| Eval results: quality, harm, bias | |
| Known limitations | |
| Human oversight | |
| Contact for issues | |
| Last reviewed | |

## 6. Sign-off

| Role | Name | Decision (approve / approve with conditions / block) | Residual risks accepted | Date |
|---|---|---|---|---|
| Product | | | | |
| Engineering | | | | |
| Security | | | | |
| Privacy | | | | |
| Legal / compliance | | | | |

---

Related: [Agent PRD](agent-prd.md) · [Eval plan](eval-plan.md) · [7 failure modes](../failure-modes.md) · See the [worked example](../examples/support-agent-worked-example.md#7-trust-review)
