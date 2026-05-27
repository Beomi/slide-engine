# Slide Engine

**Slides:** https://hhoikoo.github.io/slide-engine/

Marp-based slide build system with theme support and Claude Code integration. Presentation source content is git-crypt encrypted; only explicitly deployed HTML is public.

## Setup

```bash
make setup        # npm install
git-crypt unlock  # decrypt presentations/ (requires GPG key)
```

## Build

```bash
make html DIR=presentations/{name} THEME=bai-flat
make pdf  DIR=presentations/{name} THEME=bai-flat
```

## Structure

```
presentations/    Slide source content (git-crypt encrypted)
public/           Deployed HTML (GitHub Pages)
engine/           Marp config and build scripts
themes/bai-flat/  Theme CSS and assets
docs/guide.md     Layout class reference
```

## Presentation layout

```
presentations/{name}/
  sections/
    00-frontmatter.md   YAML frontmatter (theme, header, etc.)
    01-title.md         Title slide
    02-content.md       Content slides (separated by ---)
    ...
  research/             Research docs
  images/               Figures and generated images
  synopsis.md           Topic and structure outline
```

See `docs/guide.md` for available slide layout classes.
