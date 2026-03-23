---
name: stale-note-nudge
description: "Surfaces Obsidian inbox notes older than 30 days"
version: "1.0.0"
source: custom
---

# Stale Note Nudge

Find inbox notes that have been sitting unprocessed and gently surface them.

## Steps

1. **List all files** in `hunter obsidian/inbox/` (exclude `attachments/`, `inbox.md`, and `saved-links.md`).
2. **Check modification dates** for each file using `stat` or `ls -la`.
3. **Filter to stale notes:**
   - 30-60 days old → "aging" (worth a look)
   - 60-90 days old → "stale" (probably forgotten)
   - 90+ days old → "buried" (needs a decision)
4. **Present a short summary** grouped by staleness:

```
buried (90+ days)
  · design-resources.md — last touched Nov 2025
  · portfolio-redesign.md — last touched Oct 2025

stale (60-90 days)
  · crypto.md — last touched Jan 2026

aging (30-60 days)
  · content-ideas.md — last touched Feb 2026
```

5. **For each note**, briefly say what's in it (read first few lines) so the user can decide without opening it.
6. **Suggest action** for each: file it (suggest folder), archive it, or leave it.

## Rules

- Keep the output short and scannable — this runs inside other skills.
- Don't nag about notes under 30 days old.
- Don't move or modify anything — just surface and suggest.
- If no stale notes exist, say "inbox is fresh" and move on.
- When running inside `/weekly-review`, keep to 5 lines max unless there are buried notes.

## Chain Hints

- If the user wants to act on suggestions, hand off to **inbox-triage** for the actual moves.
- Works well embedded in `/weekly-review` and `/daily-digest`.
