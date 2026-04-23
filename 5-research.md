# Research Dossier for newsletter/5.md

Raw research material organized by section. Every item includes source path and verbatim/paraphrased status. Pick and choose.

**Source files referenced:**
- `internal-docs/research/ideas/anatomy-of-agent-harness.md` — Pachaar X thread (the thin-harness quote already in the outline)
- `internal-docs/research/ideas/harness-engineering-101-aidb.md` — NLW's AI Daily Brief episode (the "inner vs outer harness" framing is here)
- `internal-docs/research/ideas/harness-engineering-best-practices-anthropic-openai.md`
- `internal-docs/research/ideas/externalization-in-llm-agents.md` — Zhou et al. 2026 academic paper (21 authors, ~50 pages)
- `internal-docs/research/ideas/from-agent-loops-to-structured-graphs.md` — Hu Wei 2026 (~51 pages, 70 systems surveyed)
- `internal-docs/research/ideas/willison-agentic-engineering-lennys-podcast.md`
- `internal-docs/research/ideas/spec-driven-development-landscape.md`
- `internal-docs/research/ideas/codescene-agentic-quality-patterns.md`
- `internal-docs/research/tools/everything-claude-code-deep-dive.md` (ECC plugin, 1300+ files)
- `internal-docs/research/tools/claude-mem.md` (60k stars)
- `internal-docs/research/tools/superpowers.md` (116k stars, Jesse Vincent / obra)
- `internal-docs/research/tools/bmad-method.md` (43.1k stars)
- `internal-docs/research/tools/augment-intent.md`
- `internal-docs/research/misc/comparison-loops.md`

---

## Section 1: Inner vs Outer Harness — Böckeler's split

The newsletter's whole framing hangs off one article: **Birgitta Böckeler**, "Harness engineering for coding agent users," martinfowler.com, **April 2, 2026**. She draws the line cleanly enough that Sections 2–4 just fill in what sits on either side.

### 1.1 The framing source

- **Primary piece:** "Harness engineering for coding agent users" — Böckeler, martinfowler.com, April 2, 2026 — https://martinfowler.com/articles/harness-engineering.html. Introduces the builder-vs-user harness split and the guides-vs-sensors framework (Section 3 unpacks these).
- **Earlier memo:** "Harness Engineering – first thoughts," Feb 17, 2026 — https://martinfowler.com/articles/exploring-gen-ai/harness-engineering-memo.html. Preceded the April piece with a three-category lens: *context engineering, architectural constraints, garbage collection*.
- **Author:** Distinguished Engineer at Thoughtworks, 20+ years as developer/architect, focus on AI-assisted delivery. Adjacent talks: "From autocomplete to agents" (QCon London, April 2025), "State of Play: AI Coding Assistants" (QCon London, March 2026).

### 1.2 Vocabulary — who coined what

Böckeler's actual vocabulary vs. the community shorthand used throughout this newsletter (verified against the source, not paraphrase):

| Term used here | Böckeler's term | Count | Attribution |
|---|---|---|---|
| **outer harness** | *outer harness* / *user harness* | 2× each, interchangeable | ✓ Böckeler |
| **inner harness** | *builder harness* | 1× | *"inner harness"* does not appear in Böckeler's piece. The shorthand traces to **Nathaniel Whittemore** ("Harness Engineering 101," *The AI Daily Brief*, per `harness-engineering-101-aidb.md`). |
| **guides / sensors** | *guides / sensors* | — | ✓ Böckeler (mirrors Fowler/Thoughtworks' broader engineering usage) |

If citing once, Böckeler is load-bearing — she provides the whole framework (outer harness + guides/sensors + the concentric-circles diagram + the steering loop). NLW contributed the catchier *inner/outer* pairing.

### 1.3 Why the split matters

Two Böckeler quotes do the work:

> "A well-built outer harness serves two goals: it increases the probability that the agent gets it right in the first place, and it provides a feedback loop that self-corrects as many issues as possible before they even reach human eyes."

> "To let coding agents work with less supervision, we need ways to increase our confidence in their result."

The two goals line up 1:1 with her guides/sensors split (§3.1 #3–4). The confidence line states the economic motivation: confidence is what lets a human move from editor to supervisor.

Her closing principle — **the steering loop** — is the meta-pattern that ties both halves together:

> "Whenever an issue happens multiple times, the feedforward and feedback controls should be improved to make the issue less probable to occur in the future, or even prevent it."

> "The human's job in this is to **steer** the agent by iterating on the harness."

### 1.4 The one-line premise

> "Engineering a user harness for a coding agent is a specific form of context engineering." — Böckeler, April 2026

The rest of this issue is a structural argument about where that engineering should happen. **Section 2** inventories what the builder/inner harness already ships. **Section 3** catalogs what the user/outer harness adds. **Section 4** argues that the next honest step is to move the coordination logic *out of the agent's context window entirely* — which is what agent-runner is built to do.

---

## Section 2: The Inner Harness — what the tool ships with

The **inner harness** is everything the agent vendor (Anthropic, OpenAI, etc.) builds into Claude Code, Codex, or equivalent products *before you touch it*. It's the part you don't control.

**Primary reference:** Pachaar's 12-component survey (`anatomy-of-agent-harness.md`), distilled here to eight. Supporting: Aetna Labs' three-layer model, Hu Wei's scheduling-theory lens, Charriere's convergence argument, and Anthropic's own Managed Agents framing.

### 2.1 The eight components

**1. Orchestration loop.** The core driver — ReAct or plan-and-execute. Charriere calls it the commoditizing essence: *"goal + tools + loop until done."* Hu Wei formalizes it as *"a non-deterministic, single-ready-unit scheduler"* — exactly one action ever dispatchable, chosen by opaque LLM inference. Aetna groups it under *"execution layer."* Pachaar lists it as component #1 and calls its thickness (ReAct vs. plan-and-execute) the 2nd of his seven architectural decisions.
  - Sources: `anatomy-of-agent-harness.md` (component #1; decision #2), Charriere §2.3A (below), `from-agent-loops-to-structured-graphs.md`, Aetna three-layer model (`harness-engineering-101-aidb.md`)

**2. Tool interface.** How the model reaches beyond text — file edit/read, shell, grep, web search, MCP adapters. Millidge's analogy via Pachaar: *tools = device drivers*. Claude Code ships ~7 built-ins (Edit, Write, Bash, Read, Grep, WebSearch, Task) plus MCP via `.mcp.json`. Codex ships equivalents configured via TOML.
  - Sources: `anatomy-of-agent-harness.md` (component #2), `everything-claude-code-deep-dive.md`

**3. Context management.** What's in the working window *right now* — compaction, trimming, observation masking, just-in-time retrieval. Stanford's "Lost in the Middle" (via Pachaar): 30%+ degradation when key content falls mid-window. Claude Code exposes `/clear` and `/compact`; Anthropic/OpenAI best practices call context *"a scarce resource."*
  - Sources: `anatomy-of-agent-harness.md` (component #4), `everything-claude-code-deep-dive.md`, `harness-engineering-best-practices-anthropic-openai.md`

**4. State & session persistence.** What outlives a single turn or crash. Three canonical models: LangGraph checkpointing, OpenAI's Item/Turn/Thread sessions, Claude Code's git-as-checkpoint. Codex publishes the same model as an explicit session API. Pachaar groups this as *"state management"* (component #7).
  - Sources: `anatomy-of-agent-harness.md` (component #7; state-management triad), `everything-claude-code-deep-dive.md` §10 (Codex)

**5. Error recovery.** What the loop does when a single step goes wrong — tool-call retry, malformed-output reprompting, rate-limit backoff, crash handling. Scoped to what the vendor ships and the user can't override. (Pachaar lumps this with "verification loops" as components #8 and #10, but verification — the *"write checks → agent implements → run checks → feed violations back"* pattern, Cherny's 2–3× claim — is almost entirely outer-harness work; it lives in §3.1 #4 *sensors*.)
  - Sources: `anatomy-of-agent-harness.md` (component #8)

**6. Guardrails & permissions.** What the loop may do without asking. Anthropic's three-stage permission model and OpenAI's three-level guardrail system are the same shape with different vocabularies. Includes tool scoping and sandboxing. Pachaar component #9.
  - Sources: `anatomy-of-agent-harness.md` (component #9), `everything-claude-code-deep-dive.md`

**7. Subagent orchestration.** Spawning isolated helpers with fresh context. Three sandbox models per Pachaar: **fork, teammate, worktree**. Claude Code exposes all three (Task tool, Teammate abstraction, WorktreeCreate hook). The firewalling is what prevents the parent's context from drowning in subagent output.
  - Sources: `anatomy-of-agent-harness.md` (component #11), `everything-claude-code-deep-dive.md`

**8. Extension surfaces.** The seams the outer harness plugs into — hook events (~18 in Claude Code), plugin slots (`skills/`, `agents/`, `commands/`), MCP servers, auto-loaded briefing files (CLAUDE.md / AGENTS.md), session lifecycle events. This is the only inner-harness component that exists *for* the outer harness; everything else in §2.1 is internal.
  - Sources: `everything-claude-code-deep-dive.md` (hooks, plugins, MCP; Codex `.codex/` in §10)

These eight cover 10 of the 11 items in Pachaar's list: prompt construction folds into #1, output parsing into #2, memory splits between #3 and #4, and error handling is #5. Pachaar's component #10 (*verification loops*) is deliberately **excluded** — it's an outer-harness pattern (§3.1 #4 *sensors*), not something the vendor ships.

### 2.2 Same thing, different names

The inner harness has been decomposed four different ways in the corpus, with substantial overlap:

| Pachaar (12 components) | Aetna Labs (3 layers) | Anthropic Managed Agents | Millidge / Von Neumann |
|---|---|---|---|
| orchestration loop | execution layer | agent loop ("brain") | CPU |
| tools | — | — | device drivers |
| context management | information layer | — | RAM |
| memory + state | information layer | event log ("session") | disk |
| error handling | — | — | OS exception handling |
| guardrails + permissions | execution layer | execution env ("hands") | OS privilege mode |
| subagent orchestration | execution layer | — | — |

(Aetna's *feedback layer* — evaluation + tracing + observability — falls mostly in the outer harness per §3.1 #4 *sensors*; only runtime tracing arguably lives inner.)

All four lenses are describing the same territory; the differences are granularity and what the author wanted to emphasize. Aetna's three-layer model is the easiest to teach; Pachaar's 12 is the most complete; Anthropic's Managed Agents framing is the most forward-looking (stable *interfaces* over stable implementations).

### 2.3 Trend: the inner harness is thinning

The three best pieces of primary evidence that the inner harness is deliberately getting lighter, not heavier:

**A. The Great Convergence (Charriere).** From `harness-engineering-101-aidb.md` Key Idea #5:
> "Claude Code was invented for coding but turned out to be a general problem-solving machine because the architecture — goal + tools + loop until done — generalizes to any computer task. This is why Linear, Notion, Cursor, Codex, Lovable, and Retool are converging on the same product shape."

The loop itself is becoming a commodity — everyone is landing on the same shape.

**B. Rajasekaran / Anthropic engineering blog — "Harness design for long-running application development"** (March 24, 2026). Anthropic engineer built a harness with explicit "sprint" decomposition for Opus 4.5; when Opus 4.6 shipped, he *deleted* the sprint machinery because the model could handle the decomposition natively. The money quote (24 words):
> "Every component in a harness encodes an assumption about what the model can't do on its own, and those assumptions are worth stress testing."
>
> "I started by removing the sprint construct entirely… With the sprint construct removed, I moved the evaluator to a single pass at the end of the run rather than grading per sprint."

This is the whole thesis, from Anthropic, on the record.

**C. "Building Effective Agents" — Erik S. & Barry Zhang, Anthropic research blog, Dec 19, 2024.** The foundational statement of the "simplest thing first" ethos that Rajasekaran 2026 applies across a model generation:
> "The most successful implementations weren't using complex frameworks or specialized libraries. Instead, they were building with simple, composable patterns."
>
> "Start with simple prompts, optimize them with comprehensive evaluation, and add multi-step agentic systems only when simpler solutions fall short."

**D. Cherny / Claude Code product principle** (via PromptLayer and Latent Space secondary coverage):
> "Claude Code is not a product as much as it's a Unix utility. This fits very well with Anthropic's product principle: 'do the simple thing first.'"
>
> Cherny: "Make every change as simple as possible. Minimal code. If you can delete lines instead of adding them, do that."

The flagship product embodies the same thesis in its architecture.

**E. Manus rebuilt 5× in 6 months (Pachaar).** *"Manus was rebuilt five times in six months, each time removing complexity."* Strongest numeric backup for the "harnesses go stale" side of the thesis.

**F. Evolutionary simplicity (Anthropic + OpenAI best practices synthesis)** — from `harness-engineering-best-practices-anthropic-openai.md` Key Idea #5:
> "Harness complexity should decrease over time as models get better… the right amount of structure is whatever the current model generation requires, and that amount should trend downward."

**G. Managed Agents as meta-harness (Anthropic).** Anthropic separated the *agent loop* (brain), *execution environment* (hands), and *event log* (session) into independent components — a platform bet that specific harnesses will keep changing, so they build around stable *interfaces* instead. NLW's one-liner: *"harness engineering is permanent as a discipline; any specific harness is disposable."*

Put together: the inner harness is **commoditizing** (Charriere, Managed Agents) AND **thinning** (Rajasekaran, Erik/Zhang, Cherny, Manus, evolutionary simplicity) simultaneously. If that's true, the differentiation moves to the outer harness — which is Section 3.

---

## Section 3: The Outer Harness — what you bring

The **outer harness** is everything you layer on top of what the tool ships with — briefings, skills, specs, sensors, memory, protocols. It's the part you control, and — per Böckeler — where developer leverage lives when the inner harness thins.

**Primary reference:** Böckeler, *"Harness engineering for coding agent users,"* martinfowler.com, April 2, 2026 (see Section 1 for full source treatment). Supporting: Zhou et al. four-domain externalization framing, Superpowers / BMAD / claude-mem as the dominant community implementations, Anthropic/OpenAI best practices, Willison, CodeScene, SDD landscape.

Böckeler's **central organizing distinction:** *guides* (feedforward — shape the agent **before** it acts) vs. *sensors* (feedback — catch issues **after**). Plus a third bucket: the durable context the agent reads, remembers, and produces. The eight components below map onto those three buckets.

### 3.1 The eight components

**1. Context briefings.** What the agent loads at session start. CLAUDE.md / AGENTS.md, Memory Bank's `projectbrief.md` / `systemPatterns.md` / `activeContext.md`, Google's SDD three-tier model (spec-first / spec-anchored / spec-as-source), Zhou et al.'s *"externalized state across time."* Böckeler's umbrella term: *"a specific form of context engineering."*
  - Sources: Böckeler §1.3, `spec-driven-development-landscape.md` (Memory Bank + Google tiers), `externalization-in-llm-agents.md`

**2. Skills library.** Named, findable, loadable-on-demand procedural expertise. Superpowers (**116k stars**, Jesse Vincent) is the market leader — markdown skill files enforcing a full design → plan → implement → review → deliver workflow. BMAD-METHOD (**43.1k stars**) ships 12+ role personas and 34+ structured workflows. Zhou et al. identify the five mechanisms any skills system needs: specification, discovery, progressive disclosure, execution binding, composition.
  - Sources: `superpowers.md`, `bmad-method.md`, `externalization-in-llm-agents.md`

**3. Guides — feedforward controls (Böckeler's term).** Rules that anticipate behavior *before* the agent acts. System-prompt directives, role instructions, hard-gate patterns. Superpowers' *"`<HARD-GATE>` Do NOT invoke any implementation skill… until the user has approved it"*. BMAD's workflow files shout *"NEVER skip steps"* and *"Execute ALL steps in exact order"* — a confession that prompt-based guides are the only enforcement available, and they aren't enough as context fills. Fowler / Thoughtworks also call this "guides" in a broader engineering sense.
  - Sources: Böckeler §1.3, `superpowers.md` (hard-gate), `bmad-method.md` (defensive instructions)

**4. Sensors — feedback controls (Böckeler's term).** Post-action observers *"optimised for LLM consumption."* Custom linters whose error messages include self-correction instructions; test runners with LLM-readable output; evaluator/reviewer agents (BMAD's Blind Hunter pattern — reviewer sees only the diff, zero context). Cherny: 2–3× quality lift from verification loops. NLW/ECC's practitioner heuristic *"hooks over prompts for reliability"* sits here too.
  - Sources: Böckeler §1.3, Cherny via `anatomy-of-agent-harness.md`, `bmad-method.md` (Blind Hunter pattern), `everything-claude-code-deep-dive.md:257` (⚠ ECC heuristic, not benchmarked)

**5. Specs as durable artifacts.** Plans and designs that live *outside* the agent's context window. Red Hat's four-pillars formulation: *"A spec says **what** should be built; a skill says **how** to use specific tools, patterns, or processes to build it."* BMAD's progressive artifact chain: briefs → PRDs → architecture → epics → stories. Independent practitioner data (Alex Cloudstar): ~42 min spec+implement ≈ 45 min iterative — same time, noticeably better first pass. Three authors at Google, Red Hat, and a practitioner blog converged on the same thesis within two weeks without cross-reference.
  - Sources: `spec-driven-development-landscape.md`, `bmad-method.md`

**6. Persistent memory.** Cross-session recall. claude-mem (**60k stars**) is the dominant coding-agent memory plugin: injects an ~800-token index rather than dumping history, reclaiming *"94% of the attention budget"* (claim) at *"~10× token savings"*. A crowded surrounding market (Mem0 53k, Graphiti 25k, Letta 22k, Supermemory 22k, Cognee 16k) is itself the evidence that the inner harness doesn't provide memory. Zhou's definition: *"externalized state across time."*
  - Sources: `claude-mem.md`, `externalization-in-llm-agents.md`

**7. Session protocols / lifecycles.** Prescribed sequences the *harness* enforces, not the agent decides. Anthropic + OpenAI best practices codify the eight-step lifecycle — **Orient → Setup → Verify → Select → Implement → Test → Update → Exit** — plus the *"one task per session"* rule. Böckeler's **steering loop** (whenever an issue recurs, improve the guide/sensor so it can't recur) is the meta-protocol above. Zhou calls this layer *"externalized interaction structure."*
  - Sources: `harness-engineering-best-practices-anthropic-openai.md`, Böckeler steering loop §1.3, `externalization-in-llm-agents.md`

**8. Code-base preparation / AI-ready surface.** Keeping the terrain agents work on clean enough that they can work reliably. CodeScene, peer-reviewed: *"AI performs best in code with a Code Health score of 9.5–10.0."* Agents otherwise *"exhibit 'self-harm mode' (writing code they cannot reliably maintain later), and will modify any codebase regardless of its state."* The outer harness isn't only what you configure — it's also what you've already cleaned up.
  - Sources: `codescene-agentic-quality-patterns.md`

**Böckeler's umbrella statement (verbatim):**
> "Engineering a user harness for a coding agent is a specific form of context engineering. The human's job in this is to **steer** the agent by iterating on the harness."

### 3.2 Same thing, different names

The outer harness has been carved up at least six different ways. Most of the carvings overlap:

| Böckeler (guides / sensors / context) | Zhou et al. (externalization) | Red Hat (4 pillars) | Google (SDD tiers) | NLW / community | Böckeler's earlier memo (3 categories) |
|---|---|---|---|---|---|
| context briefings | memory | vibes + specs | spec-first / anchored / as-source | CLAUDE.md / AGENTS.md | context engineering |
| skills library | skills | skills | — | skills / plugins | — |
| guides (feedforward) | protocols | agents | — | system prompts / hooks | architectural constraints |
| sensors (feedback) | (harness unification) | — | — | hooks / verifiers | architectural constraints |
| durable specs | protocols + memory | specs | spec-anchored | specs | — |
| persistent memory | memory | — | — | memory plugins | garbage collection |
| session protocols | protocols | — | — | lifecycle hooks | garbage collection |
| code-base prep | (environmental) | — | — | — | — |

Zhou's framing is the most theoretically clean (three sibling externalizations unified by the harness). Böckeler's is the most useful operationally (guides vs. sensors is a decision rule, not just a taxonomy). Red Hat's collapses most cleanly for stakeholders who don't already know the jargon.

### 3.3 The inner/outer boundary in one sentence

**Inner harness** = what the vendor ships (Section 2's eight components: loop, tools, context, state, verification, guardrails, subagents, extension surfaces).
**Outer harness** = what you build using the vendor's extension surfaces (Section 3's eight components: briefings, skills, guides, sensors, specs, memory, protocols, code-base prep).
The only component that spans the boundary is **(2.8) extension surfaces** — the inner harness's API, and the surface the outer harness plugs into.

---

## Section 4: The case for agent-runner

The inner harness is thinning and commoditizing (§2.3). The outer harness is where developer leverage now lives (§3). But *how* you build the outer harness determines whether that leverage is real or theatrical. Today's dominant approach — prompt-based coordination (Superpowers, BMAD, ECC) — is structurally insufficient. This section makes the argument that the coordination layer must live *outside* the agent's context window, that no existing tool occupies that niche for local developer workflow, and that this is the gap **agent-runner** is built to fill.

### 4.1 The thesis in one line

**The workflow must live outside the agent's context window.** Anything inside is a suggestion the agent can ignore once attention runs out.

Böckeler gestures at this with *"architectural constraints"* in her earlier memo — one of three outer-harness categories — but stops short of saying the constraints need their own runtime. Hu Wei (2026) takes the next step explicitly: *"lift control flow out of the context window into an explicit, static, immutable DAG."* Agent-runner is the concrete instantiation of that move, scoped to the local-CLI developer niche.

### 4.2 Why prompt-based coordination isn't enough — three structural arguments

**A. BMAD's own confession** (from `bmad-method.md`, verbatim) — the sharpest single line in the corpus:
> "BMAD's own workflow files reveal the problem: they're full of defensive instructions ('NEVER skip steps', 'do NOT stop because of milestones', 'Execute ALL steps in exact order') because the agent CAN skip steps — there's no external enforcement. As context windows fill up, these instructions compete with implementation context for attention."

And the companion observation from Codagent's own origin story (README, on why the earlier Agent Skills plugin was clunky):
> "The workflow lived inside the agent's context, which meant it was a suggestion, not a guarantee."

The most popular structured-workflow tool on the market (**43k stars**) shouts at the agent in ALL CAPS because shouting is the only enforcement mechanism a prompt-based framework has.

**B. Hu Wei's three structural weaknesses of ReAct** (from `from-agent-loops-to-structured-graphs.md`, arXiv:2604.11378, April 2026, ~51 pages, 70 systems surveyed):
> "(a) *Implicit dependencies*: 'modify code, then run tests' lives only in the context window; no structural guard against out-of-order execution.
> (b) *Unbounded recovery*: on failure the LLM autonomously decides retry/skip/replan with no contract.
> (c) *Mutable execution plans*: if the LLM revises its plan mid-execution, the original is overwritten in context, making audit impossible."

None of the three is fixable by a better prompt. They're structural properties of the loop. Hu Wei's prescription (*"multi-ready-unit scheduling + deterministic policy + bounded recovery + immutable plan versions"*) places **LangGraph as satisfying none-to-partial** on those four — meaning even the most popular inner-harness framework fails the structural bar.

**C. The error-compounding math** (Pachaar, `anatomy-of-agent-harness.md`): a 10-step process at 99% per-step success = **90.4% end-to-end**. Verification loops repair this, but *only if they actually run every time*. Which requires external enforcement — not an instruction competing with 50,000 tokens of conversation for attention.

(Plus the unbenchmarked practitioner heuristic from the ECC `chief-of-staff` agent docs: *"Hooks over prompts for reliability — LLMs forget instructions ~20% of the time."* ⚠ not a measured statistic; use as design signal, not data.)

### 4.3 Market evidence — the demand is real

**Adoption signals** — millions of installs for tools trying to impose structure through prompts:
- Superpowers (Jesse Vincent / obra): **116k stars** — prompt-based skills
- claude-mem (Alex Newman): **60k stars** — memory/compression plugin
- BMAD-METHOD: **43.1k stars** — prompt-based structured workflows

The demand for structure isn't in question; the mechanism is.

**Harness-only benchmark lifts** (no model change, from `harness-engineering-101-aidb.md` + `anatomy-of-agent-harness.md`):
- Blitzy **66.5%** vs GPT-5.4 **57.7%** on SWE-bench Pro
- LangChain TerminalBench 2.0: outside top 30 → **rank 5**
- LLMCompiler plan-and-execute: **3.6×** over ReAct
- Cherny's verification-loop claim: **2–3×** quality

The harness, not the model, is the lever — and the "harness" in each of those benchmarks is external orchestration, not a longer system prompt.

### 4.4 Why existing tools don't fill the gap

Verbatim survey from `agent-runner/docs/WHY-AGENT-RUNNER.md`:

| Category | Examples | Why they don't fit |
|---|---|---|
| **Cloud orchestrators** | AWS Step Functions, Argo, Kestra, Tekton, Azure Logic Apps, Netflix Conductor, Temporal, Prefect, Airflow | Runtimes execute in containers, cloud functions, or server-managed workers. None spawn `claude --resume <session-id>` as a local process with filesystem + git + terminal access on the developer's machine. |
| **Local task runners** | Taskfile, Just, Make | Taskfile is the closest — but `retry:` retries a single command, not a multi-step body; `vars.sh:` resolves once before any step runs (no mid-pipeline capture); no session management. The honest implementation of a verify-fix loop collapses to a bash script inside a single `cmd:` block. |
| **Agent conversation frameworks** | LangGraph, CrewAI, AutoGen | Orchestrate agent *conversations*, not CLI processes. Require Python runtime + direct API access — meaning **no subscription support** (Anthropic forbids `@anthropic-ai/claude-agent-sdk` products from working with claude.ai logins). |
| **Prompt-based coordination harnesses** | Superpowers, BMAD, ECC | Workflow lives inside the agent's context = suggestion, not guarantee (per §4.2). |
| **Agent-decided orchestration** | Augment Intent's Coordinator agent | Uses an *agent* to reason about task decomposition — exactly the nondeterminism external orchestration is supposed to eliminate. Closed-source, macOS-only, not composable. |

The gap is specific: **local CLI execution + agent session management + loop-until over multi-step bodies + mid-pipeline output capture + subscription-compatibility**. No existing tool combines those five.

### 4.5 What agent-runner is

Local CLI orchestrator in Go. YAML workflows. Treats coding agents (Claude Code, Codex, Gemini CLI) as CLI binaries to spawn — inheriting the user's existing subscription auth, no API keys required.

Five capabilities no existing tool combines (from `WHY-AGENT-RUNNER.md`):

1. **Agent session management.** `session: new | resume | inherit` as a first-class workflow concept. `inherit` walks up the parent-context chain across sub-workflow boundaries, so a verification sub-workflow can fix code using the same session that wrote it — preserving full context the agent built while writing.
2. **Mode triality.** `mode: interactive | headless | shell` coexist in a single workflow. Interactive human-in-loop collaboration + autonomous headless steps + raw shell commands, with no awkward handoffs between modes.
3. **Session-aware loops.** Inside a loop, `session: resume` chains naturally: `implement → fix₁ → fix₂ → fix₃`. The agent accumulates context across fix attempts; when the outer loop moves to the next task, `session: new` resets cleanly.
4. **Prompt-based steps.** Natural-language prompt is the unit of work, positioned at the highest-attention position in the agent's input. Prompts can invoke skills (`/codagent:propose`), interpolate parameters, and receive engine-enriched context from the OpenSpec schema.
5. **Signal-based interactive advancement.** Interactive steps advance when the agent writes `.agent-runner-signal` (via a `/continue` skill). Human works with the agent as long as needed, then signals "done."

**Design principle** (quoting `WHY-AGENT-RUNNER.md` directly):
> "Agent Runner's primitives for loops, conditionals, output capture, and sub-workflows are **not novel**. They're borrowed from well-established patterns… What's novel is the **runtime layer beneath those primitives**: session management, mode triality, prompt delivery, and signal-based advancement."

Loop-until from Logic Apps + Conductor. For-each from Kestra + CNCF Serverless Workflow. Output capture from Argo. Sub-workflows from Kestra. Conditional execution from Argo + GitHub Actions. The workflow layer is proven and boring by design — the novelty lives underneath.

### 4.6 Market-gap claim (from the competitive analysis)

From `internal-docs/competative-analysis.md`, Agent Runner + Agent Validator are the only standalone FOSS tools addressing three confirmed market gaps:

- **Pattern 7 (Plan-Execute Separation)** in the local-CLI niche — gh-aw covers CI-bound, MetaGPT/ChatDev cover code-embedded, Augment Intent covers app-embedded. Nothing covers declarative, local, agent-external orchestration.
- **Pattern 10 (Verify-Fix Feedback Loop)** — *"No established standalone FOSS tool exists. Every team builds custom verify-fix logic."* Agent Validator fills the gap; agent-runner wraps it in retry loops.
- **Pattern 12 (Durable Session Primitives)** — *"No established standalone FOSS tool exists. Every example pair built custom session management from scratch."* Agent Runner provides `new | resume | inherit` declaratively.

### 4.7 The positioning claim

Agent-runner sits between two approaches that don't work for this niche:

- **Too loose:** prompt-based coordination harnesses (workflow = suggestion, per §4.2).
- **Wrong abstraction:** generic workflow engines (not built for CLI processes that happen to be agents, per §4.4).

If Böckeler's outer-harness thesis is right — and Section 3 argues it is — then the outer harness needs a coordination-and-control layer that *lives in its own runtime*, not in the agent's. Agent-runner is that runtime.

The one-line version, borrowed from Codagent's README:
> "Agents will reliably do the right thing if you make the right thing the only option."

A prompt says *please do the right thing*. A workflow outside the agent says *this is the only thing that will happen*. That's the whole argument.

---

## Section 5: Numeric data bank

| # | Value | Claim | Source file |
|---|---|---|---|
| 1 | 5 in 6 months | Manus harness rebuilds, each removed complexity | anatomy-of-agent-harness.md |
| 2 | 10 × 99% = 90.4% | Error-compounding end-to-end | anatomy-of-agent-harness.md |
| 3 | 30%+ | "Lost in the Middle" degradation | anatomy-of-agent-harness.md |
| 4 | 3.6x | LLMCompiler plan-and-execute over ReAct | anatomy-of-agent-harness.md |
| 5 | 66.5% vs 57.7% | Blitzy vs GPT-5.4 on SWE-bench Pro (harness-only delta) | harness-engineering-101-aidb.md |
| 6 | top 30 → rank 5 | LangChain TerminalBench 2.0 harness-only jump | anatomy-of-agent-harness.md |
| 7 | 76.4% | LangChain LLM-optimized harness pass rate | anatomy-of-agent-harness.md |
| 8 | 2-3x | Cherny's claim for verification loop quality uplift | anatomy-of-agent-harness.md |
| 9 | 4.0 → 1.0 | MetaGPT ablation: removing upstream roles | comparison-loops.md |
| 10 | 116k stars | Superpowers (thin prompt-based harness) | superpowers.md |
| 11 | 43.1k stars | BMAD-METHOD (structured-workflow harness) | bmad-method.md |
| 12 | 60k stars | claude-mem (memory plugin) | claude-mem.md |
| 13 | ~20% | Rate at which LLMs forget prompt instructions ⚠ *practitioner heuristic from ECC `chief-of-staff` agent doc — not a benchmark* | everything-claude-code-deep-dive.md:257 |
| 14 | ~94% | Attention budget wasted on irrelevant past context (claude-mem claim) | claude-mem.md |
| 15 | ~10x | claude-mem token savings vs bulk injection | claude-mem.md |
| 16 | 4,456 → 682 | Augment Intent context curation ratio (~15% retention) | augment-intent.md |
| 17 | Nov 2025 | Willison's reliability inflection point (GPT 5.1, Opus 4.5) | willison-agentic-engineering-lennys-podcast.md |
| 18 | <5 people | StrongDM dark-factory team size | willison-agentic-engineering-lennys-podcast.md |
| 19 | 95% | Share of Willison's code written from phone | willison-agentic-engineering-lennys-podcast.md |
| 20 | 41% / 90%+ / 19% | Uncited Google claims: complexity increase / security vulns / engineer slowdown | spec-driven-development-landscape.md (⚠ verify before citing) |
| 21 | 80% / 70% / 60% | ECC cost reductions: Haiku subagents / thinking cap / Sonnet-not-Opus | everything-claude-code-deep-dive.md |
| 22 | 70 systems | Hu Wei "Graph Harness" paper survey scope | from-agent-loops-to-structured-graphs.md |
| 23 | 21 authors | Zhou et al. externalization paper | externalization-in-llm-agents.md |
