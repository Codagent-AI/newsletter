# Newsletter Backlog

Items queued for inclusion in upcoming issues. Each entry notes the source, the angle, and which newsletter section it fits (roundup item, analysis topic, or my-take seed).

---

## Roundup Candidates

**[The Anatomy of an Agent Harness](https://x.com/akshay_pachaar/status/2041146899319971922)** — Akshay Pachaar (Daily Dose of Data Science, 264k followers) published a comprehensive survey decomposing the agent harness into 12 components and 7 architectural decisions. Good synthesis of Anthropic/OpenAI/LangChain approaches. Notable data points: error compounding math (10 steps × 99% = 90.4% e2e), Manus rebuilt 5 times in 6 months removing complexity each time, LLMCompiler 3.6x speedup for plan-and-execute over ReAct. Research: [`ideas/anatomy-of-agent-harness.md`](../internal-docs/research/ideas/anatomy-of-agent-harness.md)

## Analysis Candidates

**"The 90.4% problem"** — Error compounding in multi-step agent workflows. The math is simple but most teams ignore it. How verification loops, deterministic sequencing, and graduated quality thresholds fight exponential failure rates. Source: Pachaar article + Agent Validator's graduated verification model.

**"Your harness is going stale"** — Harness lifecycle management. Anthropic's dead-weight-resets anecdote + Manus 5-rebuild data + the scaffolding metaphor. Every harness component should have a model-version it was added for. Source: NLW episode ([`ideas/harness-engineering-101-aidb.md`](../internal-docs/research/ideas/harness-engineering-101-aidb.md)) + Pachaar article.

**"Inner vs. Outer Harness"** — Böckeler's distinction. Most readers conflate the two. The inner harness is the vendor's problem; the outer harness is yours. Source: NLW episode.

## My Take Seeds

*(empty)*
