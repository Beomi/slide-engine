---
name: inspect
description: Visually inspect slides by capturing screenshots and analyzing them for layout/design issues. Usage: /inspect [slide-number] [presentation-name]
argument-hint: "[slide-number] [presentation-name]"
---

# Inspect Slides

Capture slide screenshots and visually analyze them for layout and design issues.

## Input

`$ARGUMENTS` may contain:
- A slide number (integer) to inspect a single slide. If omitted, inspect all slides.
- A presentation identifier: opaque ID (`p002`) or human-readable name.

## Resolve presentation

1. If an identifier is given, check `presentations/` directly or look up via `presentations/index.md`.
2. If not given, use the presentation from earlier in this conversation.
3. If neither, ask the user.

## Workflow

1. Resolve the presentation directory.
2. **Build first**: Run `/build html` to ensure `slides.md` and `output/.merged-theme.css` exist.
3. **Regenerate screenshots**:
   ```bash
   OUTDIR="presentations/{id}/output/screenshots"
   mkdir -p "$OUTDIR"
   cp "presentations/{id}/slides.md" "$OUTDIR/slides.md"
   ./node_modules/.bin/marp --no-config --images png --image-scale 2 "$OUTDIR/slides.md"
   ```
4. **Read slide images**: Use the Read tool on each PNG file.
5. **Verify slide numbers**: Screenshot filenames (`slides.NNN.png`) are authoritative. Do NOT guess from section file order.
6. **Analyze each slide** for: text overflow, layout issues, empty slides, image sizing, readability, spacing.
7. **Report findings**: slide number, issue, suggested fix referencing the section file.
