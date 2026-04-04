# Reddit Post for Newsletter #2

**Subreddit:** r/ClaudeCode

**Title:** Spec-driven development isn't Waterfall in markdown — here's what it actually is

**Flair:** Discussion

---

**Summary:** This week's issue of my harness engineering newsletter digs into spec-driven development (SDD): what it actually is (and isn't), why it matters even if you hate "writing docs," when to spec vs vibe, and how to get the benefits without the ceremony. The core idea: have the agent *interview* you to surface edge cases and design decisions you'd otherwise miss, so the human handles the high-level reasoning while the agent handles documentation and execution. I also get into why SDD isn't Waterfall (the feedback loop is minutes, not months), and the tooling landscape — including a free open-source project I'm building.

**Why it matters:** As agentic workflows become the default for building software, the bottleneck is shifting from syntax to intent alignment. You say "clean, reactive dashboard" and the agent hears something close but not quite — deprecated charting library, no auth, wrong patterns. Red Hat calls this the "encoding/decoding gap." SDD narrows that gap by forcing clarity *before* the agent starts writing code. Red Hat reports a 40% reduction in code reviews with this approach, and research shows undirected AI coding increases code complexity by 41% — that's the gap specs are designed to close. SDD gives you a persistent source of truth that survives across sessions, so agents don't start from scratch every time.

**Technical Breakdown & Lessons Learned:**

Full Disclosure: I'm the author of the linked newsletter and am building a free open-source tool ([Agent Skills](https://github.com/Codagent-AI/agent-skills)) to facilitate this workflow.

Our approach centers on what I call the "recursive interview" pattern, pioneered by Jesse Vincent's [Superpowers](https://github.com/obra/superpowers) framework. Instead of the developer manually writing a static doc, the agent asks questions one at a time, about one capability at a time. What should happen when a webhook delivery fails? What's the retry policy? Should users configure notification channels, or is that a later feature? The agent surfaces edge cases you wouldn't have thought of, then produces the spec artifacts from your answers. You review them (that step matters — catch hallucinated requirements early), but the heavy lifting is the conversation, not the typing. The system is structured so the agent can't skip to implementation: task files read from the design, which reads from the specs, which reads from the proposal. Each artifact constrains the next.

From building and using this daily, two critical lessons:

1. **Context modularization matters.** A monolithic spec stuffed into the context window leads to token exhaustion and agents ignoring the middle of long files (the U-shaped attention problem). Red Hat recommends separating "what-specs" (goals, user stories, success criteria) from "how-specs" (constraints, security, testing requirements) into modular files. In practice, a spec is a markdown file, maybe two — not a 200-page requirements document.

2. **Spec drift is the real enemy.** The biggest failure mode is when the code evolves but the spec doesn't. Done right, specs should *improve* over time, not rot. Red Hat calls this "spec co-evolution" — when an agent encounters a failure, it doesn't just fix the problem; it suggests an update to the relevant spec so the same class of error doesn't recur. Waterfall specs degraded over time. SDD specs, done right, have the opposite trajectory.

One important caveat: SDD only works at the right scope. A spec for "add webhook notifications with retry logic" is small enough to evaluate in one pass. A spec for "build the entire notifications subsystem" — webhooks, email digests, in-app alerts, user preferences, delivery tracking — that's a big batch with late feedback. That's Waterfall by definition, just running on faster hardware.

**Project/Docs:** [Agent Skills](https://github.com/Codagent-AI/agent-skills)
**Full Breakdown:** [Think Before You Prompt](https://codagent.beehiiv.com/p/think-before-you-prompt)

I'd love to hear from the community: are you using formal specs with your coding agents, or does it feel like too much ceremony? What's working, what's overhead?
