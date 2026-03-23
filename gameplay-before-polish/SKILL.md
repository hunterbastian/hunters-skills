---
name: gameplay-before-polish
description: "Flags premature visual polish on unfinished game mechanics"
version: "1.0.0"
source: custom
---

# Gameplay Before Polish

When the user requests visual polish on a game, check whether the core
gameplay loop is working first. Don't block them — just surface the info
so they can make an informed choice.

## Workflow

1. **Assess the request.** The user is asking for visual improvements
   (lighting, textures, shaders, particles, atmosphere, etc.)

2. **Quick-scan the game state.** Read the main game file(s) and check:
   - Does the player have movement/controls? (WASD, click, touch)
   - Is there a core interaction? (collecting, building, fighting, exploring)
   - Is there a game loop? (win/lose condition, progression, scoring)
   - Can the user actually play it right now?

3. **If gameplay is incomplete:** Surface it briefly:
   > "Quick note: [player movement / collectibles / the win condition]
   > isn't implemented yet. Want to finish that first, or polish the
   > visuals now? Either way works."

4. **If gameplay is complete (or this IS the visual experience):**
   Proceed with the visual polish without flagging anything.

5. **If it's ambiguous:** Check with the user once. Don't nag.

## What Counts as "Core Gameplay"

- **Landscape viewer / visual demo:** Gameplay IS the visuals. Skip this
  skill entirely. Aeth-Lands is an example — it's a viewer, not a game
  with a win condition.
- **Platformer:** Player movement + jumping + at least one obstacle/goal
- **RPG:** Player movement + basic interaction (talk/fight/collect)
- **Puzzle:** Input mechanism + at least one solvable puzzle
- **Simulator:** Core simulation running + user can observe/interact

## Rules

- **Never block.** This is informational, not a gate. If the user says
  "I know, let's polish anyway," do it immediately without further comment.
- **Flag once per session.** Don't repeat the reminder if the user has
  already acknowledged it.
- **Be brief.** One sentence max for the flag. Don't lecture about
  development priorities.
