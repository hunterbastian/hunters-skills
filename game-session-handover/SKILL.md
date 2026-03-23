---
name: game-session-handover
description: "Writes HANDOVER.md when a game dev session ends"
version: "1.0.0"
source: custom
---

# Game Session Handover

When a game dev session ends, write a structured handover log so the next
session starts with full context instead of spending 10 minutes catching up.

## Workflow

1. **Detect session end.** The user signals they're stopping, or a natural
   milestone has been reached on a game project.

2. **Identify the project.** Determine:
   - Project name and directory
   - What kind of game (3D, 2D, browser, etc.)
   - Tech stack (Three.js, Vite, TypeScript, etc.)

3. **Write the handover log.** Create or update a file called
   `HANDOVER.md` in the project root directory with this structure:

   ```markdown
   # Session Handover
   Last updated: [YYYY-MM-DD HH:MM]

   ## Project
   [Project name] — [one-line description]
   Directory: [path]

   ## What Was Built This Session
   - [Feature/change 1]
   - [Feature/change 2]
   - ...

   ## Current State
   - What works: [list working features]
   - What's broken: [list known bugs, if any]
   - What it looks like: [brief description of current visual state]

   ## Next Steps
   1. [Most important next task]
   2. [Second priority]
   3. [Third priority]

   ## Key Files
   - [file1.ts] — [what it does / what was changed]
   - [file2.ts] — [what it does / what was changed]

   ## Decisions Made
   - [Any architectural or design decisions that future-you needs to know]

   ## Blockers / Open Questions
   - [Anything unresolved that needs the user's input]
   ```

4. **Confirm to the user.** Tell them the handover was saved and what the
   top next step would be when they resume.

## Rules

- **Overwrite, don't append.** Each session writes a fresh `HANDOVER.md`.
  Old session context is no longer relevant — only the latest state matters.
- **Be specific about files.** Don't say "the main game file." Say
  `src/world.ts` — the next session needs exact paths.
- **Include the visual state.** "The meadow biome renders with green terrain,
  scattered flowers, and a blob character. Mushroom Grove is too dark."
  This saves the next session from needing to figure out what things look like.
- **Keep it under 50 lines.** This is a quick briefing, not documentation.
- **Don't block the user.** Write the handover quickly and let them go.
  Don't ask clarifying questions about what to include — use your best
  judgment from the session.

## At Session Start (For Future Claude)

When starting a new session on a game project, check for `HANDOVER.md` in
the project root. If it exists, read it first before doing anything else.
This is your shift briefing.
