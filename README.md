# Slide Engine

**Slides:** https://hhoikoo.github.io/slide-engine/

Marp-based slide build system with theme support and Claude Code integration. Presentation source content is git-crypt encrypted; only explicitly deployed HTML is public.

## Setup

```bash
make setup   # brew: git-crypt + gnupg, npm ci, attempt git-crypt unlock
```

`make setup` installs Homebrew deps, runs `npm ci`, and attempts `git-crypt unlock`. On a fresh machine without the decryption key, it prints instructions for transferring a GPG private key (or the symmetric backup key) from another device. Re-run `make unlock` after the key is in place.

## Privacy

Presentation source content is git-crypt encrypted, but the encryption is meaningless if the topic leaks elsewhere. Commit messages, branch names, PR titles MUST refer to decks by opaque ID (`p007`) only -- never by topic. See `CLAUDE.md` "Privacy (CRITICAL)" for the full rule. `public/` folder names are exempt (intentionally public artifacts).

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
