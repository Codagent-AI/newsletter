---
name: write
description: "Rewrites one newsletter outline section into publishable prose. Trigger for \"write this section\", \"turn these notes into prose\", \"draft this section from notes\", or requests to expand one labeled notes section. Ask which section if unspecified."
---

# Write

Turn one notes-only newsletter outline section into prose. Use this after that section is complete enough to draft.

## Workflow

Before writing:

1. Read the target notes or user-provided outline.
2. Read `CLAUDE.md` for project voice, structure, and editing rules.
3. Identify the single section to write.
4. Draft that section from its notes, preserving sources and unresolved questions.
5. Integrate any `PAUL:` prose with only minor edits.
6. Run a humanizer pass using the required writing-style reference.

If the user does not specify a section, ask which section to write before drafting. Do not draft multiple sections in one pass unless the user explicitly asks for multiple named sections.

If the topic, intended angle, or chosen section scope is unclear, ask before writing.

Follow the newsletter voice in `CLAUDE.md`: first-person practitioner voice, simple/direct language, no filler, no generic AI polish.

## Writing From Notes

Write prose only for the selected section. Use other sections only for continuity.

Use `JOB:` and `ARC:` to understand the section's purpose. Turn `CLAIM:` plus `EVIDENCE:` into paragraphs. Use `STRUCTURE:` and `PAYOFF:` for order and transitions. Compress `KEEP LIGHT:` material.

Preserve source links and attributions. Resolve `QUESTION:` only when the answer is present in the notes or source material; otherwise leave it as a note. Do not invent unsupported claims.

## Paul-Written Prose

`PAUL:` blocks are rough prose written by Paul in his own voice. Do not rewrite them wholesale.

Allowed edits:

- Fix typos, punctuation, obvious grammar issues, broken links, and small wording glitches.
- Make minor rewording when a sentence is rough enough to be confusing.
- Keep Paul's phrasing when it is merely rough, quirky, blunt, or incomplete but understandable.

Do not turn `PAUL:` prose into generic clean prose.

## Humanizer Requirement

Always use the `humanizer` skill before finalizing prose: "Use my writing style from `3.md` as a reference." Use humanizer as a cleanup pass, not as a license to overwrite Paul's voice.

For `PAUL:` prose, only make minor edits, because this text was already written by Paul in his voice. For note-based prose, use the humanizer to make it sound like Paul's voice. The goal is to preserve Paul's unique voice and phrasing, even if it is unorthodox or quirky, while still making it readable and engaging.

## Output Format

When editing a file, replace only the selected section with normal prose and leave unresolved items as blockquoted notes:

```markdown
Draft paragraph here.

Another draft paragraph here.

> - QUESTION: unresolved source detail.
> - DRAFT SLOT: remaining paragraph to write later.
```

When responding in chat, provide only the requested drafted section unless the user asks for explanation.

## Out of Scope

This skill does not:

- Create a new outline from scratch; use `outline`.
- Review outline completeness; use `outline`.
- Write a whole issue in one pass unless the user explicitly names multiple sections.
- Rewrite `PAUL:` prose beyond minor edits unless explicitly asked.
- Invent missing research, sources, or links.
- Perform a broad structural critique of a completed prose draft.
