---
name: inbox-triage
description: "Suggests filing locations for Obsidian inbox notes"
user_invocable: true
version: "1.0.0"
source: custom
---

# Inbox Triage

Review notes sitting in the Obsidian inbox and suggest where each should be filed.

## Steps

1. **List all files** in `hunter obsidian/inbox/` (exclude `attachments/` subfolder and `inbox.md` itself).
2. **Read each note** to understand its content and purpose.
3. **Present a triage table** with suggested destinations:

```
| note | suggested folder | reason |
|------|-----------------|--------|
| design-resources.md | career/ | design reference material |
| ski-2026.md | places/travel/ | trip planning |
```

4. **Wait for approval.** Do not move anything until the user confirms. They may:
   - Approve all moves
   - Approve some, reject others
   - Override destinations
   - Say "leave it" for notes that should stay in inbox
5. **Move approved notes** using the file system (never `rm`, always move).
6. **Update any wikilinks** across the vault that reference moved files.

## Vault Folders

- `self/` — goals, habits, vision, fashion, ancestry, writing, wishlist
- `second brain/` — dashboard, top of mind, quick access
- `people/` — family, friends, creators, public figures
- `career/` — work, internships, design, code
- `college/` — semesters, courses, advising
- `software/` — tools, apps, AI, dev, automation
- `places/` — locations, travel (use subfolders like `places/travel/EU/`)
- `projects/` — active builds, ideas, archive
- `movies shows & music/` — movies, shows, music, streaming
- `gaming/` — games, hardware
- `finance/` — investing, crypto, taxes
- `ideas/` — creative sparks
- `studio alpine/` — brand work

## Rules

- Never auto-move without confirmation.
- Never delete notes — a filename alone has value.
- Skip school notes unless user explicitly asks to process them.
- If a note could go in multiple folders, list the options and let the user decide.
- Notes with a Red Finder tag are LOCKED — skip them entirely.
- If inbox has fewer than 5 notes, just list them with suggestions inline (no table needed).
- After moving, remind user to reload Obsidian if they want to see changes immediately.
