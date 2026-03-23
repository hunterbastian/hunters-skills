---
name: scope-check
description: "Verifies a requested feature or task fits the project plan before writing code"
auto_trigger: true
trigger: |
  Auto-activates when the user requests a new feature, significant change, or
  multi-step task on a project that has a CLAUDE.md with a roadmap, task list,
  or defined scope. Do NOT trigger for: bug fixes, one-liner tweaks, questions,
  research, or tasks the user explicitly says to "just do".
version: "1.0.0"
source: custom
---

# Scope Check

Before starting a non-trivial feature, verify it fits the project's agreed
plan. This prevents drift — building things that sound useful but weren't
part of the roadmap.

## When to activate

- User requests a new feature or significant addition
- Project has a CLAUDE.md, ROADMAP.md, or task list that defines scope
- The request could plausibly be out of scope or a tangent

## When to skip

- Bug fixes (the bug exists, it needs fixing)
- Direct tweaks to existing features (color, text, spacing)
- User says "just do it" or "quick addition"
- No project plan exists to check against
- The feature is explicitly listed in the project's roadmap/tasks

## Behavior

1. **Read the project plan.** Check CLAUDE.md, HANDOVER.md, ROADMAP.md,
   or any task list in the project root for defined scope, roadmap, or
   current priorities.

2. **Compare the request.** Does this feature:
   - Appear in the roadmap or task list? → Proceed, no gate needed.
   - Extend an existing planned feature? → Proceed, note the extension.
   - Introduce something entirely new? → Flag it.

3. **If flagging**, say it briefly:
   > "This isn't in the current roadmap. Want me to add it as a task and
   > build it, or skip for now?"

   Don't lecture. Don't list reasons. One sentence, then wait.

4. **If the user confirms**, proceed and update the task list to include it.

5. **If no plan exists**, skip the check entirely. You can't gate against
   a plan that doesn't exist.

## Rules

- **Never block enthusiastically requested work.** If the user is excited
  about a feature, the scope check is informational, not a gate. Build it.
- **Never say "this is out of scope" as a refusal.** You're flagging, not
  refusing. The user always decides.
- **Keep it invisible when things match.** If the feature is in the plan,
  don't announce "scope check passed." Just build.
- **One sentence max for the flag.** Don't explain why scope matters or
  what the roadmap says. The user wrote the roadmap — they know.

## Chain Hints

After scope-check passes, the normal flow continues:
- **plan-first** — if the task is complex enough to need a plan
- **scope-guard** — during implementation, to prevent over-delivery
