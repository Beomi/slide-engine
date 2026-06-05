---
name: commit
description: Create git commits following conventional commit standards.
---

# Commit

Create a git commit following project conventions.

## Workflow

1. Run `git status` and `git diff` (staged + unstaged) to understand the current state.
2. Run `git log --oneline -5` to see recent commit style for consistency.
3. If nothing is staged, identify and stage the relevant files. Ask the user if the scope is unclear.
4. Draft a commit message: short subject line (imperative mood), optional body for context.
5. **Privacy check**: commit messages, subject AND body, MUST NOT reveal any presentation topic. Refer to decks only by opaque ID (`p007`). See CLAUDE.md "Privacy (CRITICAL)" section. Bad: `Add p007: CXL KVCache offload deck`. Good: `Add p007`.
6. Create the commit.
7. If pre-commit hooks fail: read the hook output, fix the issues, re-stage, and create a new commit. Do not use `--no-verify`.
8. Run `git status` to verify success.
9. Print the full commit message to the user.

$ARGUMENTS
