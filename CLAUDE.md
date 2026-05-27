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

- `public/` is NOT encrypted (intentionally public artifacts)
- To set up on a new machine: import the GPG key, clone, then `git-crypt unlock`
- Backup symmetric key is at `~/.gnupg/slide-engine-git-crypt.key`

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
