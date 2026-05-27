---
name: deploy
description: Build HTML and deploy to GitHub Pages. Usage: /deploy [presentation-name]
argument-hint: "[presentation-name]"
---

# Deploy

Build a presentation and publish it to GitHub Pages. Optionally also publish the PDF.

Built artifacts are committed to `public/{human-readable-name}/` on main. GitHub Actions deploys to Pages.

## Input

`$ARGUMENTS` is an optional presentation identifier: opaque ID (`p002`) or human-readable name (`k8s-backendai`).

## Resolve presentation

1. If an identifier is given, check if it matches a directory in `presentations/` directly (opaque ID). If not, read `presentations/index.md` and find the matching entry by substring.
2. If not given, use the presentation from earlier in this conversation.
3. If neither, ask the user.

## Workflow

1. Resolve the presentation directory. Look up the human-readable name from `presentations/index.md` for the public directory name.
2. Run `/build html` for that presentation.
3. Copy the built HTML:
   ```
   mkdir -p public/{human-readable-name}
   cp presentations/{id}/output/slides.html public/{human-readable-name}/index.html
   ```
4. Ask the user if they also want to publish the PDF. If yes:
   - Run `/build pdf` if not already built.
   - `cp presentations/{id}/output/slides.pdf public/{human-readable-name}/slides.pdf`
5. Regenerate the index page: scan all directories in `public/` matching `20*/`, update `public/index.html` with links to each.
6. Commit:
   ```
   git add public/
   git commit -m "deploy: {human-readable-name}"
   ```
7. Push to origin: `git push`
8. Report the Pages URL.
