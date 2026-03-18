---
name: plan-first
description: Nudges into plan mode before non-trivial implementation tasks
auto_trigger: true
trigger: |
  Auto-activates when the user requests a task that involves 5+ steps,
  multi-file changes, architectural decisions, new features, or refactors.
  Use this skill when the task clearly goes beyond a single-file quick edit.
  Do NOT trigger for: simple tweaks (color change, text edit, one-liner fix),
  single-file edits under ~20 lines, questions/research, or tasks the user
  explicitly says to "just do".
version: "1.0.0"
source: custom
---

# Plan First

Before jumping into implementation on non-trivial tasks, enter plan mode to
align on approach.

## When to activate

- Task touches 3+ files
- Task involves architecture or structural decisions
- Task has multiple valid approaches
- Task is a new feature (not a tweak to an existing one)
- Task involves unfamiliar code you haven't read yet

## When to skip

- Single-file edits under ~20 lines
- Direct bug fixes with obvious solutions
- User says "just do it" or "quick change"
- Pure content/text changes
- Tasks where you've already planned in this session

## Behavior

1. Briefly state what you're about to build and your planned approach (3-5 bullets max)
2. Call out any decisions where the user's input matters
3. Wait for a go-ahead before writing code
4. Keep it lightweight — this is a quick alignment check, not a design doc

## Rules

- Do NOT write a lengthy plan. Short bullets only.
- Do NOT ask unnecessary clarifying questions — make reasonable assumptions and note them.
- If the user seems eager to move fast, keep the plan to 2-3 lines.
- Never block progress — if the plan is obvious, state it and start building in the same message.
