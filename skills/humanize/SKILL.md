---
name: humanize
description: "Rewrites text and code comments to read like a person wrote them instead of a model: kills AI tells (throat-clearing openers, rule-of-three lists, hollow adjectives, em-dash overuse, uniform sentence rhythm), while preserving every fact, code block, and technical detail exactly. Two targets — prose (docs, PR descriptions, chat replies) and code comments (strip redundant WHAT comments, keep only non-obvious WHY). Triggers: humanize this, sound more human, less AI-generated, remove AI tics, sounds robotic, de-AI-ify, sounds like ChatGPT, clean up these comments."
---

# Humanize — strip the AI accent from text and code

This skill rewrites already-correct content so it reads like a person wrote it. It never changes meaning, facts, code behavior, or technical accuracy — it only changes surface style: word choice, rhythm, structure, and what gets said out loud versus left implicit.

## Two targets

1. **Prose** — chat replies, docs, PR descriptions, commit bodies, comments in tickets. The full rewrite pass below applies.
2. **Code comments** — same rewrite rules, plus one extra pass: delete comments that restate what the code already says, keep only comments that explain a non-obvious WHY (a constraint, a workaround, a trap). See [Code comments](#code-comments) below.

Ask which target if it isn't obvious from context — rewriting a paragraph and cleaning up comments in a diff are different jobs.

## The AI-tell checklist

Run every draft against this list before calling it done. Each one is a pattern that reads as machine-generated because a person under time pressure doesn't reach for it by default.

| Tell | Example | Fix |
|---|---|---|
| Throat-clearing opener | "Great question! Let's dive into...", "Certainly, I'd be happy to help with that." | Start with the answer. |
| Rule of three | "robust, scalable, and maintainable" | Say the one thing that's actually true, or list real distinct properties — not a rhythmic triad. |
| Hollow intensifiers | "This is a crucial and powerful feature that seamlessly..." | Cut the adjective, or replace with the concrete fact it's standing in for. |
| Uniform sentence rhythm | Every sentence 15-20 words, same subject-verb-object shape, back to back. | Vary length. Some sentences are five words. Some run on. |
| Over-signposted structure | Headers/bullets for a three-sentence idea; "Firstly... Secondly... In conclusion..." | Let prose be prose when the content doesn't need a list. |
| Redundant restatement | Answering a question by first repeating the question back. | Skip straight to the content. |
| Hedge stacking | "It's worth noting that this could potentially, in some cases, possibly..." | Pick one hedge or none. State the actual confidence level. |
| Closing summary nobody asked for | "In summary, we've covered X, Y, and Z today." | Just stop when the point is made. |
| Em dash as default punctuation | Three or more em dashes in one paragraph doing the job of commas/periods. | Use periods, commas, or parentheses; keep em dash for the one place it earns its keep. |
| Marketing verbs | "leverage," "utilize," "facilitate," "seamlessly integrate" | "use," "help," "connect." |

None of these are wrong in isolation — a human writes "certainly" too. The tell is *density*: several of these stacked in one short passage.

## What never changes

- Facts, numbers, code, commands, error strings, file paths — verbatim.
- Meaning and scope of claims (don't add confidence or hedges that weren't there).
- Technical correctness of any code touched.
- Security warnings, irreversible-action confirmations, and multi-step instructions stay in plain, unambiguous prose even if that means fewer stylistic changes — clarity wins over naturalness there.

## Levels

| Level | Scope |
|---|---|
| `light` | Strip only the loudest tells: throat-clearing openers, closing summaries, obvious hedge-stacking. Leave structure and rhythm alone. |
| `full` (default) | Everything in `light`, plus rule-of-three, hollow intensifiers, marketing verbs, over-signposting, and a rhythm pass (vary sentence length). |
| `aggressive` | Everything in `full`, plus restructuring: convert list-heavy answers to prose where a list isn't earning its place, cut any sentence that doesn't add new information. |

## Code comments

Separate pass, run on comments only — never touches code logic:

1. **Delete WHAT comments.** If the comment restates what the next line does and a reader with basic language knowledge doesn't need it, delete it. `// increment counter` above `i++` is noise.
2. **Keep WHY comments.** A comment explaining a non-obvious constraint, a workaround for a specific bug, or behavior that would surprise a reader stays — and gets the same prose-tell pass as everything else (short, no hedging, no rule-of-three).
3. **No comment-per-line habit.** A block of code with a comment above every single line is itself an AI tell. If most lines need one, the code needs better names instead — say so rather than rewriting the comments.
4. **Match the surrounding file's comment density and voice.** Don't introduce a heavily-commented style into a file that has almost none, or vice versa.

## How to invoke

Fires on natural-language requests ("make this sound more human", "clean up these comments") without needing a slash command. When a Claude Code project has this skill installed, `/humanize [level] [target]` invokes it explicitly:

```
/humanize                  # full level, prose target, on the current draft
/humanize light
/humanize aggressive code  # aggressive level, code-comment target
```

## What this skill is not

It doesn't add personality, jokes, or a different tone than what's already there — it removes machine tells from writing that's already correct, not perform a persona. It also isn't a grammar or style checker: input that's already clean and human-sounding gets returned unchanged, not padded with unnecessary edits to justify the pass.
