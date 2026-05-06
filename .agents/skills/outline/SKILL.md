---
name: outline
description: "Creates and reviews notes-only newsletter outlines. Trigger for \"new outline\", \"outline this\", \"turn research into notes\", \"add structured notes\", \"is this outline complete\", \"what's missing\", \"does the story arc work\", or requests about missing evidence, unclear payoff, or draft readiness."
---

# Outline

Create or review structured, notes-only newsletter outlines. Establish story arc, section jobs, evidence placement, open questions, and source notes without writing publishable prose.

This skill is for outline planning and outline review only.

## Discovery

Before creating, editing, or reviewing an outline:

1. Read the target file or user-provided outline.
2. Identify whether it is notes, prose, or mixed.
3. Preserve existing `PAUL:` blocks exactly unless the user explicitly asks for rewrites.
4. Read `template.md` when the outline is meant to follow the newsletter issue format.
5. If useful, inspect nearby finished issues to infer valid arc shapes and section conventions.
6. If the topic is missing, ask what the issue is about before outlining.

If the user provides a topic but not enough context, ask only for the missing blocker: intended angle, research mode, or must-include sources/examples/personal experience.

If the user asks for research, gather research first, label sources, then convert it to notes. Do not draft.

If the user asks whether an outline is complete, review what exists before suggesting additions. Do not treat missing `template.md` sections as wrong when the outline intentionally covers only the analysis piece.

## Output Rules

Everything produced by this skill must be visually marked as notes. Use blockquoted bullets, checklists, compact tables, and labels. Avoid unmarked prose paragraphs.

Default note marker:

```markdown
> - JOB:
> - ARC:
```

For longer rough draft prose by Paul, use one blockquote with a single `PAUL:` prefix and preserve the prose as paragraphs:

```markdown
## My Workflow

> PAUL:
>
> "Make it work, make it right, make it fast" - Kent Beck.
>
> As I wrote about before, it starts with SDD.
```

Never include a sentence intended to be copied into the newsletter unless it is inside a `PAUL:` block or labeled `RAW MATERIAL:` / `QUOTE CANDIDATE:` with source provenance.

## Labels

Use these labels:

- `JOB:` section purpose.
- `ARC:` story role or narrative move.
- `SEGUE:` bridge from the previous section into this section.
- `STRUCTURE:` section order, placement, transition, or outline shape.
- `CLAIM:` argument or idea the section may carry.
- `EVIDENCE:` supporting fact, case, quote candidate, data point, or example.
- `SOURCE:` link, file path, author, date, or provenance.
- `EXAMPLES:` concrete tools, products, cases, or references.
- `MATRIX:` compact 2x2 or comparison notes.
- `INITIAL THOUGHT:` rough planning note or unsourced working thought, not draft prose.
- `KEEP LIGHT:` completeness item to compress.
- `RISK:` structural, sourcing, overclaiming, or pacing issue.
- `QUESTION:` unresolved decision.
- `PAYOFF:` where a promise resolves or what a section sets up.
- `DRAFT SLOT:` prose to write later.
- `PAUL:` rough draft prose manually written by Paul. Preserve it; do not invent, rewrite, or remove it unless explicitly asked.

Order labels to make the story arc visible:

1. `JOB:`
2. `ARC:`
3. `SEGUE:`
4. `STRUCTURE:`
5. `CLAIM:`
6. `EVIDENCE:` / `SOURCE:` / `EXAMPLES:` / `MATRIX:`
7. `INITIAL THOUGHT:`
8. `KEEP LIGHT:`
9. `RISK:`
10. `QUESTION:`
11. `PAYOFF:`
12. `PAUL:`
13. `DRAFT SLOT:`

Do not force every label into every section. Keep evidence attached to the claim it supports. If `PAUL:` prose already exists, keep it under the relevant section and add planning labels only when useful.

## Outline Shape

Follow `template.md` unless the user asks for a different structure. Use this compact shape:

```markdown
# [DRAFT SLOT: title]

> - JOB:
> - ARC:
> - SEGUE:
> - STRUCTURE:
> - QUESTION:
> - DRAFT SLOT:

## [DRAFT SLOT: hook/subtitle]

> - JOB: Frame the tension, problem, or concrete observation.
> - ARC:
> - SEGUE:
> - STRUCTURE:
> - EVIDENCE:
> - RISK: Avoid table-of-contents intro.
> - PAYOFF:
> - DRAFT SLOT:

## Roundup

> - JOB: Three items worth attention.
> - ITEM:
>   - SOURCE:
>   - WHY IT MATTERS:
>   - INTERESTING DETAIL:
>   - DRAFT SLOT:

## Context Section: [DRAFT SLOT]

> - JOB: Explain framework/background/vocabulary before the argument lands.
> - ARC: Context-building section before the main turn.
> - SEGUE:
> - STRUCTURE: Place before the argument; name the later claim this enables.
> - CLAIM: Descriptive claim, not the main thesis yet.
> - EVIDENCE:
> - SOURCE:
> - KEEP LIGHT:
> - RISK:
> - PAYOFF:
> - DRAFT SLOT:

## Analysis: [DRAFT SLOT]

> - JOB: Make the main argument, not a summary.
> - ARC:
>   - Setup:
>   - Evidence:
>   - Turn:
>   - Payoff:
> - SEGUE:
> - STRUCTURE:
> - CLAIM:
> - EVIDENCE:
> - SOURCE:
> - EXAMPLES:
> - KEEP LIGHT:
> - RISK:
> - QUESTION:
> - PAYOFF:
> - DRAFT SLOT:

## My Take

> - JOB: Honest, defensible position.
> - CLAIM:
> - CONTRARY VIEW:
> - SUPPORT:
> - RISK:
> - DRAFT SLOT:

## Codagent Update

> - JOB: Specific product/project update.
> - UPDATE:
> - WHY IT MATTERS:
> - TIE-BACK TO ISSUE THEME:
> - RISK:
> - DRAFT SLOT:
```

## Story Arc

Before producing notes, identify:

- Central thesis, tension, or question.
- Sequence of claims, one bullet per major section.
- Each section's job.
- Transitions between sections.
- Payoff for the promise made by the intro.

Valid arcs include:

- `FRAME -> ARGUE SIDE A -> ARGUE SIDE B -> SYNTHESIS -> TIE-BACK`
- `FRAME -> EXPLAIN CONTEXT -> EXPLAIN CONTEXT -> ARGUMENT/TURN -> PAYOFF`
- `FRAME -> CASE STUDY -> GENERALIZE -> IMPLICATIONS -> TIE-BACK`

Do not force the argument to arrive early. Explanatory middle sections are valid when they are load-bearing context for the later turn.

## Structural Checks

Use these checks when creating or reviewing notes:

- **Framing tension:** intro creates a tension, question, anecdote, concrete observation, or dialectic; not a table of contents.
- **Concrete anchors:** major claims have projects, papers, tools, practitioners, incidents, user experiences, or personal workflow details.
- **Explanatory middle:** context sections explain what the reader needs for the later argument; they point toward the turn and compress completeness items.
- **Scoping move:** strong claims include a boundary note so the eventual draft does not overstate the point.
- **Callbacks:** later sections can echo earlier frames/examples without full recap.
- **Codagent tie-back:** product update connects to the issue theme when possible.
- **Progressive specificity:** issue moves from frame to evidence to concrete implications.
- **Evidence placement:** save full evidence for the section where it serves the argument.
- **Taxonomy risk:** equal-weight lists need `KEEP LIGHT:` and a clear later `PAYOFF:`.
- **Weak links:** argument chains should not put anecdotal/speculative points beside structural or well-sourced points without noting support level.
- **Two endings:** avoid one intellectual ending plus a tacked-on product/update ending.

## Completeness Review

Use when asked "is this outline complete", "what's missing", "does this have enough to draft", "does the story arc work", or "what else should I research".

Answer in this format:

```markdown
## Completeness Review

> - STATUS: Draftable / partially draftable / not ready.
> - ARC:
>   - Works:
>   - Missing:
> - STRUCTURE:
>   - Works:
>   - Missing:
> - CLAIMS:
>   - Strong:
>   - Weak / unclear:
> - EVIDENCE:
>   - Covered:
>   - Needs support:
> - PAYOFF:
>   - Present:
>   - Missing:
> - RISKS:
> - QUESTIONS:
> - SUGGESTED ADDITIONS:
>   - JOB:
>   - ARC:
>   - CLAIM:
>   - EVIDENCE:
>   - SOURCE:
>   - PAYOFF:
```

Suggest additions with outline labels only. Do not write the missing section.

## Research Notes

When research is part of the task, keep research separate from outline structure:

```markdown
## Research Notes

> - SOURCE:
>   - TYPE:
>   - DATE:
>   - AUTHOR / ORG:
>   - KEY FACTS:
>   - QUOTE CANDIDATES:
>   - HOW IT COULD BE USED:
>   - CONFIDENCE:
>   - OPEN QUESTIONS:
```

Then add outline notes using the relevant structure above.

## Out of Scope

This skill does not:

- Write publishable newsletter prose.
- Polish voice or sentences; use project writing guidelines for drafting/editing.
- Provide full structural critique of completed prose drafts; this skill reviews outlines and notes.
- Invent a thesis when topic/context is missing.
- Remove or rewrite `PAUL:` prose unless explicitly asked.
- Treat this newsletter as always being about AI coding agents unless the user or sources say so.
