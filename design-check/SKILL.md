---
name: design-check
description: "Auto-checks UI detail patterns after visual code changes"
version: "1.0.0"
source: custom
---

# Design Check

After writing or editing UI code, scan for missed design detail patterns.
These are small things that compound into the difference between "good" and
"great." Only speak up when something concrete can be improved.

## Workflow

1. **Scan the changed code** for these patterns:

   ### Border Radius
   - Are there nested elements with border-radius? Check for concentric
     radius: outer radius must equal inner radius + padding. Mismatched
     nested radii always look wrong.

   ### Shadows vs Borders
   - Are solid `border` styles used where a multi-layer `box-shadow` would
     add more depth? Shadows adapt to any background; solid borders don't.
   - Recommended pattern:
     ```css
     box-shadow:
       0px 0px 0px 1px rgba(0,0,0,0.06),
       0px 1px 2px -1px rgba(0,0,0,0.06),
       0px 2px 4px 0px rgba(0,0,0,0.04);
     ```

   ### Text
   - Short headings or labels without `text-wrap: balance`?
   - Dynamic numbers updating without `tabular-nums` /
     `font-variant-numeric: tabular-nums`?
   - Missing `antialiased` on body/layout? (Check once per project)

   ### Images
   - Images without the inset outline for depth?
     `outline: 1px solid rgba(0,0,0,0.1); outline-offset: -1px;`
   - In dark mode: `rgba(255,255,255,0.1)`

   ### Animation
   - Enter and exit animations with equal intensity? Exits should be
     subtler — small fixed offset (e.g., `-12px`) instead of full height.
   - Animations that aren't interruptible? CSS transitions for interactions,
     keyframe animations for one-shot sequences.
   - Large blocks animating as one? Consider splitting and staggering
     (opacity + blur + translateY, ~100ms delay between chunks).
   - Icon swaps (e.g., copy/check) without contextual animation
     (opacity + scale + blur)?

   ### Alignment
   - Buttons with text + icon where padding is equal on both sides?
     The icon side usually needs slightly less padding for optical balance.
   - Any geometric alignment that looks off in context?

2. **Filter to actionable items only.** If the code already handles a
   pattern correctly, don't mention it. If nothing is missing, stay
   completely silent.

3. **Present concisely.** List only the misses, with the specific file
   and line, what to change, and why it matters. Keep it to 1-3 items
   max — don't overwhelm.

## Rules

1. **Stay silent if nothing is wrong.** The user should only notice this
   skill when it catches something real. No "everything looks good!"
   messages. No participation trophies.

2. **Never fix things automatically.** Present the suggestions and let
   the user decide. These are taste calls, not bugs.

3. **Don't repeat scope-guard's job.** This skill suggests improvements
   to code that was just written. It does NOT suggest changes to
   unrelated code in the same file.

4. **Scale to the change.** A one-line color tweak doesn't need a full
   design audit. Only check patterns relevant to what was actually
   written.

5. **Respect the project's existing patterns.** If the project uses
   `rounded-[3px]` everywhere, don't suggest changing to `rounded-lg`.
   Work within the established system.

6. **No false positives.** Only flag something if you're confident it's
   a genuine improvement. When in doubt, stay quiet.

## Chain Hints

After design-check, consider:
- **self-eval** — if the change was non-trivial (3+ files)
- **mobile-check** — if CSS/layout was modified in a web project

## When NOT to Apply

- Logic-only changes (API routes, data fetching, state management)
- Config files (tailwind.config, next.config, tsconfig)
- Content-only text changes (copy, markdown)
- When the user explicitly says to skip review
- When /simplify or /rams was already run on the same changes
