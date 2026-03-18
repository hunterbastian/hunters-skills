---
name: continue
description: "Pick up where you left off on any project — reads state and resumes"
user_invocable: true
version: "1.0.0"
source: custom
---

# Continue

Read all available project context and resume where the last session left off.
One command to go from cold start to productive.

## Steps

1. **Identify the project.** Use the current working directory. If at `~`,
   ask which project to continue.

2. **Gather context.** Read these sources in parallel (skip any that don't exist):

   | Source | What it tells you |
   |--------|-------------------|
   | `CLAUDE.md` | Project structure, tech stack, conventions |
   | `HANDOVER.md` | What was built last, current state, next steps |
   | Auto-memory | Persisted project state and decisions |
   | `git log --oneline -15` | Recent commits — what actually shipped |
   | `git status` | Uncommitted work in progress |
   | `git diff --stat` | What's been changed but not committed |
   | Task list (if any) | Active, blocked, and completed tasks |

3. **Synthesize a briefing.** Present a short summary:

   ```
   ## [Project Name]

   **Last session:** [1-2 sentences on what was done]
   **Current state:** [what works, what's broken]
   **In progress:** [uncommitted changes, if any]

   **Next up:**
   1. [Top priority from HANDOVER.md or task list]
   2. [Second priority]
   3. [Third priority]

   Ready to pick up on #1?
   ```

4. **Wait for direction.** The user might confirm the top priority, pick
   a different task, or give new instructions. Follow their lead.

## Rules

- **Read everything before speaking.** Don't present a partial briefing
  then read more. Gather all context first, then give one clean summary.
- **Prioritize HANDOVER.md over git log.** The handover was written with
  intent — git log is just a record of commits. If they conflict, trust
  the handover.
- **Don't re-explain the project.** The user knows what their project is.
  Focus on state and next steps, not architecture overviews.
- **Keep the briefing under 15 lines.** This is a quick catch-up, not a
  status report. The user wants to start working, not reading.
- **If nothing exists** (no HANDOVER.md, no memory, fresh project), say:
  > "No previous session context found. What are we working on?"
- **Don't modify anything.** This skill only reads. No edits, no commits,
  no file creation. Pure reconnaissance.

## Works well with

- **game-session-handover** — writes the HANDOVER.md that `/continue` reads
- **plan-first** — after resuming, if the next task is complex
- **scope-check** — before starting the next feature
