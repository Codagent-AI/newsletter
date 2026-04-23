---
description: "Review a newsletter draft for structural clarity and cohesiveness: story arc, section transitions, redundancy across sections, and pacing. Complements the sentence-level voice guidelines in CLAUDE.md."
---

# Writing Structure Feedback

Review a newsletter draft for structural problems — the kind that survive sentence-level editing because they live in the relationships *between* sections, not within them.

This skill covers architecture. CLAUDE.md covers voice and sentence-level problems. Don't duplicate that work here — assume the voice pass happens separately.

## How to run the review

Read the full draft. Before writing any feedback, identify:

1. **The story arc.** What's the piece's thesis? What sequence of claims does it build? Write this down for yourself in one sentence per section — if you can't, that section may not have a clear role in the arc.

2. **Each section's job.** Every section should do exactly one of: introduce the problem, provide evidence, make the argument, or land the conclusion. If a section does two of these, it may need splitting. If it does none clearly, it may be filler.

3. **The transitions.** Read the last paragraph of each section and the first paragraph of the next as a pair. Does the second follow from the first? Or does the reader have to bridge a gap?

Then write feedback organized by issue type, not by section order. Group related problems together.

## Structural patterns that work (protect these during review)

These patterns recur across the strongest published issues. When you see them in a draft, call them out in "What works" — and never suggest edits that would break them.

### The dialectical intro (issue #3 model)

The strongest intros create a *framing tension* the rest of the piece resolves. Issue #3 opens with two camps ("review everything" vs. "agents do everything"), gives each one vivid paragraph with names and links, then describes a middle path — all in four paragraphs. The reader gets a mental map for the entire piece before they start reading.

Contrast with a weaker pattern: "here's what we'll cover" intros that explain the structure instead of creating tension. If an intro reads like a table of contents, flag it. A good intro makes the reader feel the *problem*, not the *outline*.

What to check:
- Does the intro create a tension, question, or frame the reader carries into the piece?
- Can the reader describe "what this piece is about" after the intro, without knowing the section headers?
- Does the final section resolve or pay off what the intro set up?

### Case studies anchor abstractions

Issue #1 maps one case study per loop (OpenAI → outer, MetaGPT → orchestration, Superpowers → inner). Issue #2 threads Red Hat, Cloudstar, and Superpowers throughout the SDD argument. Issue #3 uses Gas Town as the colony exemplar. When a section lists abstract components without grounding them in a concrete story or source, it reads encyclopedic. The case study is what turns a taxonomy into an argument.

What to check:
- Does each major claim have at least one concrete anchor (a project, a paper, a practitioner's experience)?
- If a section has N items in a list, are the items that matter to the thesis anchored with more detail than the completeness items?

### The "to be clear" scoping move

A voice signature: after taking a strong position, the author draws a precise boundary around it. "I'm not saying human review is without value" (#3). "I'm not arguing against using multiple agents" (#3). This prevents straw-manning of the author's own position and signals intellectual honesty. Protect this during edits — never cut a scoping clarification to save words.

### Callbacks instead of restatements

Issue #2 uses "Sound familiar?" to connect SDD-at-scale back to Waterfall without restating the argument. The reader does the connective work. This is the efficient alternative to the backward-looking summary sentences flagged as a problem below. When a transition needs to reference an earlier point, a one-phrase callback is almost always better than a restated paragraph.

### The Codagent Update ties back

Every published issue connects the product update to the analysis theme. "I'm building what's missing" (#1). "This is what SDD looks like when tooling stays out of the way" (#2). "Agent Validator catches what tools can; the human catches the rest" (#3). If the Codagent Update feels disconnected from the analysis, that's a structural problem.

### Progressive specificity

The pieces move from frame (high-level) → evidence (mid-level) → specific claims and products (ground-level). When this gradient is smooth, the piece flows. A reset back to high-level abstraction after getting specific feels like the arc looped back on itself. Flag any section that jumps up the abstraction ladder after the piece has already moved down it.

---

## What to look for (problems)

### 1. Evidence deployed in the wrong section

The most common structural problem in long-form argument pieces. Evidence shows up in a descriptive/taxonomic section where it illustrates a category, then shows up again in the argumentative section where it actually matters. The second occurrence feels like a rerun.

**The fix:** Evidence should live where it serves the *argument*, not the *taxonomy*. In the descriptive section, state the fact briefly. Save the full quote, the dramatic framing, the "they shout because shouting is all they have" for the section where it's doing persuasive work.

**Example from issue #5 review:**
> The BMAD/Superpowers "ALL CAPS / HARD-GATE" evidence appears in full in the outer harness section (Guides, item #2) *and* again in "My bet." By the time the reader hits it in "My bet," it's a rerun, not a revelation.
>
> **Fix:** Trim the evidence in the Guides definition to one sentence about the enforcement limitation. Deploy the full quotes in "My bet" where they serve the argument.

### 2. Encyclopedic sections that stall the arc

A numbered list of N components where each gets equal weight. The reader came for an argument but got a reference manual. The arc stalls because the reader can't tell which items matter to the thesis and which are included for completeness.

**The fix:** Not every item needs a full paragraph. Items central to the thesis get depth. Items included for completeness get one or two sentences. The reader should be able to feel which items the author cares about.

**Example from issue #5 review:**
> The outer harness section catalogs seven components with roughly equal weight. After the inner harness section builds momentum toward "the interesting question is what you layer on top," the reader expects acceleration — instead they get another numbered list. The arc stalls.
>
> **Fix:** Give depth to guides, sensors, and skills (central to the thesis). Compress persistent memory, code-base preparation, and on-the-loop supervision.

### 3. Promises the structure doesn't keep

The intro sets up an expectation — "X, and the piece I think is still missing" — but the payoff arrives much later than expected, or the reader has to traverse too much material before the promise is fulfilled.

**The fix:** Either move the payoff closer to the promise, or plant a signpost between them that acknowledges the gap is coming. The reader shouldn't have to hold an unresolved thread across 1,500 words of taxonomy.

**Example from issue #5 review:**
> The intro promises "the outer harness, and the piece I think is still missing." The "missing piece" doesn't surface until "My bet" — after 7 inner-harness components and 7 outer-harness components. The reader may forget the promise or lose patience.
>
> **Fix:** Add a bridge sentence at the end of the outer harness section that names the gap before "My bet" argues for it.

### 4. Weak links in a numbered argument chain

When the piece makes argument #1, #2, #3 in sequence, they should escalate or at least hold steady. If one is anecdotal where the others are structural, or speculative where the others are evidence-based, it weakens the chain.

**The fix:** Either strengthen the weak link to match the others, fold its real point into a stronger neighbor, or cut it.

**Example from issue #5 review:**
> Problems #1 and #2 are structural arguments with compounding math. Problem #3 (Opus 4.7's polarized reception) is an anecdote with a speculative parenthetical. It undercuts the rigor of the first two.
>
> **Fix:** The real point — model upgrades break existing prompts, so in-context coordination means every upgrade is a migration — is strong. Lead with that framing instead of the community reception anecdote.

### 5. Backward-looking summary sentences (structural version)

CLAUDE.md covers this at the sentence level. At the structural level, it manifests as a sentence that summarizes the section's argument right before transitioning to the next section. It feels like a conclusion but it's actually dead weight — the reader already understood the point.

**The fix:** Cut the summary sentence. Let the transition do the work.

**Example from issue #5 review:**
> "That's the core of the short." summarizes what the preceding paragraphs just argued. The sentence after it — "Everyone arguing for thinner harnesses is designing for a *hypothetical* future..." — is the actual punch. Cut the setup.

### 6. The intro explains instead of framing

An intro that tells the reader what the piece will cover ("First, we'll look at X. Then Y. Finally Z.") is a table of contents, not a frame. The reader knows the structure but doesn't feel the tension. Compare issue #3's intro — two camps, both wrong, here's the middle path — with a hypothetical: "In this issue, I'll cover code review, multi-agent colonies, and the human's role." Same content, zero pull.

**The fix:** Rewrite the intro around a tension, question, or dialectic. The reader should feel the *problem* after the intro, not the *outline*. The strongest pattern in this newsletter is the dialectical frame: position A, position B, here's what they're both missing. But a concrete anecdote (issue #1's slot machine metaphor) or a provocation also work.

**Signs of the problem:**
- The intro mentions section names or uses "first... then... finally" sequencing
- You could swap the intro for a bullet-point outline and lose nothing
- The intro doesn't create a question the reader carries into the piece

**Example — issue #3 intro (what good looks like):**
> "There are two loud camps in agentic coding right now. I think they're both wrong." Camp one: review everything (the swamp). Camp two: agents do everything (the wasteland). "Somewhere between the swamp and the wasteland, there's a pragmatic path." — In four paragraphs, the reader has the entire piece's frame, a thesis, and a reason to keep reading. Each camp gets one vivid paragraph (names, links, not straw-manned). The synthesis describes the middle path with imagery rather than arguing for it yet.

### 7. Two endings

The piece builds to an intellectual capstone (the argument lands, the thesis is proven) and then tacks on a separate ending (a product announcement, a call to action, a teaser). The reader experiences the piece as ending twice.

**The fix:** Either integrate the second ending into the first (the argument naturally leads to the announcement) or give the second ending enough substance to stand as its own closing section. The worst case is two sentences of teaser after a strong intellectual close.

**Example from issue #5 review:**
> The Zhou et al. paper is a strong capstone — peer-reviewed validation of the thesis. But then the Agent Runner mention ("Releasing soon, stay tuned") feels tacked on.
>
> **Fix:** Integrate: "Their paper describes the gap. I'm building the thing that fills it — Agent Runner, free and open source, releasing [timeframe]." One ending, not two.

## How to structure the feedback

1. **Story arc** — one paragraph summarizing the arc as you see it. This confirms you understood the piece before critiquing it, and surfaces misalignment early if the author intended a different arc.

2. **Issues** — grouped by type (not by section order). For each:
   - Name the problem in a bolded sentence.
   - Show where it occurs with a quote or section reference.
   - Explain *why* it's a problem for the reader (not just that it violates a rule).
   - Suggest a fix.

3. **What works** — briefly note transitions or structural moves that land well. This isn't politeness; it tells the author what to protect during revision.

4. **Pacing note** — if the balance between taxonomy/description and argument/thesis is off, flag it with approximate word counts. "1,800 words of taxonomy before the argument starts" is more useful than "the taxonomy sections are long."

## What NOT to do

- Don't rewrite sections. This is feedback, not a revision.
- Don't flag sentence-level voice issues — that's CLAUDE.md's job.
- Don't suggest adding sections or features the author didn't intend. Critique the structure of what's there.
- Don't soften feedback with "this is really good but..." — state the problem directly. The author asked for structural critique; give it.
- Don't number issues by importance. Group by type, and trust the author to prioritize.
