---
name: build
description: Build presentation slides to HTML or PDF. Usage: /build [format] [presentation-name]
argument-hint: "[html|pdf|html-wl|pdf-wl] [presentation-name]"
---

# Build

Compile Marp slides to HTML or PDF.

## Input

`$ARGUMENTS` may contain:
- A format: `html` (default), `pdf`, `html-wl`, or `pdf-wl`
- A presentation identifier: opaque ID (`p002`) or human-readable name (`k8s-backendai`)
- Both, in any order

## Resolve presentation

1. If an identifier is given, check if it matches a directory in `presentations/` directly (opaque ID). If not, read `presentations/index.md` and find the matching entry by substring.
2. If not given, use the presentation from earlier in this conversation.
3. If neither, ask the user.

## Workflow

1. Resolve the presentation directory.
2. Map the format to a Makefile target: `html`, `pdf`, `html-wl`, or `pdf-wl`.
3. Run: `make $TARGET DIR=presentations/{id} THEME="${THEME:-bai-flat}"`
4. If build succeeds, report the output path.
5. If building HTML (html or html-wl), open it: `open presentations/{id}/output/slides.html` (or `slides-wl.html` for whitelabel).
6. If build fails, read the error output and suggest fixes.
