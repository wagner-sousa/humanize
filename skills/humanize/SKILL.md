---
name: humanize
description: "Takes already-written text and strips the machine accent off it: uniform paragraph rhythm, hollow intensifiers, rule-of-three lists, throat-clearing openers, em-dash strings standing in for real punctuation — the patterns that mark AI-generated writing, without changing a single fact, example, or technical detail. Runs six sequential passes, each building on the last. Two targets: prose (docs, PR descriptions, commit bodies, chat replies) and code comments (delete comments that just restate the next line, keep only the ones explaining a non-obvious WHY). Use this skill whenever someone says the text sounds robotic, reads like ChatGPT, feels too uniform or over-corrected, wants it humanized or de-AI-ified, or asks to clean up comments that read like they were written by a machine."
---

# Humanize — strip the AI accent from text and code

This skill rewrites already-correct content so it reads like a person wrote it. It never changes meaning, facts, code behavior, or technical accuracy — it only changes surface style: word choice, rhythm, structure, and what gets said out loud versus left implicit.

This is a **rewrite pass on existing text**, not a content generator. Feed it a message, doc, or comment block that already exists.

## Two targets

1. **Prose** — chat replies, docs, PR descriptions, commit bodies, comments in tickets. The six passes below apply directly.
2. **Code comments** — same six passes, plus one extra step before Pattern Breaker: see [Code comments](#code-comments).

Ask which target if it not obvious from context — rewriting a paragraph and cleaning up comments in a diff are different jobs.

## What never changes

Before rewriting anything, identify what is load-bearing in the source and hold it fixed through all six passes:

- **Facts, numbers, and code** — task details, before/after values, names cited, dates, commands, error strings, file paths — verbatim. Never invent or drop one to make a sentence flow better. This extends to inferred cause, motive, or backstory: if the source says "enhances security," do not rewrite it as "closes the gaps we had" — the gaps are not in the source.
- **Concrete examples** — every example stays, tied to the same claim it illustrates. Change how it is phrased, never remove it or make it vaguer.
- **Mandatory structural blocks** — if the source has required sections, they must still exist and be identifiable after the rewrite. Soften the language inside a heading; do not delete the heading or merge two mandatory blocks into one.
- **Domain constraints already in force** — if the source avoids certain jargon for its intended reader, that constraint survives the rewrite. Humanizing tone is not license to reintroduce jargon or change the intended reader.
- **Technical correctness** of any code touched, and code behavior itself — this skill only ever rewrites comments, never logic.
- **Security warnings, irreversible-action confirmations, and multi-step instructions** stay in plain, unambiguous prose even if that means fewer stylistic changes — clarity wins over naturalness there.

If a pass below would require breaking one of these to sound more natural, do not do it — find a different way to loosen the sentence instead.

## The six passes

Run these in order, on the full text each time — each pass builds on the output of the previous one, not on the original. Treat this as an editing pipeline, not a checklist to satisfy simultaneously.

**1. Pattern Breaker.** Read the text looking specifically for the structural signature of AI writing: identical paragraph lengths, a bullet list where prose would read more naturally, a topic sentence that restates the heading, rule-of-three lists ("robust, scalable, and maintainable"), transitions like "Additionally" / "Furthermore" / "It is worth noting" doing no real work, throat-clearing openers ("Great question!", "Certainly, I would be happy to..."), unearned closing summaries. Break the symmetry — vary sentence length, cut a transition that is not earning its place, let one paragraph run longer than its neighbors if that is how the content actually breaks.

**2. Credibility Test.** Go sentence by sentence. Anything that sounds over-corrected — grammatically perfect but oddly formal for the context, or so precise it reads like it was calibrated rather than said — gets rewritten plainer. Watch for hollow intensifiers ("crucial," "powerful," "seamlessly") and marketing verbs ("leverage," "utilize," "facilitate") — cut them or replace with the concrete fact they are standing in for. Ask: would a person actually say this out loud to a colleague, or does it read like it was drafted and then polished twice?

**3. Voice Modeler.** Check whether the text reads as a neutral, hedge-everything observer. If every sentence is equally cautious and equally weighted, the writer disappears. Where the source material supports it, let a clear point of view show — this was the right fix, this part was messy, this decision was a judgment call. Small, natural shifts in register between sentences are fine; total uniformity is the tell.

**4. Imperfection Injector.** Perfect text has no story behind it. Where it fits naturally, add the kind of texture someone who actually lived the problem would include — an aside that shows judgment, a sentence that trails into the next thought instead of closing cleanly, an observation stated plainly instead of hedged. This is about *phrasing* texture, not new content: never invent a reason, a prior problem, or a piece of backstory the source did not state, even a plausible-sounding one — that is a fact violation, not a style choice. Do not force this into every paragraph, and do not "fix" it afterward — a slightly rough edge that came from a real observation is the point.

**5. Audio Test.** Read the current draft as if speaking it aloud. Mark every sentence where you would stumble, pause oddly, or where the rhythm breaks from how a person actually talks. Watch specifically for em-dash strings doing the job of commas and periods. Rewrite those specific sentences so they flow as speech — this pass is surgical, only touching the sentences that failed the read-aloud check, not a rewrite of the whole thing again.

**6. Soul Detector.** Final scan, having now seen the text several times over: pick the 2-3 lines that still sound the most artificial — the ones that would make a skeptical reader think "that is a machine sentence." Rewrite each as if explaining it to a close colleague, not presenting it. This is the last pass; if the text already feels human going in, it is fine for this to be a light touch on one or two lines rather than a rewrite.

## Code comments

Run one extra step before the six passes, on comments only — never touches code logic:

1. **Delete WHAT comments.** If the comment restates what the next line does and a reader with basic language knowledge does not need it, delete it. `// increment counter` above `i++` is noise.
2. **Keep WHY comments.** A comment explaining a non-obvious constraint, a workaround for a specific bug, or behavior that would surprise a reader stays — it then goes through the six passes above like any other prose.
3. **No comment-per-line habit.** A block of code with a comment above every single line is itself an AI tell. If most lines need one, the code needs better names instead — say so rather than rewriting the comments.
4. **Match the surrounding file's comment density and voice.** Do not introduce a heavily-commented style into a file that has almost none, or vice versa.

## After the passes

Do a final read against the "what never changes" list above — confirm every fact, example, and mandatory block from the source is still present and still accurate. Then present the rewritten text.

If asked to explain what changed, describe it in terms of the passes above (e.g. "broke the symmetry of the three paragraphs and rewrote two sentences that read as over-formal"), not as a diff.

## How to invoke

Fires on natural-language requests ("make this sound more human", "this reads like AI wrote it", "clean up these comments") in any [agentskills.io](https://agentskills.io)-compatible agent — no command syntax required. Say which target (prose or code comments) if it is not obvious from what you are pointing at.

## What this skill is not

It does not add personality, jokes, or a different tone than what is already there — it removes machine tells from writing that is already correct, it does not perform a persona. It also is not a grammar or style checker: input that is already clean and human-sounding gets returned unchanged, not padded with unnecessary edits to justify the pass.
