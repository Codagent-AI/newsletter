# Slot Machines and Safety Nets

## Prelude to a harness

It was a thing before it had a name. As is the case for all disciplines when they first emerge, I suppose.

I first knew it was a thing when I caught the agentic coding bug last year.

Agentic coding - engineers either love it or hate it. For me, it was love at first prompt. It has an addictive quality, largely because you never quite know what you’re going to get. One minute, the AI solves a bug that’s been plaguing you for hours. The next, it tries to delete your production database. That unpredictability creates a dopamine hit every time you “win.”

Coding agents are like slot machines, and I was hooked. But I didn't just want to play the game - I wanted to "beat the house". So I became obsessed: what changes could I make to win more often? To tilt the weights in my favor, so to speak.

So I got to work, using agents to build tools for building with agents. It was not smooth sailing.

The failures weren't just random - they were systematic. Agents drift when intent is ambiguous, skip steps when execution is unstructured, and declare "done" without running the verifications you asked for. They drown in noisy context, forget everything between sessions, and quietly spread bad patterns through a codebase like weeds.

And yet the solutions were not obvious. Spec-driven development tools helped define artifacts but not processes. Agents disregarded even my most strongly worded prompts. And using multiple agents like Claude and Codex together was manual and clunky.

But ~~gradually~~ quite rapidly, the industry has started to converge on a common set of solutions.

## What is harness engineering?

Early this year, a term emerged for the thing myself and others have been building: [harness engineering](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html).

Harness engineering is the discipline of making AI coding agents reliable by engineering the system *around* the model - the workflows, specifications, validation loops, context strategies, tool interfaces, and governance mechanisms that make agents more deterministic and accountable.

So what does a harness actually look like? The mental model I use is three nested loops:

1. The outer loop runs at the project level. This is where you capture intent: specs, architecture docs, the knowledge base that agents pull from. It's also where governance lives: human oversight, keeping the repo clean, making sure the codebase doesn't rot over time. Think of it as the environment the agent works in.

2. The orchestration loop runs per feature. Plan before you build - requirements, design, task breakdown - where each artifact constrains the next. Only once the plan is solid does implementation begin, one task at a time, each verified before the next starts.

3. The inner loop runs per task. Write the code, verify it works, and if it doesn't - feed the errors back and try again. How you structure that cycle determines whether the agent produces working software or confident garbage.

*(diagram)*

## The three loops in practice

This isn't hypothetical. Each loop shows up clearly in real projects. Here's one case study per loop.

### The outer loop: OpenAI's million-line experiment

A small team at OpenAI [built a product](https://openai.com/index/harness-engineering/) from an empty repo to a million lines of code, roughly 1,500 pull requests over five months. The constraint: zero manually-written code. Every line was agent-generated. They estimated 10x time savings versus writing by hand.

What makes this a case study in the outer loop is that the team's primary job wasn't writing code. It was designing the environment agents work in. When something failed, the fix was always: "what capability is missing, and how do we make it both legible and enforceable for the agent?"

The first half of that is intent, making the repo the system of record. "From the agent's point of view, anything it can't access in-context while running effectively doesn't exist. Knowledge that lives in Google Docs, chat threads, or people's heads is not accessible to the system." If a Slack conversation aligned the team on an architectural pattern but nobody wrote it down, it was invisible, to agents and to any engineer who joined later. So they moved everything into the repo: design docs, execution plans, acceptance criteria, architectural decisions. If it mattered, it was a versioned markdown file.

But not all at once. They tried a monolithic instruction file first and it failed. "A giant instruction file crowds out the task, the code, and the relevant docs, so the agent either misses key constraints or starts optimizing for the wrong ones." Instead, they built a short AGENTS.md (~100 lines) as a table of contents pointing into a structured `docs/` directory. Plans, known tech debt, and completed work were all versioned and co-located. The philosophy was progressive disclosure: give the agent a map, not a thousand-page manual.

But intent alone isn't enough. Agents replicate whatever patterns exist in the repo, including bad ones. The team spent every Friday cleaning up "AI slop." That didn't scale. So they encoded what they cared about as mechanical rules: a strict layered architecture (Types → Config → Repo → Service → Runtime → UI) enforced by custom linters that blocked the build if dependencies flowed the wrong direction. Naming conventions, structured logging, file size limits, all enforced by lint rules whose error messages doubled as remediation instructions for the agent. "This is the kind of architecture you usually postpone until you have hundreds of engineers. With coding agents, it's an early prerequisite: the constraints are what allows speed without decay."

And they fought entropy continuously. Background "garbage collection" agents scanned for deviations, graded quality, and opened targeted refactoring PRs. Human taste was captured once, then enforced on every line of code going forward. Daily standups became *more* important at this velocity, not less. When decisions happened in Slack, someone would immediately have Codex encode the decision as a guardrail. Human oversight set the direction; automated enforcement kept the codebase healthy enough for agents to keep working in it.

The key takeaways for the outer loop are: capture intent so agents know what to build, and enforce governance so the codebase doesn't rot under them.

### The orchestration loop: MetaGPT's assembly line

Give two agents an open-ended task and let them chat freely. What happens? The authors of [MetaGPT](https://github.com/FoundationAgents/MetaGPT) (ICLR 2024) call it the telephone game problem. One agent hallucinates a requirement. The next builds on it. By the third handoff, the whole thing is confidently wrong.

MetaGPT's fix is borrowed from how large enterprise organizations work: Standardized Operating Procedures. Not just "assign roles," but enforce a strict pipeline where each agent consumes a document and produces a different one that the next agent in the chain consumes. A Product Manager produces a PRD. An Architect consumes it and produces a system design. A Project Manager consumes that and produces a task breakdown. Only then does an Engineer start writing code, bounded by everything upstream. A QA agent closes the loop by testing against the original requirements. No agent can "code into a corner" because it can't start until the planning artifacts exist as concrete documents.

The results make the case for structure. When tested with roles removed one at a time, an Engineer agent working alone scored 1.0 out of 4.0 on executability. Adding each upstream role improved it step by step. With the full pipeline, executability jumped to 4.0 and human revisions needed fell from 10 to 2.5. Same model. The only difference was the harness around it.

The lesson is that the orchestration loop needs two things: structured artifacts as the coordination medium (not chat), and a separation between planning and execution that prevents agents from skipping ahead.

### The inner loop: Superpowers' subagent discipline

The inner loop is a single agent working on a single task. The outer and orchestration loops can be perfect, but if the agent botches the implementation or declares "done" without verifying, none of it matters.

The skills pack [Superpowers](https://github.com/obra/superpowers) attacks this through discipline enforcement. Rather than replacing your agent or controlling its tools, Superpowers shapes *how the agent thinks* during execution through detailed skills that prescribe methodology, verification, and review.

The core mechanism is subagent-driven development. A controller agent reads the plan, then dispatches a fresh subagent for each task. Each implementer starts with clean context: just the task description, architectural context, and working directory.

Each task uses TDD - but TDD alone isn't enough; agents lie. Superpowers' verification skill addresses this with a 5-step evidence gate: identify the command that proves your claim, run it, read the complete output, verify it matches, and only then make the claim.

Then comes review. The controller dispatches separate reviewer subagents - spec compliance first (did you build what was asked?), code quality second (is it well-structured?). The reviewers explicitly distrust the implementer and verify everything independently. If issues are found, the implementer fixes and the *same reviewer* re-evaluates.

The lesson from Superpowers is that the inner loop isn't just about tools or verification. It's about *methodology*. TDD, independent review, evidence-based completion claims, systematic debugging: these are engineering disciplines that humans learn through years of practice. Superpowers encodes them as behavioral specifications that shape the agent's approach.

## The gap

These projects prove why each loop matters. But each leaves significant gaps.

MetaGPT is a self-contained agent runtime - it replaces your coding agent rather than working with it. It doesn't integrate with Claude Code, Codex, or whatever CLI you're already using. And it requires that the whole pipeline be fully automated. For mission-critical projects, you need human-collaborative planning with hard approval gates - slower, but it catches hallucinated requirements early, before they compound downstream. And in-memory coordination is fast, but it's ephemeral. When your planning artifacts are files in a git repo, they're durable, diffable, human-readable, and reviewable. And they survive crashes.

On the other hand, Superpowers works *within* Claude Code, shaping agent behavior from the inside. But everything is prompt-based - there's no deterministic enforcement. The model might skip TDD when the context window fills up, or decide it knows better than the verification skill. And its verification is reviewer-only - no pluggable system for automated checks like type-checking, security scanning, or custom linters as hard gates independent of reviewer judgment. When the reviewer is the same model family as the implementer, they share the same blind spots.

## What's next

Most teams cobble together their orchestration loop from scripts, prompts, and manual handoffs. Better solutions are starting to emerge, but what's missing isn't more agent capabilities and plugins. *We need an entirely new kind of tooling layer that coordinates and constrains the coding agent*.

I'm building a production-grade alternative that works *with* the agents you already have. The things missing from these tools - session management across steps, crash recovery and resumption, agent-agnostic orchestration, human-in-the-loop approval gates, composable workflow primitives, and automated verification with cross-agent review - are what I'm focused on. Bring your own agent. Codagent provides the harness.

In the meantime, Agent Validator - the verification engine that powers the inner loop - is available now. It handles the verify-fix cycle: configurable validation pipelines, cross-agent code review, and the feedback loop that keeps agents honest. I'll dig into how it works in the next issue. Check out [codagent.dev](https://codagent.dev) if you want to get started now.

I'm "all in" on harnesses. What about you?