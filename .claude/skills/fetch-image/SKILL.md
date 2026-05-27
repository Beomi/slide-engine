---
name: fetch-image
description: Download an image URL to images/figures/ in the current presentation. Usage: /fetch-image <url> [filename]
argument-hint: "<url> [filename]"
---

# Fetch Image

Download an image from a URL and save it to the current presentation's `images/figures/` directory.

## Input

`$ARGUMENTS` is `<url> [filename]`:
- First argument: the image URL (required)
- Second argument: optional filename (without path). If omitted, derive from the URL or content-type.

## Resolve presentation

Use the presentation from earlier in this conversation. If none established, ask the user. Accepts opaque ID or human-readable name via `presentations/index.md`.

## Workflow

1. Resolve the presentation directory.
2. Parse URL and optional filename from `$ARGUMENTS`.
3. Create `images/figures/` if it does not exist.
4. Download using `curl -fsSL -o`.
5. Verify the download succeeded.
6. Print the relative markdown reference: `![alt](images/figures/{filename})`
