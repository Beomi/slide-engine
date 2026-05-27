---
name: list-presentations
description: List all presentations and their build/deploy status. Usage: /list-presentations
---

# List Presentations

Show all presentations with their human-readable names and deploy status.

## Workflow

1. Read `presentations/index.md` for the ID-to-name mapping.
2. For each entry, check:
   - `public/{name}/index.html` exists (deployed HTML)
   - `public/{name}/slides.pdf` exists (deployed PDF)
3. Display a table with columns: ID, Name, Sections Count, HTML Deployed, PDF Deployed.
