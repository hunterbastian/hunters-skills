---
name: skill-observer
description: "Logs skill execution outcomes for self-improvement tracking"
user_invocable: false
version: "2.0.0"
source: custom
---

# Skill Observer

The observation layer for self-improving skills. Records execution data
so patterns of failure can be detected and skills can be amended.

## How It Works

**Creation** happens automatically via a PostToolUse hook on the Skill tool.
Every skill invocation writes a `pending` entry to `~/.claude/skill-logs/YYYY-MM.jsonl`.

**Resolution** happens two ways:
1. **Stale auto-resolve**: At session start, run `bash ~/.claude/scripts/resolve-skill-log.sh stale` to resolve entries older than 1 hour as `success` (if no correction came, it worked).
2. **Correction detection**: When you detect the user correcting a skill's output, immediately run:
   ```bash
   bash ~/.claude/scripts/resolve-skill-log.sh corrected <skill-name> "<what was wrong>"
   ```

## When to Resolve as Corrected

A "correction" happens when, shortly after a skill runs, the user:
- Says "no", "don't", "stop", "that's not what I asked"
- Gives feedback that contradicts the skill's output
- Manually reverts or redoes what the skill produced
- Adds a feedback memory about the skill's behavior

When this happens, run the resolve script immediately:
```bash
bash ~/.claude/scripts/resolve-skill-log.sh corrected scope-guard "triggered on new feature, should only fire on modifications"
```

## Session Start Routine

At the start of every session, resolve stale entries:
```bash
bash ~/.claude/scripts/resolve-skill-log.sh stale
```
This ensures entries from previous sessions don't stay pending forever.

## Log Entry Format

```json
{
  "skill": "scope-guard",
  "timestamp": "2026-03-14T10:30:00Z",
  "trigger": "auto",
  "outcome": "success",
  "correction": null,
  "failure_type": null
}
```

### Outcome Values
- `pending` — just fired, waiting for resolution
- `success` — skill ran and user didn't correct it
- `corrected` — user gave negative feedback or reverted the output
- `skipped` — skill should have fired but didn't (log manually when noticed)

### Failure Types (set when corrected)
- `routing` — wrong skill triggered, or right skill didn't trigger
- `instruction` — right skill, but instructions produced bad output
- `tooling` — skill was fine, but a tool or environment assumption broke

## Amendment Trigger

When a skill accumulates **3+ corrections** within a 2-week window,
it's a candidate for amendment:

1. Gather all correction entries for the skill
2. Identify the dominant failure type
3. Propose a specific SKILL.md change
4. Present evidence and proposed change to the user
5. Never auto-apply — user reviews and approves

## Rules

1. **Log silently.** Never tell the user "I'm logging this."
2. **Be honest about outcomes.** If the user wasn't happy, that's corrected.
3. **One log file per month.** `~/.claude/skill-logs/YYYY-MM.jsonl`
4. **Amendments require evidence.** 3+ corrections minimum.
5. **Resolve stale entries at session start.** Don't let pending pile up.
