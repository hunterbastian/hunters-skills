---
name: uat-checklist
description: "Generates a browser testing checklist after building visual or interactive features"
auto_trigger: true
trigger: |
  Auto-activates after completing a feature build that involves UI, visual
  changes, interactive behavior, or game mechanics — anything where automated
  tests can't fully verify the result. Especially relevant for Three.js games,
  web apps, and any project without screenshot testing.
  Do NOT trigger for: backend-only changes, config edits, documentation,
  refactors with no visual impact, or single-line CSS tweaks.
version: "1.0.0"
source: custom
---

# UAT Checklist

After building something visual or interactive, generate a structured
checklist the user can follow in their browser to verify it works.

## Why this exists

Automated tests catch logic errors. Screenshots don't work for WebGL.
What's left is the user manually checking — but without a list, things
get missed. This skill turns "looks good to me" into a systematic check.

## When to activate

- Just finished building a UI feature or visual change
- Just finished implementing game mechanics or interactions
- The project is a Three.js game, web app, or anything with a browser UI
- Changes affect layout, animation, responsiveness, or user interaction

## When to skip

- Pure backend/API changes with no UI
- Single-property CSS tweaks (one color, one font size)
- Documentation or config changes
- User explicitly says "don't need to test this"

## Behavior

1. **Review what was built.** Look at the files changed and the feature
   scope to understand what needs manual verification.

2. **Generate the checklist.** Write 3-8 specific, actionable items:

   ```
   ## Test checklist

   - [ ] [Specific thing to check — what to look for]
   - [ ] [Another specific thing — include expected behavior]
   - [ ] [Edge case worth trying]
   - [ ] [Mobile/resize behavior if applicable]
   ```

3. **Be specific, not generic.** Bad: "Check that the page looks right."
   Good: "Click the card — it should flip with a 0.3s animation and show
   the back content."

4. **Include the how.** If testing requires specific steps (resize to
   mobile width, open devtools, press a key), say so.

5. **Present after the build summary.** The checklist comes after you
   report what was built, not before.

## Checklist categories to consider

- **Happy path** — does the feature work as intended?
- **Edge cases** — empty states, long text, rapid clicks, resize
- **Responsiveness** — does it work on narrow viewports?
- **Interaction** — hover states, focus indicators, keyboard navigation
- **Performance** — any visible jank, lag, or layout shift?
- **Game-specific** — camera behavior, collision, controls feel right?

## Rules

- **3-8 items max.** More than 8 and the user won't do it. Prioritize
  the items most likely to catch real issues.
- **No obvious items.** Don't include "open the page" or "check it loads."
  The user will do that anyway.
- **Tailor to the project.** A Three.js game needs different checks than
  a Next.js marketing page. Use project context.
- **Don't block on the checklist.** Present it and move on. The user
  tests when they're ready — don't ask "did you test it?"
- **If the user reports an issue**, fix it and regenerate only the
  relevant checklist items, not the whole list.

## Chain Hints

UAT checklist naturally follows:
- **self-eval** — code quality was already checked
- **scope-guard** — scope was already enforced
- Now the user checks the actual result in their browser
