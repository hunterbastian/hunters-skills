---
name: note
description: Create a new Obsidian note with vault conventions
user_invocable: true
version: "1.0.0"
source: custom
---

# Create Obsidian Note

Create a note in the user's Obsidian vault at `~/Documents/Obsidian Vault/hunter obsidian/`.

## Steps

1. **Parse the request.** Extract the title, content, and optionally a folder and tags from the user's message.
2. **Determine the folder.** Match to an existing vault folder. If unclear, use `inbox/`. Never create new top-level folders without asking.
3. **Generate the filename.** Use `lowercase-with-dashes.md` (e.g., "My Cool Note" → `my-cool-note.md`).
4. **Write the file** with this structure:

```markdown
---
title: {{Note Title}}
date: {{YYYY-MM-DD}}
tags: [{{comma-separated tags if provided}}]
---

{{Content using · (middle dot) as separator, never • (bullet)}}
```

5. **Confirm** the file path and a one-line summary. Do not open the file or try to reload Obsidian.

## Rules

- Always use `·` (middle dot U+00B7) as a list/separator character, never `•` (bullet).
- Always use lowercase-with-dashes for filenames and folder names.
- Never edit CLAUDE.md when asked to create a note.
- Do not reorganize existing notes or folders unless explicitly asked.
- If the user dictates content loosely, clean it up into coherent prose/bullet points but preserve their meaning exactly.

## Vault Folders

archive, assets, books, career, claude, college, finance, gaming, health, inbox, journal, library, movies shows & music, obsidian setup, photography, places, projects, second brain, self, software, studio alpine
