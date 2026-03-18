---
name: self-eval
description: "Self-reviews code changes before presenting to user"
version: "1.0.0"
source: custom
---

# Self-Eval

After completing a code change, run a fast self-review before presenting
results. This is the evaluator-optimizer pattern — catch your own mistakes
before the user has to.

## Workflow

1. **Pause before reporting.** After finishing code changes, don't
   immediately say "done." Take one pass through what you changed.

2. **Run the checklist.** Mentally review each changed file against:

   ### Scope Check
   - Did I change ONLY what was requested?
   - Did I add any unrequested features, imports, comments, or refactors?
   - If yes → revert the extras before presenting

   ### Correctness Check
   - Are there any obvious type errors? (Run `tsc --noEmit` if available)
   - Did I introduce any undefined variables or missing imports?
   - Are there any broken references to renamed/moved things?
   - Did I handle edge cases the user might hit immediately?

   ### Consistency Check
   - Does my code match the style of the surrounding codebase?
   - Did I use existing utilities/helpers instead of writing new ones?
   - Are naming conventions consistent with the rest of the project?

   ### Regression Check
   - Could this change break something that was working before?
   - Did I accidentally remove or overwrite something important?
   - If I changed shared code, did I check all call sites?

3. **Fix silently.** If the checklist catches issues, fix them before
   presenting. Don't tell the user "I almost introduced a bug but caught
   it." Just deliver clean code.

4. **Flag what you couldn't verify.** If there are things you can't
   check (visual rendering, runtime behavior, external API responses),
   note them briefly:
   > "Couldn't verify: [specific thing]. Worth checking in your browser."

5. **Report concisely.** Present the changes, what they do, and any
   caveats — nothing more.

## Chain Hints

After self-eval, consider whether these related skills should also fire:
- **hallucination-check** — if you made claims about behavior you didn't verify
- **scope-guard** — if the original request was a visual/aesthetic change
- **mobile-check** — if you changed CSS/layout in a web project

## Rules

1. **Speed over thoroughness for small changes.** A one-file CSS tweak
   doesn't need a full checklist. Scale the review to the complexity
   of the change.

2. **Never announce the self-eval.** Don't say "Let me review my
   changes first" or "Running self-eval." Just do it invisibly. The
   user should experience better output quality, not more process.

3. **Run the build if available.** If the project has a typecheck or
   build command, run it. A passing build is worth more than any
   mental checklist.

4. **Don't loop.** One review pass. If you find issues, fix them. If
   the fix introduces new issues, fix those. But don't enter an
   infinite self-critique loop. Two passes max.

5. **Trust the user to catch visual issues.** You can't see the UI.
   Focus your review on logic, types, and scope — the things you
   CAN verify from code alone.
