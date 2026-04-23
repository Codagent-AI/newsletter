# Newsletter

Harness engineering patterns, case studies, and lessons. Published as a newsletter covering the discipline of building reliable systems around AI coding agents.

## Contents

- `template.md` — Issue format. Every new issue follows this structure.
- `1-welcome.md` — Issue #1 (published). The three-loop model and case studies from OpenAI, MetaGPT, and Superpowers.
- `2.md` — Issue #2 (in progress). Working draft following the template structure.

## Writing Guidelines

All content must align with the positioning and narrative in `../README.md`. Key constraints:

- **Voice**: First-person, practitioner perspective. Simple, direct, almost colloquial. When editing, preserve the human author's tone and phrasing — even if it seems quirky. Polish and improve; don't rewrite.
- **Thesis**: The answer isn't a better model — it's a better harness. Every issue reinforces this.
- **Format**: Follow `template.md` exactly. Punchy title, evocative hook, roundup of three items, analysis piece, a take, Codagent update.
- **Roundup items**: 2-3 sentences each. What it is, why it matters, what's interesting. No filler phrases like "this is a great read."
- **Analysis section**: Take a real position. Not a summary — a take. Use concrete examples.
- **Codagent Update**: Specific over vague. "Agent Runner now supports X" beats "we're making progress."

## Voice — Detailed Guidelines

When drafting or editing newsletter content, watch for two main problems.

### Problem 1: Unnecessary restatements

Short sentences or transitions that restate what the surrounding text already says. They sound like insight but add nothing. Cut them or fold the point into the preceding sentence.

**Examples — sentences to kill:**

| Sentence | Why it's bad |
|---|---|
| "But the trajectory is clear." | The evidence you just listed already made the trajectory clear. The reader doesn't need you to announce that. |
| "The case is getting stronger every day." (standalone) | Same — just restating that the evidence is compelling. |
| "The parts of code review that are mechanical have been automated." | You just listed maintainability, standards, bugs — all mechanical, all automated. This sentence tells the reader what you already showed them. |
| "The math is simple." | The math is about to follow. Let it speak for itself. |

**Exception — short punchy sentences that set up what follows are good:**

| Sentence | Why it works |
|---|---|
| "This isn't hypothetical." | Makes a claim the reader hasn't seen proven yet and dares them to doubt it before the evidence lands. |
| "I think they're both wrong." | Sets up the argument. You haven't shown why yet. |
| "The queue is breaking." | Punchy conclusion to a specific claim, not a vague restatement. |

**The test:** does the sentence look *backward* (summarizing what you just said) or *forward* (setting up what's next)? Kill the backward ones, keep the forward ones.

### Problem 2: Unnecessarily complex phrases

AI defaults to formal, textbook-style language. The voice here is colloquial. When a simple word works, use it.

**Examples — before and after:**

| AI version | Correct version |
|---|---|
| "And there's an economic misallocation problem." | "And there's a waste problem." |
| "The failure mode here isn't hypothetical." | "This isn't hypothetical." |
| "For correctness verification, deterministic tools and AI reviewers are systematically better than human eyes" | "For catching bugs, tools are just better" |
| "this is exactly where AI code review tools excel, systematically and consistently" | "this is where AI code review tools shine" |
| "But the mechanical verification still needs to happen inside the loop too: catching bugs, enforcing standards, checking for security holes." | "But someone still has to catch bugs, enforce standards, check for security holes." |
| "at a pace and consistency that humans can't match" | Cut entirely — redundant |
| "Here's the critical point:" | Just say the thing. |
| "Think about what a reviewer actually checks." | "What does a reviewer actually check?" |
| "Queuing theory says: when arrival rate exceeds service rate, the queue grows without bound." | Cut — textbook narration |

**Patterns to watch for:**
- **Throat-clearing openers:** "Here's the critical point", "The math is simple", "Think about what..."
- **Textbook labels:** "economic misallocation problem", "queuing theory says", "correctness verification"
- **Adverb-heavy qualifiers:** "systematically and consistently", "at a pace and consistency that humans can't match"
- **Imperative-sounding setups:** "Think about X" — prefer just asking the question ("What does X?")

## Naming

Issues are numbered sequentially: `1-welcome.md`, `2.md`, `3.md`, etc. Add a slug when the title is known (e.g., `2-verification.md`).