---
name: scope-guard
description: "Prevents over-delivery on visual and aesthetic changes"
version: "1.0.0"
source: custom
---

# Scope Guard

When the user asks for a visual or aesthetic change, make ONLY that change.
Nothing else. This is the single most important behavioral rule.

## Rules

1. **Only change what was explicitly requested.** If the user says "make the
   button blue," change the button color to blue. Do not also adjust its
   padding, font size, border radius, shadow, hover state, or position.

2. **Do not add unrequested features.** A color change is not an invitation
   to add animations. A font swap is not an invitation to restructure the
   typography scale. A spacing tweak is not an invitation to refactor the
   layout system.

3. **Do not "improve" nearby code.** If you notice something else that could
   be better in the same file, leave it alone. The user will ask for those
   changes if and when they want them.

4. **Do not add comments, docstrings, or type annotations** to code you
   didn't change as part of the requested modification.

5. **Confirm scope before acting if ambiguous.** If the request could mean
   multiple things ("make it look better"), ask what specifically should
   change rather than making broad assumptions.

6. **Report only what you changed.** After making the change, state the
   specific modification in one sentence. Do not add suggestions for
   additional improvements unless the user asks "what else could we do?"

## Examples

### Good
User: "Make the header font bigger"
Action: Change the header font-size class. Nothing else.

### Bad
User: "Make the header font bigger"
Action: Change the header font-size, also adjust line-height, also add
letter-spacing, also tweak the mobile breakpoint, also update the color
to "feel more balanced."

### Good
User: "Change the card background to a lighter shade"
Action: Update the background color value. Nothing else.

### Bad
User: "Change the card background to a lighter shade"
Action: Update the background, also add a subtle shadow, also round the
corners more, also add a hover effect "while we're at it."

## Chain Hints

After applying scope-guard, consider:
- **self-eval** — if the change touched 3+ files, run the self-review checklist
- **mobile-check** — if the change modified CSS/layout in a web project
- **hallucination-check** — if you claimed the change "fixes" something without verifying

## When NOT to Apply

- When the user explicitly asks for a broader change ("redesign this section",
  "overhaul the styling", "make this page look completely different")
- When the user asks you to use a skill like /interface-craft or /rams that
  inherently involves broader review
- When the user says "and anything else you think looks good" or similar
  open-ended permission
