---
name: export-notes
description: Extract speaker notes from slides.md into a printable document. Usage: /export-notes [presentation-name]
argument-hint: "[presentation-name]"
---

# Export Notes

Extract speaker notes from Marp slides into a standalone printable document.

## Input

`$ARGUMENTS` is an optional presentation identifier: opaque ID (`p002`) or human-readable name.

## Resolve presentation

1. If an identifier is given, check `presentations/` directly or look up via `presentations/index.md`.
2. If not given, use the presentation from earlier in this conversation.
3. If neither, ask the user.

## Workflow

1. Resolve the presentation directory.
2. **Parse section files**: Read each `sections/*.md` file in sort order. For each slide (split on `---`):
   - Extract the slide title (first `#` or `##` heading)
   - Extract speaker notes (content inside `<!-- ... -->` blocks that are NOT directives)
   - Track slide number
3. **Generate output**: Write `output/speaker-notes.md`.
4. **Report**: Print the output path. Note any slides missing speaker notes.
