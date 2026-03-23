---
name: session-value-log
description: "Logs concrete session value when user wraps up"
version: "1.0.0"
source: custom
---

# Session Value Log

At the end of a working session, generate a concise value receipt showing
what was accomplished in concrete, measurable terms. This builds the user's
portfolio of evidence for the value AI brings to their work.

## Workflow

1. **Detect session end.** The user signals they're wrapping up, or asks
   for a summary of what was accomplished.

2. **Gather evidence.** Check:
   - `git diff --stat` for files changed (if in a git repo)
   - `git log --oneline` for commits made this session
   - Your own conversation history for tasks completed

3. **Generate the value receipt.** Output in this format:

   ```
   ── Session Receipt ─────────────────────────────
   Date: [YYYY-MM-DD]
   Project: [name]
   Duration: [approximate, based on conversation length]

   ## Shipped
   - [Feature/fix 1] → [concrete outcome]
   - [Feature/fix 2] → [concrete outcome]

   ## Time Saved
   - [Task that would have taken X hours manually] → [done in Y minutes]

   ## Decisions Made
   - [Decision 1]: chose [option] because [reason]

   ## Skills/Tools Built
   - [Any reusable skills, configs, or automations created]

   ## Commits
   - [hash] [message]
   - [hash] [message]
   ─────────────────────────────────────────────────
   ```

4. **Append to the running log.** Create or append to
   `~/.claude/session-logs/value-log.md` with a condensed version:

   ```markdown
   ## [YYYY-MM-DD] — [Project Name]
   - [1-line summary of each item shipped]
   - Commits: [count]
   - Files changed: [count]
   ```

## Rules

1. **Be concrete, not vague.** "Built 3 auto-triggered skills" not
   "worked on skills." "Fixed mobile layout at 390px viewport" not
   "improved mobile."

2. **Quantify when possible.** Number of files, commits, features,
   bugs fixed. Hard numbers build the case.

3. **Don't inflate.** If the session was a short fix, the receipt should
   be short. Don't pad it to look more impressive.

4. **Include decisions.** Decisions made are value delivered, even when
   no code was written. Choosing NOT to do something is also a decision
   worth documenting.

5. **Keep the running log lean.** The appended log entry should be 3-5
   lines max per session. The full receipt is shown once in conversation;
   the log is the compressed archive.

6. **Skip for game projects.** Game projects use game-session-handover
   instead. Don't double up.

7. **Create the log directory silently.** If `~/.claude/session-logs/`
   doesn't exist, create it without mentioning it.
