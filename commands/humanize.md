---
disable-model-invocation: true
description: 'Rewrites the given text (or the assistant''s last draft) to remove AI-generated tells while preserving every fact and technical detail exactly.'
argument-hint: '[level: light|full|aggressive] [target: prose|code] [text or empty = last draft]'
allowed-tools: [
  'Read',
  'Edit',
  'Skill'
]
---

Load the `humanize` skill and apply it to `$ARGUMENTS`.

Parse `$ARGUMENTS` for an optional level (`light`/`full`/`aggressive`, default `full`) and target (`prose`/`code`, default `prose`) at the start, then treat the rest as the content to rewrite. Empty content means: rewrite the assistant's own last message in this conversation.

Run the AI-tell checklist and code-comment rules from the skill exactly as documented — do not invent new rules here. Return only the rewritten content, not a description of what changed, unless the user asked for a diff explanation.
