# Slide Engine

Marp-based presentation generation system with theme support, sectioned slide authoring, and Claude Code integration.

## Layout

```
slide-engine/
├── CLAUDE.md
├── .gitattributes            # git-crypt filter rules
├── .github/workflows/
│   └── deploy-pages.yml      # Deploy public/ to GitHub Pages
├── Makefile                  # make html/pdf DIR=... THEME=...
├── package.json              # marp-cli + markdown-it-cjk-friendly
├── presentations/            # git-crypt encrypted (private source content)
│   ├── index.md              # ID-to-name mapping (encrypted)
│   └── p{NNN}/               # Opaque presentation ID
│       ├── sections/         # Slide files (00.md, 01.md, ...)
│       ├── research/         # Research docs (r00.md, r01.md, ...)
│       ├── images/           # Figures and generated images
│       └── synopsis.md       # Topic and structure outline
├── public/                   # Deployed HTML + PDF (served by GitHub Pages)
├── docs/
│   └── guide.md              # Layout class reference
├── engine/
│   ├── marp.config.js        # Marp engine config
│   └── scripts/              # assemble, cite, mermaid, variant, theme merge, postprocess
├── themes/bai-flat/          # CSS theme + assets
└── .claude/                  # Skills, agents, rules, output style
```

## Encryption

Presentation source content (`presentations/`) is encrypted via git-crypt. On GitHub the files appear as encrypted blobs. Locally, after `git-crypt unlock`, they are plaintext.

- `public/` is NOT encrypted (intentionally public artifacts; folder names there may reveal topic and that is acceptable)
- To set up on a new machine: `make setup` (installs git-crypt, gnupg, npm deps), then import the GPG key and `git-crypt unlock`
- Backup symmetric key is at `~/.gnupg/slide-engine-git-crypt.key`

## Privacy (CRITICAL)

The git-crypt encryption is meaningless if the topic leaks through unencrypted surfaces. **Never write what a deck is about anywhere outside `presentations/` and `public/`.**

Unencrypted surfaces that MUST stay opaque:

- **Commit messages**: refer to decks by opaque ID only (`p007`), never by topic. Bad: `Add p007: CXL KVCache offload benchmark deck`. Good: `Add p007` or `Update p007 sections`.
- **Branch names**: use IDs or generic verbs (`p007-edits`, `fix-build`), never topic keywords.
- **PR titles and bodies**: same rule. Topic words belong inside the encrypted files only.
- **File names outside `presentations/`**: scripts, configs, issues should not embed topic strings.
- **Tags, release notes, anything pushed to the remote**: opaque IDs only.

The `/commit` skill and any commit message you author MUST follow this. If unsure whether a word leaks the topic, omit it.

## Build

```bash
make html    DIR=presentations/{name} THEME=bai-flat
make pdf     DIR=presentations/{name} THEME=bai-flat
make html-wl DIR=presentations/{name} THEME=bai-flat  # whitelabel
make pdf-wl  DIR=presentations/{name} THEME=bai-flat  # whitelabel
```

## Skills

| Skill | Usage |
|-------|-------|
| `/new-presentation <topic>` | Scaffold `presentations/{name}/` |
| `/list-presentations` | List presentations with deploy/PDF status |
| `/generate-slides [name]` | Generate slides from synopsis |
| `/build [format] [name]` | Compile slides |
| `/research <source>` | Add research docs |
| `/fetch-image <url>` | Download image to images/figures/ |
| `/deploy [name]` | Build + push HTML (+ optional PDF) to Pages |
| `/inspect [slide] [name]` | Visual screenshot + analysis |
| `/export-notes [name]` | Extract speaker notes |
| `/commit` | Git commit |

## Output style

This project uses the `concise` output style (`.claude/output-styles/concise.md`). Terse, fragment-first voice for chat, comments, commits, docs. Covers both English and Korean. Subagents import it via `@.claude/output-styles/concise.md`.

## Delegation policy

When a skill or agent exists for a task, delegate to it.
